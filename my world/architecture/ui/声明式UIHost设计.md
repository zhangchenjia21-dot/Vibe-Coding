---
title: my world｜声明式 UI Host 设计
type: supporting-architecture
status: active-supporting-design
version: 1.1
created: 2026-08-26
updated: 2026-08-26
canonical_map: ../../MY_WORLD_架构_CURRENT.md
scope: G2-03 / G5 UI projection / G6 / G8
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

## 2. Responsive

三栏是桌面长期骨架，不是最小窗口硬约束。

```text
wide desktop
→ Player Host | Narrative Host | World Surface Host

narrow window
→ Narrative remains primary
→ Player / World collapse, hide, drawer or overlay
```

G2/G6 用真实窗口 UAT 决定 breakpoint，不提前冻结跨项目像素常量。

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
Badge              → Label + Theme style
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

与：

```text
contributes to surface.X
```

同一 Extension Surface identity 出现两个 Owner 时不得靠加载顺序静默覆盖。

内部能力至少保留：stable surface identity、stable contribution identity、source/owner identity，以及跨 owner contribution 的显式依赖。

外部 machine ID / schema 留到 G8。

---

## 9. Action Intent

声明式 UI 只能请求受控意图，例如：

- prefill composer；
- open surface；
- open entity detail；
- request retry / regenerate；
- request explicit save/load flow。

Action Intent 是 UI → Application/Domain 的请求，不是资产直接执行 mutation。

禁止：任意 GDScript callback、任意 method-name dispatch、任意 NodePath execution、filesystem/OS command、World Pack 直接写 authoritative state。

---

## 10. Narrative / Save / Timeline UX

中央 Narrative 只暴露高频、低风险动作：

```text
active generation → Cancel
latest GM generation → Regenerate / Retry
```

在正式 Turn/Persistence Domain 成立后，可评估：

```text
latest player turn → Edit latest input & retry
```

历史 Turn 默认：

```text
read / scroll / inspect
```

**不默认在每条历史内容旁提供 `回到这里` 或任意一键 Rewind。**

Save / Load 是明确高影响操作，未来优先放在 World Surface Host 的 Save / Timeline 区域，并遵守：

> **Save Point != Timeline Node.**
>
> **Reversibility != frictionless arbitrary rewind.**

详细语义见：`../persistence/时间线存档与可逆性设计.md`。

---

## 11. UI Preference != Game State

Surface 顺序、面板展开状态等 UI Preference 可以持久化，但默认不属于 canonical World / Timeline State。

```text
Definition recommends
→ Host initializes
→ Player customizes
→ supported player preference wins
```

Restore 游戏历史默认不回滚纯 UI Preference，除非以后有明确产品裁定。

---

## 12. External World Pack / Mod 必须晚于 Host Proof

顺序固定：

```text
真实固定产品 UI
↓
稳定 Host Slots
↓
真实 Runtime / RPG Projection
↓
handwritten internal definitions
↓
Internal Declarative UI Host vertical proof
↓
capability vocabulary 收敛
↓
G8 external declaration schema
↓
validator / adapter / authoring UX
```

禁止先冻结一个巨型外部 JSON Schema，再倒逼 Host 支持任意能力。

---

## 13. 跨阶段路线

### G2
真实 Narrative/Input + 三 Host Slots；固定手写 UI；不做通用 Renderer。

### G3–G5
随着 Game / Save / Character / Relationship / NPC / Faction 等真实语义出现，建立 player-safe projections，让 Host Slots 消费真实数据。

### G6
完成三栏 RPG Experience + Internal Declarative Host vertical proof + safe vocabulary + responsive / Theme / navigation + UAT。

### G8
只有 Internal Host 能力真实证明后，才外部化 World Pack / Mod contribution schema、ownership/dependency、validator 和 authoring helper。

复杂脚本沙箱继续 Deferred。
