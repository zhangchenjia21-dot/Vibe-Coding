---
name: lifecycle-templates
description: 提供领域架构调查、所有权矩阵、术语注册表、依赖类型、迁移台账、收敛审核、聚合/实例桥接、状态机序列测试与决策记录等可复用治理模板。需要结构化的资产、架构、迁移或生命周期模板时使用。
whenToUse: 需要产出领域调查、所有权矩阵、迁移台账、收敛审核、聚合/实例桥接、状态机序列测试矩阵、决策记录等治理文档模板时。
---

# lifecycle-templates v1.6

> [!abstract]
> 为生命周期与资产/模块治理提供可复用模板。
>
> 以下模板均为结构骨架，可按项目术语替换。

---

# T01｜Domain / Capability Architecture Survey

```markdown
# Domain Architecture Survey

## 目标范围

## Candidate Domains
| Domain | Business Meaning | Expected Consumers | Generic? | Candidate Owner | Status |
|---|---|---|---|---|---|

## Shared Foundation Candidates

## World / Product-specific Domains

## Provisional Owners

## Risks
- duplicate owner:
- dependency inversion:
- future extraction:
```

---

# T02｜Canonical Ownership Matrix

```markdown
| Fact / State | Canonical Owner | Definition Contributor | Readers | Writers | Notes |
|---|---|---|---|---|---|
```

必查：

- 是否存在第二 Writer；
- Derived 是否被当 Writer；
- Reference 是否越权。

---

# T03｜Semantic Terminology Registry

```markdown
| Term | Meaning | Owner | Scale | Kind | Similar / Conflicting Terms |
|---|---|---|---|---|---|
```

Kind 示例：

- authoritative state
- event
- projection
- reference
- cache
- action
- permission

---

# T04｜Relation Typing Matrix

```markdown
| Consumer | Provider | Relation Type | Condition | Missing-provider Behavior |
|---|---|---|---|---|
```

Relation Type：

- hard
- optional
- handoff
- feature-conditional
- read-only-reference
- UI-contribution

---

# T05｜Authoritative / Derived / Reference / Cache Classification

```markdown
| Object | Classification | Source | Recomputable? | Can Mutate Business State? | Persistence |
|---|---|---|---|---|---|
```

---

# T06｜Aggregate / Instance Bridge

```markdown
# Aggregate / Instance Bridge

Aggregate Owner:
Instance Owner:

## Materialize
Trigger:
Transaction:
Double-count prevention:

## Aggregate Back
Eligibility:
Transaction:

## Save / Restore
## Deletion / Merge
## Regression Cases
```

---

# T07｜Cause / Process / Consequence Chain

```markdown
Cause Owner
↓ Handoff
Process / Resolution Owner
↓
Persistent Consequence Owner
↓ optional propagation
Higher-level Owner
```

审计问题：

- 是否跳过中间层？
- Source 是否直接写 Target state？
- Handoff 是否包含足够 provenance？

---

# T08｜Supersession / Migration Ledger

```markdown
| Legacy Concept | Action | New Owner / Target | Dependency Rebind | Compatibility Required? | Notes |
|---|---|---|---|---|---|
```

Action：

- DELETE
- KEEP
- MIGRATE
- MERGE
- SPLIT
- RENAME
- REBIND
- LEGACY-REFERENCE

附：

```markdown
Legacy asset status:
Replacement:
Do-not-install rule:
Preflight conflict:
```

---

# T09｜Cluster Convergence Audit

```markdown
# Cluster Convergence Audit

Assets / Modules:

## Ownership
## Semantic Namespace
## Hard / Optional / Conditional Dependency
## Handoffs
## Hard Cycles
## Derived / Reference Authority
## Aggregate / Instance
## UI Owner / Contributor
## Version Consistency
## Migration Residue
## Program / Business Authority
## Regression
## Final Result
```

---

# T10｜Surface Owner / Contributor Matrix

```markdown
| Surface / Workspace | Owner | Contributors | Conditional Activation | Conflict Rule |
|---|---|---|---|---|
```

检查：

- duplicate owner；
- feature off empty shell；
- layout authority；
- UI preference vs business state。

---

# T11｜Provisional Owner Review

```markdown
Provisional Domain:
Current Owner:
Why provisional:
Expected future consumers:
Genericity test:
Extraction trigger:
Migration strategy:
Decision:
- keep provisional
- promote to shared core
- keep domain-specific
```

---

# T12｜Decision / Pending Integration Ledger

```markdown
| ID | Decision | Scope | Immediate? | Target Documents | Integration Milestone | Status |
|---|---|---|---|---|---|---|
```

避免每条小决策即时升级全部主文件。

---

# T13｜State-machine Sequence Test Matrix

```markdown
| Scenario | Initial State | Operation Sequence | Expected State | Expected Side Effects | Forbidden Side Effects |
|---|---|---|---|---|---|
```

至少包含：

- positive
- same-state/no-op
- repeat
- invalid order
- interruption
- retry
- return baseline
- fault injection

---

# T14｜Logical Identity Contract

```markdown
Business identity:
Immutable identity inputs:
Mutable metadata:
Technical key:
Conflict behavior:
Replay behavior:
```

---

# T15｜Replacement Decision Record

```markdown
Old design:
New design:
Why new design supersedes:
Real compatibility obligation:
Unique old deltas:
Migration plan:
Deletion plan:
Rollback / recovery:
Completion criteria:
```

---

# T16｜Asset / Module Lifecycle Status

```markdown
Status:
- exploration
- semantic-candidate
- audited-current
- runtime-bound
- released
- deprecated
- legacy-reference
- archived

Allowed downstream dependency:
Reopen condition:
```

---

# v1.6 Change Log

新增：

- Domain Architecture Survey；
- Canonical Ownership Matrix；
- Semantic Terminology Registry；
- Relation Typing Matrix v2；
- Authority Classification；
- Aggregate / Instance Bridge；
- Cause Chain；
- Supersession Migration Ledger；
- Cluster Convergence Audit；
- Surface Owner Matrix；
- Provisional Owner Review；
- Decision Ledger；
- Sequence Test Matrix；
- Logical Identity Contract；
- Replacement Decision Record；
- Lifecycle Status Template。

---

## DSH 执行适配

模板本身是文档骨架，在 DeepSeek Harness 下：

- 产出模板文档：用 `write` 写入当前工作区，按模板标题命名（T01~T16）。
- 回填真实项目数据前先读事实：用 `read`/`glob`/`grep` 收集仓库与代码事实，不得用占位符冒充真实值。
- 需要用户裁定术语或边界：用 `ask_user_question` 一次性确认，避免反复打断。
- 审核模板产物正确性：用 `subagent_fork` 隔离复核。