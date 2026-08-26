---
title: my world｜声明式 UI Host 架构
status: current-canonical-supporting-architecture
version: 1.0
created: 2026-08-26
updated: 2026-08-26
scope: G2-03 / G6 / G8 UI architecture
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
historical_evidence: SillyTavern G8 Runtime-extensible UI Host
---

# my world｜声明式 UI Host 架构 CURRENT

## 0. 文档定位

本文件冻结 `my world` 第一代 RPG UI 的长期 Host 架构方向。

它继承 SillyTavern 第二版后期 `G8 Runtime-extensible UI Host` 已经完成的设计与实现经验，但**只继承语义与经过验证的 Host 边界，不迁移旧 TypeScript / React / HTTP / Browser 实现**。

历史证据表明，旧项目已经真实完成并关闭过 `G8-WEB-04 Host`；后来 G8 因 Narrative / Playable Runtime 等产品体验问题重新打开，而不是因为声明式 UI Host 没有实现。因此该设计应视为已经付过较高工程成本、具备现实证据的前代资产。

本文件与以下 current source 协同：

- `MY_WORLD_项目启动总纲_CURRENT.md`：产品定义；
- `MY_WORLD_核心设计原则_CURRENT.md`：跨阶段产品 / Runtime 原则；
- `MY_WORLD_总体规划路线图_CURRENT.md`：何时实现各层能力；
- `MY_WORLD_Foundation架构决策_v1.0_2026-08-26.md`：Godot / GDScript / same-process 等 Foundation 技术边界。

正式原则：

> **Definition declares what should be expressed; Host owns how it is rendered.**
>
> **Runtime owns live truth; UI remains a projection.**
>
> **Prove Host capability internally before externalizing it to World Pack / Mod schema.**

---

# 1. 产品级 UI 骨架：左主角 / 中 Narrative / 右世界

第一代桌面 RPG 主界面采用以下长期骨架：

```text
┌──────────────┬──────────────────────────────┬──────────────────┐
│ Player Host  │ Narrative Host               │ World Surface    │
│              │                              │ Host             │
│ 主角立绘      │ AI GM Narrative              │ 概览 / 人物 / ... │
│ 身份 / 状态   │ Player Turn                  │ 地图 / 任务 / ... │
│ 属性摘要      │ Streaming / Turn Actions     │ Timeline / ...   │
│ 装备 / 资源   │ Composer                     │ Extension Surface│
└──────────────┴──────────────────────────────┴──────────────────┘
```

三个区域的语义：

### Left｜Player Host

回答：

> **我是谁，我现在怎么样。**

长期可承载：

- 主角立绘；
- 姓名 / 身份 / 当前地点；
- Player Status；
- Player Character Detail；
- 高频属性 / 资源 / 装备摘要；
- 由 World Pack / Mod 合法贡献的玩家状态组件。

它不是第二套 Character State，也不是复杂编辑器。

### Center｜Narrative Host

回答：

> **现在发生了什么，我接下来想做什么。**

它始终是视觉和交互重心，长期承载：

- AI GM Narrative；
- 玩家自然语言输入；
- streaming；
- cancel；
- regenerate / retry；
- edit-and-retry；
- 后续 rewind / “回到这里” / branch 快捷操作；
- Narrative Contextual contribution；
- 生成和错误状态。

`my world` 不把中心区设计成普通聊天软件的气泡流。Narrative 应保持适合长篇阅读的互动小说 / TRPG 阅读体验。

### Right｜World Surface Host

回答：

> **这个世界当前有哪些值得我查看的信息。**

长期承载 Core Surface 与 Extension Surface。候选包括：

- 概览；
- 人物；
- 关系 / 势力；
- 任务 / 线索；
- 物品；
- 地图；
- Timeline / Save；
- World Pack / Mod 增加的扩展 Surface。

具体 Tab 数量、命名和分组**当前不冻结**；应由真实游戏需求和 G6 UAT 收敛。

---

# 2. Responsive：三栏是桌面骨架，不是最小窗口硬约束

宽窗口优先三栏同时可见；窄窗口不能为了保留三栏而压缩 Narrative 到不可阅读。

正式响应原则：

```text
wide desktop
→ Player Host | Narrative Host | World Surface Host

narrow window
→ Narrative Host remains primary
→ Player / World panels collapse into drawer / toggle / overlay
```

当前不冻结具体 breakpoint 数值；G2/G6 通过真实窗口 UAT 确定。

---

# 3. Host Slot 先于通用声明系统

从 G2-03 起，主界面应按稳定 Host Slot 组织，但**不要求立刻实现通用 Declarative Renderer**。

推荐语义结构：

```text
GameShell
├─ PlayerPanelHost
│  ├─ PortraitSlot
│  ├─ PlayerStatusSlot
│  └─ PlayerCharacterDetailSlot
│
├─ NarrativeHost
│  ├─ Conversation / Narrative Stream
│  ├─ NarrativeContextualSlot
│  ├─ TurnActionSlot
│  └─ Composer
│
└─ WorldSurfaceHost
   ├─ CoreSurfaceContainer
   └─ ExtensionSurfaceContainer
```

G2 可以全部由手写 Godot UI 实现。关键是先让 ownership / placement 正确，避免未来声明化时推翻整个产品布局。

原则：

> **Host Slots now; generalized declarative rendering later.**

---

# 4. 声明式 UI 的正式定义

声明式 UI **不是让 World Pack / Mod 编写 Godot UI 代码**。

正式模型：

```text
Definition declares
- what capability / information should appear
- intended placement
- safe component kind
- surface ownership / dependency
- bounded action intent

Runtime / Projection owns
- authoritative live values
- player-visible information boundary
- materialized safe data

Host owns
- Godot Control instantiation
- layout
- responsive behavior
- Theme / visual system
- navigation / overflow
- accessibility / input behavior
- safe intent dispatch

Player owns
- supported UI preferences such as surface order
```

Definition 不拥有 pixel layout，也不拥有 Runtime truth。

---

# 5. Godot 实现映射

Godot 已经提供声明式 UI Host 最昂贵的一部分通用表现能力。

推荐映射：

```text
Host layout       → Control / Container / anchors
Card              → PanelContainer + VBoxContainer
Meter             → ProgressBar + Label
Badge             → Label + Theme style
Fact / Status List→ VBoxContainer / ItemList
Action List       → Button / contextual actions
Surface           → Host-owned navigation + content container
Secondary View    → host-owned sub-navigation
Map Overlay       → Godot 2D overlay / Control projection
Theme             → Godot Theme
Reusable renderer → PackedScene / small GDScript component factory
```

第一代不要求严格复刻旧 TypeScript discriminated union。可以使用 GDScript typed classes、enum、Dictionary validation 或 Resource-like internal definitions，但不得让 Godot `Resource` 自动成为游戏世界 truth。

---

# 6. Internal Declarative UI Host v0.1

G6 的目标不是一次做成通用 Mod 平台，而是先用**手写 internal definitions**证明 Host 能力。

第一版 safe component vocabulary 只需要覆盖真实游戏已经出现的需求，例如：

```text
section
card
badge
meter
status_list
fact_list
action_list
```

只有真实需求出现后再增加：

```text
filter
map_overlay
secondary_view
notice
...
```

禁止因为“未来可能需要”一次复制旧 Host 的全部 vocabulary。

Internal Definition 可以表达：

- placement；
- component kind；
- label / title；
- bounded visual data；
- stable contribution identity；
- optional surface / secondary view ownership；
- bounded Action Intent。

---

# 7. Declarative Structure ≠ Live Data

声明结构和当前运行值必须分离。

例如 Definition 可以声明：

> “Player Status 需要一个 Mana Meter。”

但它不能通过任意路径表达式自己读取：

```text
game.player.stats.mana
${state.xxx}
NodePath 到任意 Runtime Node
任意 GDScript expression
任意 SQL/query
```

正式数据流：

```text
Authoritative Runtime State
→ player-safe domain projection
→ bounded Host contribution materialization
→ UI ViewModel / safe component data
→ Declarative UI Host
→ Godot Control tree
```

即使第一代 same-process，也保留这个语义边界。

理由不是限制模型 Narrative，而是防止 UI / Mod 成为第二事实源或获得任意 Runtime 读取权。

---

# 8. Surface Ownership 与 Contribution

未来动态 UI 必须区分：

```text
owns surface.X
```

和：

```text
contributes to surface.X
```

Extension Surface 同一 identity 同时出现两个 Owner 时不能靠加载顺序静默覆盖。

第一代内部实现至少保留：

- stable surface identity；
- stable contribution identity；
- source / owner identity；
- explicit dependency when cross-owner contribution is needed。

具体外部 machine ID / schema 留给 G8。

---

# 9. Action Intent：UI 请求意图，不直接改世界

声明式 UI 可以有受控 Action Intent，例如未来：

- prefill composer；
- open surface；
- open entity detail；
- request retry / regenerate；
- request timeline navigation。

Action Intent 是 UI → Application/Domain 的**请求**，不是资产直接执行 mutation 的能力。

禁止：

- 任意 GDScript callback；
- arbitrary method name dispatch；
- arbitrary NodePath execution；
- arbitrary filesystem / OS command；
- World Pack 直接写 authoritative state。

这属于 Mod / 系统完整性硬边界，与 `Model freedom first` 不冲突。

---

# 10. Timeline / Turn Action 与 UI Host

中央 Narrative 的可逆性快捷操作应是 Host 的一级产品能力。

近期 G2：

```text
latest GM turn
→ regenerate / retry
→ cancel active generation
```

随着 G3 Timeline 成立：

```text
Turn footer / contextual action
→ 回到这里
→ edit-and-retry
→ 从这里继续 / branch
```

推荐原则：

- 最新一轮快捷动作可以较明显；
- 历史 Turn 不永久铺满大按钮；
- 旧 Turn 可通过轻量 `...` / timeline affordance 打开操作；
- 完整 Timeline 管理放入 World Surface Host 的 Timeline Surface。

快速操作与完整 Timeline 视图应共用同一 Domain 能力，而不是维护第二套历史。

---

# 11. Player UI Preference 独立于 Game State

像 Surface 顺序、面板展开状态等 UI Preference 可以持久化，但默认不属于 canonical World / Timeline State。

原则：

```text
Definition recommends
→ Host initializes
→ Player customizes
→ supported player preference wins
```

Restore 游戏时间线默认不应把纯 UI Preference 一并回滚，除非未来 Product Owner 对某个具体偏好另有裁定。

G6 只在真实需要时实现；不为理论未来建立完整 preference framework。

---

# 12. World Pack / Mod 外部声明必须晚于 Internal Host 证明

必须保持顺序：

```text
真实固定产品 UI
↓
稳定 Host Slots
↓
真实 Runtime / RPG 数据投影
↓
handwritten internal UI definitions
↓
Internal Declarative UI Host vertical proof
↓
Host capability vocabulary 收敛
↓
G8 external World Pack / Mod UI declaration schema
↓
Validator / Adapter / Authoring UX
```

禁止：

> 为了某个尚未验证的外部资产需求，先冻结一个巨型 JSON Schema，再倒逼 Host 支持任意能力。

正式继承旧项目的经验：

> **Host capability first; external asset protocol second.**

---

# 13. 跨阶段实施路线

## G2｜只建立产品骨架与 Host Slots

G2-03 应：

- 把中央 Narrative / Input 做成真实主体验；
- 确立左 Player Host / 中 Narrative Host / 右 World Surface Host 的结构；
- 支持窄窗口折叠思路；
- 只展示当前真实存在的功能；
- 不实现通用 declarative renderer；
- 不为未来 World Pack 创建空 Surface / 假按钮。

## G3–G5｜让真实 Domain 数据进入 Projection

随着 Game / Timeline / Character / Relationship / NPC / Faction 等真实语义出现：

- 建立 player-safe UI projections；
- 让已有 Host Slots 消费真实数据；
- 不让 UI 自己维护第二状态。

可以出现极小 internal contribution object，但不提前冻结外部资产协议。

## G6｜Internal Declarative UI Host

G6 负责：

- 完整三栏 RPG Experience；
- Portrait / Scene / Map / RPG panels；
- internal safe component vocabulary；
- internal definitions → Host → real Godot Control vertical proof；
- Core / Extension Surface 基础能力；
- bounded Action Intent；
- responsive / Theme / navigation；
- 真实 UAT。

G6 Gate 需要证明声明式能力**改善可扩展性但没有让 UI 变成工程工具**。

## G8｜Externalize to World Pack / Mod

只有 G6 Internal Host 的真实 capability 已证明后，G8 才可以冻结：

- external UI contribution schema；
- external Surface ownership / dependency naming；
- component declaration schema；
- validator；
- World Pack / Mod adapter/compiler；
- authoring preview / helper。

复杂脚本沙箱仍默认 Deferred。

---

# 14. 成本控制原则

完整声明式 UI Host 的真正成本主要在：

- ownership；
- identity；
- data projection；
- contribution composition；
- validation；
- external Mod compatibility。

Godot 已显著降低：

- renderer；
- layout；
- reusable controls；
- Theme；
- 2D overlay；
- responsive container；
- same-process integration。

因此我们采用渐进成本模型：

```text
G2 固定 UI / Host Slots
→ G3–G5 real projections
→ G6 internal declarative host
→ G8 external mod schema
```

不一次支付完整通用平台成本。

---

# 15. Non-scope / 禁止误读

本架构不授权当前阶段建设：

- arbitrary UI scripting；
- React/Web runtime；
- embedded browser；
- arbitrary state query DSL；
- arbitrary GDScript execution from assets；
- universal UI schema；
- 全部旧 G8 component vocabulary；
- G2 阶段完整 Mod UI Host；
- UI 反向成为 Game State owner。

它也不要求所有界面都声明化。稳定、核心、产品级固定界面可以继续直接使用 Godot Scene / Control 实现。

> **Declarative where variability creates product value; handwritten where stability is simpler.**

---

# 16. 当前裁定

截至 2026-08-26：

- 本架构为 current supporting architecture；
- 当前 G2-02 Provider Adapter 任务**不受影响，继续有效**；
- G2-03 Task Packet 必须读取本文件，并以三 Host Slot 骨架实现 Narrative Conversation View；
- G6 必须拥有 Internal Declarative UI Host vertical proof，而不只是堆叠手写页面；
- G8 必须在 G6 Host capability proof 之后，才设计外部 World Pack / Mod 声明式 UI contract；
- 旧 SillyTavern Host 的 TypeScript / React / Browser / HTTP 代码不得直接迁移。
