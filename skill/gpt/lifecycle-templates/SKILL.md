---
name: lifecycle-templates
description: Provide reusable governance templates for canonical product definition, core-purpose and baseline validation, ownership, dependency typing, migration, convergence audits, state-machine sequences, stage-gate classification, host/protocol readiness, declarative data boundaries, real-instance validation, playability completeness, affordance consistency, instance evolution, and integration-of-meaning UAT. Use when a structured product-definition, architecture, migration, platform/extension, AI-product, game-runtime, or lifecycle template is needed.
---

# lifecycle-templates v2.0

> [!abstract]
> 为产品定义、生命周期、资产、模块、AI 产品与长期实例治理提供可复用模板。
>
> 模板是结构骨架；应替换为项目术语，并根据成熟度删减无关部分。
>
> v2.0 在 v1.9 的 Product Definition 模板基础上，新增 Primary Purpose、Core Value、Support Capability、Simple Baseline 与 Early Reality Check，避免功能和工程设计在没有产品最高坐标系时自行膨胀。

---

# T00｜Canonical Product Definition / Product Spec

```markdown
# Canonical Product Spec

Status:
Version / Current Path:
Product Owner:
Last Updated:

## 1. Primary Purpose / Job To Be Done
One-sentence primary purpose:

User mainly uses this product to:

If this fails, the product is considered failed even when engineering is correct because:

## 2. Target Users / Actors
| Actor | Goal | Key Need | Explicitly Not Target? |
|---|---|---|---|

## 3. Product Promise
One-sentence promise:

## 4. Core Experience / Core Value Loop
Entry
↓
User motivation / situation
↓
Meaningful action
↓
System / world response
↓
Visible consequence / value
↓
Reason to continue

Non-negotiable core experience:

## 5. Core User Journey
Entry
↓
Key action
↓
System response
↓
Value delivered
↓
Continued use

## 6. Current Alternative / Simple Baseline
Current user alternative:
Simple baseline:
Core dimensions where the product must not be clearly worse:

## 7. Functional Requirements
| ID | Requirement | Must / Should / Later | Core Value or Support? | Acceptance |
|---|---|---|---|---|

## 8. Non-functional Requirements
- security / privacy:
- persistence / recovery:
- performance / latency:
- compatibility / platform:
- accessibility / resilience:

## 9. Support Capabilities / Guardrails
| Capability / Guardrail | Risk / Need Addressed | Core Value Protected | Core Value Potentially Restricted | Narrower Boundary Possible? | Owner Decision Needed? |
|---|---|---|---|---|---|

## 10. Domain Semantics
| Term | Product Meaning | Must Not Be Confused With | Notes |
|---|---|---|---|

## 11. Scope / Non-scope
### In Scope
### Deferred
### Explicit Non-scope

## 12. Constraints
- technology:
- provider / model:
- deployment:
- data:
- cost:
- license / policy:

## 13. Success / Acceptance
| ID | Product Success Condition | Engineering Evidence | Real User / Owner Evidence | Baseline Comparison |
|---|---|---|---|---|

## 14. Early Reality Check Plan
Earliest runnable core loop:
Who tests it:
Minimum real-use sequence:
Failure signals that reopen Product / Architecture:

## 15. Open Questions / Blockers
| Question | Blocking? | Owner | Resolution Needed Before |
|---|---|---|---|

## 16. Decision Ledger
| ID | Decision | Why | Evidence / Source | Affected Scope |
|---|---|---|---|---|

## Product Definition Gate
- [ ] primary purpose / JTBD is explicit
- [ ] target user / actor is clear
- [ ] product promise is clear
- [ ] core experience / core value loop is coherent
- [ ] non-negotiable core is explicit
- [ ] core-value capabilities vs support capabilities are separated
- [ ] current alternative / simple baseline is identified
- [ ] core user journey is coherent
- [ ] must-have vs deferred vs non-scope are separated
- [ ] key domain semantics are defined enough for architecture
- [ ] high-impact guardrails have explicit risk/value tradeoff
- [ ] hard constraints are known
- [ ] success criteria include real product-value evidence
- [ ] earliest owner/user reality check is planned
- [ ] blocking open questions are resolved or explicitly block architecture
- [ ] unique current Canonical Product Spec source exists

Result:
- NOT READY FOR ARCHITECTURE
- READY FOR DOMAIN / CAPABILITY SURVEY
```

规则：讨论、聊天和原型不是 Canonical Product Spec 的替代物。Gate 前允许 spike / prototype / mock，但必须标记为 exploration；不得据此冻结长期 Schema / Protocol / Domain Model。若简单基线在核心用途上明显更好，优先重开产品/架构假设，而不是继续堆支撑性能力。

---

# T01｜Domain / Capability Architecture Survey

```markdown
# Domain Architecture Survey

Product Definition Gate:
Canonical Product Spec:
Primary Purpose:
Core Value / Core Loop:
Simple Baseline:

## 目标范围

| Domain | Business Meaning | Core Value or Support? | Expected Consumers | Generic? | Candidate Owner | Status |
|---|---|---|---|---|---|---|

## Shared Foundation Candidates
## Product / World-specific Domains
## Provisional Owners
## Guardrail Risks
## Risks
- duplicate owner:
- dependency inversion:
- future extraction:
- product-spec contradiction:
- core-value regression:
```

---

# T02｜Canonical Ownership Matrix

```markdown
| Fact / State | Canonical Owner | Definition Contributor | Readers | Authorized Writers | Persistence / Lifecycle | Notes |
|---|---|---|---|---|---|---|
```

必查：第二 Writer、Derived 越权、Reference 越权、模型直写、UI 直写。

---

# T03｜Semantic Terminology Registry

```markdown
| Term | Meaning | Owner | Scale | Kind | Lifecycle | Similar / Conflicting Terms |
|---|---|---|---|---|---|---|
```

Kind 示例：authoritative state、event、projection、reference、cache、declaration、materialized view、source template、local instance、runtime state。

---

# T04｜Relation Typing Matrix

```markdown
| Consumer | Provider | Relation Type | Condition | Missing-provider Behavior | Context Inclusion? |
|---|---|---|---|---|---|
```

Relation Type：hard、optional、handoff、feature-conditional、read-only-reference、UI-contribution、compiler-target。

---

# T05｜Authority Classification

```markdown
| Object | Classification | Canonical Source | Recomputable? | May Mutate Business State? | Persistence |
|---|---|---|---|---|---|
```

Classification：Authoritative、Derived / Materialized View、Reference、Cache、Legacy Evidence。

---

# T06｜Aggregate / Instance Bridge

```markdown
# Aggregate / Instance Bridge

Aggregate Owner:
Instance Owner:

## Materialize
Trigger:
Typed input:
Validation:
Identity:
Transaction:
Double-count prevention:

## Aggregate Back / Merge
Eligibility:
Transaction:

## Save / Restore / Branch
## Deletion / Deduplication
## Regression Cases
```

---

# T07｜Cause / Process / Consequence Chain

```markdown
Cause Owner
↓ typed Handoff
Process / Resolution Owner
↓
Persistent Consequence Owner
↓ optional propagation
Higher-level Owner
```

检查：是否跳过中间层、Source 直接写 Target、Handoff 是否保留 provenance。

---

# T08｜Supersession / Migration Ledger

```markdown
| Legacy Concept | Action | New Owner / Target | Dependency Rebind | Compatibility Required? | Unique Delta | Completion Evidence |
|---|---|---|---|---|---|---|
```

Action：DELETE、KEEP、MIGRATE、MERGE、SPLIT、RENAME、REBIND、LEGACY-REFERENCE。

---

# T09｜Cluster Convergence Audit

```markdown
# Cluster Convergence Audit

Assets / Modules:

## Primary Purpose / Core Value Alignment
## Simple Baseline Comparison
## Guardrail Tradeoffs
## Product Spec Alignment
## Ownership / Authority
## Semantic Namespace
## Source / Instance / Runtime Separation
## Dependency / Handoff
## UI Owner / Contributor
## Aggregate / Instance
## Migration Residue
## Fixture vs Production Proof
## Contract vs Capability vs Product UAT
## Declarative Structure vs Live Data
## Real Creation / Real Instance
## Playability Completeness
## Affordance Consistency
## Context Sufficiency
## Long-session Evolution
## Integration of Meaning
## Regression / Recovery
## Final Result
```

---

# T10｜Surface Owner / Contributor Matrix

```markdown
| Surface / Workspace | Owner | Contributors | Conditional Activation | Data Owner | Conflict Rule |
|---|---|---|---|---|---|
```

检查 duplicate owner、feature-off empty shell、layout authority、preference vs business state、source identity。

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
- promote shared core
- keep domain-specific
```

---

# T12｜Decision / Pending Integration Ledger

```markdown
| ID | Decision | Scope | Immediate? | Product Purpose Impact | Product Spec Impact | Reopen Product Definition Gate? | Affected Stage / Task DAG | Target Sources | Integration Milestone | Status |
|---|---|---|---|---|---|---|---|---|---|---|
```

发现新 current decision 后必须检查 Primary Purpose、Product Spec、Product Definition Gate、Stage Gate、Current Task、Prerequisite、Task DAG、Protocol Timing 与 Next Recommendation。

---

# T13｜State-machine Sequence Test Matrix

```markdown
| Scenario | Initial State | Operation Sequence | Expected State | Expected Side Effects | Forbidden Side Effects | Recovery / Replay |
|---|---|---|---|---|---|---|
```

至少：positive、negative、repeat、invalid order、interruption、retry、return baseline、fault injection、active → dormant → active。

---

# T14｜Logical Identity Contract

```markdown
Business identity:
Immutable identity inputs:
Mutable metadata:
Source provenance:
Local-instance identity:
Runtime identity:
Technical key:
Conflict behavior:
Replay behavior:
```

---

# T15｜Replacement Decision Record

```markdown
Old design:
New design:
Why superseded:
Real compatibility obligation:
Unique old deltas:
Migration plan:
Deletion plan:
Rollback / recovery:
Zero-reference gate:
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
- product-proven
- released
- deprecated
- legacy-reference
- archived

Allowed downstream dependency:
Reopen condition:
```

---

# T17｜Stage Exit Critical-path Classification

```markdown
# Stage Exit Classification

Current maturity:
- exploration
- internal/personal
- alpha
- release

Primary Purpose:
Core Value Gate:

| Remaining Item | Class A/B/C/D | Changes Downstream Contract? | Changes Core Value? | Stage Objective Minimum? | Underlying Semantics Proven? | Required Now? | Revisit Trigger | Non-regression Constraint |
|---|---|---|---|---|---|---|---|---|

## Required Critical Path
## Deferred Backlog
## Why Deferral Is Safe
## Re-entry Gate
```

A = downstream architecture prerequisite；B = stage objective minimum；C = product consumption / UX maturity；D = polish / scale / future platform。决定 Primary Purpose 是否成立的缺口不得误归 C/D。

---

# T18｜Host / Platform → External Protocol Readiness

```markdown
# Host / Protocol Readiness

Host / Platform:
Future Protocol / Asset / Plugin / Compiler:
Primary Purpose Supported:

## Internal Capability
- typed declaration:
- canonical owner:
- safe vocabulary:
- source identity:

## Production Vertical
internal declaration
→ bootstrap / server
→ official projection
→ real consumer

## Fail-closed
- duplicate owner:
- invalid dependency:
- unsupported capability:
- arbitrary code / state access:
- partial assembly exposure:

## Protocol May Declare
## Protocol Must Not Invent

Result:
- NOT READY
- READY FOR PROTOCOL DESIGN
```

---

# T19｜Declarative Structure / Live Data Boundary

```markdown
Declaration owner:
Runtime authoritative owner:
Projection / materializer owner:
Consumer / Host:

## Declaration may describe
## Declaration may NOT contain
- arbitrary state path
- callback
- expression / eval
- arbitrary query DSL

## Data Flow
Authoritative State
→ bounded projection
→ server-side materializer
→ safe DTO
→ consumer

## Provenance / Identity
## Hidden-data Regression
```

---

# T20｜Deferred Product Capability Ledger

```markdown
| Capability | Core Value or Support? | Why Deferred | Underlying Authority Proven By | Downstream Schema Impact | Revisit Stage | Re-entry Gate | Non-regression Constraint |
|---|---|---|---|---|---|---|---|
```

---

# T21｜Owner-aligned Execution Matrix

```markdown
| Task / Diff | Primary Owner | Implementation Scope | Self-validation | Product Value Gate? | Git Integration Owner | Independent Reviewer | Cross-owner Integration Needed? |
|---|---|---|---|---|---|---|---|
```

默认 task-owned diff 由 Executor 完成 Gate + precise commit/push；纯代提交不形成第二开发周期。产品体验是否成立由 Product Owner /真实用户验收，不由 Executor 自宣。

---

# T22｜Engineering Correctness / Product Value / Playability Matrix

```markdown
# Engineering vs Product Completeness

Primary Purpose:
Simple Baseline:

| Dimension | Evidence | Result | Missing / Risk |
|---|---|---|---|
| Core Purpose / Core Value | | | |
| Simple Baseline Comparison | | | |
| Authority / Transaction | | | |
| Persistence / Recovery | | | |
| Real User Entry | | | |
| Real Created / Imported Instance | | | |
| Minimum Useful Content / Capability | | | |
| Multi-step Continued Use | | | |
| Meaningful Feedback | | | |
| Desire / Reason to Continue | | | |
| Long-session Evolution | | | |

Engineering Gate:
Stage Objective Gate:
Product Value Gate:
Product UAT Gate:
Final Decision:
```

禁止用 Engineering PASS 自动推出 Stage/Product PASS；若 Simple Baseline 在核心价值上明显更好，必须记录为产品阻塞而不是“未来体验优化”。

---

# T23｜Contract → Capability → Production Semantics Trace

```markdown
| Capability | Core Value or Support? | Contract | Production Owner | Real Data Source | Implementation | Failure Fallback | Official Consumer | State-sensitive? | Fixture Proof | Production Vertical | Product UAT |
|---|---|---|---|---|---|---|---|---|---|---|---|
```

成熟度：Contract only → Fixture proven → Production vertical proven → Product UAT proven。

---

# T24｜Real Creation / Fixture Realism Vertical

```markdown
# Real Instance Vertical

Rich Fixture:
Official User Creation / Import Path:
Primary Purpose:

## Input Mapping
| User Field / Choice | Canonical Instance Target | Runtime / Product Consumer | Evidence |
|---|---|---|---|

## Minimum Instance
- real objects / entities:
- capabilities:
- connections / relations:
- stable identities:

## Compare
| Capability | Rich Fixture | Real Created Instance | Gap |
|---|---|---|---|

## Continued-use Test
Step 1:
Step 5:
Step 10:
Core-value verdict:

Result:
```

---

# T25｜Affordance Consistency Audit

```markdown
| Visible Affordance | Concrete / Ambient | Authoritative Referent | Current Capability | Mutation / Action Path | Next-step Test | Result |
|---|---|---|---|---|---|---|
```

规则：具体可交互对象必须有 referent；ambient 不得在下一步变成 phantom target；Narrative / UI 不得宣称未提交变化。

---

# T26｜Source Template / Local Instance / Runtime State Model

```markdown
# Three-layer Instance Model

## Source Template
Owner:
Version / hash:
Immutable rules:

## Local Canonical Instance
Identity:
Source provenance:
Mutable field policy:
Private field policy:
Revision:

## Runtime State
State owner:
Transient / mechanical fields:
Formal mutation path:

## Bind / Clone
Full copy or snapshot + overlay:

## Model-generated Mutation
Typed proposal:
Validation:
Commit:

## Save / Restore / Branch
## Source Upgrade / Migration
## Regression
```

---

# T27｜Long-session World / Instance Evolution Audit

```markdown
# Long-session Evolution Audit

Primary Purpose at short horizon:
Would user plausibly want to reach interaction 10?:

## 10 interactions
New content source:
Identity continuity:

## 50 interactions
Growth owner:
Save / Restore:
Context size:

## 100 interactions
New entity / object creation:
Instance mutation:
Branching:
Archive / retrieval:

## Required Answers
- where do new objects come from?
- where are they stored?
- how are refs stabilized?
- how does source remain unchanged?
- how does context remain bounded?
- how does the user find and use grown content?
- why would the user still want to continue?

Result:
```

---

# T28｜Integration-of-Meaning UAT Corpus

```markdown
# Meaning UAT

Primary Purpose:
Core Value Loop:
Simple Baseline:

| User Input / Task | Interpretation | Program / Domain Outcome | Canonical State | Narrative / Response | Product Projection | Next-step Capability | Semantic Consistency | Core Value Delivered? |
|---|---|---|---|---|---|---|---|---|

## Negative Cases
- visible object without referent
- response claims uncommitted state change
- user setup collected but not consumed
- placeholder presented as intelligent capability
- context starved
- guardrail blocks legitimate core behavior
- product requires user to manually compensate for missing core loop
- simple baseline is clearly better on primary purpose

## UAT Cadence
- earliest core-loop reality check: as soon as runnable
- short vertical: 1–3 steps
- real instance: 5–10 steps
- stage meaning UAT: 10–20 steps
- long-session thought experiment: 100 steps

Final Result:
```

---

# T29｜Core Value Preservation / Baseline Comparison

```markdown
# Core Value Preservation Audit

Primary Purpose:
Core User Value:
Non-negotiable Core:
Current Alternative / Simple Baseline:

## Major Architecture / Guardrails
| Design / Constraint | Why It Exists | Core Value Protected | Core Value Restricted | Narrower Alternative | Product Owner Acceptance Needed? |
|---|---|---|---|---|---|

## Baseline Comparison
| Core Dimension | Simple Baseline | Current Product | Better / Same / Worse | Evidence |
|---|---|---|---|---|

## Falsification Signals
- what user behavior would prove the product purpose is not being delivered?
- what would make us reopen Product Definition?
- what is the earliest real-use test?

Result:
- PASS: core value preserved / improved
- BLOCKED: architecture or guardrail harms core value
- REOPEN PRODUCT DEFINITION
```

---

# v2.0 Change Log

相对 v1.9：

- T00 从“产品规格先于架构”进一步升级为“Primary Purpose / Core Value 先于功能列表”；
- T00 新增 Core Experience / Core Value Loop、Non-negotiable Core、Current Alternative / Simple Baseline、Support Capabilities / Guardrails、Early Reality Check；
- Product Definition Gate 新增核心用途、支撑性能力区分、Guardrail tradeoff、简单基线与最早真人验证；
- T01 / T09 / T12 / T17 / T18 / T20 / T21 / T23 / T24 / T27 / T28 加入 Core Value alignment；
- T22 升级为 Engineering / Product Value / Playability 三层矩阵，并加入继续使用意愿与简单基线对照；
- 新增 T29 Core Value Preservation / Baseline Comparison，专门识别 Guardrail 反客为主与复杂系统输给简单替代方案；
- 保留 v1.9 的 Ownership、Migration、Playability、Real Instance、Affordance、Capability Provenance、Source / Local / Runtime、Long-session 与 Integration of Meaning 全部模板。