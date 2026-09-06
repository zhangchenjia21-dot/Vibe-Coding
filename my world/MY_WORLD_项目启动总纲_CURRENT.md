---
title: my world｜项目启动总纲
status: current-canonical-product-spec
version: 2.2
created: 2026-08-25
updated: 2026-09-07
product_definition_gate: PASS
current_phase: G6
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
local_project_dir: D:\AI\Projects\my-world
---

# my world｜项目启动总纲 CURRENT

## 0. 文档职责

本文件拥有 `my world` 的产品定义：为什么做、给谁做、核心价值、核心体验、第一代产品形态、范围与成功标准。

其它 authority：

- 当前 Task / PASS / UAT：`MY_WORLD_CURRENT_STATUS.md`
- 系统架构：`MY_WORLD_架构_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 阶段 / Task Axis：`MY_WORLD_总体规划路线图_CURRENT.md`

正式起点：

> **迁移 SillyTavern / The World / DSH 已验证的产品经验，不迁移宿主债务。**

---

## 1. Primary Purpose / Job To Be Done

> **让单个玩家通过自然语言，与优秀 AI GM 在一个长期持续、可保存、可恢复、会自主演化的 2D RPG 世界中长期游玩。**

核心循环：

```text
阅读 GM Narrative
→ 用自然语言决定行动
→ 世界与人物按自身因果回应
→ 后果进入 durable Game-local Reality
→ 玩家通过真正的 RPG 信息 / mechanics / 动态 UI 理解当前处境
→ Save / Restore / reopen 后继续同一世界
→ 形成只属于本局的历史
```

---

## 2. Core Value

相比“直接打开通用模型陪我玩 RPG”，`my world` 必须提供：

- 长期持续世界，而不是一次聊天设定；
- 高自由度自然语言行动；
- 高质量、可长篇展开的 AI GM Narrative；
- 自主 NPC / World Evolution；
- durable World Truth + Knowledge boundary；
- 原生可靠 Save / Restore / Recovery；
- 多个独立 Game；
- World / Character / Expansion 组合式建局；
- Character / People / Open Threads / Inventory / Mechanics 等真实 RPG 信息体验；
- **Internal Dynamic UI：界面能根据当前 Game 真正拥有的信息与 mechanics 组织呈现，而不是每种作品都依赖固定手写页面；**
- 未来可扩展 portrait / scene / map / Mod / authoring；
- local-first、single-player-first。

核心价值：

> **长期持续 AI 世界 + 优秀自由 AI GM + 可逆持久化 + 会适配实际游戏内容的原生 RPG 体验。**

---

## 3. Product Form

### 3.1 第一代形态

> **2D 对话式 RPG / 互动小说。**

核心：

```text
AI GM Narrative
+
Player natural-language input
```

不以自由移动 3D 世界为目标。

### 3.2 三 Host 长期骨架

```text
Player Status Host | Narrative Host | World Information Host
```

```text
Player Status Host
→ portrait + 高频真实 mechanics/status HUD

Narrative Host
→ 现在发生什么？我接下来做什么？
→ GM Narrative + natural-language composer

World Information Host
→ 角色 / 人物 / 事务 / 行囊 / 系统 / 世界等玩家主动查询信息
```

Narrative 永远是视觉与交互重心。

> **Workspace is organized for truth maintenance; UI is organized for player decisions.**

### 3.3 Internal Dynamic UI 是 V0 核心能力

Owner 于 2026-09-07 明确：动态 UI 不属于“闭环后的体验优化”，而属于 V0 核心产品能力。

正确顺序：

```text
多个真实固定 consumer
→ 观察重复 UI vocabulary
→ Internal Dynamic UI Host v0.1
→ V0 Core Closure Reality Gate
→ 后续更多信息 Surface 复用该 Host
→ G8 才考虑外部 Declarative UI contract
```

V0 动态 UI 只消费内部 player-safe typed projection / contribution；不允许 Source/Expansion 作者任意脚本、任意 callback 或直接 mutation World Truth。

---

## 4. Primary Source Assets

第一代：

```text
World Pack
Character Card
Expansion Pack
```

共享 stable Source identity / version / exact generation，但不强塞万能 Schema。

World Pack 定义 T0 前世界参考、Entry/T0、world/GM material、authored assets。

Character Card 定义 reusable Character Source，可作为 Player Character 或 Guaranteed NPC；Guaranteed NPC 不自动意味着 opening appearance、同地点、互相认识或玩家知情。

Expansion Pack 定义可组合 mechanics / GM / Runtime capability。成熟顺序：真实 gameplay effect → durable state → player-facing consumer → repeated consumer → internal dynamic UI vocabulary → 最后才可能 external authoring contract。

---

## 5. 第一代建局路线

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

第一代不支持无 World 建局、一句自由文本直接生成完整世界、Draft 绕过 Source Library、Creator Draft 直接成为 Game Truth。

---

## 6. V0 Core Experience / Core Closure

V0 不以“页面数量”判定完成，而以一条完整真实游戏闭环判定：

```text
Launch / New Game / Continue
→ GM opening
→ recommendations + free-form Player action
→ OOC / GM Guidance
→ durable World / NPC consequence
→ Character / People / Open Threads 更新
→ Public d20 / System 可查看
→ 至少一个真实 Inventory mutation
→ 多个真实 Surface / mechanic contribution 经 Internal Dynamic UI Host 呈现
→ Save
→ exit / reopen
→ Continue / Restore
→ World + information + Inventory + mechanics + dynamic UI 一起回到当前历史
→ 继续正常游玩
```

Owner 必须通过连续真实试玩确认 `V0 Core Game Loop = PRODUCT PASS`。

**在此 Gate 前只做闭环 Stage Minimum；扩展功能和体验增强默认不得插队。**

---

## 7. Non-negotiable Product Principles

### Model freedom
> **Model Freedom First. Reversibility over prevention.**

### Narrative
> **Visible Narrative First. Narrative richness over artificial brevity.**

### Player / World / GM
```text
Player owns Attempt
World owns Consequence
GM owns Playability of the Consequence
```

### Living World
> **Source provides inertia; actors create history.**
>
> **Off-screen != Inactive. Persistent != Fully Simulated.**

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
>
> **Dynamic UI adapts presentation; it does not gain semantic authority.**

### Development order
> **Core closure first. Vertical before platform. Consumer before Creator.**

Dynamic UI v0.1 is authorized only after multiple real internal consumers exist; external UI protocol remains later.

---

## 8. Current G6 Product Direction

当前已 grounded / integrated：

- 概览；
- 角色；
- 重要经历；
- 人物（Owner UAT pending）；
- 存档；
- Five Recommended Actions（Owner UAT pending）。

接下来的 Core Closure 顺序冻结为：

```text
关闭 MW-018 + MW-019 UAT
→ OOC + Character-guided Recommendations
→ 事务 / Open Threads
→ System / Public d20 consumer
→ 事实型 Inventory
→ Internal Dynamic UI Host v0.1
→ V0 Core Closure Reality Gate
```

母版仍可包含：

```text
概览 / 角色 / 重要经历 / 人物 / 事务 / 行囊 / 系统 / 地图 / 存档
```

但只有满足 `real player question + real owner + player-safe projection + real product value` 才出现。禁止 fake HP / location / inventory / faction / quest state 或空页。

---

## 9. Persistence / Long-term Requirement

长期区分：Application / Game / World State / Timeline / Save Point / Conversation / Agent Context / UI Preference。

要求：durable mutation 一致、close/reopen 仍为同一世界、Restore 不泄漏未来、多 Game 独立、old Game 不被 Source update 静默改写。

Dynamic UI 不持有独立 gameplay truth；重开 / Restore 后由 current player-safe projection 重建。

---

## 10. Context Requirement

```text
System Total State
!= Runtime Relevant Set
!= Model-visible Working Set
```

Transcript 不是 World DB；长期世界不能靠无限塞 Prompt 维持。V0 Core Closure 后优先进入 Context Orchestrator / long-session hardening。

---

## 11. Post-closure Direction

V0 Core Closure 后再依次成熟：

1. Long-session Context + Structured Output Reliability；
2. Knowledge Integrity / Provenance / Epistemic / Correction；
3. Shared History / Organization / World Chronicle / Consequence Diff；
4. Narrative Preference / Bookmark / Notes / Chronicle export / Game Manifest；
5. Provider model split / Compatibility / Profiles / Usage / Debug Mode；
6. Source Library 作品化 / Reference / Creator；
7. Standalone Alpha / Release Validation。

这些能力已通过产品讨论，但不阻塞 V0 Core Closure。

---

## 12. Visual / Map Direction

portrait / scene / authored-map 仍是长期能力，但当前 implementation deferred until real authored first-party demand。

```text
map image != topology / travel / current location / GIS authority
```

不在真实需求前造自动地图生成或完整空间引擎。

---

## 13. Explicit Non-scope Before Proven Need

默认不做：Multiplayer / cloud backend、3D 自由移动、full-universe per-NPC tick、Universal ECS / giant EventBus、arbitrary external code execution、自动地图生成、Local LLM Hosting、TTS/STT、marketplace、provider routing mesh、为理论未来需求预造大量扩展点。

External Declarative UI 仍不得早于 Internal Dynamic UI Host 的真实 production evidence。

---

## 14. Product Success / Acceptance

最终成功意味着玩家真实感受到：

1. AI GM 值得长期互动；
2. 世界会自己活着；
3. 自然语言行动自由；
4. 世界变化长期可靠；
5. Save / Restore 可信；
6. World / Character / Expansion 能形成清楚建局路径；
7. 多 Game 独立长期存在；
8. UI 按玩家需要组织真实信息；
9. **不同实际 Game 内容 / mechanics 能通过受控 Dynamic UI 得到合适呈现，而不是被固定 UI 限死；**
10. 长局后仍可玩、可恢复、可理解；
11. 后续 Source / Mod 可扩展而不污染核心 Runtime。

Product-facing Engineering PASS 不能替代 Owner UAT。
