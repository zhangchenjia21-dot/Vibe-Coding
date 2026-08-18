---
title: G8 Stage UAT｜语义物化与活世界阻塞发现
status: current-finding
version: 1.0
date: 2026-08-18
project: 酒馆游戏新版主体
scope:
  - g8-stage-uat
  - creation-semantics
  - game-local-assets
  - living-world
  - product-projection
---

# G8 Stage UAT｜语义物化与活世界阻塞发现 v1.0

## 0. 裁定

项目所有者在 `sillytavern@52d0421bc58449ac8763681816bc7a84de93b385` 的真实 Stage UAT 中再次发现：

> G8-UAT-01 已关闭 Phantom / Narrative Authority / Empty T0 的第一层问题，但当前 Creation → Runtime 仍然没有**正确理解并物化 Creation 语义**，而是用 deterministic placeholder 满足数量 Gate。

因此：

```text
G8 Stage UAT = FAIL / BLOCKED
G8 = REOPENED
G9 = NOT AUTHORIZED
```

新的 blocker cluster：

> `G8-UAT-02｜Game-local Semantic Materialization + Living World Convergence`

---

## 1. UAT 证据

### 1.1 Opening Character 被压成单一占位 NPC

玩家 Creation 中描述的是多类开局人物（如导师 / 同行者 / 对手），实际 Runtime 只出现一个：

`附近的人`

且 Narrative 把多个角色语义合并到同一人：

> “神情既是导师的审视，又带几分同行者的熟稔”

这是 semantic cardinality / materialization failure。

### 1.2 世界没有主动剧情增长

进入游戏后没有具体 opening cast / hook / scene development；Narrative 只能围绕既有占位对象反复描写。

当前世界仍表现为：

```text
static seed
+ Narrative realization
```

而不是可持续增长的 AI RPG world。

### 1.3 地点占位符被当成正式地点

G8-UAT-01 为满足“至少一个 destination”而确定性创建：

`附近区域`

玩家真的移动后，Runtime canonical Scene 仍叫“附近区域”，而不是结合世界/剧情生成一个具体地点。

### 1.4 多个随身物品被压成单一 Item

Creation 中多个资源 / 随身物品被编译为一个 generic item，例如：

`随身物品`

或把整段资源文本当一个 Item description。

因此物品栏无法表达真实 inventory cardinality。

### 1.5 Information Surface 混入操作账本

当前 Product Information Surface 直接消费 `recentEvents`，导致：

- `行动已完成`
- `你前往附近区域`

被展示成“信息”。

这是 Product IA / semantic projection 错误：Formal Event journal ≠ player knowledge/information。

### 1.6 Player Profile 投影不足

Creation 已存在：

- identity
- background
- goals
- important past
- personality
- language style

但左侧人物摘要只得到有限 publicDescription，导致角色栏几乎为空。

---

## 2. 根因

### 2.1 Quantitative Gate 冒充 Semantic Gate

G8-UAT-01 的 Minimum T0 证明了：

```text
NPC count >= 1
Destination count >= 1
optional Item count >= 1
```

但没有证明：

```text
这些实体是否正确对应用户设定的语义和基数
```

因此出现：

```text
“导师、同行者、对手”
→ 1 个附近的人

“工具、日用品、钱币”
→ 1 个随身物品

需要一个可探索地点
→ 1 个附近区域
```

### 2.2 Free-text Creation Field 被错误当成 Entity Definition

Creation 字段本质可能是：

- semantic guidance；
- cast guidance；
- resource bundle；
- opening situation；
- world / story constraint；

而当前 deterministic compiler 直接尝试从字符串截出一个 display label，再创建单一实体。

这是错误抽象。

### 2.3 Final Create zero-model-call 约束被用成了语义解释替代品

“Final Create 不依赖模型”本来用于保证 deterministic / no-key creation，不意味着：

> arbitrary free text 可以在没有 semantic materialization step 的情况下被可靠编译成结构化世界。

本轮 UAT 证明：如果 AI-assisted Creation 继续接收自由语义设定，就必须存在正式 Semantic Materialization Boundary；不能靠字符串 fallback 假装理解。

### 2.4 G8/G9 阶段边界过度保守

此前将 general World Materialization 全部放到 G9，导致 G8 Stage UAT 只能拿静态 deterministic placeholder 过最低可玩 Gate。

真实 UAT 证明：

> 完整 Asset Ecosystem 可以留 G9；但**最小 Game-local Asset Materialization + Living World vertical** 已经属于 AI RPG Stage Playability prerequisite。

---

## 3. Blocker 分级

### P1｜Creation Semantic Materialization

复杂 Creation fields 不能再被压成单一 placeholder entity。

### P1｜Living World / Opening Beat

AI-created game 必须主动形成具体 opening cast / concrete location / immediate hook；世界不能只等玩家在静态 seed 里循环观察。

### P1｜Concrete Place Materialization

禁止把 `附近区域`、`新的地点` 等 system fallback 长期作为正常玩家可见 canonical Scene。

### P1｜Inventory Semantic Materialization

多物品 / 资源 bundle 必须成为正确的多个 Game-local Item/Resource，或进入明确的非 Item resource owner；不得合并成一个 generic item。

### P1｜Product Projection Semantics

Information Surface 只呈现 player information / knowledge，不呈现 generic action ledger。

### P1｜Player Profile Completeness

Player Product Profile 必须消费已定义的公开 identity / background / goals / past / personality / language style。

---

## 4. 新原则

### 4.1 Semantic Fidelity Before Minimum Count

```text
Entity count passes
!=
Creation semantics materialized correctly
```

### 4.2 Creation Field != Entity Record

每个 Creation field 必须先声明它是：

- scalar definition；
- structured list；
- semantic guidance；
- world constraint；
- materialization brief；
- runtime state seed；
- product-only preference；

再决定 compiler / model / runtime owner。

### 4.3 Placeholder 不得成为正常世界内容

`附近的人 / 附近区域 / 随身物品` 这类 fallback 只允许用于 safe degraded diagnostics，不得作为正常 configured AI-assisted Game 的 player-facing canonical content。

### 4.4 AI-assisted Creation 可以使用 bounded semantic materialization

当前正式方向调整：

- Manual / no-key creation 继续允许 deterministic path；
- AI-assisted creation 不再要求“所有 semantic materialization 都必须零模型调用”；
- 可在 Final Create 内部或其前置事务中执行 bounded typed materialization call；
- 只有 Program validated result 才进入 Game-local Canonical Assets；
- 失败必须 fail-safe，不得回退到语义错误的 placeholder 世界。

### 4.5 Living World 是 Stage Playability prerequisite

G8 exit 前不要求完整 G9 asset ecosystem，但至少需要 internal typed proof：

```text
Creation semantic brief
→ concrete game-local opening assets
→ Runtime binding
→ proactive opening beat
→ plausible unknown local place/NPC request
→ bounded materialization
→ stable identity
→ authoritative commit
→ Narrative realization
```

---

## 5. 下一步

正式新任务：

`G8-UAT-02｜Game-local Semantic Materialization + Living World Convergence`

G8-UAT-01 仍保留为已通过的 authority / rich-context / dynamic-five 基础，不回滚其成功设计。
