---
title: my world｜项目启动总纲
status: current-canonical-product-spec
version: 2.1
created: 2026-08-25
updated: 2026-09-06
product_definition_gate: PASS
current_phase: G6
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
local_project_dir: D:\AI\Projects\my-world
---

# my world｜项目启动总纲 CURRENT

## 0. 文档职责

本文件拥有 `my world` 的产品定义：为什么做、给谁做、核心价值、核心体验、第一代产品形态、范围与成功标准。

它不重复维护：

- 当前 Task / PASS / UAT：`MY_WORLD_CURRENT_STATUS.md`
- 系统架构：`MY_WORLD_架构_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 阶段 DAG：`MY_WORLD_总体规划路线图_CURRENT.md`

正式起点：

> **迁移 SillyTavern / The World / DSH 已验证的产品经验，不迁移宿主债务。**

---

## 1. Primary Purpose / Job To Be Done

> **让单个玩家通过自然语言，与优秀 AI GM 在一个长期持续、可保存、可恢复、会自主演化的 2D RPG 世界中长期游玩。**

玩家的主要行为：

```text
阅读 GM Narrative
→ 用自然语言决定行动
→ 世界与人物按自身因果回应
→ 重要状态 / 人物 / 关系 / 历史 / 后果长期存在
→ UI 帮助玩家理解当前世界
→ 继续形成只属于本局的历史
```

---

## 2. Core Value

相比“直接打开通用模型陪我玩 RPG”，`my world` 必须提供：

- 长期持续世界，而不是一次聊天设定；
- 高自由度自然语言行动；
- 高质量、可长篇展开的 AI GM Narrative；
- 自主 NPC / World Evolution；
- durable world truth + knowledge boundary；
- 原生可靠 Save / Restore / Recovery；
- 多个独立 Game；
- World / Character / Expansion 组合式建局；
- 角色、人物、世界、机制、存档等真正的 RPG 信息体验；
- 未来可扩展 portrait / scene / map / Mod / authoring；
- local-first、single-player-first。

核心价值：

> **长期持续 AI 世界 + 优秀自由 AI GM + 原生 RPG 游戏体验。**

---

## 3. Product Form

### 3.1 视觉 / 交互

第一代：

> **2D 对话式 RPG / 互动小说。**

核心：

```text
AI GM Narrative
+
Player natural-language input
```

长期加入：Character Sheet、People、Inventory、Journal、Mechanic、Save/Timeline、Map、portrait、scene art、audio/transition 等。

不以自由移动 3D 世界为目标。

### 3.2 三栏长期骨架

```text
Player Host | Narrative Host | World Surface Host
```

分别回答：

```text
Player Host
→ 我是谁？我现在怎么样？

Narrative Host
→ 现在发生了什么？我接下来想做什么？

World Surface Host
→ 这个世界有哪些值得我主动查看的信息？
```

Narrative 永远是视觉与交互重心。

来自 The World 的正式经验：

> **Workspace is organized for truth maintenance; UI is organized for player decisions.**

因此玩家 IA 不需要与数据库表、Domain owner 或 Source 文件结构一一对应。

### 3.3 运行形态

- local-first；
- single-player-first，预计长期单人；
- 第一代无服务器账户 / 多人同步依赖；
- 通过 Provider API 使用模型；
- Local Model 是未来能力，不是 blocker。

---

## 4. Primary Source Assets

第一代：

```text
World Pack
Character Card
Expansion Pack
```

共享 stable Source identity / version / exact generation，但不强塞万能 Schema。

### World Pack

定义 T0 前世界参考、Entry/T0、world/GM material、authored assets。

### Character Card

定义 reusable Character Source，可作为 Player Character 或 Guaranteed NPC。

Guaranteed NPC 只意味着从 Final Create 起属于 canonical cast，不自动意味着第一幕出现、同地点、互相认识、存在关系或每回合进入 Context。

Character 可以额外拥有 bounded player-facing presentation projection；human-player UI 不直接读取 raw GM/private Source prose。

### Expansion Pack

定义可组合机制 / GM / Runtime capability。必须先证明真实 gameplay effect，再证明 durable state，再证明 UI consumer；最后才外部化 authoring/UI contract。

---

## 5. 第一代建局路线：Asset-only New Game

```text
Main Menu
→ New Game
→ Exactly 1 World Pack
→ Entry / T0
→ 0..N Expansion
→ Exactly 1 Player Character Card
→ 0..N Guaranteed NPC Cards
→ minimal settings
→ Compatibility Review
→ Atomic Final Create
→ independent Game-local Reality
→ real AI GM Opening
```

当前 minimal settings 包括 Game display name、Protagonist Control Mode、必要 opening supplement 等。

第一代明确不支持：

- 无 World Pack 建局；
- 一句自由文本直接生成完整世界并开局；
- 无 Character Card 临时建局；
- Draft 绕过 Source Library 进入 Game；
- Final Create 自动发布 Source；
- Creator Draft 直接成为 Game truth。

---

## 6. Core Experience / Core Loop

```text
Launch
→ Main Menu
→ Continue / New Game
→ Runtime 恢复 current world + bounded Context
→ GM Narrative
→ Player natural-language action
→ world / actor / mechanic consequences
→ durable commit
→ player-safe UI projection
→ Save / leave / reopen / restore
→ continue same lived world
```

长期同时存在：

```text
World Loop
局势 → 事件 → 后果 → 时间推进

Life Loop
自由活动 → 日常 → 人物互动 → 关系 / 人格积累
```

> **Compress dead time; stop at meaningful choice.**

---

## 7. Non-negotiable Product Principles

### Model freedom

> **Model Freedom First. Reversibility over prevention.**

普通可逆错误优先 Context / Retry / Restore，不建立 Narrative hard-gate forest。

### Narrative

> **Visible Narrative First.**
>
> **Narrative richness over artificial brevity.**

### Player / World / GM

```text
Player owns Attempt
World owns Consequence
GM owns Playability of the Consequence
```

### Living World

> **Source provides inertia; actors create history.**
>
> **Off-screen != Inactive.**
>
> **Persistent != Fully Simulated.**

### Knowledge

```text
World Truth != actor Knowledge != human-player disclosure
```

### Reversibility

> **Player owns the timeline.**

### Source

> **Source defines the starting reference; game-local reality owns lived history.**

### UI

> **UI is a projection of game truth, not a second truth source.**
>
> **Canonical ownership != player information architecture.**

### Development order

> **Vertical before platform. Consumer before infrastructure.**

---

## 8. G6 Product UI Direction

当前 G6 已证明：Player Host / World Overview 可以安全消费 real ViewModel；张琛完整 player-facing profile 已通过 Owner UAT。

下一阶段不是一次做齐所有传统 RPG 页面，而是收敛玩家 IA。

当前讨论母版：

```text
概览
角色
人物
行囊
事务
系统
地图
存档
```

出现规则：

```text
real player question
+ real domain owner
+ player-safe projection
+ real product value
```

禁止为了“看起来像 RPG”制造 fake HP / location / inventory / faction / quest state 或空页。

当前倾向：

```text
Player Host
→ 长期收敛为高频 HUD

角色 Surface
→ 完整 Character Sheet

人物 Surface
→ player-known / relation-oriented People view
```

最终分类仍由 Owner 讨论冻结，不在本总纲里提前锁死。

Discussion Draft：

`architecture/ui/G6_SURFACE_INFORMATION_ARCHITECTURE_DRAFT_V0_1.md`

---

## 9. Persistence / Long-term World Requirement

原生区分：

```text
Application
Game
World State
Timeline
Save Point
Conversation
Agent Context
UI Preference
```

要求：

- durable mutation 持续一致；
- close/reopen 仍是同一世界；
- Save / Restore 可信；
- Restore 不泄漏未来 Context；
- 多 Game 独立；
- old Game 不被 Source update 静默改写；
- arbitrary per-turn public rewind 不是默认目标。

---

## 10. Context Requirement

世界和历史可以长期增长，ordinary Turn working set 必须有界：

```text
System Total State
!= Runtime Relevant Set
!= Model-visible Working Set
```

Transcript 不是 World DB；长期世界不能靠无限塞 Prompt 维持。

---

## 11. Visual / Map Product Direction

portrait / scene / authored-map 是重要长期能力，但当前 G6 Visual Runtime re-entry 已明确：

```text
implementation deferred until real authored first-party demand
```

地图长期有价值，但：

```text
map image != topology / travel / current location / GIS authority
```

不在真实产品需求前造自动地图生成或完整空间引擎。

---

## 12. Explicit Non-scope Before Proven Need

默认不做：

- Multiplayer / cloud account / server backend；
- 3D 自由移动；
- full-universe per-NPC tick simulation；
- Universal ECS / giant EventBus；
- giant universal Asset/UI Schema；
- arbitrary external code execution；
- automatic map generation without evidence；
- Creator before real Source/UI consumers；
- external Declarative UI before G6 internal patterns；
- Local LLM Hosting；
- TTS/STT；
- marketplace / provider routing mesh；
- 为理论未来需求预造大量扩展点。

---

## 13. Simple Baseline

核心比较基线仍是：

> **The World / DSH + 同类优秀模型。**

独立版不能因为工程更复杂而在这些维度明显退化：

- GM Narrative；
- natural-language freedom；
- long-lived world feel；
- NPC quality；
- immersion；
- operation tax。

如果复杂系统在核心体验上输给简单基线，必须重开分析，而不是用测试数量宣布成功。

---

## 14. Product Success / Acceptance

最终成功意味着玩家真实感受到：

1. AI GM 值得长期互动；
2. 世界会自己活着；
3. 自然语言行动自由；
4. 世界变化长期可靠；
5. Save / Restore 可信；
6. World / Character / Expansion 能形成清楚建局产品路径；
7. 多 Game 能独立长期存在；
8. UI 按玩家需要组织信息，而不是暴露工程结构；
9. 长局后仍可玩、可恢复、可理解；
10. Source / Mod 可扩展而不污染核心 Runtime。

Product-facing Engineering PASS 不能替代 Owner UAT。

---

## 15. Current Open Questions

当前真正仍开放的问题：

- G6 Player Host 与 World Surface 的长期信息分工；
- `概览 / 角色 / 人物 / 行囊 / 事务 / 系统 / 地图 / 存档` 哪些应成为一级 Surface；
- Character Sheet 与 People Surface 的第一版安全 projection 范围；
- Inventory / Thread / Faction / Map 何时拥有足够真实 Domain 进入 UI；
- Expansion mechanic-state consumer 的第一版；
- Internal Declarative UI Host 何时有足够多个真实 consumers 可以抽象；
- G7 bounded context / 长局性能；
- G8 authoring / external Mod UI contract。

这些问题必须由当前真实产品证据逐步回答，不得为了完整度提前实现。
