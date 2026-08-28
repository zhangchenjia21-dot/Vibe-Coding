---
title: my world｜项目启动总纲
status: current-canonical-product-spec
version: 2.0
created: 2026-08-25
updated: 2026-08-28
product_definition_gate: PASS
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
local_project_dir: D:\AI\Projects\my-world
---

# my world｜项目启动总纲 CURRENT

## 0. 文档职责

本文件只拥有 `my world` 的**产品定义**：为什么做、给谁做、核心价值、核心体验、第一代建局方式、范围与成功标准。

它不重复维护：

- 当前 Task / PASS / UAT：`MY_WORLD_CURRENT_STATUS.md`；
- 系统架构与专题设计：`MY_WORLD_架构_CURRENT.md`；
- 跨阶段设计哲学：`MY_WORLD_核心设计原则_CURRENT.md`；
- 阶段与 Task DAG：`MY_WORLD_总体规划路线图_CURRENT.md`；
- 可复用开发路径与历史经验：`experience/AI_RPG开发路径与阶段设计经验_v1.0_2026-08-28.md`。

正式起点：

> **迁移 SillyTavern / The World / DSH 已验证的产品经验，不迁移宿主债务。**

---

## 1. Primary Purpose / Job To Be Done

> **让单个玩家通过自然语言，与优秀 AI GM 在一个长期持续、可保存、可恢复、会自主演化的 2D RPG 世界中长期游玩。**

玩家主要不是操作传统地图角色移动，也不是阅读预写分支剧情，而是：

```text
阅读 GM 叙事
↓
用自然语言决定行动
↓
世界与人物按自身因果回应
↓
状态、关系、地点、历史与后果长期保留
↓
继续形成属于本局的历史
```

---

## 2. Core Value

相比“直接打开通用模型让它陪我玩 RPG”，`my world` 必须提供：

- 长期持续的世界，而不是一次聊天的临时设定；
- 高自由度自然语言行动，而不是封闭选项树；
- 高质量 AI GM Narrative 与临场创造力；
- 自主 NPC / Faction，而不是只等待玩家触发的角色；
- 世界事实、知识、关系与后果可以长期存在；
- 原生可靠的 Save / Restore / Recovery；
- 角色立绘、场景图、地图、人物与机制 UI 等真正 RPG 产品体验；
- 可安装、可组合、可创作的 World Pack / Character Card / Expansion Pack / Mod；
- local-first、single-player-first，不依赖服务器才能开始玩。

核心价值：

> **长期持续 AI 世界 + 优秀自由 AI GM + 原生 RPG 游戏体验。**

---

## 3. Product Form

### 3.1 视觉 / 交互

第一代是：

> **2D 对话式 RPG / 互动小说。**

主体验：

```text
AI GM Narrative
+
玩家自然语言输入
```

逐步加入角色立绘、场景图、世界/区域地图、角色/关系/任务/势力/物品/机制等 RPG Surface，以及适度动画、音效和转场。

不以自由移动 3D 世界为目标。

### 3.2 运行形态

- 本地优先；
- 单人优先，预计长期保持单人；
- 第一代不建设服务器依赖、账户系统和多人同步；
- 模型先通过本地游戏调用远程 Provider API；
- Local Model 是未来扩展，不是第一代 blocker。

### 3.3 Primary Source Assets

第一代正式内容来源分为三类主资产：

```text
World Pack
Character Card
Expansion Pack
```

共同拥有稳定 Source identity / version / exact generation，但三类资产不强行塞进一个万能语义 Schema。

#### World Pack

定义 T0 前的参考世界、世界/GM instructions、Source Lore、Entry/T0、初始世界材料和 authored assets。

#### Character Card

定义一个可复用角色 Source。它既可以被明确选择为玩家角色，也可以被明确选择为“本局保证纳入”的 NPC。

玩家选择某张 NPC Character Card 的正式含义是：

> **这个 exact Character Source 从建局开始就是本局 canonical cast 的一部分。**

但它**不自动意味着**：

- 第一幕出现；
- 与玩家同地点；
- 玩家认识他；
- NPC 认识玩家；
- 自动建立关系；
- 每 Turn 进入 Context。

出现时间、位置、关系和离屏行动由本局世界因果与后续 Runtime 决定。

#### Expansion Pack

定义可组合的额外机制/GM/runtime capability Source。第一代允许 `0..N` Expansion；玩家可以明确选择“本局不使用拓展包”。

G4 第一轮试玩不要求真实 Expansion；先证明 World + Character 的资产建局主循环，再单独接入第一个真实 Expansion Runtime effect，避免同时调试三类复杂性。

---

## 4. 第一代建局产品路线：Asset-only New Game

第一代正式收缩为**只有一条创建游戏主路径**：

```text
Main Menu
→ New Game
→ 选择 Exactly 1 World Pack
→ 选择 Entry / T0（若 World 提供）
→ Expansion Pack：0..N，可明确选择 none
→ 选择 Exactly 1 Player Character Card
→ 选择 0..N Guaranteed NPC Character Cards
→ 完善少量本局设定
→ Compatibility Review
→ 明确 Final Create
→ 独立 Game-local Reality
→ 进入真实 AI GM Opening
```

“完善少量本局设定”第一代至少允许：

- Game display name；
- Protagonist Control Mode：`Full / Light / Narrative`；
- optional opening supplement / player note；
- World-specific 必要最小参数只有在真实 Pack 证明需要时才增量加入。

默认推荐 `Light Delegation`：GM 可以压缩旅途与低价值琐事，但必须在 meaningful choice、重大承诺、路线/关系/阵营等关键玩家决定前停下。

### 第一代明确不支持的建局路径

为降低早期工程量和避免再次重建复杂 Creation Platform，第一代不支持：

- 无 World Pack 创建游戏；
- 从一句自由文本自动生成完整新世界后直接建局；
- 无 Character Card 的临时玩家角色建局；
- 绕过 Source Library 直接把临时 Draft / 任意文件塞进 Final Game；
- Final Create 时临时自动发布资产；
- Creator Draft 直接成为 Game truth。

这些不是永久否定，只是第一代明确不做。若未来真实玩家需求成立，再从候选路线增量增加其它建局途径。

---

## 5. Core Experience / Core Loop

```text
启动应用
↓
Main Menu
↓
Continue Existing Game / Asset-only New Game
↓
进入世界
↓
Runtime 恢复当前世界与必要 Context
↓
AI GM Narrative / 玩家自然语言行动
↓
世界、NPC、Faction 与机制产生发展
↓
需要长期存在的变化 durable
↓
UI 投影当前游戏真相
↓
Save / 离开 / 以后继续同一世界
```

长期同时存在：

```text
World Loop
局势 → 事件 → 后果 → 时间推进

Life Loop
自由活动 → 日常 → 人物互动 → 关系 / 人格积累
```

节奏原则：

> **Compress dead time; stop at meaningful choice.**

---

## 6. Non-negotiable Product Principles

### Model freedom

> **Model freedom first. Reversibility over prevention.**

开放式 AI RPG 不以消灭所有模型错误为目标。普通可逆错误优先通过 Context、Cancel、Regenerate/Retry、明确 Save/Restore 等恢复，而不是层层增加 Narrative whitelist、Regex、Confirmation 与 Validator。

### Player / World / GM

```text
Player owns Attempt
World owns Consequence
GM owns Playability of the Consequence
```

玩家可以尝试任何行动，但世界不保证成功。

### World agency

> **Source provides inertia; actors create history.**
>
> **Off-screen != Inactive.**

玩家不是唯一历史创造者；重要 NPC / Faction 有自己的目标、风险、义务和 next move。

### Knowledge

```text
World Truth != NPC Knowledge != Player Knowledge
```

知识边界是世界可信度目标，但不能无限扩张成 Narrative 审查系统。

### UI truth

> **UI is a projection of game truth, not a second truth source.**

### Reversibility

> **Player owns the timeline.**
>
> **Reversibility != frictionless arbitrary rewind.**
>
> **Save Point != Timeline Node.**

局部生成错误应低成本纠正；重大历史恢复要表达明确意图。内部 Timeline 不自动成为每个历史 Turn 的公开回档按钮。

### Source / Game-local separation

> **Source defines the starting reference; game-local reality owns lived history.**

Source 的更新、安装新版本或外部资产变化不得静默改写已有 Game。

---

## 7. Foundation Strategy

> **Commodity Foundation, Owned Game Semantics.**
>
> **Engine-native, not engine-semantic-coupled.**

成熟 Engine/Foundation 负责窗口、渲染、2D、UI、输入、字体、图片、音频、动画、资源与平台打包等通用能力。

`my world` 自己拥有：

- Application / Game lifecycle；
- Game；
- World；
- Timeline；
- Save Point；
- Conversation / Turn；
- Agent Context；
- NPC / Knowledge / Relationship / Faction；
- Thread / Quest / Mechanic State / World Event；
- Source Library / Game Creation Composition；
- World Pack / Character Card / Expansion Pack / Mod semantics。

当前技术结论与详细导航统一见 `MY_WORLD_架构_CURRENT.md`。

---

## 8. Product UI Direction

应用级产品入口：

```text
Main Menu
├─ Continue / Game Library
├─ New Game
└─ Quit
```

局内长期桌面骨架：

```text
Player Host | Narrative Host | World Surface Host
左主角信息  | 中央叙事与输入 | 右世界信息
```

Narrative 永远是局内视觉与交互重心。

声明式 UI Host 的顺序必须是：

```text
真实固定 UI
→ stable Host Slots
→ real Domain projections
→ Internal Declarative Host
→ external World Pack / Mod UI contract
```

第一代 Expansion 在 G4 只要求真实 Runtime effect；不因为“拓展包未来会有专用 UI”就提前建设任意外部 UI 插件平台。

---

## 9. Persistence / Long-term World Product Requirement

必须原生区分：

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

关键产品要求：

- durable world mutation 不依赖周期模型 consolidation 才收敛；
- 关闭 / 重开仍是同一世界；
- 玩家可以明确 Save / Load；
- Restore 同时恢复世界和相符 Context；
- 被回滚未来不能泄漏给 AI；
- Load 旧 Save 不应轻易不可逆销毁刚才的 current future；
- 多个 Game 在 G4 后可以独立共存和切换；
- arbitrary per-turn rewind 当前不是默认第一代功能。

---

## 10. Source Library / Game Library Product Requirement

第一代 New Game 只能从 Managed Local Source Library 中选择正式 Source。

Library 必须区分：

```text
Source stable identity
Source version
Exact immutable generation / content fingerprint
Current installed generation for future New Game
Historical generations still pinned by existing Games
```

第一代 New Game UI 默认只展示当前安装版本，不提供复杂历史版本 picker；Program 在玩家明确选择时 pin exact generation。

这既降低 UI 状态复杂度，又保证旧 Game 不会因 Source 更新而变化。

Game Library 则拥有多个独立 Game 的产品生命周期；它与 Source Library 是两个不同 owner。

---

## 11. Context Product Requirement

世界和历史可以长期增长，但 ordinary Turn 的 model working set 必须保持有界：

```text
System Total State
!= Runtime Relevant Set
!= Model Visible Working Set
```

Source Library 中安装的资产、Game 中 materialized 的角色、玩家已知角色和当前模型可见内容都不是同一个集合。

Transcript 不是 World DB；不能通过不断扩大 Prompt 来维持长期世界。

---

## 12. Staged Product Proof

为了避免再次把多个复杂能力一次性压到第一次试玩，G4 必须分阶段证明：

### First Playable A — World + Character

```text
真实 World Pack
+ Exactly 1 Player Character Card
+ 0..N Guaranteed NPC Character Cards
+ Expansion = none
→ Asset-only New Game
→ real DeepSeek Opening
→ continuous play
→ Save / reopen / Continue
→ Owner UAT
```

先回答：

> **只凭正式 World + Character，我们能不能可靠创建一局真正想玩的游戏？**

### First Playable B — Add Real Expansion

A 通过后才增加：

```text
真实 Expansion Pack
→ exact selection/binding
→ 至少一个真实可观察 Runtime effect
→ persistence / reopen
→ Owner UAT
```

先证明 `Expansion selected != database name only`。完整机制专用 UI 与外部 UI contract 后置到 G6/G8。

### G4 final — Two Asset Families

最终再用历史/低魔型与高魔/幻想型两组差异明显的 Primary Source 组合证明不是首个世界的特例。

---

## 13. Explicit Non-scope Before Core Vertical Is Proven

默认不做：

- Multiplayer / server backend / cloud account / cloud save；
- 3D 自由移动；
- 无资产自由文本建局 / AI 空白世界 Creation Platform；
- Creator 在 G4 提前进入产品关键路径；
- 自动地图生成；
- 全世界逐 NPC tick 模拟；
- Universal ECS / 大型 Event Bus；
- 通用巨大 Asset Schema / Protocol；
- 第一代 Source 历史版本 chooser；
- 第一代 Expansion feature/module 复杂勾选树；
- 任意 Expansion 外部代码执行；
- G4 就建设外部任意 Declarative UI 插件平台；
- 复杂脚本沙箱 / Steam Workshop；
- Local LLM Hosting；
- TTS / STT；
- 自动 Provider routing / fallback mesh / marketplace；
- 为未来理论需求提前建设大量扩展点。

---

## 14. Simple Baseline

当前最重要比较基线：

> **The World / DSH + 同类优秀模型。**

独立版可以在工程能力上逐步建设，但核心体验不能因为“架构更正确”而明显退化：

- GM Narrative 质量；
- 自然语言自由度；
- 长期世界感；
- NPC 质感；
- 沉浸感；
- 操作税。

如果复杂系统在核心价值上明显差于简单基线，必须重开产品/架构分析，不能用测试数量或文档完整度宣布成功。

---

## 15. Product Success / Acceptance

产品最终成功不是“所有模块都实现”，而是玩家真实感受到：

1. AI GM 值得长期互动；
2. 世界会自己活着，不只围绕主角刷新；
3. 自然语言行为自由；
4. 世界变化长期可靠；
5. Save / Restore 可信；
6. World / Character / Expansion 组合能形成清楚、可重复的建局产品路径；
7. 多个 Game 能独立长期存在；
8. UI 让世界更可理解，而不是变成工程工具；
9. 长局以后仍然保持可玩、可恢复、可理解；
10. World Pack / Character Card / Expansion / Mod 可以扩展内容而不污染核心 Runtime。

Product-facing Task 的 Engineering PASS 不能替代 Owner UAT。

---

## 16. Open Questions

当前继续由后续真实阶段证据回答：

- G4 Multi-Game 的最简单物理存储形态；
- 第一代 Primary Source contract 的最小字段集合；
- Guaranteed NPC 在 G5 中何时进入 active working set / world evolution；
- 第一个真实 Expansion 应采用哪条最薄但可观察的 Runtime capability；
- G5 bounded event/priority-driven autonomous world evolution；
- G6 Internal Declarative UI Host vocabulary；
- G7 bounded context 与长局性能阈值；
- G8 外部 Mod UI / authoring protocol；
- 何种真实需求足以增加第二个正式 product-facing Provider。

这些问题不得在当前阶段无证据预建。