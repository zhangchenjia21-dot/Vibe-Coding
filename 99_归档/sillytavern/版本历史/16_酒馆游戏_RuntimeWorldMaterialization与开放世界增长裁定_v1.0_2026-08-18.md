---
title: 酒馆游戏｜Runtime World Materialization 与开放世界增长裁定
status: current
version: 1.0
date: 2026-08-18
project: 酒馆游戏新版主体
scope:
  - runtime-world-materialization
  - dynamic-character
  - dynamic-location
  - dynamic-item
  - open-world-growth
  - g9-upstream
---

# 酒馆游戏｜Runtime World Materialization 与开放世界增长裁定 v1.0

> [!abstract] 核心裁定
> 酒馆游戏最终不得采用“开局时预枚举全部角色、地点、物品后永不增长”的静态世界模型。
>
> **模型可以在运行过程中提出新的角色、地点、物品、连接与其它开放世界内容；但模型不拥有 authoritative world mutation。Program / Runtime 负责验证、物化、分配稳定 identity、建立引用关系、原子提交与持久化。**
>
> 本裁定不要求当前 G8-UAT-01 实现 general JIT world generation；它是 G9 Runtime Foundation 与后续 asset-spec 的正式上游约束。

---

## 1. 为什么必须支持运行期世界增长

如果 Runtime 只能使用开局时已经存在的实体：

```text
Opening Definition
→ fixed Character / Place / Item set
→ no new authoritative entities
```

则开放式剧情很快会退化为：

- 玩家只能访问预先列出的地点；
- 只能与预先列出的角色互动；
- Narrative 不能安全引入新的具体对象；
- World Pack / Character Card 被迫预枚举大量低价值细节；
- 长期剧情无法自然生成新人物、新地点、新线索与新局势。

这不符合本项目的开放式 AI RPG 目标。

因此正式目标是：

```text
Canonical World Anchors
+
Runtime State
+
Player Input / Formal Event / Story Need
↓
Model World Materialization Proposal
↓
Program Validation / Constraint Check
↓
Typed Materialization Plan
↓
Atomic Runtime Commit
↓
New Authoritative Character / Place / Item / Connection / Fact
↓
Narrative realization
```

---

## 2. Model 可以创造内容，但不能直接创造 authoritative truth

必须区分：

```text
Model-generated content proposal
!=
committed world fact
```

Model 可以提出：

- 新角色 public profile；
- 新地点 public description；
- 新地点与当前地点的候选连接；
- 新物品；
- 新角色的 bounded private seed / motive / knowledge proposal；
- 与现有世界兼容的新局部事实；
- 为当前剧情需要产生的新实体关系。

但在 Program 接受并提交前：

- 没有 stable Runtime ref；
- 不属于 Game State；
- 不得作为下一回合 authoritative interactable；
- Narrative 不得把 proposal 当成已经发生的世界变化。

---

## 3. Program / Runtime 的唯一权威职责

Program 至少负责：

### Structural Validation

- proposal 类型合法；
- ID / ref 不冲突；
- owner / namespace 合法；
- 引用对象存在或在同一 materialization plan 内创建；
- visibility / lifecycle / ownership 合法。

### World Constraint Validation

- 不与既有 canonical facts 冲突；
- 不覆盖已存在实体；
- 不违反 World Pack / Runtime Definition 的 hard constraints；
- 不绕过 disabled Feature / Module；
- 不创造当前系统没有能力承载的机制状态。

### Identity

stable semanticRef 由 Program 分配或验证；Narrative label 不等于 identity。

### Atomicity

如果一个剧情需要同时创建：

```text
新酒馆
+ 酒馆与当前街道 Connection
+ 酒馆老板 Character
```

应形成一个 bounded materialization plan，并以原子方式提交。

任何关键部分失败：

```text
partial authoritative creation = 0
```

### Persistence / Recovery

一旦 materialized：

- Save / Restore 必须保存；
- Crash / Recovery 不得重复创建；
- committed materialization 必须拥有 idempotent identity / execution boundary。

---

## 4. Narrative-visible concrete interactable 的规则

G8-UAT-01 的 `No Phantom Interactable` 原则继续成立，但含义不是“禁止新实体”。

正确解释：

> **一个具体的新人物 / 地点 / 物品在被 Narrative 当作可交互对象呈现之前，必须先成为 authoritative Runtime referent，或者与当前 Formal Turn 在同一个受控 materialization + commit 边界中成为 authoritative referent。**

因此：

```text
Model 先写：“前面有一家酒馆”
→ 下一回合再想办法补 Runtime
```

禁止。

而：

```text
Player：去附近找一家酒馆
↓
Semantic：open-world destination attempt
↓
World Materialization Model proposes tavern
↓
Program validates + creates Place/Scene/Connection
↓
optional Move resolved
↓
one atomic Formal Turn
↓
Narrative：你找到了并走进酒馆
```

可以成为未来正式能力。

---

## 5. 预定义事实与动态事实的关系

World Pack / Character Card / Expansion 不应负责穷举整个未来世界。

它们更像：

```text
Canonical Anchors
+ Hard Constraints
+ Domain Rules
+ Named Important Entities
+ Generation / Materialization Boundaries
```

Runtime 动态生成的是：

```text
Local concrete world instances
```

例如一个城市 World Pack 可以定义：

- 城市名称与区域结构；
- 社会制度；
- 已知重要势力；
- 技术 / 魔法水平；
- 地理 /文化约束；
- 重要固定 NPC；

但不需要预先写出：

- 每一个酒保；
- 每一个小巷；
- 每一家普通酒馆；
- 每一个临时旅人。

这些可以在 Runtime 需要时 materialize。

---

## 6. Dynamic Character Materialization

未来新 Character proposal 至少需要 bounded typed fields，例如语义上：

```text
public identity / display name
public description
initial location
visibility
role / immediate story relevance
bounded private seed（可选）
initial player-safe relationship state（可选）
```

Model 可以生成角色内容，包括 private motive / knowledge seed；但只有通过 Program contract 后才成为 canonical private truth。

不得：

- 让 Narrative 文本本身成为 Character database；
- 根据显示名称临时猜同一人；
- 每次再次出现时重新生成身份。

一旦 materialized，Character 使用稳定 ref 并进入正常 Continuity / Save / Context Orchestration。

---

## 7. Dynamic Place / Connection Materialization

新地点不能只有一段 Narrative 文本。

至少需要：

```text
Region / Place / Scene placement
stable ref
public label / description
visibility
connection to existing world graph
bounded traversal semantics
```

Program 验证图结构后才加入 authoritative world graph。

玩家输入：

> “找附近的酒馆。”

若世界约束允许但当前图中没有已物化酒馆，未来 Runtime 可以选择：

```text
materialize local tavern
→ connect current area
→ optionally move player
```

而不是永久返回“世界中没有酒馆”。

---

## 8. Materialization Scope 必须有界

禁止每一轮为了“更丰富”自动扩张整个世界。

触发应来自明确需要，例如：

- 玩家主动探索未知但合理的地点；
- 当前剧情需要引入新 NPC；
- Formal Event 需要生成新的局部对象；
- Expansion / World rule 明确产生新实体；
- 已有抽象实体需要从 aggregate materialize 为 instance。

原则：

> **Just-in-time world materialization, not speculative world explosion。**

只创建完成当前剧情/行动所需的最小局部世界。

---

## 9. 与 Runtime Context Orchestration 的关系

Materialized world 可以持续增长，但 ordinary model context 仍必须 bounded。

```text
Authoritative World Entities ↑↑↑
ordinary Turn Context ≈ bounded
```

新实体一旦 materialized：

- 存入 Runtime / DB；
- 不自动进入每回合 Prompt；
- 只有 current relevant entities 通过 Context Orchestration JIT projection 进入模型 working set。

因此：

> **World growth != Prompt growth。**

本裁定与第 15 号 Runtime Context Orchestration 是互补关系。

---

## 10. 与 G8-UAT-01 的边界

当前 G8-UAT-01 仍只负责：

- Minimum Playable Runtime T0；
- no phantom interactable；
- P0 Narrative authority；
- bounded rich context；
- dynamic five suggestions。

它明确不实现 general JIT World Materialization。

这样可以先修复当前 authority 漏洞，而不会把 UAT blocker fix 扩成开放世界生成平台。

G8 修复后，如果玩家请求当前不存在的具体地点，允许安全 clarification / unsupported；不得幻觉已到达。

---

## 11. 对 G9-02 的正式传播

当前 G9 DAG 顺序不变：

```text
G9-01 Compatibility Audit
↓
G9-02 Runtime Asset Binding
      + Context Orchestration Foundation
      + Runtime World Materialization Foundation
↓
G9-03 asset-spec vNext
↓
G9-04 Game Asset Adapter / Compiler
↓
G9-05 Creator rebuild
```

G9-02 在 Schema Freeze 前应使用 internal typed / handwritten definitions 证明最小纵切：

```text
Player Input requests plausible unknown local destination / NPC
↓
Semantic / Router identifies materialization need
↓
Model returns bounded typed proposal
↓
Program validates against current world constraints
↓
Program assigns stable refs
↓
atomic materialization commit
↓
new entity appears in authoritative PlayerSession projection
↓
Narrative may reference/interact with it
↓
Save / Restore / Recovery retain identity
```

至少验证 Character + Place/Scene/Connection 两类 materialization。

---

## 12. 对 G9-03 asset-spec 的约束

asset-spec 不应要求资产作者预枚举所有未来 Runtime entities。

未来 machine contract 需要表达的是已被 Runtime 证明的：

- canonical anchors；
- immutable / hard world constraints；
- materialization eligibility / boundary；
- allowed entity kinds；
- owner / namespace；
- visibility / information boundary；
- generation-relevant bounded context；
- identity / provenance；
- optional materialization policy。

最终字段名不在本裁定冻结。

原则：

> **Runtime World Materializer Before Asset Materialization Schema。**

---

## 13. Safety / Quality Boundary

动态生成世界不等于模型拥有无限写权限。

禁止：

- 覆盖既有 canonical fact；
- 删除/改写已有角色身份来适配新剧情；
- 绕过 Program permission / capability；
- 在 Narrative 中先创建、事后补数据库；
- 每回合无条件生成大量实体；
- 未经 validation 创建 hidden super-power / secret rule；
- 让新实体自动进入全部模型上下文。

正确边界：

> **Model authors candidates; Program owns reality。**

---

## 14. 最终原则

酒馆游戏的世界不是：

```text
static authored database
```

也不是：

```text
LLM free-form hallucinated story
```

而是：

```text
Authored / Created World Anchors
+
Authoritative Runtime State
+
Bounded Model-generated World Proposals
+
Program-validated JIT Materialization
+
Persistent Continuity
```

这样剧情可以持续产生新人物、新地点和新事物，同时保持 Save / Restore、Knowledge Boundary、Runtime Authority 与长期一致性。