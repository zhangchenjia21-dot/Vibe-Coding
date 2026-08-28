---
name: tavern-asset
description: 用核心优先架构、canonical 所有权、类型化依赖、有界运行时上下文契约、特性/模块激活层级、定义注册表投影、大型关系图确定性相关子图投影、后台程序推进、有界充分上下文、保留 owner 的跨 owner 投影连接、结果门控延续激活与 UI surface 所有权，来设计、审核、迁移、收敛、组织与维护酒馆 World Pack、Character Card、Expansion Pack、资产族与通用资产库。当 current 核心文件或本 Skill 变化时，同一任务内交付新文件并同步权威 GitHub current 源。不用于运行时实现。
whenToUse: 酒馆（SillyTavern 系）世界包、人物卡、拓展包、资产族、通用资产库的讨论、创作、修订、审核、迁移、收敛、治理与仓库维护；不用于运行时实现。
---

# tavern-asset v1.0

> [!abstract] 定位
> 用于 Tavern 资产的讨论、创作、修订、迁移、审核、收敛、资产族治理、通用资产库治理和资产仓库维护。
>
> v1.0 完整继承 v0.9，并吸收 `EP-KINSHIP-CORE v0.1` 与 `EP-POLITICS-CORE v0.1` 两个独立大型关系图领域共同验证的稳定规则：
>
> - **Large Relation Graph / Deterministic Subgraph Projection**：长期大型关系图保存语义上真正 Canonical 的主关系 / 源事实；可确定重算的 pairwise / aggregate 关系保持 Projection；Program 先按当前问题选取 relevant path / subgraph，再向模型提供 player-safe、owner-preserving 的最小充分语义切片。
>
> v0.9 的 Bounded Cross-Owner Projection Join、Outcome-Gated Continuation Activation、Registry Projection、`Bounded != Starved`、Narrative authoritative referent、Background Program Progression 与 Feature / Module Activation 全部继续有效。
>
> 人工 Skill 版本继续遵守一位小版本序列：`v0.9 → v1.0 → v1.1`；不产生 `v0.10 / v1.0.1` 等人工维护编号。

---

# 0. 强制执行顺序

1. 识别任务：规划 / 讨论 / 创作 / 修订 / 通用库维护 / 审核 / 迁移 / 退役 / 仓库重构 / Blueprint / Bundle。
2. 先恢复 GitHub 最新 current：相关 Blueprint、Core、资产版本、正式裁定与本 Skill；多聊天项目执行 Freshness / Decision Propagation。
3. 新资产族或大规模扩展先做 `Domain Survey → Genericity → Shared Foundation → Canonical Owner`。
4. 未获明确授权时先讨论；用户已授权的批次不重复确认。
5. 执行 Ownership / Namespace / Relation / Dependency / Definition-Instance / Open Attempt / Program Authority / Runtime Relevance-Context / Surface Gate。
6. 创作、修订或迁移。
7. 单资产 Audit；依赖簇形成后 Cluster Convergence；阶段结束后 Family / Generic Library Convergence；必要时执行 Future Explosion Review / User Reflection。
8. 旧资产被替代时执行 Supersession / Migration。
9. 同步受影响的 Asset Family / Generic Library Workspace。
10. current 核心文件、Skill、Blueprint、版本矩阵或治理资料变化时执行 Repository Sync Gate。
11. 同轮 >=3 个正式文件时生成 Transport ZIP + manifest。
12. G9-03 machine contract 真正冻结前，不把 Markdown 语义稿伪装成最终 JSON Schema / Runtime API / Router API / Bundle Contract。

---

# 1. GitHub Repository Registry

| Owner | Repository | 职责 |
|---|---|---|
| Skill | `zhangchenjia21-dot/Skill` | GPT / DSH Skill；本 Skill current = `skill/gpt/tavern-asset/SKILL.md` |
| Game | `zhangchenjia21-dot/sillytavern` | Runtime / Product UI / 当前代码与测试 |
| Assets | `zhangchenjia21-dot/sillytavern-assets` | World Pack / Character Card / Expansion / Asset Family / Generic Library |
| Vibe-Coding | `zhangchenjia21-dot/Vibe-Coding` | 项目治理、阶段、核心裁定、Lifecycle / Harness |

路由：

```text
Skill → Skill repo
Game runtime / implementation → sillytavern
Asset semantics / workspaces → sillytavern-assets
Project / lifecycle governance → Vibe-Coding
```

## 1.1 Remote-current-first

正式任务前先读取目标 remote current。若远端比当前聊天 / 本地稿更新，先合并差异，禁止旧稿覆盖新事实。

多聊天并行时，发现新 current / superseding decision 后，不只“读到”，还要检查其是否改变当前 Asset Task DAG / G9 timing / Owner / Host boundary。

## 1.2 Local Delivery + GitHub Sync

用户要求更新核心文件 / Skill / GitHub，或本 Skill 判定 current source 必须升级时，同一任务默认同时：

```text
Local full-file delivery
+
GitHub authoritative current sync
+
readback verification
+
commit SHA report
```

## 1.3 Supersede-in-place

- 固定路径 Skill 直接 update；
- 带版本 current 文件：先创建新 current，再移出旧 active current；
- Git 历史承担普通版本留档；
- active 目录禁止多个“最新 / 最终 / new-final”并列。

---

# 2. Repository-aware Asset Architecture

```text
sillytavern-assets/
├─ 世界包/
├─ 人物卡/
├─ 拓展包/
├─ 资产族/
└─ 通用资产库/
```

## 2.1 Canonical Asset Store

World Pack / Character Card / Expansion 正文唯一物理事实源位于对应按类型目录。Asset Family Workspace 与 Generic Library Workspace 只做索引、Blueprint、Ownership、Dependency、Context Contract、Audit、版本与规划，不复制第二份正文。

## 2.2 Asset Family

Asset Family 是长期创作 / 维护单位，不等于 Bundle。允许成员索引、蓝图、版本锁、Owner / Dependency / Handoff、创作规范、研究、审核、平台需求。

## 2.3 Generic Library

`通用资产库/` 管理跨世界资产的身份、版本、Owner、Dependency、Consumer、Runtime Context Contract、Roadmap、Audit。

`first consumer != generic asset identity`。

## 2.4 Context Sidecar

成熟 audited-current 资产若业务 Domain Semantics 完全不变，只缺横切 Runtime Context Binding，可以在 Generic Library Workspace 使用唯一 Context Sidecar：

- 精确绑定 `asset_id + asset_version`；
- `canonical_domain_ownership: none`；
- 不新增业务能力；
- 不改变 Owner / Formal Outcome；
- 同一资产版本只有一个 active Context Contract source。

冲突优先级：

```text
Canonical Asset Domain Semantics
>
Context Sidecar Binding Metadata
```

若 Context Audit 暴露真实 Domain / Dependency / Program Authority 问题，必须修正文，不得用 Sidecar 遮盖。

---

# 3. Core-first / Shared Foundation

新资产族生产前建立：

```text
Domain → Candidate Owner → Consumers → Generic/World-specific → Status
```

Genericity Gate：去掉世界 / 题材 / 文化名仍成立，且有多个独立消费者的长期机制，优先抽 Generic Core。

Shared Foundation Gate：多个消费者会重复拥有同一长期事实时，先做共享上游，再做下游。

默认顺序：

```text
Domain Survey
→ Shared / Generic Core
→ Generic Downstream
→ World-specific Domain
→ Theme / Content
→ Character Batch
→ Convergence
```

Provisional Owner 必须写 extraction trigger / migration plan，不能被默认为长期架构。

---

# 4. Canonical Ownership Gate

每类长期正式事实只能有一个 Canonical Owner。必须区分：

- Definition Contributor；
- Instance State Owner；
- Source / Provenance；
- Reader；
- Projection / Cache；
- UI Contributor。

`Definition Contributor != Instance State Owner`。

Derived score / summary / projection 默认可重算、不可反写 Canonical State。

Aggregate 与 named instance 必须分离，例如：

```text
Formation Casualties != Named Character Health
Bulk Resource != Discrete Item
```

跨域链按 `Cause → Process → Consequence` 分层，每层只修改自己拥有的事实。

---

# 5. Semantic Namespace

维护 `Term / Meaning / Owner / Scale / Kind / Lifecycle`。

同名不同 Owner 或尺度必须 rename / namespace / scale-separate / merge。命名冲突可能是 Ownership 冲突，不只是文案问题。

---

# 6. Typed Relations

必须明确：

- Hard Dependency；
- Optional Integration；
- Feature / Module Conditional Dependency；
- Provider → Consumer；
- Handoff；
- Contribution；
- Reference / Read-only；
- UI Owner / Contributor（如适用）。

禁止一个模糊 `dependency` 覆盖全部关系；禁止 Hard Cycle；禁止 fallback 创建第二事实源。

正式冻结：

> **Dependency Graph != Context Inclusion Graph。**

依赖意味着组合 / 权威关系，不意味着下游每次模型调用都加载上游全文。

## 6.1 Feature / Module Conditional Dependency

大型 Package 若内部能力可独立启停，依赖应尽量下沉到实际 Feature / Module：

```text
Package supports X
!= Package hard-dep X provider

Module X enabled / invoked
→ conditional provider requirement
```

关闭的 Feature / Module 不产生其 Conditional Dependency，也不应进入 Router Directory。

---

# 7. Definition / Instance

```text
Definition → instantiate → Runtime / Game State
```

资产更新不静默覆盖已存在实例；实例变化不回写 Definition。T0/bootstrap/resolver 只产生 Instance Candidate。

---

# 8. Open Attempt

Skill / Class / Module / Role / Authority / Resource / Relationship stage 不能成为玩家输入白名单。它们影响 Formal Effect、成功可能、代价与后果，不删除合理 Attempt。

Context Router / Capability Directory 同样不是白名单。Router 未命中预定义 Domain 时，可 generic interpretation / clarification / unresolved candidate；不能因此宣告玩家行为非法。

---

# 9. Program Authority

资产可定义 Grammar、Definition、Capability、Permission、Resolution semantics、Event、Handoff、UI intent。

资产 / 模型不得拥有：

- RNG / Dice；
- Program Judge；
- Formal Outcome；
- direct State Mutation；
- Atomic Commit；
- Save / Restore authority。

强能力可以真强，但必须显式权限化。

能由 Program deterministic、安全、可测试地执行的逻辑默认留在 Program，不为了“规则在资产正文里”就塞进模型推理。

## 9.1 Background Program Progression

长期机制中的：

- Need accumulation；
- timer / cooldown；
- routine automation；
- deterministic resource consumption；
- threshold bookkeeping；
- other deterministic lifecycle progression；

若 Program 可可靠执行，则：

```text
Background Runtime Progression
!= Model Activation
```

只有出现玩家自由语言理解、开放式语义、重要决策 / warning、或 Handoff 需要另一个 Domain 参与时，再构建 Model Working Set。

不得把持续机制错误实现为“每个 tick 都调用模型”。

---

# 10. Surface Ownership / Contribution

先判断：Extension Surface Owner / Contributor / Core Surface Contribution / Entity Detail / Player Status / Contextual / Alert / Settings-Creation / Map Overlay。

规则：
- unique Extension Surface 一个 Owner；duplicate Owner fail closed；
- Contributor 不因显示而成为 Owner；
- Feature OFF 不强制空 Surface；
- 资产声明 IA / intent，不执行任意 JS / React / DOM / CSS；
- Host 拥有布局、安全、响应式和 Accessibility；
- UI preference 不是 Game State。

---

# 11. Supersession / Migration

新 Core / 新架构完整取代旧 Owner 时：

```text
retire old owner
→ preserve legacy evidence
→ classify unique deltas
→ migrate/rebind
→ remove obsolete production path
```

Ledger 使用 DELETE / MERGE / MIGRATE / RENAME / SPLIT / KEEP AS REFERENCE / REBIND。

未发布且无真实兼容义务时，完整 supersession 优先于长期 compatibility adapter。

---

# 12. Character Card Gate

重要 Character Card 额外检查：价值排序、欲望、恐惧、判断方式、核心矛盾、偏见/盲点、风险偏好、压力反应、改变条件、语言/情感表达、制度立场。

T0 后现实历史 / 原卡 Definition 不能覆盖 Runtime Character Agency / current state。

---

# 13. Runtime Relevance / Context Contract

## 13.1 基本集合

```text
Asset Library
!= Game Enabled Asset Set
!= Current Runtime Relevant Set
!= Current Model Visible Working Set
```

`Enabled != Prompted`；`Runtime Relevant != Model Visible`；`Full Asset != Prompt`。

对于内部 Feature / Module 化 Package，进一步：

```text
Asset Library / Installed
!= Game Package Included
!= Feature Enabled
!= Module Enabled
!= Current Runtime Relevant
!= Current Model Visible
```

**Package capability breadth != Router / Context breadth。**

对于拥有大型 Definition Registry / Library 的 Domain：

```text
Domain Active
!= Full Definition Registry Visible
```

对于长期大型关系图：

```text
Whole Relation Graph
!= Model Prompt
```

## 13.2 Routing Profile / Directory

每个可路由单元至少有极小：ID / Name / One-line Scope / 少量 Typical Semantics。

Router 只看当前 Game 真正启用的 Profile：
- 不看整个 Asset Library；
- 对大型 Package 不暴露关闭的 Feature / Module；
- Profile 不是关键词白名单，也不复制完整规则。

## 13.3 Model-first Router

```text
Player Input
+ Enabled Routing Profiles
+ minimal scene / active context
↓
Router Model
↓
immediate Domain / Intent candidates
```

Program 不重复自然语言语义路由，只做：
- structural validation；
- state-mandatory augmentation。

## 13.4 Router 不预测所有后果

Router 只负责 immediate relevance；后续：

```text
Formal Event / State Change
→ typed Handoff
→ downstream activation
```

## 13.5 Context Contract 最小问题

每个新 Core / 重要 Expansion 至少回答：

1. Routing Profile；
2. Immediate Activation；
3. State-mandatory Activation；
4. Downstream Activation；
5. No-load Conditions；
6. Minimal Read Set；
7. Model-needed Semantics；
8. Program-owned Logic；
9. Output Candidate；
10. Handoff / Information Boundary；
11. Context Cost / Bounded Strategy；
12. Feature / Module Activation Hierarchy（如适用）；
13. Background Program Progression（如适用）；
14. Definition Registry / Library Projection（如适用）；
15. Bounded Sufficiency / Context Completeness；
16. Narrative Affordance / Runtime Referent（如适用）；
17. Cross-Owner Projection Join（多 Provider / Theme / Composite 时）；
18. Outcome-Gated Continuation Activation（Coupling / Reaction / multi-stage 时）；
19. **Large Relation Graph / Deterministic Subgraph Projection（长期、多节点、多边关系图时）。**

当前不冻结统一 token budget。

## 13.6 Purpose-built Context

Semantic Interpretation 与 Narrative 默认使用不同 Context；Narrative 不重新裁定 Program 已确定的 Formal Outcome。

## 13.7 长期成本原则

```text
Game State / History ↑↑↑
ordinary Turn Context ≈ bounded

Enabled assets ↑↑↑
unrelated Turn Context ≈ stable

Relation Graph ↑↑↑
local query Context ≈ current-relevant subgraph
```

成本主要由 Current Relevant Complexity 决定，而非 Session Length、总资产数或全图规模。

## 13.8 Definition Registry / Library Projection

如果资产维护大型 Spell / Invocation / Recipe / Technique / Item Template / Rule Definition Registry，Domain 激活后仍必须分层：

```text
Enabled Registry
→ actor-accessible / authorized subset
→ intent-relevant candidate retrieval
→ selected Definition Projection
→ Model
```

默认禁止把完整 Registry 作为“Domain Context”注入模型。

## 13.9 Bounded Sufficiency｜有界但不饥饿

`Bounded Context` 不是“尽可能少 token”，而是：

> **完成当前模型职责所需的最小充分高信号集合。**

禁止为了缩短上下文而丢掉决定当前语义正确性的必要信息，例如当前 actor identity / goal、selected Definition、target/ref、当前 outcome、关键关系来源与必要历史。

如果压缩导致模型只能输出泛化、回避或产生 phantom affordance，则属于 **starved context**。

## 13.10 Narrative Affordance / Runtime Referent

玩家可见并且暗示“之后还能继续与之互动”的具体人物、物品、地点、连接、召唤物、政权、家系、议题等 durable/interactable affordance，必须有 authoritative Runtime referent 或由正式 Event / Materialization 产生 referent。

Narrative 不得通过“写得像存在”绕过 Runtime Entity / State Ownership。

## 13.11 Cross-Owner Projection Join

当 Theme / Composite / current task 依赖多个 Provider：

```text
current purpose
→ request bounded projection from each relevant Canonical Owner
→ preserve provenance / ownership
→ current-purpose join
```

正式冻结：

```text
Hard Dependency != Transitive Prompt Inclusion
```

Consumer 不因为 join 获得 Provider state Ownership；禁止把 Provider 全文、全状态和 transitive dependencies 递归塞进 Prompt。

## 13.12 Outcome-Gated Continuation Activation

对 Martial Coupling / Reaction / Counter / multi-stage chain：

```text
Potential continuation != Current Relevant downstream context
```

只有 authoritative upstream Outcome / Trigger 成立后，才加载 continuation 所需 Definition / Owner projection。

## 13.13 Large Relation Graph / Deterministic Subgraph Projection

当领域包含长期、多节点、多边关系图时，先区分：

```text
Canonical primary relation / source fact
vs
Deterministically derivable path / pairwise / aggregate projection
```

正式链：

```text
Large Relation Graph
→ persist semantically primary canonical relations
→ avoid materializing all derivable pairwise relations
→ Program selects current-relevant paths / subgraph deterministically when possible
→ owner-safe + player-safe bounded projection
→ Model only for open semantic work
```

必须避免：

- `Whole Relation Graph = Domain Prompt`；
- 为方便查询把所有可重算 pairwise relation 镜像成第二事实源；
- 用模型做可稳定程序化的 graph traversal / common ancestor / direct recognition / scope filtering；
- 图规模扩大后普通无关 Turn context 随之线性 /平方增长。

Reference：

- Kinship：parentage / adoption 为 Canonical；sibling / cousin / common ancestor / generation distance 多为 Projection；
- Politics：Authority / Claim / Recognition / Control / Issue / Agreement 为 Canonical；综合 legitimacy / loyalty /势力摘要不可反写 Canonical State。

注意：不是“所有关系都只存最原始边”。若某关系本身就是长期正式事实，例如 Membership、Recognition、Agreement、Shared Bond，就应持久化。

---

# 14. Context Contract Location / Retrofit

新资产默认 Context Contract 内建正文。

成熟旧资产若业务语义不变，可以 Sidecar；Context Index 必须维护唯一 canonical location。

资产正文版本变化时，必须重新审核对应 Sidecar；Sidecar stale 时不得继续视为 PASS。

---

# 15. Convergence / Stress Test

Asset Audit：每资产完成后。

Cluster Convergence：检查 Ownership / Dependency / Handoff / Terminology / Version / Surface / Definition-Instance / Runtime Context。

Family / Generic Library Convergence：额外检查 duplicate owner、hard cycle、stale refs、migration residue、context explosion、machine binding gaps。

Context Composition Stress Test 至少覆盖：
- Typical Valid Bundle；
- Heavy Valid Bundle；
- Worst Reasonable Valid Bundle；
- unrelated turn；
- single-domain；
- 2–3 domain integration；
- high-coupling event；
- long session；
- Router miss / over-select / unknown ID；
- state-mandatory augmentation；
- downstream activation；
- background deterministic progression without model calls；
- Feature / Module Directory pruning；
- large Definition Registry scaling；
- selected Definition projection completeness；
- **large Relation Graph scaling**；
- **current-relevant deterministic subgraph / path projection**；
- **derivable pairwise relation mirroring = 0**；
- bounded-sufficiency / anti-starvation；
- durable narrative affordance referent validation；
- owner-preserving bounded cross-owner projection join；
- transitive dependency prompt expansion = 0；
- outcome-gated continuation / miss-branch no-load。

重点风险：
- transitive full-context explosion；
- pairwise integration / relation explosion；
- state mirroring；
- prompt-owned deterministic mechanics；
- disabled feature leakage；
- background tick → repeated model-call explosion；
- Domain active → full registry prompt explosion；
- Whole Relation Graph → full prompt explosion；
- derived pairwise projection → duplicate canonical state；
- 过度压缩导致 starved context；
- Narrative phantom durable/interactable affordance；
- multi-provider Theme → transitive full-context explosion；
- potential continuation → premature downstream context loading。

---

# 16. Architecture Reflection / Future Explosion

在 Shared Foundation、2–5 个高耦合模块成簇、阶段收口、Schema Freeze / 大迁移前，或用户主动质疑长期方向时执行。

至少问：
- 规模扩大 5–10 倍是否仍成立？
- 是否线性 / 平方 / 组合爆炸？
- 是否 duplicate Owner / state mirroring / dual path？
- 模块增加是否扩大无关 Agent / Model context？
- abstraction 是否有真实消费者？
- 是否过度模块化导致 shotgun change？
- 成本 / latency 是否随无关规模增长？
- Domain / Registry 扩大后是否把全部 Definition 泄漏进 Prompt？
- Relation Graph 扩大后是否把整图或大量无关边泄漏进 Prompt？
- 是否为所有 pairwise 派生关系制造持久状态？
- Program 是否能先做 deterministic graph query / filtering？
- Context 是否为了“bounded”被裁到不足以正确理解当前任务？
- Narrative 是否生成没有 Runtime referent 的 durable/interactable affordance？

涉及真实产品体验、长期成本或不可逆权衡时，先给事实 / 风险 / 推荐，再邀请用户反思；普通技术判断不甩给用户。

---

# 17. Creator Authorability / G8-G9 Boundary

每个资产回答：Creator 需要什么有限 Primitive；Runtime / Host 需要什么；Routing / Activation / Minimal Projection / selected Definition / relevant-subgraph Projection 是否可作者化；是否依赖任意脚本；有哪些 binding gap。

Host / Platform 能力必须先在 Game 内部被真实证明，再让外部 Asset Schema 声明。

Semantic Asset 阶段可以继续设计 Context Contract，但在 G9-03 正式授权前不能冻结：
- final JSON / Schema fields；
- Router API；
- Context Compiler；
- relation query DSL / arbitrary state path / selector / expression DSL；
- token budget；
- Creator machine fields。

当前 G9-02BC 已证明 Built-in Domain Host / JIT Projection / bounded join；资产工作可以对齐这些内部能力，但不反向要求 Host 发明未经验证的 external protocol。

---

# 18. Repository-aware Update

资产 / Contract / Core 变化后至少检查：
- member index / Blueprint；
- current matrix；
- Owner / Dependency / Handoff；
- Context Contract Index / status；
- Audit / Platform requirements；
- Consumer / reference world；
- supersedes；
- G9 requirements corpus。

不能只改单文件让 Workspace stale。

---

# 19. Formal Deliverables

- 正式语义成果默认实际 `.md`，YAML 可解析；
- Frontmatter 仅人工治理，不冒充 machine schema；
- >=3 正式文件同轮生成 Transport ZIP + manifest；
- current core / Skill 变化必须 Local Delivery + GitHub Sync；
- 写后回读头 / 关键段 / 尾，确认版本、状态、乱码、截断和 supersession。

---

# 20. Recommended Gate Set v1.0

```text
G0  Current Source / Freshness / Decision Propagation
G1  Discussion / Authorization
G2  Domain Architecture Survey
G3  Genericity / Shared Foundation
G4  Canonical Ownership
G5  Semantic Namespace
G6  Relation Typing / Feature-Module Dependency
G7  Dependency-first / Cycle
G8  Definition / Instance
G9  Canonical vs Projection / Reference / Cache
G10 Aggregate vs Instance / Bridge
G11 Cause / Process / Consequence
G12 Open Attempt
G13 Program Authority / Background Progression
G14 Information Boundary
G15 UI Surface Owner / Contributor
G16 Creator Authorability / Host-before-Protocol
G17 Blueprint Drift
G18 Supersession / Migration
G19 Regression / Sequence
G20 Cluster / Family / Generic Library Convergence
G21 Repository Architecture / Single Physical Source
G22 Repository Sync / Supersede-in-place
G23 Obsidian / YAML / Deliverable / Full-file Readback
G24 Runtime Relevance / Feature-Module Activation / Registry Projection / Large-Graph Subgraph Projection / Bounded-Sufficient / Cross-Owner Join / Outcome-Gated Working Set
G25 Architecture Reflection / Future Explosion Audit
```

---

# 21. v1.0 Change Log

相对 v0.9：

- 版本按人工序列升级为 v1.0；
- 新增 **Large Relation Graph / Deterministic Subgraph Projection**；
- 正式区分 Canonical primary relation / source fact 与 deterministically derivable pairwise / aggregate projection；
- Context Contract 从 18 项扩展为 19 项；
- Program Authority 增加确定性 graph traversal / relevant-subgraph selection 优先；
- Stress Test 新增 large relation graph scaling、relevant-subgraph projection、derived pairwise mirroring = 0；
- Future Explosion 新增 whole-graph prompt / pairwise state explosion 检查；
- Reference evidence：`EP-KINSHIP-CORE v0.1` + `EP-POLITICS-CORE v0.1` 两个独立大型图领域；
- 更新 G9 边界：02BC 已证明 JIT Projection shared foundation，G9-03 前仍不冻结 relation query DSL / final machine schema；
- 完整保留 v0.9 的 Bounded Cross-Owner Projection Join、Outcome-Gated Continuation、Registry Projection、Bounded-Sufficient、Background Progression、Feature-Module 与 Repository-aware 规则。

---

## DSH 执行适配

本 Skill 面向资产设计/创作/审核/迁移/收敛（非运行时实现），在 DeepSeek Harness 下：

- 读取资产源与治理文件：用 `read`/`glob`/`grep` 读取酒馆资产目录（世界包、人物卡、拓展包、资产族、通用资产库）与 current index / blueprint / 版本矩阵。
- 产出/更新资产文档与治理文件：用 `write` 写入工作区；迁移/退役按 Gate 落盘决策记录与迁移 Ledger。
- 授权门（Discussion / Explicit Authorization）：用 `ask_user_question`，一次一问、附推荐项；已授权批次不重复确认。
- 跨资产审核 / 独立复核 / 收敛审核：用 `subagent_fork` 隔离审核，不依据单资产自述。
- 资产来源需联网核验：用 `web_search` 附 URL；GitHub 事实核验用 git / GitHub API（Freshness）。
- 仓库同步义务：本 Skill 的权威源是 `zhangchenjia21-dot/Skill` 仓库 current；本机安装版落后时应按用户规则升级并同步。