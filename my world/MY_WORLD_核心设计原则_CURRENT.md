---
title: my world｜核心设计原则
status: current-canonical-core-design
version: 2.0
created: 2026-08-26
updated: 2026-09-06
stage: G6 RPG Experience & Internal Declarative UI Host
scope: cross-stage product and runtime semantics
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜核心设计原则 CURRENT

## 0. 文档定位

本文件记录跨阶段长期有效的产品 / Runtime / UI 设计原则。

主要证据来源：

- SillyTavern 历史项目的 World / Context / Save / Runtime / UI 经验；
- The World / DSH 的真实长局与面板实验；
- `my world` G1–G5 已关闭 implementation / UAT；
- G6 MW-011 Player Host / Character Profile 的 Owner UAT。

本文件继承**语义与失败经验**，不迁移旧宿主实现。

Authority：Owner 当前明确指令 > 当前 Product / Principles / Architecture / Roadmap > verified implementation/evidence > 历史经验。

核心总纲：

> **Model Freedom First.**
>
> **Visible Narrative First.**
>
> **Narrative richness over artificial brevity.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**
>
> **Vertical before platform. Consumer before infrastructure.**

---

# 1. 不以“AI 永不犯错”为架构目标

开放式 AI RPG 的错误空间无法穷举。

普通、可逆、局内错误优先：

```text
better Context
→ Regenerate / Retry / correction
→ Save / Restore when needed
```

不要默认：

```text
一种模型错误
→ Regex / whitelist / validator / confirmation
→ prompt/state-machine forest
```

真正硬约束集中在 secrets、OS/filesystem authority、atomic durability、Save/Restore integrity、corruption、unsafe writer ambiguity、不可逆外部副作用。

---

# 2. 玩家拥有时间线

> **Player owns the timeline.**
>
> **Reversibility != frictionless arbitrary rewind.**

必须区分：

```text
Cancel / Regenerate / Retry / latest correction
Save Point
Restore
Timeline Node
Recovery Checkpoint
Physical Backup
```

Save Point != Timeline Node；Recovery != Save；Backup != 玩家历史。

Restore 必须同时恢复世界与一致 model context，不能“DB 回去但 AI 仍记得未来”。

---

# 3. Narrative 是主要游戏内容

Narrative 不是状态摘要，也不是隐藏菜单。

> **Scene-led length; context-enabled richness.**

禁止为了 UI 整齐、成本或回合一致性默认硬加：

- 固定字数；
- “请简短回答”；
- 为小文本框截断正文；
- 无真实理由的低 `max_tokens`。

简单过渡可以短，重要场景可以展开。

产品 UI 必须适应长文本，而不是反过来逼 GM 变成短消息机器人。

---

# 4. Context bounded 不能 Context starved

```text
System Total State
!= Runtime Relevant Set
!= Model-visible Working Set
```

以及：

```text
Source Library
!= Game Selected Source
!= T0 Projection
!= Game-local Entity Set
!= Player-known Set
!= Runtime Relevant Set
!= Model-visible Working Set
```

> **Bounded context != starved context.**

如果 Narrative 变泛，先检查相关人物、地点、关系、冲突、近期事件、知识是否被错误裁掉。

---

# 5. Runtime 拥有 durability，不拥有创作审查权

> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

模型可以创造 Narrative、人物行为、事件、新实体、新语义、后果。

Program 强约束：stable identity、authority、atomic write、Timeline/Save/Restore、physical integrity、crash/retry/recovery。

Runtime 不应膨胀成 Narrative 审查委员会。

---

# 6. Source → Game-local → Runtime 永久分层

```text
Reusable Source
→ exact selection / Entry-T0
→ selected T0 projection
→ Atomic Final Create
→ Game-local Canonical Reality
→ Runtime State
```

> **Source defines the starting reference; game-local reality owns lived history.**
>
> **Source provides inertia; actors create history.**

Source update 不静默改 Existing Game；Runtime 不反写 Source；runtime-generated entities 不要求伪造 Source ancestry。

---

# 7. T0 future answer 必须隔离

> **Do not show the model a post-T0 answer and then ask it to forget that answer.**

隔离的是 future canon answer，不是人物深度。

保留截至 T0 已成立的人格、经历、能力、关系惯性、知识来源、制度/地理/资源压力和开放目标。

> **No convergence force. No divergence force. Causality first.**

预训练历史知识不是未来 world truth；穿越者记忆可以是角色知识，但不是事件调度器。

---

# 8. 第一代 Source 精确，不万能

Primary Source：

```text
World Pack
Character Card
Expansion Pack
```

共享最薄 identity / exact-generation seam，各自拥有真实需求驱动的语义。

> **玩法拉出 Schema；不要让玩法迁就万能 Schema。**

---

# 9. Existing Game exact-pin immutable generation

```text
stable asset identity
!= exact generation
```

文字与视觉 declared Source 都遵守 exact-generation ancestry。

Source Library 可保留历史 generations；New Game 第一代不需要复杂历史版本 chooser。

---

# 10. Character Card 是 reusable Character Source

Character Card 不等于玩家专用卡。

```text
Guaranteed in Game
!= Guaranteed in Opening
!= same Scene
!= Player knows Character
!= relationship exists
!= automatic Context inclusion
```

Player-facing Character presentation 必须经过独立安全 projection；不得因为“这是玩家角色”就直接把 GM reference/private prose 扔进 UI。

---

# 11. Game-local semantics 可以演化

> **Source schema is not the possibility ceiling of the Living World.**

本局可以产生新的长期 meaning，但必须：

- game-local；
- durable；
- 使用已有 owner 而不造 duplicate truth；
- Save/Restore/Timeline reversible。

---

# 12. 世界独立存在，但不是全模拟器

```text
Persistent != Fully Simulated
World Independence + Player Spotlight
```

世界和 NPC 可以离屏行动；重要性决定 attention / simulation resource，不决定 durable identity 是否存在。

`hold` 是合法 World Evolution 结果。

---

# 13. NPC 是 actor，不是 response surface

> **An NPC is not a response surface. An NPC is an actor with a life that continues without the player.**

好关系不等于永远同意。

重要 NPC 可以有目标、义务、底线、代价、独立下一步，但其私密 agency material 不自动成为 human-player UI 信息。

---

# 14. Knowledge boundary 是可信度目标

```text
World Truth
!= actor Knowledge
!= human-player disclosure
!= omniscient GM Context
```

GM 全知不能变成 NPC 全知；Player UI 也不能从 omniscient truth 中临时过滤。

优先用 provenance / context / projection 解决，不为普通 knowledge mistake 无限加 Narrative hard gate。

---

# 15. Meaningful choice 要有不同风险结构

```text
Player owns Attempt
World owns Consequence
GM owns Playability of the Consequence
```

有意义的路线差异不仅是文案方向不同，还应可能在可行性、难度、态势、失败代价上不同。

> **Dice decides uncertainty. Dice does not erase character.**

---

# 16. Expansion 按 Source → effect → state → UI → protocol 成熟

```text
G4: Source + exact binding + observable effect
G5: durable mechanic/world semantics
G6: real state → real player-facing consumer
G8: proven internal capability → external authoring/UI contract
```

> **Expansion binding != gameplay effect.**
>
> **Gameplay effect != UI consumer.**
>
> **UI consumer != external protocol.**

---

# 17. Application Lifetime != Game Session Lifetime

```text
Launch Application
→ Main Menu
→ open Game Session
→ play
→ close Game Session
→ back to Main Menu
→ Application remains alive
```

Source Library != Game Library。

---

# 18. New Game selection 显式、精确、可 Review

Chooser 打开、列表可见、默认 focus 都不构成 authoritative selection。

Final Create 前必须投影 exact World / Entry / Player / NPC / Expansion / settings 供玩家 Review。

Final Create 原子、可重放、处理 double click / retry / response loss / crash window。

---

# 19. Creation success != playable game

DB row、manifest PASS、parser PASS 都不能证明“这局值得玩”。

现实 Gate：创建后是否立刻进入具体、可继续的 situation；Source + Runtime 是否真的形成有质感的世界。

Engineering PASS 不能替代 Owner UAT。

---

# 20. UI 是 Projection，不是第二真相

> **UI is a projection of game truth, not a second truth source.**

Main Menu、Game Library、Player Host、People、Relationship、Inventory、Map、Save、Mechanic Surface 都只能投影对应 owner。

UI 可以发 bounded intent，但不能偷偷维护自己的 gameplay truth。

---

# 21. 玩家 IA 不等于后台 truth ownership

来自 The World 真实 UI 试验的正式长期经验：

> **Workspace is organized for truth maintenance; UI is organized for player decisions.**

在 `my world` 中进一步表达为：

```text
canonical/domain ownership
!= player information architecture
!= debug/authoring information architecture
```

一个玩家页面可以安全聚合多个 owner；一个 owner 也可以投影到多个页面。前提是 projection boundary 明确、没有第二事实源。

---

# 22. 三 Host 按玩家问题分工

```text
Player Host
→ 我是谁？我现在怎么样？

Narrative Host
→ 现在发生什么？我接下来想做什么？

World Surface Host
→ 我主动想查看哪些角色/世界/系统信息？
```

Narrative 永远是视觉/交互中心。

Player Host 长期倾向高频紧凑 HUD，不应永久承担完整 Character encyclopedia。

完整角色详情、人物图鉴、事务、行囊、系统、地图、存档等属于 secondary RPG information surfaces 的候选。

---

# 23. 不为了“像 RPG”制造假页面或假状态

一个 Surface 进入产品至少要有：

```text
real player question
+ real domain owner
+ player-safe projection
+ non-trivial product value
```

禁止：

- fake HP/location；
- authored starting possessions 冒充 dynamic Inventory；
- keyword 从 Narrative 猜 Quest；
- actor-private truth 冒充 People page；
- 空 Tab；
- 为 UI 先造无消费价值的 Domain。

> **No fake RPG state for UI completeness.**

---

# 24. HUD != Character Sheet

G6 MW-011 UAT 证明：Character Card 信息太薄会让 Player Host 没价值；但完整 Character profile 全常驻左栏又会产生信息架构混杂。

当前接受 rich left panel 作为 transitional state；长期方向：

```text
Player Host = high-frequency HUD
Character Surface = full Character Sheet
```

迁移必须在右侧 Surface 结构经 Owner 讨论后进行，不为搬字段重新打开已关闭 MW-011。

---

# 25. Declarative UI 必须晚于多个真实 consumer

正式顺序：

```text
fixed real UI
→ stable Host slots
→ real Domain projections
→ multiple real Surfaces
→ repeated component patterns
→ Internal Declarative UI Host
→ bounded Action Intent
→ G8 external contract
```

因此当前 `MW-013` 在 G6 Surface 尚未充分展开时保持 HOLD。

禁止 external World/Character/Expansion UI schema 反向逼迫 Host 支持 arbitrary capability。

---

# 26. Visual runtime 由真实 visual consumer 拉出

当前 portrait / scene / authored-map resolver deferred，不是永久取消。

保护：

```text
authored visual presentation
!= gameplay/world/location/knowledge authority

map image
!= topology/current location/travel/pathfinding/GIS
```

没有成熟第一方 visual demand 时，不为路线图形式完整造 media infrastructure。

---

# 27. Owner UAT 放在风险边界

推荐持续使用：

```text
Conversation UAT
Persistence UAT
World+Character UAT
Expansion UAT
Living World UAT
RPG Surface / IA UAT
Long-session UAT
Authoring UAT
Alpha UAT
```

Agent 负责 build / tests / fixtures / logs；Owner 负责“这是否像一个值得玩的产品”。

---

# 28. Independent Review 审证据，不只 rerun tests

必须检查 assertion 是否 vacuous、mock 是否绕过 production path、dirty worktree 是否冒充 candidate、exact Source 是否被 alias、UI 是否真的消费 safe authoritative projection。

Implementer self-report 不是 PASS。

---

# 29. Creator / external protocol 必须晚于真实 consumer

推荐：

```text
hand-authored Source
→ contract
→ managed library
→ select / create
→ real play
→ real UI consumers
→ then Creator / external contract
```

不要再次出现 Creator 很完整，但玩家核心路径仍不成立。

---

# 30. Correction Budget

同一 seam 连续返修时：

```text
first correction = focused root fix
second correction = root-neighbor audit
still failing = redesign seam
```

未发布阶段优先修正确模型，不叠 compatibility special-case forest。

---

# 31. 当前阶段直接要求

## G6

```text
Player-safe projection / ViewModel / first consumer   DONE
Visual Runtime re-entry                              AUDITED / DEFERRED
Surface / Information Architecture                   CURRENT
real Character / People / other grounded Surfaces    NEXT AFTER OWNER FREEZE
Expansion mechanic-state consumer                    LATER
Internal Declarative UI Host                         AFTER MULTIPLE REAL CONSUMERS
Action Intent / responsive / Theme / navigation      AFTER HOST NEED IS PROVEN
```

## G7

长局目标：bounded, not starved；真实长局性能与恢复是 Gate。

## G8

Authoring / Mod / external declarative UI 只从 G4–G7 已证明能力外部化，不预造万能平台。
