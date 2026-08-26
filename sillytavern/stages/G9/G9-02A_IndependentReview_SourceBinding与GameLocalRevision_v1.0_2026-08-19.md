# G9-02A｜Independent Review｜Source Binding + Game-local Revision v1.0

状态：`PASS / CLOSED`
日期：2026-08-19
审核对象：`zhangchenjia21-dot/sillytavern@04603e1e4a3270e9f5740b5957cf545a2bd001d0`
Base：`cdbd9cd7ff0b5b9a5672156066478b57f732307c`

## 1. 结论

```text
G9-02A implementation          PASS
Independent Review             PASS / CLOSED
P0                             0
P1                             0
G9-02BC Shared Foundation      AUTHORIZED / NEXT
G9-03                          NOT AUTHORIZED
```

本审核以 exact SHA 为唯一实现对象，不以 Codex 自报 PASS 代替代码证据。

---

## 2. Freshness

审核开始时：

- `sillytavern/main` HEAD = `04603e1e4a3270e9f5740b5957cf545a2bd001d0`；
- 无后续实现提交覆盖该对象；
- `Vibe-Coding/main` 最新变化仅为 Grok Build 执行治理与 Prologue Discussion Draft，不改变 G9-02A acceptance；
- `Skill/main` 最新仍为 `c18ab6588055132e9ff5922cbe864961f355ae16`；
- G9-02A Task Base → Current HEAD 仅包含目标实现 commit。

Freshness：`PASS`。

---

## 3. Architecture Review

### 3.1 Source / Local / Runtime 分层

新增 `SourceAssetDescriptor` / `GameLocalSourceLineage`，Source 只按值进入 Runtime：

```text
stableRef
assetType
version
contentHash
boundAtRevision
```

Runtime 不持有 source path、callback、可写句柄或反向同步能力。

结果：

```text
Source Asset
!= Game-local Canonical Asset
!= Runtime State
```

保持成立。

### 3.2 Common metadata extensibility

`GameLocalAssetMetadata` 新增：

- open `ownerNamespace`；
- open `assetKind`；
- provenance / visibility；
- optional source lineage；
- created turn/revision；
- definition revision；
- last modified turn/revision。

`AdditionalGameLocalAssetDefinition` 已证明 future owner seam 不依赖 Core 五类 display switch；测试使用 `extension.objectives / objective` 作为 future seam。

未把所有业务 payload 塞入 arbitrary generic JSON table；现有 specialized payload 继续由 typed Runtime tables 持有。

### 3.3 Typed mutation boundary

当前 production mutation union 仅覆盖：

- Item `publicDescription`；
- Character `publicDescription`；
- Scene `publicDescription`。

Model Provider 只能 author `proposedPublicDescription`，不能选择 target、owner、field、source lineage、revision、holder、position 或 DB path。

Program 校验：

- Semantic need 与当前 Candidate 精确匹配；
- public target；
- `runtime.core` owner；
- typed asset kind；
- value bounds / no-op；
- protected disclosure；
- expected definition revision。

不存在 arbitrary JSON Patch / SQL / state path / callback / eval。

### 3.4 Semantic ownership

`assetMutationNeed` 进入 Runtime-only Semantic Proposal，由 Semantic AI 判断是否存在 durable enrichment need。

Program 不通过“检查/查看/信件”等关键词或正则重新做开放 NLP。

普通无 durable need 的 Turn 可保持 `assetMutationNeed = none`，不会固定增加 mutation Provider call。

### 3.5 Atomic commit

Mutation Plan 包含：

- `fromValue`；
- `toValue`；
- `expectedDefinitionRevision`；
- `nextDefinitionRevision`；
- committed turn / game revision。

SQLite Formal Turn transaction 使用 compare-and-swap：

```text
public_description == fromValue
+
definition_revision == expectedDefinitionRevision
+
owner / kind / visibility match
```

才同时写 payload + metadata revision；冲突 fail closed。

因此 Product/Narrative 不形成第二 mutation authority。

---

## 4. Save / Restore / Branch / Recovery

### Save / Restore

Canonical Save Snapshot 已纳入完整 `gameLocalAssets` metadata 与 mutated typed payload。

Restore：

- 校验 Core Game-local identity；
- 对 current known asset 校验 Source lineage 逐字段等值；
- 恢复 description + definition revision + lineage；
- restore transaction 中途故障整体 rollback。

### Branch

Restore 到旧 Save 后的新 mutation 形成独立未来；已有 archive / branch evidence 继续复用 G6 authority。

### Recovery

`assetMutation` Plan 进入 durable Semantic execution artifact。

```text
crash after semantic_ready
→ recover
→ reuse persisted mutation plan
→ no Semantic recall
→ no Asset Mutator recall
→ exactly one definition revision
→ exactly one Formal Turn
```

G7 exactly-once authority 未回滚。

---

## 5. Security / Disclosure

DeepSeek Asset Mutator request 只包含：

- player input；
- semantic purpose；
- selected public target；
- current public description；
- public region/place/scene labels。

不包含 source lineage、private profile、hiddenFact 或 DB state。

Strict Tool schema `additionalProperties=false`；非法 arbitrary path fail closed。

受保护 hidden candidate、hidden target / protected disclosure 不能写入 public description。

No Phantom / private-public boundary 保持成立。

---

## 6. Test / Evidence Review

Exact commit 新增 focused G9 suite，覆盖至少：

- Strict Tool request / schema；
- same source → two games isolation；
- source version later changes → old game unchanged；
- Item / Character / Scene typed mutation；
- save-before / save-after；
- restore → branch divergence；
- semantic_ready crash → recovery exactly-once；
- restore fault rollback；
- legacy 0011 migration；
- ordinary observation zero mutation call；
- holder transfer zero mutation call；
- invalid transfer+mutation conflict reject；
- hidden/protected disclosure reject；
- future owner/kind seam。

Codex 报告的本地 supplemental evidence：

```text
G9 focused            9/9 PASS
G5                    207 PASS
G6                    17 PASS
G7                    20 PASS
G8                    208 PASS
G8 Creation           19 PASS
G8 Product E2E        6 PASS
Full                  682/682 PASS
Typecheck             PASS
Lint                  PASS
Product build         PASS
Launcher smoke        PASS
Disclosure            PASS
Diff-check            PASS
```

真实 DeepSeek smoke 报告：3 committed Turns、Semantic 3、Asset Mutation 1、Narrative 6、12 checks 全 PASS、hidden disclosure 0、hard safety failure 0。

GitHub 当前无独立 CI status / workflow run，因此上述执行数字继续归类为 implementer local supplemental evidence；Independent Review 的架构结论来自 exact-SHA code / contract / tests 检查。

---

## 7. Non-blocking Note｜Legacy createdRevision

pre-G9 legacy `game_local_asset` 原本只有 `created_turn`，没有可恢复的 exact `created_revision` 历史字段。

0011 migration 对旧记录以既有 `created_turn` 作为 `created_revision / last_modified_revision` 回填种子；G9-native 新记录则使用真实 current game revision。

裁定：

- 不阻塞 G9-02A；
- 不需要为不可重建的历史数据伪造更复杂迁移；
- future consumer 不得把 legacy 回填值当成可证明的历史 Event revision；
- 若未来出现必须依赖 exact pre-G9 creation revision 的消费者，再单独建立 migration provenance，而不是现在扩任务。

---

## 8. Closure

```text
G9-02A Source Binding + Game-local Revision Foundation
PASS / CLOSED
```

授权下一工程对象：

> **G9-02BC Shared Runtime Foundation Convergence**

它是 G9-02B / 02C 之间的 cross-slice shared-foundation implementation，不新增生命周期阶段。

下一任务只应冻结并 production-proof shared rails：Runtime Domain Host、owner-typed extension seam、Package/Feature/Module activation、typed candidate/event/handoff/projection seam、Context Projection/Orchestrator boundary。

不得借机扩完所有 Domain、完整 Context Router、external asset-spec、Adapter、Creator 或 Prologue Runtime。
