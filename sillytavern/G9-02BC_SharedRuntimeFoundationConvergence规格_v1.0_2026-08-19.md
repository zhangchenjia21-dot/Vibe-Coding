# G9-02BC｜Shared Runtime Foundation Convergence 规格 v1.0

状态：`CURRENT SPEC / ACTIVE NEXT`
日期：2026-08-19
类型：cross-slice shared-foundation implementation
所属阶段：G9-02B / G9-02C
前置：`G9-02A PASS / CLOSED`

> 本任务不是新增生命周期阶段。它是在 G9-02B breadth 与 G9-02C breadth 之前，先冻结两者共同依赖的高耦合 Runtime rails，降低后续 Grok Build 扩展时的架构自由度与返工风险。

---

## 1. Outcome

在 `sillytavern@04603e1e4a3270e9f5740b5957cf545a2bd001d0` 的 G9-02A foundation 上，建立并 production-proof：

```text
Program-owned Built-in Domain Module Registry / Host
↓
Game-local Package / Feature / Module Binding + Activation
↓
owner-scoped Canonical Record / Runtime State extension seam
↓
typed Candidate / Change / Event / Handoff / Projection seam
↓
Routing Directory boundary
↓
validated selection
↓
JIT Context Projection Host
↓
bounded owner-preserving context join boundary
```

本任务只完成 shared primitives + one reference vertical；不扩完全部 Domain，不完成完整 Model Router，不冻结 external asset-spec。

---

## 2. 为什么最后一次深度实现优先做这里

G9-02A 已解决：

- Source binding / lineage；
- Game-local stable identity / definition revision；
- typed existing-asset mutation；
- Save / Restore / Branch / Recovery。

剩余最大返工半径位于 02B 与 02C 交界：

```text
Domain ownership
+ activation
+ domain canonical/state persistence
+ formal change extension
+ handoff
+ model-visible projection
+ router boundary
```

若这一层错误，G9-03 Schema、G9-04 Compiler、G9-05 Creator 与全部 Expansion 都会被错误 contract 放大。

因此先用高深度 Agent 固定 rails，再由 Grok Build 扩 breadth。

---

## 3. Authority / Core Invariants

### INV-BC01｜Program-owned code

```text
Asset / Game-local Binding
= data + identity + configuration

Runtime Domain Module
= Program-owned built-in code
```

Asset 不得上传或注入：

- JS / TS；
- callback；
- eval；
- script；
- expression language；
- arbitrary query / state path。

Game-local binding 只能引用 Program 已注册的 module identity；未知 module fail closed。

### INV-BC02｜三层事实继续成立

```text
Source Asset
!= Game-local Canonical Record
!= Runtime Domain State
```

Definition / canonical record 回答“它是什么”；Runtime domain state 回答“它现在怎么样”。

不得因为扩展性重新合成一个 generic live blob。

### INV-BC03｜Canonical Owner 唯一

每个 domain record / state / event / handoff / projection 必须保留：

- `ownerNamespace`；
- `moduleRef` 或等价 stable module identity；
- owner-specific kind/version；
- stable record/state ref where applicable。

Host 不根据 display name、title、topic、文本相似度猜 Owner。

### INV-BC04｜Core metadata 复用 G9-02A

Game-local domain definition record 应复用/对接 G9-02A common `GameLocalAssetMetadata` identity/lineage/revision seam，不建立第二套 local identity/provenance authority。

### INV-BC05｜Payload 必须 owner-typed

允许实现 generic storage envelope，但只有在：

```text
registered Program module
+ explicit owner
+ explicit schema/version
+ module-owned validator/codec
+ Program validation
```

全部通过后才能 persistence / commit。

禁止：

```text
arbitrary JSON blob
+ arbitrary path mutation
+ frontend/model direct write
```

现有 Core specialized tables 不要求迁移到 generic domain storage。

### INV-BC06｜Runtime State extensible but separated

不得继续假设所有未来 state owner 永远只有：

```text
character | item | relationship
```

应建立独立 owner-scoped Domain State seam，同时保留现有 Core state owner。

Domain canonical record 与 Domain runtime state 必须是不同 contract / lifecycle。

### INV-BC07｜Formal reality still Program-owned

Model / Domain Provider 可提出 typed candidate / proposal；Program / Domain Host 校验后才能进入 formal change。

所有 domain mutation 必须：

- 在 Formal Turn authority 内；
- 与 game revision atomic；
- conflict fail closed；
- Save / Restore 可恢复；
- Crash / retry 不重复正式结果。

### INV-BC08｜Package / Feature / Module 三层分离

内部 Runtime 至少表达：

```text
Package Included
!= Feature Enabled
!= Module Enabled
```

Module 可用性由显式 binding / dependency / activation 得出。

Disabled module：

- 不可执行 candidate；
- 不可产生 domain change；
- 不可被 Router 选择；
- 不可提供 JIT prompt projection；
- 不可通过 handoff 被间接复活。

### INV-BC09｜Dependency Graph != Context Inclusion Graph

Hard dependency 只证明 capability / owner availability。

```text
A depends on B
```

不得自动变成：

```text
A context always includes all B state
```

### INV-BC10｜Handoff 不是共享所有状态

Handoff 至少明确：

- from owner/module；
- to owner/module；
- trigger / outcome identity；
- bounded typed payload / refs；
- continuation semantics。

不得借 handoff 传 entire Runtime State / source asset / whole dependency context。

### INV-BC11｜Routing Directory 是 Context Selection 前置目录

Router future model-visible input 不应看到 Runtime total state。

Shared foundation 至少建立：

```text
enabled/pruned module registry
→ bounded routing-directory projection
```

目录只含选择模块所需的最小安全 metadata，不含完整 domain state。

### INV-BC12｜JIT Projection after selection

```text
Routing selection
↓ Program validates selected enabled modules
↓ JIT owner projection
↓ bounded owner-preserving join
```

不得在 routing 前把所有 Domain projection 预先拼进 Prompt。

### INV-BC13｜Projection != Authority

Context Projection：

- read-only；
- bounded；
- owner-preserving；
- purpose-specific；
- 不可反写 canonical state；
- 不可携带 callback/query/statePath。

### INV-BC14｜Bounded != Starved

Projection 必须保留完成当前职责所需的：

- identity；
- relevant refs；
- current player-safe / model-authorized state；
- domain constraints；
- required outcome / handoff context。

但 context size 不应随 total registry / total session assets 线性增长。

### INV-BC15｜#17 People Directory guard

本 shared foundation 不要求完成完整 Player-known Character Directory，但设计不得阻塞：

```text
Canonical Character Truth
!= Player-known Character Directory
!= Current Scene Visible Character Set
```

Grok Build 后续 02B breadth 将在该 Host / projection seam 上实现长期 People Directory。

### INV-BC16｜Opening Scenario 不进入本轮

`G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md` 仍是 Discussion Draft。

本任务不得实现：

- PROLOGUE_SCENARIO_MODE；
- scenario convergence；
- Creator prologue UI；
- external Scenario wire。

---

## 4. Internal Contracts｜必须形成但不冻结 external wire

精确 TypeScript 名称可由实现决定，但语义上至少需要以下内部合同。

### 4.1 Built-in Domain Module Registration

至少表达：

- stable module identity；
- owner namespace；
- Program-known module kind/version；
- supported Feature / capability identity；
- required hard dependencies；
- routing-directory descriptor provider；
- context projection provider；
- candidate/change validation hook or equivalent bounded port；
- domain persistence codec/repository boundary where needed。

注册表必须由 Program composition root 构建，不从 asset code 动态执行。

### 4.2 Game-local Activation / Binding

至少能区分：

- package included；
- feature enabled；
- module enabled / disabled；
- module binding identity；
- dependency availability；
- module-local configuration reference / bounded value where applicable。

本轮只做 internal handwritten binding，不解析 Markdown / external manifest。

### 4.3 Owner-typed Canonical Record Seam

至少可承载一个非 Core 五类的 Game-local domain record：

- stable ref；
- owner/module identity；
- record kind/version；
- G9-02A common metadata / lineage hook；
- visibility / information boundary；
- validated owner payload。

### 4.4 Owner-typed Runtime State Seam

至少表达：

- state identity / owner；
- state kind/version；
- current value payload；
- validation / mutation policy；
- formal revision / persistence boundary。

Definition record 与 Runtime state 不得共用一个“万能 mutable JSON”。

### 4.5 Domain Formal Change Seam

至少能让 registered module 对一个已验证 candidate 产生 bounded formal domain change，并在当前 Formal Turn transaction 内提交。

必须保持：

- expected revision / conflict protection；
- atomicity；
- durable execution compatibility；
- no direct model DB write。

### 4.6 Domain Event / Handoff Seam

至少形成 internal typed envelope，保留 owner/module identity。

不要求本轮实现所有 downstream domain continuation，只需证明：

- event/handoff 可形成；
- disabled / unknown recipient fail closed；
- payload bounded；
- 不产生 transitive context inclusion。

### 4.7 Routing Directory / Projection Boundary

至少形成：

```text
Enabled Domain Modules
→ Routing Directory Entries
```

以及：

```text
validated selected module refs
+ purpose/current refs
→ JIT owner projections
→ bounded owner-preserving join object
```

完整 Model-first Router 逻辑属于 G9-02C breadth；本任务允许 deterministic/test selector 来证明 boundary。

---

## 5. Mandatory Reference Vertical

必须通过**同一 production Host / composition path**注册一个 handwritten built-in reference module；可以只在 test/bootstrap injection 中启用，但不得绕过正式 Host 直接操作存储。

Reference vertical 至少证明：

```text
handwritten Package/Feature/Module binding
↓
Program registry resolves built-in module
↓
module enabled
↓
one owner-typed Game-local canonical record
+
one owner-typed Runtime domain state
↓
validated domain candidate/change
↓
one atomic Formal Turn commit
↓
domain event or handoff envelope
↓
JIT Context Projection
↓
player-safe / model-safe bounded read
↓
Save / Restore reproduces canonical record + runtime state
```

同一 vertical 的 disabled variant 必须证明：

```text
module disabled
→ candidate unavailable
→ change unavailable
→ routing entry absent
→ projection unavailable
→ handoff cannot revive module
```

Reference module 不应成为新的产品功能；它只证明 production rails。

---

## 6. Scale / Context Boundary Proof

至少构造一个大 registry fixture，例如几十到上百个 enabled-capable module descriptors。

验证：

```text
Total registered modules ↑↑↑
Selected modules = small bounded subset
JIT projection count / joined context ≈ selected subset
```

不得出现：

- routing 前 materialize 所有 projections；
- dependency recursive prompt expansion；
- selected A 自动加入所有 transitive dependency state。

本任务无需真实大模型 Router；重点是 infrastructure 让后续 Router **只能**走 bounded path。

---

## 7. Persistence / Recovery Gate

新增 Domain record/state/change 后至少验证：

### Save-before / after

```text
save S1
→ domain formal change
→ restore S1
```

恢复旧 record/state。

```text
domain change
→ save S2
→ later change
→ restore S2
```

恢复 S2 record/state + same stable owner/ref。

### Branch

Restore 后新 Domain change 可形成独立未来，不污染 archived future。

### Crash / retry

若 domain candidate/change 已进入 durable artifact：

- recover 不重复 Provider / authoring step；
- formal domain state 不重复；
- event/handoff 不重复；
- Formal Turn exactly once。

如果 reference vertical 完全 deterministic、没有新 Provider，仍必须证明 retry/replay 不 duplicate domain change/event。

---

## 8. Migration

若新增 SQLite table / columns：

- append-only migration id；
- old G8/G9-02A DB 可启动；
- rollback on migration failure；
- 不删除 G9-02A lineage/revision；
- 不强迫 Core state 迁移到 domain storage；
- legacy data 不能被静默重新解释成某个 Expansion Owner。

---

## 9. Tests

至少需要 focused：

1. built-in module registration / duplicate identity fail closed；
2. unknown asset-bound module fail closed；
3. package included != feature enabled != module enabled；
4. disabled module candidate/change/router/projection/handoff all unavailable；
5. hard dependency availability；
6. dependency does not imply prompt inclusion；
7. owner-typed canonical record；
8. owner-typed Runtime state；
9. atomic domain formal change；
10. event/handoff owner identity；
11. JIT projection after selection；
12. large registry bounded projection；
13. Save / Restore / Branch；
14. retry/recovery exactly-once；
15. no arbitrary code/path/query/eval；
16. G9-02A lineage/mutation regression；
17. G5/G6/G7/G8 full authority regression。

---

## 10. Non-scope

本任务禁止扩张为：

- 所有真实 Expansion 的完整实现；
- Player-known Character Directory 完整产品 vertical；
- 完整 Model-first Context Router；
- complete background progression framework；
- external asset-spec / JSON Schema；
- Markdown / YAML parser；
- Bundle；
- Game Asset Adapter / Compiler；
- Creator；
- Objective Engine；
- Prologue Runtime；
- Game Delete / Provider model selector；
- UI redesign。

---

## 11. Closure Gate

任务完成后只能宣布：

```text
G9-02BC SHARED FOUNDATION READY FOR INDEPENDENT REVIEW
```

不得宣布：

- G9-02B CLOSED；
- G9-02C CLOSED；
- G9-02 CLOSED；
- G9-03 AUTHORIZED；
- asset-spec FROZEN。

Shared foundation review PASS 后：

```text
Grok Build
→ G9-02B breadth completion
→ G9-02C breadth completion
→ Integrated Closure
```
