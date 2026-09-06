---
title: my world｜架构总览
status: current-canonical-architecture-map
version: 3.0
created: 2026-08-26
updated: 2026-09-06
current_phase: G6
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜架构 CURRENT

## 0. 文档职责

本文件拥有 `my world` 的当前**架构地图与专题导航**。

它回答：系统如何分层、核心 owner / boundary 是什么、Source 如何进入 Game、Runtime 如何拥有 lived reality、UI 如何安全投影，以及当前 G6 的产品 Host / Surface 方向。

其它 Authority：

- 产品目的：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 阶段 DAG：`MY_WORLD_总体规划路线图_CURRENT.md`
- Current Task / PASS：`MY_WORLD_CURRENT_STATUS.md`

> **Root is map; subfolders are depth.**

---

## 1. 产品 / 系统四层

```text
RPG Experience Layer
Application Main Menu / New Game / Game Library
+ Player Host / Narrative Host / World Surface Host
↓
The World Runtime
Game / World / Timeline / Save / Conversation /
NPC / Knowledge / Agency / World Evolution / Mechanics / Context
↓
Engine Adapter
Godot UI / IO / Network / Assets / Lifecycle / Persistence binding
↓
Godot 4.7.2
Window / Control / 2D / Input / Font / Image / Audio / Packaging / Debug
```

核心原则：

> **Commodity Foundation, Owned Game Semantics.**
>
> **Engine-native, not engine-semantic-coupled.**

因此：

```text
World != SceneTree
NPC != Node
Save != Resource dump
Timeline != Scene history
SQLite table != automatic business owner
Source Asset != current Game World
Source Library != Game Library
Application Lifetime != Game Session Lifetime
UI layout != gameplay truth
```

---

## 2. 技术基线

```text
Host             Godot 4.7.2
Distribution     Standard / non-.NET Windows x64
Language         GDScript
Runtime          same-process Godot Runtime
Provider         current configured production Provider
Source           JSON manifest + package-local files where appropriate
Persistence      SQLite via godot-sqlite
Game topology    One Game = One SQLite
Source Library   managed immutable filesystem generations
```

继续保持：

- Domain 不依赖 Scene / Node / Resource 生命周期；
- Provider Adapter 极薄；
- Persistence representation 与业务 Domain / UI 分离；
- UI、Transcript、Prompt、Cache、Markdown、Godot Resource 不自动成为 authoritative gameplay truth；
- 不建设 IPC、通用 ORM、DI/EventBus 或 framework forest。

---

## 3. Canonical Ownership

```text
Primary Source Assets
→ reusable authored pre-game source

Managed Source Library
→ installed exact immutable generations

Game Creation Composition
→ one New Game's exact selections / Entry / role / settings

Atomic Final Create
→ freezes selected Source projection + provenance into a new Game

Game Domain
→ Game identity / lifecycle / provenance

World / Character / gameplay Domains
→ game-local lived authoritative reality

Conversation
→ accepted Player + GM narrative truth

Knowledge / Agency / Evolution
→ bounded durable living-world semantics

Timeline / Save / Recovery
→ reversible history / named restore intent / protection semantics

Context Assembly
→ derived model-visible working set

Persistence
→ durable representation / transaction / migration / recovery mechanics

Application UI
→ application lifecycle intents

In-game UI
→ player-safe projection + bounded intent dispatch
```

正式原则：

> **Persisted by SQLite != owned semantically by Persistence.**
>
> **Canonical truth ownership != player information architecture.**

Derived / Snapshot / Cache / Transcript / Prompt / ViewModel 默认不可反向成为第二 live truth。

---

## 4. Source → Game-local → Runtime

第一代 Primary Source：

```text
World Pack
Character Card
Expansion Pack
```

长期分层：

```text
Reusable immutable Source
↓ explicit exact selection + Entry/T0
T0-scoped Source Projection
↓ Atomic Final Create
Game-local Canonical Reality
↓ current execution
Runtime State
↓ player-safe projection
UI ViewModel / Surface
```

核心不变量：

> **Source defines the starting reference; game-local reality owns lived history.**
>
> **Source provides inertia; actors create history.**

### 4.1 Exact generation

Stable Source identity 与 exact generation 永久区分。

Existing Game pin exact generation；Source update 不得静默改旧 Game。

Fingerprint 覆盖 declared package bytes；coverage != runtime visibility。

### 4.2 T0 quarantine

```text
immutable package total content
!= selected T0 projection
!= Game-local reality
!= runtime relevant set
!= model-visible working set
```

> **Do not show the model a post-T0 answer and then ask it to forget that answer.**

隔离 future answer，但保留截至 T0 的人格、经历、能力、局限、关系惯性、知识来源、制度/地理/资源压力与开放目标。

### 4.3 Character Card

Character Card 是 reusable Character Source，不是玩家角色专用卡。

当前 v0.2 还允许 optional presentation-only：

```text
player_profile
```

它只服务 human-player Character presentation，必须通过 Game-local frozen projection 进入 UI；不能把 raw `semantic_sections` / `gm_reference` / `gm_private` 直接交给 Player Surface。

当前张琛 accepted generation：

```text
asset_id: character.han_end.zhang_chen
version: 0.1.1
```

---

## 5. Game Creation / Final Create

第一代固定 asset-only：

```text
Exactly 1 World Pack exact generation
+ selected Entry/T0
+ 0..N Expansion
+ Exactly 1 Player Character
+ 0..N Guaranteed NPC Characters
+ Game display name / protagonist control / bounded settings
→ Compatibility Review
→ Atomic Final Create
```

Chooser open/list visibility != selection；只有显式选择具体 item 才写 Composition。

Final Create 是 Program-owned durable transaction flow：Provider calls = 0；必须处理 double click / retry / response loss / intent mismatch / crash window。

Guaranteed NPC 只表示 canonical cast，不自动表示 opening appearance / same place / player knowledge / relationship / ordinary Context inclusion。

---

## 6. Game-local Evolvable Semantics

> **Source schema is not the possibility ceiling of the Living World.**

Final Create 后允许出现 Source 未预见的新长期语义，但必须：

- 不修改 Source；
- 不伪造 Source ancestry；
- 复用已有 canonical Domain owner；
- durable；
- Timeline / Save / Restore reversible。

Runtime-created NPC / Place / Item / Event 可以只有 game-local identity + runtime-generated provenance。

---

## 7. G5 Living World Architecture — CLOSED

G5 已建立：

```text
free-form accepted Narrative
→ best-effort semantic materialization
→ durable consequence

World Truth
!= actor Knowledge
!= human-player disclosure

stable actor roster
→ independent NPC agency

latest world state
→ selective World Evolution
→ hold OR bounded event
```

保护原则：

- Narrative acceptance 不依赖 semantic/Knowledge/Agency/Evolution lane 成功；
- Player foreground wins；
- off-screen actor 可以行动；
- `hold` 是合法 World Evolution 结果；
- Public d20 是 mechanics grounding，不是第二 world truth；
- 不建 full-universe simulator / universal entity graph / fixed event cadence。

G5 current details 由 `architecture/world/` supporting decisions 持有。

---

## 8. Persistence / Timeline / Reversibility

```text
Persistent World State
!= Save Point
!= Timeline Node
!= Recovery Checkpoint
!= Physical Backup
```

```text
Cancel / Regenerate / correction
= 高频低风险

Save Point
= player-named long-term restore intent

Restore
= high-impact active-future change

Recovery Checkpoint
= Runtime protection

Timeline Node
= internal durable history anchor
```

Restore 必须恢复世界与一致 Context；不能 DB 回到过去而 model context 仍含未来。

Game-local semantic evolution 同样进入 Timeline/Save/Restore。

---

## 9. Application / Multi-Game Lifecycle

```text
Application Lifetime != Game Session Lifetime
```

```text
Launch
→ Main Menu
→ open selected Game
→ in-game UI
→ safely close/cancel Game-owned activity
→ back to Main Menu
→ Application remains alive
```

One Game = One SQLite。

Source Library 与 Game Library 永久分离：

```text
Source Library → reusable content generations
Game Library   → independent lived Games
```

---

## 10. Context Architecture

必须保持：

```text
Source Library
!= Game Selected Source Set
!= T0 Projection
!= Game-local Entity Set
!= Player-known Set
!= Runtime Relevant Set
!= Model-visible Working Set
```

以及：

```text
System Total State
!= Runtime Relevant Set
!= Model-visible Working Set
```

> **Bounded context != starved context.**

G7 才系统化长局 retrieval / performance；G6 UI 不得反向成为 Context authority。

---

## 11. G6 RPG Experience Architecture

### 11.1 三 Host

```text
Player Host | Narrative Host | World Surface Host
```

职责：

```text
Player Host
→ 我是谁？我现在怎么样？
→ 长期是高频、紧凑 HUD

Narrative Host
→ 现在发生什么？我下一步做什么？
→ GM Narrative + natural-language composer
→ 永远是视觉/交互中心

World Surface Host
→ 我主动想查看哪些世界/角色/系统信息？
→ secondary RPG information surfaces
```

正式继承 The World 已验证原则：

> **Workspace is organized for truth maintenance; UI is organized for player decisions.**

后台 truth owner 不直接决定玩家 Tab 结构。

### 11.2 Current safe projection chain

MW-011 已建立：

```text
Runtime / frozen Source
→ player-safe domain projection
→ presentation-only RPG Host ViewModel
→ Player Host / World Overview
```

Character profile：

```text
Character player_profile
→ selected projection
→ Final Create frozen Game-local profile
→ fail-closed Player Character Profile Projection
→ ViewModel
→ Player Host
```

禁止 leaf UI 接收 omniscient `world_state` 后再自行过滤。

### 11.3 Surface appearance rule

一个 RPG Surface 只有在以下都成立时才能进入：

```text
real player question
+ real domain owner
+ player-safe projection
+ non-trivial product value
```

禁止为了 UI 完整度创建：

- fake HP / location；
- fake Inventory；
- fake Relationship / Faction；
- keyword-guessed Quest；
- empty tabs。

### 11.4 Current IA discussion

当前 Draft：

`architecture/ui/G6_SURFACE_INFORMATION_ARCHITECTURE_DRAFT_V0_1.md`

候选长期母版：

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

当前不冻结最终 Tab 数量/命名。

强候选顺序：

```text
Character Sheet / 角色
→ People / 人物
→ next grounded Surface by evidence
→ mechanic-state / 系统
```

当前 MW-011 rich left panel 是 accepted transitional state；长期 Player Host 倾向收缩为 HUD，完整 Character Sheet 进入右侧 `角色`。

### 11.5 Visual Runtime

G6 re-entry audit 已完成：

```text
Runtime Asset Resolution = DEFERRED
portrait / scene / authored-map = DEFERRED
```

当前没有成熟 first-party visual consumer。未来 re-entry 时仍保持：

```text
authored visual presentation
!= gameplay/world/location/knowledge authority

map image
!= topology/current location/travel/pathfinding/GIS
```

### 11.6 Internal Declarative UI Host

正确顺序：

```text
fixed real UI
→ stable Host slots
→ multiple real Domain consumers
→ repeated component patterns
→ Internal Declarative UI Host
→ G8 external contract
```

`MW-013` 当前 HOLD / NOT AUTHORIZED YET。

不要让 internal definition 变成 query language、Runtime binding、arbitrary callback 或 external Mod schema。

---

## 12. Expansion Capability Layers

成熟顺序：

```text
G4
Expansion Source + exact binding + observable Runtime effect
↓
G5
mechanic/world semantics + durable state
↓
G6
real mechanic state → player-facing System consumer
↓
G6 later
repeated consumers → Internal Declarative UI Host
↓
G8
proven internal capability → external authoring / UI declaration
```

> **Expansion selected != gameplay effect.**
>
> **Gameplay effect != UI consumer.**
>
> **UI consumer != external protocol.**

---

## 13. Business-module layering

```text
L3 外交层
↓
L2 流程层
↓
L1 器件层
↓
L0 公理层
```

向下跳层允许；向上依赖禁止；跨业务模块通过公开 L3；Bootstrap 是 composition root；不为形式完整创建空层、空类、总线。

---

## 14. 当前 G6 Execution / Decision Order

```text
MW-011 Player Host / ViewModel                PRODUCT PASS / CLOSED
↓
Visual Runtime re-entry audit                 DONE / DEFER IMPLEMENTATION
↓
G6 Surface / Information Architecture Audit   ACTIVE — OWNER + GPT DISCUSSION
↓
freeze first grounded Surface
↓
Task Shape + assign Codex or KimiCode
↓
Independent Review
↓
Owner UAT
↓
repeat grounded consumers
↓
Expansion mechanic-state consumer
↓
re-evaluate MW-013 Internal Declarative UI Host
```

Current Task 只看 `MY_WORLD_CURRENT_STATUS.md`。

---

## 15. 专题导航

```text
architecture/
├─ foundation/
├─ persistence/
├─ source/
├─ world/
└─ ui/
   ├─ 声明式UIHost设计.md
   ├─ G6_RPG_HOST_VIEWMODEL_V0_1_DECISION.md
   ├─ G6_PLAYER_CHARACTER_PROFILE_PROJECTION_V0_1_DECISION.md
   ├─ G6_VISUAL_RUNTIME_REENTRY_AUDIT_2026-09-06.md
   ├─ G6_ROUTE_CORRECTION_AFTER_MW011_UAT_2026-09-06.md
   └─ G6_SURFACE_INFORMATION_ARCHITECTURE_DRAFT_V0_1.md
```

历史 `zhangchenjia21-dot/the-world` 只作为产品证据 / 参考实现，不是当前 implementation authority。

新架构事实先更新本 Map；详细 trade-off / contract / migration / evidence 写 supporting doc。历史版本依赖 Git history，不并列多个 current。
