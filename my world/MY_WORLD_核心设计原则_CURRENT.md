---
title: my world｜核心设计原则
status: current-canonical-core-design
version: 1.2
created: 2026-08-26
updated: 2026-08-28
stage: G4 Primary Source Assets & Local Game Creation
scope: cross-stage product and runtime semantics
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜核心设计原则 CURRENT

## 0. 文档定位

这份文件记录 `my world` 跨阶段长期有效的**核心产品与 Runtime 设计原则**。

它综合：

- SillyTavern 新版主体的 World / Context / Save-Restore / Runtime Materialization / Asset Creation 经验；
- The World / DSH 的长期真实试玩与失败模式；
- `my world` G1–G3 的真实 Windows / Provider / Persistence 证据；
- 2026-08-28 对历史开发步骤、Primary Source、Game Creation 和 G4 顺序的再次审计与 Owner 裁定。

本文件继承语义，不迁移旧宿主实现。

发生冲突时：用户当前明确指令 > 当前 Product / Principles / Architecture / Roadmap > verified implementation/evidence > 历史经验。

核心总纲：

> **Model freedom first. Reversibility over prevention.**
>
> **Narrative richness over artificial brevity.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

---

# 1. 不要以“AI 永不犯错”为架构目标

开放式 AI RPG 的错误空间无法穷举。

不要默认采用：

```text
发现一种模型错误
→ Regex / whitelist
→ 新 Confirmation
→ 新 Narrative Validator
→ Prompt / State Machine 继续膨胀
```

历史已经证明，这会让 Narrative 越来越拘谨、玩家操作税增加、系统自身更容易出错。

正式原则：

> **对可逆游戏内错误，优先降低恢复成本，而不是提高模型表达成本。**

真正需要硬约束的是：Secrets、OS/filesystem authority、atomic durability、Save/Restore integrity、corruption、unsafe writer ambiguity 和不可逆外部副作用。

---

# 2. 玩家拥有时间线

AI RPG 的容错基础设施应包括：

```text
Cancel
Regenerate / Retry
latest-turn correction
edit-and-retry
explicit Save / Restore
必要的 Recovery / future Branch capability
```

正式原则：

> **Player owns the timeline.**
>
> **Reversibility != frictionless arbitrary rewind.**
>
> **Save Point != Timeline Node.**

高频低风险纠错靠近 Narrative；重大历史恢复表达明确玩家意图。玩家拥有最终历史决定权，不代表每个 Timeline Node 都必须是一键公开回档按钮。

---

# 3. Narrative 是主要游戏内容，不是状态摘要

> **Narrative richness over artificial brevity.**
>
> **Scene-led length; context-enabled richness.**

默认禁止仅为了 UI、延迟、成本或“回合整齐”而增加：

- 固定每 Turn 字数；
- 固定最低/最高字数；
- 默认“请简短回答”；
- 为界面方便截断正文；
- 无真实产品理由的 `max_tokens` 硬上限。

简单过渡可以短；重要场景可以充分展开。

> **Richness means useful density + immersion + causality, not filler.**

Narrative 不应被降级成隐藏菜单：

```text
你进入酒馆。
你发现一个人。
1. 询问
2. 离开
3. 战斗
```

除非玩法确实需要明确选项，否则玩家继续用自然语言表达任意行动。

产品 UI 应承受长 Narrative：真 streaming、稳定滚动、合理 readable width、Cancel、bounded history/context；不要逼模型适配一个小文本框。

Narrative Quality 最终必须由 Owner 真实游玩判断，Engineering 指标不能代替沉浸、人物质感、世界推进和“是否值得继续读”。

---

# 4. Context bounded 不能变成 Context starved

正式区分：

```text
System Total State
!= Runtime Relevant Set
!= Model Visible Working Set
```

以及：

```text
Source Library
!= Game Selected Source Set
!= Game-local Entity Set
!= Player-known Set
!= Runtime Relevant Set
!= Model Visible Working Set
```

目标：世界、资产与历史可以无限增长，而 ordinary Turn context 主要由**当前相关复杂度**决定。

但：

> **Bounded context != starved context.**

如果 Narrative 越来越泛化，先排查相关地点、人物、关系、冲突、近期事件与知识是否被错误裁掉，而不是用最低字数或更强格式规则掩盖。

---

# 5. Runtime 拥有 durability，不是创作审查权

旧口号 `Program owns facts; Model writes prose` 如果被理解成“Program 必须先批准所有游戏语义”，会过度限制 AI RPG。

当前正式口号：

> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

模型可以负责：

- Narrative；
- NPC 行为/动机；
- 世界事件；
- 新人物/地点/物品；
- 开放式后果；
- 复杂语义判断与世界创造。

Runtime / Program 必须负责：

- stable identity；
- atomic write / transaction；
- Save / Restore / Timeline；
- physical integrity；
- Secret / OS authority；
- durable representation；
- crash/retry/recovery correctness。

> **自由发生在游戏语义与创作层；强约束集中在不可逆基础设施边界。**

---

# 6. Source → Game-local → Runtime 永久分层

```text
Reusable Source Assets
World Pack / Character Card / Expansion Pack
↓ explicit selection + exact generation
Game Creation Composition
↓ Final Create / materialize / bind
Game-local Canonical Reality
↓ current execution
Runtime State
```

核心原则：

> **Source defines the starting reference; game-local reality owns lived history.**
>
> **Source provides inertia; actors create history.**

运行中的游戏不得把本局变化反写 Source。

Source update 不得静默改写已有 Game。

Runtime-created NPC / Place / Item 可以只有 game-local identity + `runtime_generated` provenance，不要求伪造 Source ancestry。

---

# 7. 第一代 Source 要精确，不要万能

第一代 Primary Source：

```text
World Pack
Character Card
Expansion Pack
```

共享最薄 identity：

```text
asset_id
asset_type
version
exact immutable generation / content fingerprint
```

但各类 Source 按真实需求拥有自己的语义，不建立巨大 universal Asset Schema。

历史教训：

> **玩法应该拉出 Schema；不要让玩法迁就先写好的万能 Schema。**

未发布阶段 v0.1 若设计错误，优先直接修，不建设不存在用户的兼容层森林。

---

# 8. Existing Game 必须 exact pin immutable Source generation

稳定逻辑 ID 只回答“这是什么资产”，不能证明“这一局当时用了哪一份内容”。

因此 Existing Game 必须 pin exact immutable generation。

这条规则覆盖文字和视觉资源：portrait / scene / map 也不能继续指向会被覆盖的 mutable external folder。

> **Source stable identity != exact source generation.**

Managed Source Library 可以保留多个 generation；第一代 New Game UI 不必同时展示历史版本。

---

# 9. 第一代只支持 asset-only New Game

为了降低早期工程量，第一代固定一条正式建局路径：

```text
Exactly 1 World Pack
+ Entry/T0
+ 0..N Expansion Pack
+ Exactly 1 Player Character Card
+ 0..N Guaranteed NPC Character Cards
+ minimal settings
→ Compatibility Review
→ Atomic Final Create
```

第一代不支持：

- no-World creation；
- no-Character player creation；
- AI blank-world direct creation；
- Draft direct-to-Game；
- Final Create automatic Publish；
- arbitrary creation form DSL。

这不是永久否定；其它创建途径等 Primary Source / Creator / Living World 成熟后再按真实需求增加。

---

# 10. Character Card 不是“玩家角色专用卡”

Character Card 是 reusable Character Source。

第一代玩家可明确选择：

```text
Exactly 1 Player Character
0..N Guaranteed NPC Characters
```

Guaranteed NPC 表达：

> **这个角色从 Final Create 起属于本局 canonical cast。**

必须区分：

```text
Guaranteed in Game
!= Guaranteed in Opening
!= same Scene
!= Player knows Character
!= relationship exists
!= automatic Context inclusion
```

如果以后真实需求需要“第一幕必须出现”，优先让 Entry / placement 表达，不提前恢复复杂 `bound_only / opening_character / player_character` taxonomy。

---

# 11. Expansion 先证明行为，再证明 UI，再外部化协议

历史已经证明：

```text
Source exists
Manifest exists
Binding exists
```

仍然可能没有真实 gameplay effect。

所以：

> **Expansion binding != real gameplay effect.**

正式成熟顺序：

```text
G4
Source + exact binding + real observable Runtime effect
↓
G5
mechanic/world semantics + durable state
↓
G6
real state → Internal UI consumer
↓
G8
proven internal capability → external UI/authoring contract
```

不要倒序预造 Mod UI platform。

---

# 12. Application Lifetime != Game Session Lifetime

正式应用入口需要：

```text
Launch Application
→ Main Menu
→ open selected Game Session
→ play
→ close Game Session
→ back to Main Menu
→ Application still alive
```

Main Menu 不应只是覆盖在自动打开的 `current-game.sqlite` 上。

这条 seam 应在 multi-Game 之前成立，否则 Game Library 到来时还要重新拆 Application/Game 生命周期。

---

# 13. Source Library != Game Library

Source Library 回答：

> 安装了哪些可复用 Source generation？

Game Library 回答：

> 已经开始了哪些独立 Game？

两个 owner 不能混成一个资产/存档大管理器。

G3 one-current-Game 是正确的阶段收缩；真正出现多 Game 消费者时再升级 lifecycle，而不是在 Persistence Foundation 提前平台化。

---

# 14. New Game selection 必须显式、精确、可 Review

必须冻结：

> **Chooser open / list visibility / source mode != authoritative selection.**

只有玩家点击具体资产才构成选择。

Final Create 前必须有 Compatibility Review，把实际 exact World / Entry / Player / Guaranteed NPC / Expansion / settings 投影给玩家。

第一代不提供历史 Source version picker，可以降低 sibling-version UI aliasing 风险；内部仍必须 exact pin。

---

# 15. Final Create 必须是原子、可重放的产品事务

Wizard 中间步骤编辑 Composition，不边选边创建 Game。

推荐语义：

```text
editing
→ exact review
→ Program-derived create fingerprint
→ creating
→ materialize/bind
→ created
```

必须考虑：

- double click；
- response loss；
- retry same request；
- mismatched intent；
- partial bootstrap；
- crash window。

> **成功响应丢失不能导致第二个 Game，也不能让用户永久卡在 stale request。**

---

# 16. First Playable 要分层，不一次压上所有复杂性

正式 G4 经验：

```text
First Playable A
World + Character
→ real Provider
→ Owner UAT

then

First Playable B
Add real Expansion
→ observable effect
→ Owner UAT
```

原因：World/Character 主要是 Source→materialization；Expansion 还带 Runtime capability / mechanic / UI 风险。

> **先隔离风险，再叠能力。**

---

# 17. Creation Success != Playable Game

数据库有 Game、parser PASS、manifest PASS 都不能证明游戏可玩。

旧项目真实出现过：只有 1 Region / 1 Place / 1 Scene / Player，没有 NPC / Connection / Item，Narrative 只能自己编酒馆和路人。

因此必须设置现实 Gate：

> **玩家创建后是否立刻进入具体、可继续玩的 situation？**

G4 先证明 Source/Composition 能让真实 AI GM 给出具体 Opening；G5 再负责 authoritative semantic materialization / JIT NPC/Place/Item / Living World。

不要为了避免空壳世界又把完整 World Materializer 提前塞回 Creation Platform。

---

# 18. Source 提供惯性，行动者创造历史

> **Source provides inertia; actors create history.**
>
> **Off-screen != Inactive.**

历史 Source 可以定义 T0 前事实和参考轨迹，但 T0 后的新历史应由：

```text
玩家行动
+ NPC 自主目标
+ Faction 策略
+ 制度 / 资源 / 信息 / 地理
+ 偶发事件
```

共同产生。

必须避免 Protagonist Causal Monopoly：玩家不能是唯一新历史创造源。

但持久世界也不等于每个 NPC 每 tick 调模型。G5 优先采用事件/优先级驱动 World Evolution。

---

# 19. Knowledge Boundary 是可信度目标，不是无限 Narrative 规则源

> **World Truth != NPC Knowledge != Player Knowledge.**

NPC 应使用有世界内来源的信息。

但一次普通 knowledge mistake 不自动授权新增全局 Regex / hard reject。

优先顺序：

1. Context / provenance；
2. model input；
3. regenerate/correction；
4. 高频严重问题再增加正式 mechanism。

真正 Secret / credential / private system data leakage 仍是硬安全边界。

---

# 20. UI 是 Projection，不是第二真相

> **UI is a projection of game truth, not a second truth source.**

Main Menu、Game Library、People、Relationship、Inventory、Map、Save、Mechanic Surface 都只能投影对应 owner。

UI 可以发 intent，但不能偷偷维护自己的 Game metadata、relationship、location 或 save truth。

声明式 UI 的顺序：

```text
fixed real UI
→ stable Host Slots
→ real Domain consumer
→ Internal Declarative Host
→ external contract
```

---

# 21. Save / Restore / Recovery 永久区分

```text
Persistent World State
!= Save Point
!= Recovery Checkpoint
!= Timeline Node
!= Physical Backup
```

Save 发生在稳定 committed boundary。

Restore 必须恢复世界与一致 Context；不能“DB 回去了，但模型仍记得未来”。

Physical Backup 是存储灾难恢复，不是玩家 Save。

这些 G3 已关闭语义不得被 G4 multi-Game / Source Library 重新混淆。

---

# 22. 确定性后台推进不等于模型调用

Program 可以安全确定推进的 timer / cooldown / routine bookkeeping / 明确阈值，不需要为了“AI 世界”强行调用模型。

模型用于开放语义、人物选择、世界创造和 Narrative。

> **AI-first does not mean model-call-everything.**

---

# 23. Owner UAT 应放在风险边界，不只放阶段最后

历史反复证明，Late Owner UAT 会导致整段路线返工。

推荐：

```text
Conversation First Playable
Persistence Reality UAT
World+Character First Playable A
Expansion First Playable B
Living World UAT
RPG UI UAT
Long-session UAT
Authoring UAT
Alpha UAT
```

Owner 负责产品体验判断；Agent 负责 fixtures/logs/build/crash/regression 等工程劳动。

---

# 24. Independent Review 要审“证据是否真的证明了目标”

IR 不只是 rerun tests。

必须检查：

- assertion 是否 vacuous；
- marker 是否真的进入被排除分支；
- mock 是否绕过 production path；
- proof-only binding 是否冒充 gameplay；
- exact Source selection 是否被 stable ref alias；
- crash/retry 是否真使用原请求身份；
- UI 是否投影真实 authoritative state。

`my world` G3-07 IR-01 已真实证明这类 review 能发现“代码可能没错，但证据天然会 PASS”的问题。

---

# 25. Creator 必须晚于真实 Source consumer

推荐：

```text
hand-authored Source
→ contract
→ managed library
→ select
→ Final Create
→ real play
→ prove consumption
→ then Creator
```

不要再次出现 Creator 很完整，但玩家仍不能拿资产真正开局的情况。

Creator 到 G8 再处理 Draft / Import / Publish / revision / AI-assisted typed editing / ChangeSet / Undo。

---

# 26. Correction Budget：连续返修同一 seam 时停止补丁

推荐：

```text
correction-01 = focused root fix
correction-02 = root-neighbor / sibling / crash-window audit
still failing = redesign seam
```

未发布阶段更应该修正确模型，而不是无限叠 compatibility 和 special case。

---

# 27. 阶段路线的直接要求

## G4

按顺序：

```text
Application/Game Session seam
→ World+Character Source
→ Managed Source Library
→ Multi-Game
→ Asset-only Wizard
→ Atomic Create / Materialization
→ First Playable A
→ Expansion real vertical
→ First Playable B
→ Asset Resolution
→ Two-family reality
```

## G5

重点：Minimum Playable T0、durable semantic materialization、NPC/Faction Agency、Knowledge、World Evolution、Meaningful Choice。

## G6

真实 mechanic / domain state 先成为 consumer，再抽 Internal Declarative UI Host。

## G7

Context / long-session 目标是 bounded not starved；真实长局是 Gate。

## G8

只把已经内部证明的 Source / UI / Mod capability 外部化；Creator 不能重新建立第二套 Source Store / Runtime / Final Create。

## G9

Alpha / Release 以长期真实游戏可用为目标；到阶段前再判断是否需要将 Windows Release 单独拆段。

---

# 28. 已明确反对的默认模式

1. Program owns facts; Model writes prose 作为全局创作限制。
2. No Phantom = Narrative hard rejection。
3. 每发现一种误解就增加 Regex / Confirmation。
4. 用巨大规则集换模型“永远正确”。
5. Dependency = Prompt inclusion。
6. Persistent world = every NPC every tick model call。
7. Source history = future event scheduler。
8. 恢复世界 = 重发聊天让模型猜。
9. 为 UI / latency convenience 默认把 Narrative 压短。
10. 用固定最低字数制造丰富感。
11. Platform before first playable vertical。
12. Creator before Source consumer。
13. Parser / Manifest PASS = playable PASS。
14. Binding exists = Expansion works。
15. Mutable external Source path = old Game source。
16. Stable ref = implicit latest exact generation。
17. Chooser open = auto selection。
18. Main Menu overlay = proper Application lifecycle。
19. Late Owner UAT。
20. Infinite correction chain。

---

# 29. 最终设计口号

> **模型自由创造世界。**
>
> **Narrative 丰富但不灌水，篇幅由场景决定。**
>
> **Runtime 让世界持续。**
>
> **Source 精确且不可静默改写旧 Game。**
>
> **行动者共同创造历史。**
>
> **Context 只携带当前真正需要、但足够丰富的世界。**
>
> **玩家拥有时间线。**
>
> **Vertical before platform. Consumer before creator. Reality gate before abstraction.**

任何新的架构、规则、Prompt、Validator、UI 或流程，如果明显削弱这些原则，必须证明它解决的是一个已经真实发生且价值高于新增复杂度的问题。