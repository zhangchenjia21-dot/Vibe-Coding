# G9-02A｜Source Binding 与 Game-local Revision Foundation 规格 v1.0

状态：`CURRENT SPEC / ACTIVE`
日期：2026-08-18
类型：Runtime foundation implementation
上游：
- `G9-01_资产兼容性审计与G9-02基础门禁_v1.0_2026-08-18.md`
- `G9阶段启动与G9-02实施切片裁定_v1.0_2026-08-18.md`
- #15 Runtime Context Orchestration v1.2
- #16 Runtime World Materialization / Game-local Assets v1.1

## 1. Outcome

在不解析 external Markdown、不冻结 asset-spec 的前提下，建立并证明：

```text
Source Asset Descriptor / Snapshot Identity
↓ bind
Game-local Canonical Identity + Lineage
↓ typed definition mutation
Game-local Definition Revision
↓ Runtime / Product projection
↓ Save / Restore / Branch / Recovery
```

完成后，G9-02B 可以在这套 Game-local foundation 上挂 Expansion / Domain records。

---

## 2. Source Binding Invariants

### INV-A01｜Source 与 Local 分离

```text
Source Asset
!= Game-local Asset
```

同一个 Source 可以绑定到多局；不同 Game 的 local identity / revision 独立。

### INV-A02｜Source Lineage 可验证

每个 source-bound local asset 至少语义上保留：
- source stable identity；
- source asset type；
- source version；
- source content hash / snapshot identity；
- bind ancestry。

精确字段名只属于 internal contract，不冻结 external G9-03 wire。

### INV-A03｜Source 更新不静默污染旧局

```text
source v1 bind → Game A
source later becomes v2
Game A remains v1-derived local reality
new Game B may bind v2
```

### INV-A04｜Runtime 不反写 Source

所有剧情演化只写 Game-local layer / Runtime State。

---

## 3. Game-local Metadata / Revision

当前 G8 coarse provenance 不足。

G9-02A 至少需要 internal common metadata 语义：

- stable local identity；
- asset kind / owner namespace；
- source binding lineage optional；
- provenance / origin；
- created turn/revision；
- current definition revision；
- last modified turn/revision；
- visibility / information boundary metadata where needed。

允许继续使用现有 Character / Scene / Item operational tables；不要求把所有 payload 塞进一个 generic JSON table。

但 common metadata 必须能被 Save / Restore / future Domain Host 使用。

---

## 4. Typed Existing Asset Mutation

新增 Program-owned typed definition mutation boundary。

最小 production union 至少覆盖：

### Character
- player-safe `publicDescription` evolution；
- private profile evolution（若实现，必须 hidden only）。

### Scene / Place
- public scene description evolution（按现有数据模型选择最小正确对象）。

### Item
- player-safe known/public description evolution。

Identity / source lineage / owner namespace 不可被普通剧情 patch。

正确链：

```text
current player action / formal semantic need
↓
Model typed asset patch proposal（仅在确有开放内容 authoring 时）
↓
Program validates:
  target ref
  owner
  allowed mutable field
  visibility
  source immutability
  value bounds
↓
working Game-local definition revision
↓
Narrative realizes updated canonical truth
↓
one atomic Formal Turn commit
```

禁止：

```text
Narrative prose first
→ parse prose
→ patch DB
```

也禁止 arbitrary path / arbitrary JSON patch / SQL from model。

---

## 5. Mutation Need / Model Activation

不是每个 Turn 都调用 Asset Mutation Model。

是否需要 durable definition enrichment 属于 AI semantic judgment / bounded authoring decision；Program 不通过关键词猜测。

允许实现方式：
- current Semantic result 携带 optional typed `assetMutationNeed`；或
- 由一个仅在明确 inspection/discovery/materialization-like semantic need 下激活的 bounded authoring step。

不得让普通 dialogue / wait / unrelated observe 固定增加一个 mutation model call。

---

## 6. Mandatory UAT Reference Case｜重要信件

起点：已有 canonical Item：

```text
重要信件
publicDescription = 已知的基础描述
```

玩家：

```text
打开信封，仔细查看信件内容/封缄细节
```

若模型合理发现一个应长期保留的 player-safe Item detail，例如：

```text
信封以魔力火漆封缄，封面印有流转微光的失衡符文
```

必须能够形成：

```text
Item local definition patch
↓
Program validates
↓
publicDescription revision
↓
Product Item dossier immediately reflects new canonical description
```

之后：
- 转移 Item holder 不应才触发 description 更新；
- Save 后 Restore 保留新描述；
- Restore 到 discovery 前 Save 恢复旧描述；
- Branch 可产生不同后续 description evolution；
- source fixture / source snapshot 保持不变。

本 Reference Case 关注 canonical evolution，不要求本轮修 G11 的“carried card 是否直接显示 description”的视觉 polish；Product detail / DTO 只需能读取 canonical 新值。

---

## 7. Save / Restore / Branch

Canonical Save 必须纳入：
- source binding identities / required lineage state；
- local definition revision metadata；
- mutated definition values；
- existing G8 topology / runtime state。

Mandatory cases：

### SAVE-A

```text
save S1
→ mutate Item description
→ restore S1
```

Expected：old description / old local revision。

### SAVE-B

```text
mutate
→ save S2
→ later mutate again
→ restore S2
```

Expected：S2 description + same stable local ref + same source lineage。

### SAVE-C

```text
restore S1
→ branch mutation B
```

Expected：branch B can diverge without corrupting archived future A。

### SAVE-D

restore transaction fault → full rollback。

---

## 8. Crash / Recovery / Idempotency

若 model patch proposal 已生成并形成 durable execution artifact：

```text
crash before commit
→ recover
```

必须：
- 不重复调用 patch-authoring Provider；
- 不生成第二 revision；
- 不重复 Formal Turn；
- 不重复 Event；
- exactly-once commit。

response-loss after commit 继续按 G7 canonical replay。

---

## 9. Two-game Isolation Proof

使用同一 source descriptor：

```text
Source Item/Card X v1 hash H
→ Game A bind
→ Game B bind
```

Game A local patch 后：
- Game A local value changes；
- Game B remains unchanged；
- Source fixture remains unchanged；
- local refs / revisions do not collide。

---

## 10. Handwritten Normalized Fixture Only

G9-02A 可以使用真实资产语义构造 fixture，但不得实现：
- Markdown frontmatter parser；
- Obsidian link resolver；
- external asset manifest；
- bundle import；
- Creator asset picker。

建议 fixture source descriptors 取材：
- Han World Pack v0.2.3；
- 刘备 Character Card v0.1.2；
- EP-CHAR-CORE v0.1.5；
- 一个 Item source fixture。

外部解析属于 G9-04。

---

## 11. Objective / Extension Guard

G9-02A 不实现 Objective Engine。

但 common lineage / revision metadata 设计不得只绑定 `region/place/scene/character/item` 五种硬编码业务语义，必须为 G9-02B 的 owner-typed Extension record 留正式扩展 seam。

本轮可通过 internal fake owner/kind contract test 证明 metadata 层不依赖 display label 或固定五类业务 payload。

---

## 12. Regression / Authority

不得回滚：
- G5 continuity；
- G6 Save/Restore/Branch；
- G7 Recovery/Idempotency；
- G8 Semantic-gated Materialization；
- No Phantom；
- Player Agency；
- Product player-safe projection；
- private/public boundary。

新增 definition patch 后，Narrative 仍只能描述 Program 已接受的 working canonical truth。

---

## 13. Required Gates

至少：

```text
focused source-binding tests
focused game-local mutation tests
focused save-before/after mutation tests
focused two-game isolation
focused crash/recovery exactly-once
private disclosure negative tests
G5
G6
G7
G8
full
Typecheck
Lint
Product build
Launcher smoke
Disclosure
Diff-check
```

Real DeepSeek targeted gate 仅在 offline 全绿后运行，并至少证明：
- source-bound Item inspection → typed durable description patch；
- Product reads updated canonical value；
- ordinary unrelated Turn does not call mutation authoring provider；
- source unchanged；
- hidden disclosure 0。

---

## 14. Non-scope

本轮不做：
- G9-02B Expansion Host；
- G9-02C full Context Orchestrator；
- external asset-spec；
- Markdown Adapter / Compiler；
- Creator；
- full Objective Engine；
- Game Delete；
- Provider model selector；
- arbitrary script / eval / callback assets。

---

## 15. Closure

完成后状态只能是：

```text
G9-02A IMPLEMENTATION READY FOR INDEPENDENT REVIEW
```

不得宣布：
- G9-02 CLOSED；
- G9-03 authorized；
- asset-spec frozen。
