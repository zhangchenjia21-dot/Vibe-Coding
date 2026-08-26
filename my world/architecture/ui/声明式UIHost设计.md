---
title: my world｜声明式 UI Host 设计
type: supporting-architecture
status: active-supporting-design
version: 1.3
created: 2026-08-26
updated: 2026-08-26
canonical_map: ../../MY_WORLD_架构_CURRENT.md
scope: G2 / G5 UI projection / G6 / G8
historical_evidence: SillyTavern G8 Runtime-extensible UI Host
---

# 声明式 UI Host 设计

## 0. 定位

本文件是 `MY_WORLD_架构_CURRENT.md` 的 UI 深度设计，不是独立的顶层 Authority。

它继承 SillyTavern 第二版后期已经真实实现并关闭过的 `G8 Runtime-extensible UI Host` 经验，但只继承经过验证的 Host 语义，不迁移旧 TypeScript / React / Browser / HTTP 实现。

正式原则：

> **Definition declares what should be expressed; Host owns how it is rendered.**
>
> **Runtime owns live truth; UI remains a projection.**
>
> **Host capability first; external asset protocol second.**

---

## 1. 第一代产品骨架

```text
┌──────────────┬──────────────────────────────┬──────────────────┐
│ Player Host  │ Narrative Host               │ World Surface    │
│              │                              │ Host             │
│ 主角立绘      │ AI GM Narrative              │ 概览 / 人物 / ... │
│ 身份 / 状态   │ Player Turn                  │ 地图 / 任务 / ... │
│ 属性摘要      │ Streaming / Turn Actions     │ Save / Timeline  │
│ 装备 / 资源   │ Composer                     │ Extension Surface│
└──────────────┴──────────────────────────────┴──────────────────┘
```

### Player Host

回答：**我是谁，我现在怎么样。**

长期承载：主角立绘、姓名/身份、当前地点、高频状态、属性/资源/装备摘要，以及合法的 World Pack / Mod 玩家状态贡献。

它不是第二套 Character State，也不是复杂编辑器。

### Narrative Host

回答：**现在发生了什么，我接下来想做什么。**

长期承载：GM Narrative、玩家自然语言输入、streaming、cancel、regenerate/retry、未来经过正式 Domain 支持的 latest-turn correction，以及 Narrative contextual contribution。

中央不是普通 ChatGPT / IM 气泡流；GM Narrative 应拥有长篇、连续、适合互动小说 / TRPG 阅读的空间。

### World Surface Host

回答：**这个世界有哪些值得查看的信息。**

候选 Surface：概览、人物、关系/势力、任务/线索、物品、地图、Save、Timeline 与 World Pack / Mod Extension Surface。

具体 Tab 数量、命名和分组当前不冻结；必须由真实游戏需求与 G6 UAT 收敛。

---

## 2. Responsive / Wide-screen Layout

三栏是桌面长期骨架，不是最小窗口硬约束。

### 2.1 三栏共同参与横向伸缩

Owner G2-03 UAT 已证明：如果最大化后只让中央 Narrative 扩张，而左右栏近似保持固定宽度，三栏会在大屏上失去信息架构意义。

因此正式规则：

> **Narrative First != Narrative Only.**
>
> **三个 Host 在宽屏下都必须参与横向扩张。**

第一版可用的比例调优基线约为：

```text
Player Host      ~18%
Narrative Host   ~60%
World Host       ~22%
```

右栏可以略宽于左栏，因为人物、关系、任务、地图、Save/Timeline 等 Surface 的信息密度更高。

这不是永久锁死的设计 token；G6 仍可通过真实 RPG 数据与 UAT 调整。

### 2.2 Minimum usable width

Side Host 不能为了维持三栏而无限缩窄。

第一版建议量级：

```text
Player Host min  ~250 px
World Host min   ~310 px
```

同时所有侧栏文字 / 卡片必须拥有正常 wrap / clip / container constraint，不能越过相邻 Host 边界。

如果当前窗口已经无法同时满足：

```text
Player usable width
+
Narrative usable width
+
World usable width
```

则应该进入折叠模式，而不是继续压缩左右栏。

### 2.3 Narrow behavior

```text
wide / sufficient
→ Player | Narrative | World
→ all three proportionally expand

narrow / insufficient
→ Narrative remains primary
→ Player / World collapse, hide, drawer or overlay
```

Breakpoint 应由最低可用空间决定，不把一个固定像素数字当成架构本身。

### 2.4 默认启动与回归尺寸

当前桌面产品默认启动采用：

> **Maximized Window（非 Exclusive Fullscreen）**

理由：更适合长期桌面 AI RPG、方便真实大屏 UAT，同时保留标题栏、Alt+Tab、最小化和还原。

回归验证继续保留：

```text
Maximized desktop → primary UAT
1280x720          → normal windowed regression
960x540           → narrow responsive regression
```

### 2.5 Narrative readable width

Narrative Host 的总宽度可以增长，但正文行宽不应无限增长。

长期建议：Host 内部保留独立 readable text column；在超宽桌面上，额外空间可用于 scene art、portrait、contextual UI、氛围与留白，而不是把每行 Narrative 横向拉满。

当前 G2 已采用 bounded readable width；后续可随真实视觉设计调整，不因此建立复杂布局 framework。

---

## 3. Host Slots now; generalized rendering later

从 G2-03 起建立稳定 placement：

```text
GameShell
├─ PlayerPanelHost
│  ├─ PortraitSlot
│  ├─ PlayerStatusSlot
│  └─ PlayerCharacterDetailSlot
├─ NarrativeHost
│  ├─ NarrativeStream
│  ├─ NarrativeContextualSlot
│  ├─ TurnActionSlot
│  └─ Composer
└─ WorldSurfaceHost
   ├─ CoreSurfaceContainer
   └─ ExtensionSurfaceContainer
```

G2 可以全部由手写 Godot Control / Container 实现。关键是 ownership 与 placement 正确，不需要先制造通用 Host framework。

---

## 4. 声明式 UI 的正式模型

```text
Definition declares
- what capability / information should appear
- intended placement
- safe component kind
- surface ownership / dependency
- bounded action intent

Runtime / Projection owns
- authoritative live values
- player-visible boundary
- materialized safe data

Host owns
- Godot Control instantiation
- layout / responsive
- Theme
- typography / font scaling
- navigation / overflow
- input / accessibility
- safe intent dispatch

Player owns
- supported UI preferences
```

Definition 不拥有 pixel layout，也不拥有 Runtime truth。

---

## 5. Godot 映射

推荐映射：

```text
Host layout        → Control / Container / anchors
Card               → PanelContainer + VBoxContainer
Meter              → ProgressBar + Label
Badge               → Label + Theme style
Fact / Status List → VBoxContainer / ItemList
Action List        → Button / contextual actions
Surface            → Host-owned navigation + content container
Secondary View     → Host-owned sub-navigation
Map Overlay        → Godot 2D overlay / Control projection
Theme              → Godot Theme
Reusable renderer  → PackedScene / small component factory
```

Godot 显著降低 renderer、layout、Theme、2D overlay 和 same-process glue 成本；真正昂贵的仍是 ownership、identity、projection、composition、validation 和 external Mod compatibility。

---

## 6. Internal Declarative UI Host v0.1

G6 先用手写 internal definitions 证明 Host capability，不一次做成完整 Mod 平台。

第一版 vocabulary 只覆盖真实已出现需求：

```text
section
card
badge
meter
status_list
fact_list
action_list
```

只有真实需要后再增加 `filter / map_overlay / secondary_view / notice / ...`。

Internal Definition 可表达：placement、component kind、label/title、bounded visual data、stable contribution identity、可选 surface ownership 与 bounded Action Intent。

---

## 7. Declarative Structure != Live Data

禁止 Definition 通过任意表达式直接读取 Runtime：

```text
game.player.stats.mana
${state.xxx}
任意 NodePath
任意 GDScript expression
任意 SQL/query
```

正式数据流：

```text
Authoritative Runtime State
→ player-safe domain projection
→ bounded contribution materialization
→ UI ViewModel / safe component data
→ Declarative UI Host
→ Godot Control tree
```

即使第一代 same-process，也保留这个语义边界。

---

## 8. Surface Ownership / Contribution

未来动态 UI 必须区分：

```text
owns surface.X
```

和：

```text
contributes to surface.X
```

同一 Extension Surface identity 不能由多个 owner 靠加载顺序静默覆盖。

第一代内部实现至少保留 stable surface identity、stable contribution identity、source/owner identity，以及需要跨 owner contribution 时的 explicit dependency。

具体外部 machine ID / schema 留到 G8。

---

## 9. Action Intent

声明式 UI 可以请求受控 Intent，例如未来：

- prefill composer；
- open surface；
- open entity detail；
- request retry/regenerate；
- request Save/Timeline navigation。

Intent 是 UI → Application/Domain 的请求，不是资产直接 mutation。

禁止 arbitrary GDScript callback、arbitrary method dispatch、任意 NodePath execution、filesystem/OS command、World Pack 直接写 authoritative state。

---

## 10. Reversibility 与 UI

中央 Narrative 当前只直接暴露低风险操作：

```text
active generation → Cancel
latest generation → Regenerate / Retry
```

历史恢复由正式 Save / Load / Timeline 语义承担。

不默认：

```text
每个历史 Turn
→ 一键“回到这里”
```

完整 Timeline / Save 未来可作为 World Surface Host 的 Surface，但内部 Timeline Node 不自动成为玩家可点击 Load Point。

详细语义见 `../persistence/时间线存档与可逆性设计.md`。

---

## 11. Player UI Preference / Typography

UI Preference 默认独立于 canonical World / Timeline State。

可属于 UI Preference 的长期候选包括：

- Surface 顺序 / 展开状态；
- 可调 splitter 宽度；
- 字体大小 / UI scale；
- 其它纯呈现偏好。

### 11.1 当前字体基线

G2-03 Owner UAT 指出第一版界面整体字号偏小，但不阻塞 Conversation Spine 功能正确性。

当前裁定：

> **第一代默认采用中等、偏可读的字体大小，而不是尽量缩小字号来换空间。**

G2-04 允许做一次小型 typography baseline 调整，让标题、正文、按钮、侧栏说明、状态文本和 Composer 在桌面最大化与普通窗口下都达到舒适阅读量级。

这是 bounded polish：

- 不建立全局 Settings framework；
- 不实现字体文件管理；
- 不持久化字号选项；
- 不为了调字号重构整个 Theme 系统。

### 11.2 未来玩家可选字号

进入真实 RPG Experience / UI Preference 阶段后，玩家应可以选择受支持的字体大小或 UI text scale，例如 `small / medium / large` 或等价的有限档位。

原则：

```text
Default = medium readable baseline
Player choice = supported UI preference
Font size preference != World State
Font size preference != Timeline history
```

具体档位、缩放算法、持久化位置由 G6 真实 UI UAT 决定，不在 G2 提前冻结。

---

## 12. World Pack / Mod 外部声明时序

必须保持：

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

禁止为了尚未验证的外部资产需求，先冻结巨型 JSON Schema 再倒逼 Host 支持任意能力。

---

## 13. 跨阶段路线

### G2

- 固定三 Host 产品骨架；
- Narrative 主体验；
- 宽屏三栏共同伸缩；
- 默认 Maximized Window；
- 窄窗口合理折叠；
- 默认 medium readable typography baseline；
- 不做通用 declarative renderer / UI preference framework。

### G3–G5

- 真实 Game / Character / Relationship / NPC / Faction 等 Domain 数据进入 player-safe projection；
- Host 消费真实数据，不维护第二状态。

### G6

- 完整三栏 RPG Experience；
- Portrait / Scene / Map / RPG panels；
- internal safe component vocabulary；
- Internal Declarative UI Host vertical proof；
- bounded Action Intent；
- responsive / Theme / navigation；
- 根据真实需要评估 splitter / persisted UI preference；
- 支持玩家可选字体大小 / UI text scale，并保持其独立于 World / Timeline。

### G8

在 G6 capability 经 UAT 证明后，再外部化 World Pack / Mod UI declaration、Surface ownership、validator 与 authoring helper。

复杂脚本沙箱仍默认 Deferred。
