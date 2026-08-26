# G8 Stage UAT｜交互响应性与空壳世界阻塞发现 v1.1

状态：`CURRENT UAT FINDING / P0 + P1 BLOCKED`
日期：2026-08-18
代码基线：`sillytavern@3ad5b419e9fb289488a18ded82c5a77f2cdd376c`
supersedes: `G8_StageUAT_交互响应性阻塞发现_v1.0_2026-08-18.md`

> v1.1 吸收项目所有者继续 Stage UAT 后的第二组真实证据：AI-created game 不只是 Narrative 响应偏保守，而是当前 Creation→Runtime 生成了几乎没有可交互实体/连接的空壳世界；同时 Narrative 在 unsupported world-changing input 上出现了“文字宣称已移动，但 canonical state 未移动”的 Program Authority 冲突。
>
> 在本阻塞收口前：G8 不得 PASS/CLOSED，G9-01 不得开始。

## 1. 新增真实 UAT 证据

玩家连续输入：

```text
前往最近的酒馆
进入酒馆
进入酒馆
观察酒馆四周
```

Narrative 先后描述：

- 酒馆距离码头十几步；
- 玩家站到酒馆门前；
- 玩家推门进入酒馆；
- 酒馆里有桌子、码头汉子和柜台后的人。

但随后 canonical player-safe world projection 仍显示：

```text
玩家仍站在河港码头
四周看不见其他人
没有真实酒馆 Scene
没有真实酒馆 NPC
```

因此玩家看到的叙事和 Runtime authoritative state 已发生直接冲突。

## 2. P0｜Narrative Authority Violation

当前正式原则是：

> Program Final Outcome is authoritative；Narrative 不得新增正式位置、时间、实体、状态或结果。

但当前实现存在缺口：

1. Runtime Candidate Directory 只包含 authoritative connections / characters / items；
2. AI-created game 的 `connections=[]`，所以“进入酒馆”不存在合法 move destination；
3. `validateRuntimeSemanticAuthorization()` 对 non-world-changing candidate 的分支会在检查 `additionalWorldChangingIntent` 之前返回；
4. 因此模型若把 world-changing input 错分为 `no_action / observe / interaction / read-only-like semantic`，Program 可能没有把“仍存在未解决 world-changing intent”提升为 clarification / rejection；
5. Narrative 随后可以在 no-change / non-move Formal Outcome 上写出看似已经移动/进入新地点的自然语言；
6. 当前 narrative realization validation 主要检查 required outcome refs、Knowledge/Commitment 和时间一致性，没有一个可靠的“不得宣称未提交 location/entity affordance”边界。

这不是单纯文风问题，而是玩家可见叙事越过 Program authority。

正式分类：

> **P0 Authority Boundary Blocker**

要求：

```text
Unsupported / unresolved world-changing intent
!=
non-world-changing narrative success
```

必须 fail closed 为 clarification / unsupported / safe non-commit response，绝不允许 Narrative 伪装已经发生世界变化。

## 3. P1｜Creation → Runtime 生成 Minimum Playable T0 失败

当前 `MinimalRuntimeDefinitionAdapter` 最终仍生成：

```text
1 Region
1 Place
1 Scene
Player
characters = []
items = []
relationships = []
connections = []
knowledgeFacts = []
```

但 Creation Project 已经收集：

- `important_opening_characters`
- `initial_resources_and_carried_items`
- `current_goals_and_attachments`
- `important_past_experiences`
- `language_expression_style`
- `opening_location`
- `current_situation`
- `immediate_problem_or_opportunity`

这些信息没有形成真正 Runtime affordance。

结果：

> AI-assisted Creation 实际创建的是 Narrative shell，而不是 Minimum Playable Game Instance。

正式分类：

> **P1 Stage Objective Minimum / Product Critical Path Blocker**

## 4. No Phantom Affordance

正式冻结：

> **Narrative-visible concrete interactable affordance must have an authoritative Runtime referent.**

具体包括：

- 具体可搭话人物；
- 具体可拿取/查看/使用物品；
- 具体可进入/前往地点；
- 看起来像可以继续交互的具体设施。

Narrative 可以写非具体化 ambient background：

- 远处人声；
- 模糊人群；
- 风、雨、雾、灯光、气味；

但若把某个对象具体化到玩家自然会尝试交互，它必须已经有 Runtime ref，或先通过未来正式 World Materialization 能力建立 ref 后再展示。

G8-UAT-01 不实现通用动态 World Materialization；本轮只先消除 phantom affordance。

## 5. Bounded Context != Starved Context

当前 NarrativeSafeContext 对玩家/人物信息裁剪过度。

Creation 中已有玩家安全：

- player identity；
- public background；
- current goals / attachments；
- important past experiences；
- language / personality style。

当前 Narrative setup context 没有完整提供这些字段。

同时 current participants 主要只有：

```text
characterRef + displayName
```

缺少足够的 player-safe public description / presentation profile。

因此玩家输入“想起自己的经历与目标”时，Narrative 只能生成泛化空话。

正式原则：

> **Bounded Context != Starved Context。**

只加载当前任务真正需要的信息，但不能把回答当前任务所必需的玩家安全信息裁掉。

## 6. Narrative Freedom Envelope

需要明确区分：

### 允许 Narrative 自由发挥

Ephemeral / non-authoritative realization：

- NPC 普通寒暄；
- 即时语气、表情、小动作；
- 拒绝、犹豫、玩笑、反问；
- 不建立持久事实的 conversational attitude；
- 氛围、感官、节奏。

### Narrative 不得自行建立

Durable / authoritative consequence：

- 新 Location / movement；
- 新 Character / Item / Place；
- Knowledge truth；
- Commitment；
- Relationship state mutation；
- inventory / time / mechanical state；
- hidden fact；
- Formal Event / Resolution outcome。

同时继续保护 Player Agency：

- 不替玩家增加未表达的羞耻、后悔、喜欢、害怕等内心事实；
- 玩家显式 inner expression / backstory recall 时，可以使用玩家已经定义的公开背景和目标进行自然 realization。

## 7. G8-UAT-01 Minimum Playable T0 要求

本轮不追求完整 procedural world generation，但 AI-created game 至少不能是 1 Scene 空壳。

最低 Gate：

```text
Opening Game Instance
├─ current playable Scene
├─ >= 1 visible non-player Character
├─ >= 1 public reachable destination / Connection
└─ if Creation defined carried resources/items:
   └─ >= 1 authoritative carried/visible Item projection
```

所有对象必须：

- stable Runtime ref；
- player-safe public description；
- 可进入 Candidate Directory；
- Save/Restore 后保持 identity；
- 不依赖 Narrative 临时造 ref。

G8 只要求 Minimum Playable Seed；更大世界、JIT 地点/NPC materialization、资产驱动 world graph 留给后续正式 capability review。

## 8. Stage 状态

```text
Engineering Exit Gate = historical PASS at 3ad5b419...
Stage UAT = FAIL / BLOCKED
G8 = REOPENED FOR UAT FIX
G9 = NOT AUTHORIZED
```

当前必须新增：

> `G8-UAT-01｜Playable Runtime Seed + Narrative Authority Convergence`

修复完成后重新执行：

```text
Focused Real Provider UAT Gate
→ Engineering Regression
→ Project Owner Stage UAT
→ G8 PASS/CLOSED decision
```
