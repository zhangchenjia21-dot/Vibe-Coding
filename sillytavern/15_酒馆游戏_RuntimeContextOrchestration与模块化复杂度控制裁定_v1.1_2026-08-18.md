---
title: 酒馆游戏｜Runtime Context Orchestration 与模块化复杂度控制裁定
status: current
version: 1.1
date: 2026-08-18
project: 酒馆游戏新版主体
scope:
  - runtime-context
  - asset-activation
  - model-routing
  - modularity
  - feature-module-activation
  - background-progression
  - g9-upstream
supersedes: 15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.0_2026-08-18.md
---

# 酒馆游戏｜Runtime Context Orchestration 与模块化复杂度控制裁定 v1.1

> [!abstract] 核心裁定
> 酒馆游戏继续采用 World Pack / Character Card / Expansion 的模块化资产架构，但**资产数量、Package 能力宽度、长期世界状态规模，都不得直接等价为单次模型 Working Set 大小**。
>
> 长期世界复杂度沉淀在 Runtime / Database / authoritative state；模型只读取完成当前职责所需的最小、高信号、可有界 Projection。
>
> v1.1 在 v1.0 基础上吸收 Context Retrofit Wave 2 的两个真实资产证据：
>
> 1. `Background deterministic progression != Model Activation`；
> 2. `Package Included != Feature Enabled != Module Enabled != Runtime Relevant != Model Visible`。
>
> 本裁定继续作为 G9-02 Runtime Asset Binding + Context Orchestration Foundation 的正式上游；它冻结产品与架构语义，不冻结最终 JSON / Schema / Router API / token budget。

---

# 1. 审计结论

当前方向继续 PASS：

- World Pack / Character Card / Expansion 分离；
- Canonical Owner / Typed Dependency / Handoff；
- Core-first / Shared Foundation；
- 一局允许启用多个 Expansion；
- Model-first Semantic Routing；
- Program-owned deterministic authority。

禁止：

- `Enabled Expansion = 每 Turn 全量 Prompt`；
- `Package 支持很多能力 = Router 看见全部能力`；
- `持续状态存在 = 每 tick 调一次模型`；
- 让模型承担全部长期记忆、状态推进、RNG、Formal Outcome 与 Commit。

模块化不是风险；**未被隔离的上下文复杂度才是风险**。

---

# 2. Activation / Visibility 集合必须分离

基础集合：

```text
Asset Library
!= Game Enabled Asset Set
!= Current Runtime Relevant Set
!= Current Model Visible Working Set
```

对内部含 Feature / Module 的大型 Package：

```text
Asset Library / Installed
!= Game Package Included
!= Feature Enabled
!= Module Enabled
!= Current Runtime Relevant
!= Current Model Visible
```

含义：

- Package Included：本局组合中包含该 Expansion；
- Feature Enabled：Package 内一级玩法真正开启；
- Module Enabled：该 Feature 下的具体能力真正开放；
- Runtime Relevant：当前 input / state / event 需要该单元参与；
- Model Visible：当前模型职责确实需要看到的最小 projection。

**任何前一层都不能自动推出后一层。**

---

# 3. Full Asset Definition != Model Prompt Payload

完整资产 Markdown 是 Semantic Authoring / Governance Source，面向 Creator、作者、Reviewer、G9 binder / compiler 与 Runtime binding 设计。

目标链：

```text
Full Semantic Asset
↓ compile / bind
Runtime Definition
↓ feature/module activation
Current Relevant Projection
↓
Model Context Slice
```

资产正文可以完整；真正必须有界的是 Runtime Projection。

---

# 4. Model-first Semantic Routing

## 4.1 Router 输入

```text
Player Input
+
Enabled Routing Directory
+
minimal Current Scene / Active Context Summary
```

Directory 只包含当前 Game 真正可用的 routing units：

- 普通 Expansion → Package / Domain Routing Profile；
- Feature/Module Package → 只包含 Enabled Feature / Module Profiles。

关闭能力不进入 Router Prompt。

## 4.2 Routing Profile

每个 profile 只提供：

```text
ID
Name
One-line Scope
small Typical Semantics
```

不是完整规则，不是关键词白名单。

## 4.3 Router 输出

Router 只输出 immediate Domain / Feature / Module / Intent candidate，以及 uncertainty / clarification signal。

它回答：

> **“理解和处理这次玩家输入，现在需要哪些语义能力？”**

---

# 5. Program 的职责：机械校验，不重复 NLP

Router 后 Program 只做：

### Structural Validation
- ID 真实存在；
- 当前 Package / Feature / Module 确实 enabled；
- 输出结构合法；
- 模型没有虚构 capability ID。

### State-mandatory Augmentation
根据 authoritative active state 补充必然参与的 Domain。

例如 Active Combat Context 中“我继续撤退”，Program 可以补 Combat，而不是重新做一次自然语言分类。

### Feature / Module Fail-closed
若 Feature / Module OFF：
- 不允许其 routing ID；
- 不产生其 conditional dependency；
- 不创建其 conditional state / surface；
- 不因为 Package 理论上支持而偷偷运行。

---

# 6. Router 不负责预测全部后果

禁止一次路由提前加载所有可能下游。

正确链：

```text
Player Input
→ immediate routing
→ Formal Event / State Change
→ Typed Handoff
→ downstream activation
```

例如公开战斗：Combat immediate；Health / Reputation 等在正式 Event 后按 Handoff 条件进入。

---

# 7. Dependency Graph != Context Inclusion Graph

Hard / Optional / Conditional / Handoff / Contribution / Read-only 描述资产语义与运行约束，不是 Prompt 展开规则。

```text
Consumer requires Provider
!=
load Provider full definition into model
```

Runtime 应读取 authoritative Provider facts，形成 bounded projection；如果 Program 已能 deterministic 使用，甚至不需要进入模型。

## 7.1 Module Conditional Dependency

对模块化 Package：

```text
Package supports Healing
!= package hard-dep Health for every game/turn

Healing Module Enabled + invoked
→ Health provider requirement
```

Conditional dependency 与 Feature / Module activation 同步 fail-closed。

---

# 8. Background Program Progression

Wave 2 正式冻结：

> **Background deterministic progression != Model Activation。**

适用例：

- Nutrition / Hydration / Sleep deficit accumulation；
- timer / cooldown；
- routine automation；
- deterministic resource consumption；
- threshold bookkeeping；
- deterministic lifecycle progression。

只要 Program 可以安全、确定、可测试地推进：

```text
World Time / State Tick
→ Program updates authoritative state
→ no model call required
```

只有出现下列情况才按需构建模型 Context：

- 玩家自由语言需要解释；
- 玩家作出 override /开放式选择；
- 需要生成具有决策意义的 warning / semantic summary；
- 某个 threshold 形成正式 Handoff，需要下游 Domain 的开放式语义参与；
- 当前任务确实需要模型解释该状态。

`Runtime Relevant != Model Visible` 在后台推进中尤其重要。

---

# 9. Handoff 同时是 Context Complexity Boundary

Handoff 保护 Ownership，也防止跨域全文加载。

例如：

```text
Survival deficit threshold
→ Health-relevant Cause
→ Health
```

Survival 不需要知道完整 Health 内部规则；Health 也不需要加载全部 Survival history，只接受 bounded Cause / context。

---

# 10. Expansion Runtime Context Contract

每个可进入 Runtime 的 Expansion 至少在语义上回答：

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
13. Background Program Progression（如适用）。

当前不冻结最终 machine field 名或 token budget。

---

# 11. 不同模型职责使用不同 Context

至少区分：

### Semantic Interpretation Context
回答玩家想做什么，只提供 input、minimal scene、relevant refs、activated semantic slices。

### Narrative Context
读取 Formal Outcome、player-safe scene、必要 personality / recent events；Narrative Model 不重新裁定正式机制结果。

Routing / semantic / narrative 不假设共享一个不断增长的“大 Prompt”。

---

# 12. 长期成本模型

正式目标：

```text
Game State / Event History ↑↑↑
ordinary Turn Model Context ≈ bounded
```

```text
Enabled Assets ↑↑↑
unrelated Turn Model Context ≈ stable
```

```text
Package supports Modules ↑↑↑
Enabled Router Directory only reflects active subset
```

```text
Background progression frequency ↑↑↑
Model call frequency does not automatically ↑↑↑
```

成本主要由 **Current Relevant Complexity** 决定，而不是 Session Length、总数据库规模或 Package 理论能力数量。

---

# 13. Context Composition Stress Test

G9-02 / G11 用真实 Provider 至少测试：

- Typical / Heavy / Worst Reasonable Bundle；
- unrelated turn；
- single-domain；
- 2–3 domain integration；
- high-coupling Event；
- long session；
- Router miss / over-select / unknown ID；
- state-mandatory augmentation；
- downstream Handoff activation；
- **Feature / Module Directory pruning**；
- **disabled Module fail-closed**；
- **background deterministic progression with zero model call**；
- **threshold / player decision 才触发 model context**。

关键观测：correctness、routing stability、information boundary、context size、cost、latency、model-call count。

---

# 14. 模块化架构原则

继续采用：

> **Modularity-first, not module-count-first。**

优化：High Cohesion / Low Coupling / Canonical Ownership / Bounded Context / Stable Contracts / Independent Testability / Localized Blast Radius。

大型 Expansion 内部允许 Feature / Module 模块化，但不能让模块数量反向扩大常驻 Router / Prompt。

默认 Modular Monolith；先逻辑模块化，再在真实部署 / 隔离需求出现时物理分布式。

---

# 15. Open Attempt 不得被 Router 破坏

Router / Feature Directory 是 Context Selection，不是玩家行为白名单。

未命中预定义 Domain / Module 时：
- 不自动宣告 Attempt illegal；
- 可 generic interpretation / clarification / unresolved candidate；
- Program 仍按正式世界规则决定 Formal Effect。

Feature OFF 表示该显式玩法能力不存在，不表示玩家自然语言中与其类似的普通世界行为一定被禁止；应由其它合法 Domain / generic world rule 处理。

---

# 16. Periodic Architecture Audit + User Reflection

在 Shared Foundation、2–5 个高耦合模块成簇、阶段收口、Schema Freeze / 大迁移前，以及用户主动质疑长期方向时执行。

除既有问题外新增检查：

- 后台持续状态是否被错误实现为高频模型调用？
- Package / Feature / Module 层级是否导致 Router Directory 按理论能力而不是实际启用能力膨胀？
- Conditional Dependency 是否在 Module OFF 时仍被错误激活？

涉及产品体验、成本或不可逆权衡时才邀请用户反思；普通工程判断由 Agent 先完成。

---

# 17. 对 G9-02 的正式要求

当前 G9 Task DAG 保持：

```text
G9-01 Compatibility Audit
→ G9-02 Runtime Asset Binding + Context Orchestration Foundation
→ G9-03 asset-spec vNext Machine Contract
→ G9-04 Game Asset Adapter / Compiler
→ G9-05 Creator rebuild
```

本 v1.1 **不改变 DAG 顺序**，属于对 G9-02 成功标准的 P2 约束增强。

G9-02 在 Schema Freeze 前必须用 handwritten/internal runtime profiles 证明：

```text
Player Input
+ pruned Enabled Feature/Module Directory
+ minimal scene
→ Router Model
→ immediate candidates
→ Program structural validation
→ state-mandatory augmentation
→ Current Relevant Set
→ bounded JIT Projection
```

并额外证明：

```text
background deterministic progression
→ Program only
→ no model call
```

以及：

```text
Module OFF
→ no routing profile
→ no conditional dependency
→ no module state/surface
```

原则继续是：

> **Runtime Context Orchestrator Before Asset Context Schema。**

---

# 18. 当前阶段边界

G8-WEB-04 Host 已 PASS / CLOSED；G8 仍需完成 WEB-05 Technical Migration Closure、WEB-08 Controlled Multi-action Core Slice 与 Exit Gate。

在 G8 CLOSED 前：
- Semantic Asset / Context Contract 设计可继续；
- 可以形成 G9-02 handwritten requirements corpus；
- 不启动正式 asset-spec machine schema / Creator / Adapter implementation。

---

# 19. Wave 2 证据

### EP-SURVIVAL
证明：持续 Need / routine automation / resource consumption 可以由 Program 在后台推进；玩家没有相关语义决策时，不应调用模型。

### EP-TRAVELER-SYSTEM
证明：一个 Package 可以拥有独立 Feature 与大量 Module；Router / dependency / context 必须只按当前启用子集裁剪，不能按 Package 全能力展开。

这些证据来自真实已有 Expansion，而不是为未来 Schema 假想能力。

---

# 20. 生效与传播

本 v1.1 生效后：

- `tavern-asset` 应吸收 Feature / Module Activation 与 Background Program Progression Gate；
- Generic Library Context Pattern 应升级；
- Wave 3 Magic / Divine Retrofit 继续按新 Pattern；
- G9-02 启动时必须重新 Freshness / Decision Propagation，读取这些 current requirements；
- 本轮不要求打断当前 G8-WEB-05 工作，也不提前启动 G9。