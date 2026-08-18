# G8-UAT-02｜Independent Review 阻塞发现 v1.0

状态：`CURRENT REVIEW FINDING / RETURN REQUIRED`
日期：2026-08-18
代码对象：`sillytavern@2c7e6a4cd85e1f3c52350c1b85ae70c99864b940`
上游规格：`G8-UAT-02_GameLocalSemanticMaterialization与活世界收口规格_v1.0_2026-08-18.md`

## 1. 结论

```text
G8-UAT-02 Implementation = REVIEWED / RETURN REQUIRED
Independent Review       = FAIL
P0                        = 0 confirmed
P1                        = 5
Stage UAT                 = NOT AUTHORIZED
G8                        = ACTIVE / UAT FIX
G9                        = NOT AUTHORIZED
```

本次实现主方向正确，不要求推翻重做。已真实建立：Creation Field Semantic Audit、typed Creation Materialization、configured AI semantic materialization、concrete opening entities、Product Player Profile、Information/Journal 分离、Runtime JIT Place/NPC、Game-local provenance 与 Formal Turn 内原子 materialization。

但以下 5 个 P1 会破坏已冻结的 Save/Restore、Narrative Authority、Context Activation 或 Semantic Fidelity，因此必须返修后重新 Independent Review。

---

## 2. P1-01｜Save / Restore 没有跨 Game-local topology revision

### 现状

Runtime 已能在 Turn 中新增 Place / Scene / Connection / Character，但 Canonical Save Snapshot 仍主要保存旧式位置、持有、状态、连续性数据，并在 Restore 时要求 snapshot refs 与当前实体 refs `exactRefs`。

因此存在：

```text
Save at revision N
↓
revision N+1 materialize Tavern / NPC
↓
Restore revision N
```

旧 snapshot 与当前实体集合不再一致；旧存档可能失败，或即使部分恢复，后来 materialize 的 world topology 仍残留。

### 必修

Save/Restore 必须把 Game-local topology / asset revision 纳入 canonical snapshot 语义。

至少证明：

1. Save BEFORE materialization → materialize Tavern/NPC → Restore old save → Tavern/NPC 不再存在；
2. Save AFTER materialization → 后续移动/变化 → Restore → 同一 stable refs 恢复；
3. Restore old save 后产生新 Branch，可 materialize 不同未来，不产生 identity collision；
4. `game_local_asset` provenance ledger 与恢复后的实体集合一致，不残留 orphan provenance；
5. 整个 Restore 仍是 G6 atomic transaction。

---

## 3. P1-02｜World Materializer 被错误做成每 Turn 前置模型调用

### 现状

当前 FormalTurn Flow 在 configured world materializer 可用时，先调用 World Materializer，再调用 Semantic Provider。

后果：

- 普通 observe/dialogue/inner/read-only 等也会先调用 materializer；
- materializer transport/structure failure 会让本来不需要物化的普通 Turn 返回 `MODEL_UNAVAILABLE`；
- ordinary model-call count 随 materializer 固定增加；
- 违反 #15 v1.2 的 `Runtime Relevant != Model Visible` 与 outcome/need-gated activation。

### 必修

改为显式 need-gated activation：

```text
Player Input
→ Semantic / Router detects bounded unresolved local materialization need
→ only then World Materializer
→ Program validates/materializes
→ exact action candidate resolve / bounded semantic continuation if required
```

要求：

- ordinary observe/dialogue/inner/read-only/wait 在无需未知实体时 World Materializer calls = 0；
- materializer failure 不得污染 unrelated Turn；
- 不要用 Program 新造通用中文 NLP parser 判断 materialization need；
- materializer 仍只接 bounded current-local context。

---

## 4. P1-03｜Opening Beat 绕过 Narrative Authority，重新引入 Phantom

### 现状

Creation Materializer 直接输出 `openingBeat.narrative`，Player Session 把它原样作为 Turn 0 玩家可见 Narrative。

Program 当前只验证 openingBeat 声明的 Scene/Character/Item refs 合法，不验证 prose 中是否出现未 materialize 的 concrete interactable。

当前测试 fixture 已出现：

```text
“导师手里攥着一封被雨水打湿的信”
```

但 `itemLocalRefs = []`，canonical items 中没有这封信。

这就是 opening-turn Narrative-only phantom。

### 必修

推荐改为：

```text
Creation Materializer
→ structured OpeningBeat semantic plan
  - hook/summary
  - openingSceneRef
  - characterRefs
  - itemRefs
  - optional player-safe situation facts
→ Program validation
→ Narrative Provider realizes Turn 0
  under location/interactable authority + Player Agency + disclosure rules
```

Materializer 不再直接拥有 final player-visible prose authority。

Turn 0 必须遵守 G8-UAT-01：
- No Phantom Interactable；
- Player Agency；
- hidden/private disclosure boundary；
- concrete entity must resolve to canonical ref。

---

## 5. P1-04｜Creation private/public semantic class 未形成真正信息边界

### 现状

字段审计已把 `important_relationships` 等分类为 `private_seed`，但 configured Creation Materializer 仍收到一个扁平 `creatorAuthoredContext: Record<string,string>`。

因此 semantic class 只是审计 metadata，并未约束 Provider：private-only 原文理论上可以被模型复制进 publicDescription / openingBeat / public Place/Character 内容。

当前 hidden disclosure smoke 只检查生成后的 `privateSeed` 不进入 Product，没有证明 Creation private source marker 不被 public output echo。

### 必修

Materializer 输入必须成为 purpose-built typed/partitioned context：

```text
public authoring context
private seed context
world constraints
materialization briefs
```

至少保证：
- public output path 不直接暴露 private-only raw source；
- private seed 只进入 private canonical field；
- public Narrative / Product / publicDescription 不可回显 private marker。

新增 unique private marker regression：放入 private Creation field，断言 marker 不出现在任何 public materialization / opening / Product / Narrative。

---

## 6. P1-05｜Creation Item 缺少 holder / placement 语义

### 现状

`CreationMaterializedItemCandidate` 只有 name/description，没有 holder/location。

Adapter 因而把所有 materialized Item 都写成：

```text
initialHolderRef = playerRef
```

于是无法表达：
- NPC 手里的物品；
- Scene 中可见物品；
- 开场 hook 中属于别人或环境的物品。

这也直接诱发 P1-03 的“导师手里的信”无法 canonicalize。

### 必修

给 internal Creation Item plan 增加 typed placement/holder semantics，例如：

```text
holderKind = player | character | scene
holderLocalRef?
```

或等价的现有 Runtime 可表达结构。

Program 必须验证 holder ref；Adapter 正确绑定；Inventory 只显示 player-held items；Narrative opening 只能引用正确 holder/location 的 canonical Item。

---

## 7. 已确认可保留，不要重做

以下实现方向已通过独立检查，可在返修中保持：

- Creation Field Semantic Audit；
- bounded strict Creation Semantic Materializer；
- Program stable identity allocation；
- generic placeholder rejection；
- multi-role / multi-item cardinality方向；
- manual/no-key 不伪造 generic NPC/Place/Item；
- Product Player Profile 结构化投影；
- Information Surface 只显示 Knowledge；
- Game-local provenance ledger；
- Runtime materialization 与 Formal Turn 同 transaction；
- JIT Place / Character stable identity 基本纵向；
- G8-UAT-01 的 location/interactable authority、No Phantom、Player Agency、exactly-five suggestions；
- G7 durable semantic artifact / recovery seam。

---

## 8. 返修 Gate

返修完成前至少新增并通过：

1. save-before-materialization → materialize → restore-old → topology rollback；
2. save-after-materialization → restore → same stable identity；
3. old-save branch → different future materialization；
4. ordinary observe/dialogue/inner/read-only → World Materializer call count = 0；
5. materializer failure on unrelated input does not block Turn；
6. structured OpeningBeat → Narrative realization，禁止 unreferenced concrete entity；
7. unique private Creation marker zero public disclosure；
8. NPC-held / scene-held / player-held Item placement；
9. G5/G6/G7/G8/full/typecheck/lint/build/launcher/disclosure/diff-check；
10. targeted Real DeepSeek Gate rerun。

---

## 9. 当前下一步

> `G8-UAT-02 narrow return fix → Independent Review rerun`。

未通过 Independent Review 前，不授权 Project Owner Stage UAT，不启动 G9。
