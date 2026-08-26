---
title: G9-05G｜Independent Review｜最终收口
status: final-pass-closed
version: 1.0
updated: 2026-08-22
reviewed_repository: zhangchenjia21-dot/sillytavern
reviewed_branch: agent/g9-05g-primary-asset-e2e
reviewed_exact_sha: a97b4bae6a3bd9308ecb8c092b96bce81dd43700
integrated_main: a97b4bae6a3bd9308ecb8c092b96bce81dd43700
formal_code_base: 26d23d47c5f5ac42d3e1029654a64eda831c4db1
---

# G9-05G｜Primary Asset End-to-End Closure｜Independent Review 最终收口

## 1. Verdict

```text
P0 = 0
P1 = 0

G9-05G0 Real EP-CHAR Runtime Binding = PASS / CLOSED
G9-05G Primary Asset End-to-End Closure = PASS / CLOSED
Primary Asset End-to-End Closure Gate = PASS / CLOSED

Library Product Increment = AUTHORIZED / NEXT
G10 Provider Expansion = NOT AUTHORIZED
Release = NOT AUTHORIZED
```

Reviewed exact implementation SHA：

```text
a97b4bae6a3bd9308ecb8c092b96bce81dd43700
```

Integration：

```text
sillytavern/main
= a97b4bae6a3bd9308ecb8c092b96bce81dd43700
force = false
```

未产生 merge / squash / rebase / 新 integration SHA。

GitHub combined status 对 reviewed SHA 返回 `statuses: []`，workflow runs 为空；因此本审查不声明 CI green。GPT 当前环境未独立 checkout 仓库执行本地命令，独立 local command 状态为 `NOT RUN`。Verdict 基于 exact-SHA diff、current canonical specs、production path、测试源码、Git ancestry 与 recovery state-machine 独立审计。

---

## 2. Correction history

```text
26d23d47c5f5ac42d3e1029654a64eda831c4db1  Formal Code Base
↓
db1533fc3b2cea0769e8f8c1a6fd466d3444bbae  initial Task Packet
↓
e86fefd3f965b75ee9ac1501418f438ca1124446  G9-05G0 implementation
↓
a9a3bb0a534efaab6db00f2c3a62efcc19818946  G9-05G E2E implementation
↓
b9697da6af865f8752a038881a349e0a74a91274  correction-01 packet
↓
d560b6213676d292ca64c49d20f77d442b339b9b  correction-01 implementation
↓
00973e4fae6ac8d0a2cc4b2fc2dee876815dc982  correction-02 packet
↓
a97b4bae6a3bd9308ecb8c092b96bce81dd43700  correction-02 / final implementation
```

### correction-01 closed

P1-01：Capability foundation activation 已锁定 exact Source ownership：

```text
packageRef = package:EP-CHAR-CORE
featureRef = feature:character-capability
runtimeModuleRef = builtin:character-capability.evidence.v1
packageIncluded = true
featureEnabled = true
moduleEnabled = true
```

foreign package 即使复用相同 Program module 且三 flags 全 true，也不得物化 EP-CHAR capability foundation。

P1-02：Creator Publication 已区分：

```text
current candidate
→ current Program Host strict validation

immutable historical Source catalog
→ structure / integrity / identity / dependency / cycle 仍验证
→ stale historical Program refs 不要求继续存在于 current Host
```

因此 historical `EP-CHAR-CORE@0.1.5` 可保留 G9-04 proof identity，同时默认 production Host 不重新注册 proof module，仍能显式 source_revision 发布 production-bound `0.1.6`。current candidate 的 unknown runtime ref 继续 fail closed。

### correction-02 closed

发现并关闭 append-after-durable crash window：

```text
preparePublishing durable
→ Source append durable
→ crash before finalizePublished
```

恢复时 Source catalog 已包含 intended candidate。最终实现只从 historical catalog 中排除与 candidate **完整 exact snapshot** 相同的副本：

```text
assetRef
+ assetType
+ version
+ contentHash
```

candidate 本身始终按 current Program Host strict validate；same assetRef+version 但 different hash 不被过滤，继续触发 integrity/version conflict fail closed。

恢复语义：

```text
Host B incompatible
→ retry FAIL CLOSED
→ Source remains one exact snapshot
→ Draft remains publishing

Host A restored
→ retry revalidates candidate
→ existing exact Source is reused
→ no second append
→ finalize same Draft
→ replayed = true
```

Source Library 的 InMemory 与 SQLite 实现本来就把同版本同 digest append 定义为幂等 `appended:false`，同版本不同 digest 定义为 `VERSION_CONFLICT`；最终 Host Gate 与 Store exactly-once contract 已一致。

---

## 3. Primary Asset E2E closure evidence

正式真实语义 corpus：

```text
World:
汉末三国_天下未定_World_Pack_v0.2.3

Character:
刘备__Character_Card__v0.1.2
→ Creator source_revision
→ 0.1.3 capability-evidence

Expansion:
人物能力与技艺_Expansion_Pack_v0.1.5
→ Creator source_revision
→ 0.1.6 production Runtime binding
```

真实资产 Gate 使用冻结 Git blob SHA-1；缺真实资产 root 时明确 `NOT RUN`，blob 不符明确 `FAIL`，禁止 synthetic fallback。

已形成的纵向证明：

```text
real World / Character / Expansion manuscripts
→ validated published Source exact snapshots
→ Use My Assets exact selection
→ TavernGameAssetManifestV1
→ selected-set dependency closure
→ deterministic Game-local materialization
→ G9-04 binding / lineage
→ production RuntimeDomainModule binding
→ bounded capability evidence projection
→ playable session / formal turn
→ Save / Continue / Restore
→ crash / resume / recovery
→ Source version isolation
```

Expansion enabled / disabled 负对照证明 Feature / Module enablement 会改变真实 Runtime routing/projection 行为，而不是只改变一条数据库 binding。

Character `bound_only` 继续保持 No-Phantom：只绑定 Source，不创建 live Runtime Character，也不创建 capability foundation record。

private Source section 不进入 player-safe capability record / projection。

EP-CHAR v1 slice 保持 read-only：不拥有 XP、成长、技能等级、Dice、Formal Outcome 或 mutation candidate。

---

## 4. Permanent lessons

### 4.1 Source ownership != Program capability reuse

相同 `runtimeModuleRef` 不等于相同 Source package owner。Program-owned foundation 需要 exact package / feature / module identity gate。

### 4.2 Historical Source != current Program capability

历史 immutable Source 可以引用当前 Program 已退休的 runtime identity；它必须仍可读取、审计、创建新 revision，但不能因此把旧 capability 重新注册成 current Program truth。

### 4.3 Publication exactly-once must enumerate crash windows

正式 Publish/Bootstrap transaction 必须至少检查：

```text
before prepare
prepare durable → before append
append before durable
append durable → before finalize
finalize durable → response lost
```

不能只测试 happy path 或一个 crash point。

### 4.4 Correction Budget

同一错误及其直接衍生错误采用 correction budget：

```text
第一次失败
→ focused correction

第二次失败
→ correction 前扩大同根因 boundary / crash-window review

若 correction-02 后仍出现同根因 P1/P0
→ STOP local patching
→ 强制 Root-Cause / Boundary Review
→ 重新审计 transaction model / ownership seam / API abstraction
```

本阶段 correction-02 最终 PASS，因此没有触发 transaction-model redesign；但该预算规则作为后续正式工程治理原则保留。

---

## 5. Next Gate

G9 主资产链首次真实闭环已经成立。下一阶段不再继续为 World / Character / Expansion 增加 Creator 类型，也不重复证明三类主资产建局。

```text
NEXT = Library Product Increment
```

下一阶段必须先冻结 Library 的产品定位、Source/Runtime 使用边界、检索/上下文权限与与现有三类主资产的关系，再生成正式 Agent Task Packet。

在新的 Library canonical spec 与 Task Packet 出现前：

```text
G10 Provider Expansion = NOT AUTHORIZED
Release = NOT AUTHORIZED
```
