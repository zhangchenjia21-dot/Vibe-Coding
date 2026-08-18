---
title: 酒馆游戏｜Runtime Context Orchestration 与模块化复杂度控制裁定
status: current
version: 1.0
date: 2026-08-18
project: 酒馆游戏新版主体
scope:
  - runtime-context
  - asset-activation
  - model-routing
  - modularity
  - g9-upstream
---

# 酒馆游戏｜Runtime Context Orchestration 与模块化复杂度控制裁定 v1.0

> [!abstract] 核心裁定
> 酒馆游戏继续坚持 World Pack / Character Card / Expansion 的模块化资产架构，但**启用的资产数量不得等价为单次模型上下文大小**。
>
> 长期世界复杂度应主要沉淀在 Runtime / Database / authoritative state 中；模型每次推理只读取完成当前职责所需的**最小、高信号、可有界 Working Set**。
>
> 本裁定是 G9 `tavern-asset-spec vNext`、Runtime Asset Binding、Context Router 与真实模型压力测试的正式上游；当前只冻结产品与架构语义，不冻结最终 JSON / Schema / token budget / Router API。

---

# 1. 审计结论

本轮针对“Expansion 越多，模型是否最终无法稳定运行”的风险完成架构审计。

结论：

- World Pack / Character Card / Expansion 分离：**PASS**；
- Canonical Owner / Typed Dependency / Handoff：**PASS，并继续强化**；
- Core-first / Shared Foundation：**PASS**；
- 一局允许启用多个 Expansion：**PASS WITH CONTEXT ORCHESTRATION**；
- `Enabled Expansion = 每 Turn 全量 Prompt`：**禁止**；
- 让模型同时承担全部机制状态、判定和长期记忆：**禁止**；
- Runtime Context Orchestration：**必须成为 G9 正式能力**。

模块化本身不是复杂度问题；真正风险是**复杂度泄漏到模型上下文**。

---

# 2. 四层集合必须分离

正式冻结：

```text
Asset Library
!=
Game Enabled Asset Set
!=
Current Runtime Relevant Set
!=
Current Model Visible Working Set
```

含义：

- Asset Library：用户可用的全部资产；
- Enabled：当前 Game Instance 选择并编译进入玩法组合的资产；
- Runtime Relevant：当前输入 / Event / State 实际涉及的 Domain；
- Model Visible：当前这一轮模型职责真正需要看到的最小 projection。

**Enabled 不自动进入 Prompt。Runtime Relevant 也不自动进入 Prompt。**

如果某规则可以由 Program deterministic 执行，它可以属于 Runtime Relevant，但无需进入 Model Visible Set。

---

# 3. Full Asset Definition != Model Prompt Payload

完整 World Pack / Character Card / Expansion Markdown 是：

> Semantic Authoring / Governance Source。

它面向：

- Creator；
- 资产作者；
- Reviewer；
- G9 compiler / binder；
- Runtime binding 设计。

不得把“完整语义稿存在”解释为：

> 每次 Turn 把完整资产正文注入模型。

目标链：

```text
Full Semantic Asset
↓
G9 Compile / Bind
↓
Runtime Definition
↓
Current Relevant Projection
↓
Model Context Slice
```

因此资产正文可以保持语义完整；需要严格控制的是 Runtime Projection，而不是 Canonical Definition 的表达完整度。

---

# 4. Model-first Semantic Routing

## 4.1 Router 输入

第一轮语义路由优先由模型承担。

Router Model Call 输入：

```text
Player Input
+
当前 Game Instance 的 Enabled Capability / Expansion Directory
+
极小 Current Scene / Active Context Summary
```

Directory 不是 Expansion 全文，只提供极小 Routing Profile，例如：

```text
ID
Name
One-line Scope
少量典型语义提示
```

只发送当前 Game Instance 已启用资产，不发送整个 Asset Library。

## 4.2 Router 输出

Router 只输出：

- immediate Domain candidates；
- Intent candidates；
- 必要的 routing uncertainty / clarification signal。

它回答：

> “理解和处理这次玩家输入，现在涉及哪些语义能力？”

## 4.3 Program 不重复语义路由

Router 后 Program 只执行：

### Structural Validation

- ID 是否存在；
- ID 是否属于当前 Enabled Set；
- 输出结构是否合法；
- 是否出现模型虚构的 capability ID。

### State-mandatory Augmentation

Program 根据已经存在的 authoritative runtime state 补充必然参与的 Domain。

例如：

```text
当前已有 Active Combat Context
+
玩家表达“我转身逃走”

Router: Movement
Runtime mandatory: Combat

Final Relevant Set = Movement + Combat
```

这不是 Program 再做一遍自然语言理解，而是正式 State Owner 对既有状态约束的确定性补充。

---

# 5. Router 不负责预测全部后果

禁止要求第一轮 Router 预测：

> 当前输入最终可能影响的所有下游 Domain。

正确链：

```text
Player Input
↓
Immediate semantic routing
↓
Formal Event / State Change
↓
Typed Handoff / downstream consumer activation
↓
Further consequences
```

例如：

```text
玩家当众攻击县令
↓
Router: Character / Combat
↓
Formal public assault Event
↓
Health / Law / Reputation / Relationship / Politics
按各自正式 Handoff 与 State 条件后续激活
```

Router 不需要在第一轮同时理解所有下游实现。

---

# 6. Dependency Graph != Context Inclusion Graph

资产语义依赖不能机械转化成 Prompt 依赖。

例如：

```text
War hard-dep / consumes Organization
```

不意味着：

> War Turn 必须把完整 ORG Core 一起发给模型。

正确：

```text
War
↓ read authoritative ORG facts
Runtime builds bounded projection
↓
model sees only the facts needed by current reasoning
```

Hard / Optional / Handoff / Contribution / Read-only 关系是资产语义与运行约束；是否进入当前 Model Context 必须由 Context Orchestration 独立决定。

---

# 7. Handoff 同时是复杂度边界

Handoff 除了保护 Canonical Ownership，还承担：

> **Context Complexity Boundary**。

例如：

```text
Combat
→ Physical Impact Event
→ Health
```

Combat 不需要读取 Health 的完整内部规则；它只产生自己有权产生的 Candidate / Event，Health 再由自身 Owner 处理。

因此禁止用“跨域方便”作为把多个 Core 全文一起加载到同一次模型推理中的理由。

---

# 8. 每个 Expansion 的 Context Contract

G9 前先冻结语义要求；G9 再设计机器字段。

每个可进入 Runtime 的 Expansion 至少回答：

1. **Routing Profile**：ID、名称、一句话 Scope、典型语义提示；
2. **Activation**：什么 input / Event / State 使本 Domain 与当前工作相关；
3. **No-load Conditions**：什么情况下明确不应进入当前 Working Set；
4. **Minimal Read Set**：最少读取哪些 authoritative facts；
5. **Model-needed Semantics**：哪些语义确实需要模型理解；
6. **Program-owned Logic**：哪些确定性规则、状态更新和校验不应消耗模型推理；
7. **Output Candidate**：模型最多可以提出什么 Candidate；
8. **Handoff**：结果向哪个 Owner 传递；
9. **Information Boundary**：是否存在 hidden truth / player-safe projection 风险；
10. **Context Cost**：是否可能导致上下文膨胀，以及怎样保持 bounded。

当前不冻结统一 token 数值。

---

# 9. 不同模型职责使用不同 Context

一个 Turn 不假设只有一个“大 Prompt”。

至少区分：

## Semantic Interpretation Context

回答：

> 玩家想做什么？

只包含当前语义理解必要的：

- Player Input；
- 当前相关 Ref；
- minimal scene facts；
- activated semantic slices；
-必要 Character / World context。

## Narrative Context

回答：

> Program 已正式决定发生了什么，怎样以玩家安全方式叙述？

它读取：

- Formal Outcome；
- player-safe scene state；
- relevant personality / tone；
-必要 recent events。

Narrative Model 不重新裁定 Combat / Health / Politics / Economy 等正式结果。

---

# 10. 长期成本模型

正式目标：

```text
Game State Size ↑↑↑
不应导致
Ordinary Turn Model Context Size 同比例 ↑↑↑
```

```text
Enabled Asset Count ↑↑↑
不应导致
Unrelated Turn Model Context Size 同比例 ↑↑↑
```

模型成本应主要由：

> **Current Relevant Complexity**

决定，而不是由：

- 已经玩了多少 Turn；
- 数据库里有多少历史 Event；
- 资产库有多少资产；
- 当前 Game Instance 启用了多少与本 Turn 无关的 Expansion。

长期世界记忆属于 Runtime / Database，不属于不断膨胀的聊天历史。

---

# 11. Context Composition Stress Test

G9 / G11 必须使用真实目标 Provider 做组合压力测试。

至少建立：

```text
Typical Valid Bundle
Heavy Valid Bundle
Worst Reasonable Valid Bundle
```

并分别测试：

- 普通无关 Turn；
- 单 Domain Turn；
- 2–3 Domain 联动 Turn；
- 高耦合 Formal Event；
- 长期 Session；
- Router 漏选 / 多选 / unknown ID；
- state-mandatory augmentation；
- downstream Event/Handoff activation。

关键指标包括：

- correctness；
- routing stability；
- information boundary；
- context size；
- cost；
- latency；
- enabled asset growth 对 unrelated turn context 的影响。

最重要的趋势 Gate：

> 增加与当前 Turn 无关的 Enabled Asset，不应显著扩大 Model Working Set。

---

# 12. 模块化架构原则

项目继续采用：

> **Modularity-first, not module-count-first.**

优化目标：

- High Cohesion；
- Low Coupling；
- Canonical Ownership；
- Bounded Context；
- Stable Contract；
- Independent Testability；
- Localized Blast Radius。

不是：

> 文件越多 / 服务越多 / 抽象越多越好。

默认优先：

> **Modular Monolith**

即在单仓库 / 单产品 / 单部署体内建立强模块边界、Owner-aligned 目录、typed contract 与独立测试。

只有出现真实的独立部署、独立扩容、安全隔离、故障隔离、团队边界或外部消费者需求时，才考虑进一步物理服务化。

---

# 13. Open Attempt 不得被 Router 破坏

Capability Directory / Router 是：

> Context Selection 与语义任务分配机制。

它不是玩家动作白名单。

若 Router 没有命中一个预定义 Domain：

- 不得自动宣告玩家行为非法；
- 可以进入 generic interpretation；
- 必要时澄清；
- 也可以产生 unresolved candidate；
- Program 仍按正式 world rules 判断 Formal Effect。

---

# 14. 阶段性架构审计 + 用户反思 Gate

本轮用户反思提前暴露了“模块越多是否导致模型上下文爆炸”这一潜在长期问题。

因此项目正式增加：

> **Periodic Architecture Audit & User Reflection Gate**

目的不是增加形式流程，而是在设计仍便宜可改时，主动寻找那些：

> 当前看起来可行，但随着规模、时间、消费者、状态或内容增长会发生非线性爆炸的问题。

至少在以下节点执行：

1. 一个新 Shared Foundation / Core 簇形成后；
2. 2–5 个高耦合模块完成 Cluster Convergence 时；
3. 一个正式阶段准备 CLOSED 前；
4. 大规模迁移 / Schema Freeze / Platform Contract Freeze 前；
5. 用户主动提出“这个方向真的对吗 / 长期会不会出问题”时立即执行。

审计问题至少包括：

- 当前设计规模扩大 5–10 倍后是否仍成立？
- 是否存在线性、平方或隐式组合爆炸？
- 是否把状态 / 上下文 /依赖复制成第二事实源？
- 模块数量增加是否导致 Agent / Model 必须理解更多无关内容？
- 当前 abstraction 是否被真实消费者证明，而不是为了未来想象提前平台化？
- 是否有暂时省事、未来迁移代价极高的 provisional owner / dual path？
- 是否出现过度模块化：每次修改必须跨许多模块且没有独立生命周期？
- 是否有用户体验或成本问题尚未被工程 Gate 覆盖？

用户反思不是把普通技术判断甩给用户。

只有当问题涉及：

- 产品体验；
-长期维护偏好；
-成本权衡；
-不可逆架构；
-是否接受某种复杂度；

才主动邀请用户从真实使用者 / 项目所有者视角反思。

---

# 15. 对 G9 的正式要求

G9 设计 `tavern-asset-spec vNext` 时必须纳入：

- typed dependency graph；
- Routing Profile；
- Runtime Activation / Context Contract；
- Model-first Router contract；
- Program structural validation；
- state-mandatory augmentation；
- bounded Runtime Projection；
- dependency graph 与 context inclusion graph 分离；
- context composition validator / test harness；
- Context Cost observability；
- Creator 对 Context Contract 的可作者化方式。

不得在 G8 / 当前 Semantic Asset 阶段提前冻结最终机器字段。

---

# 16. 生效与下一步

本裁定生效后：

1. Shared Foundation 继续推进；
2. 已有 ORG Core 补 Context Contract；
3. Reputation / Kinship / Politics / Economy / War 从正式创作开始即必须通过 Runtime Relevance / Context Gate；
4. `tavern-asset` Skill 增加 Context Gate；
5. `lifecycle-dev-process` 增加 Modularity-first 偏好与 Periodic Architecture Audit / User Reflection Gate；
6. 完成上述回写后继续 `EP-REPUTATION-CORE`。
