---
title: my world｜架构总览
status: current-canonical-architecture-map
version: 3.1
created: 2026-08-26
updated: 2026-09-06
current_phase: G6
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜架构 CURRENT

## 0. 文档职责

本文件拥有 `my world` 的当前**架构地图与专题导航**。

它回答：系统如何分层、核心 owner / boundary 是什么、Source 如何进入 Game、Runtime 如何拥有 lived reality、模型如何整理开放语义、UI 如何安全投影，以及当前 G6 的 Host / Surface 路线。

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
+ Player Status Host / Narrative Host / World Information Host
↓
The World Runtime
Game / World / Timeline / Save / Conversation /
Character / NPC / Knowledge / Agency / World Evolution / Mechanics / Information Curation / Context
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

Information Curator model
→ interprets accepted game meaning for enabled player-information surfaces
→ decides semantic importance / current Character summary / protagonist milestones

Information Curation Runtime
→ normalized durable model-curated information material
→ current-turn/timeline binding / idempotence / reversible projection

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
>
> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

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
↓ model-driven curation / player-safe projection
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

当前 v0.2 允许 optional presentation-only `player_profile`。它必须通过 Game-local frozen projection 进入玩家侧，不能把 raw `semantic_sections` / `gm_reference` / `gm_private` 直接交给 Player Surface。

MW-014 之后，frozen `player_profile` 是当前 Character 的**起始材料**而不是 lived Character 的永久最终快照。当前人物信息由 Post-turn Information Curator 随 lived history 整理并通过 player-safe projection 输出。

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
- 已有正式 canonical owner 时不制造第二份 truth；
- durable；
- Timeline / Save / Restore reversible。

Runtime-created NPC / Place / Item / Event 可以只有 game-local identity + runtime-generated provenance。

对于“什么重要、人物是否真正改变、某事件是否值得进入信息栏”等开放语义，优先交给模型判断；不要让 Runtime 通过关键词、分数、事件规则树去复刻模型理解。

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

Restore 必须恢复 World + Conversation + model-curated current information 到一致当前历史；不能 DB 回到过去而 Context / Character / Important Experiences 仍含未来。

Game-local semantic evolution 与 Information Curation 都必须服从 Timeline / Save / Restore currentness。

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

G7 才系统化长局 retrieval / performance；G6 UI 与 curation projection 不得反向成为 Context authority。

---

## 11. G6 RPG Experience Architecture

### 11.1 三 Host

```text
Player Status Host | Narrative Host | World Information Host
```

职责：

```text
Player Status Host
→ 角色立绘 + 高频实时角色/机制状态 HUD
→ 不承担“我是谁”的 biography/profile
→ 没有合法 portrait / status contribution 时允许收窄、折叠或隐藏

Narrative Host
→ 现在发生什么？我下一步做什么？
→ GM Narrative + natural-language composer
→ 永远是视觉/交互中心

World Information Host
→ 玩家主动查看的角色 / 世界 / 系统信息
→ 概览 / 角色 / 重要经历 / 人物 / 事务 / 行囊 / 系统 / 地图 / 存档等 grounded Surfaces
```

正式继承：

> **Workspace is organized for truth maintenance; UI is organized for player decisions.**

后台 truth owner 不直接决定玩家 Tab 结构。

### 11.2 Projection / curation chain

MW-011 建立第一条 player-safe UI projection；MW-014 建立第一条 model-curated lived Character / milestone vertical：

```text
accepted Player + GM Narrative
+ frozen starting Character material
+ bounded current Character / recent milestones
↓
Post-turn Information Curator model
↓
bounded structured curation
↓
normalized durable currentness
↓
player-safe Character + Important Experiences projection
↓
World Information Host consumer
```

保护：

- Curator 是 background semantic maintenance，不是 Narrative Finalize Gate；
- leaf UI 不接收 omniscient `world_state` 后自行过滤；
- UI render / reopen 不要求 Provider call；
- Regenerate / Restore 必须使 stale curation 非 current。

### 11.3 Frozen Character + Important Experiences

Canonical：

- `architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md`
- `architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`

```text
角色 / Character
→ “现在的我是谁”
→ evolving current Character Sheet
→ 当前状态，不是 mutation log

重要经历 / Important Experiences
→ “我是怎样走到现在的”
→ protagonist-centered milestone history
```

Character 第一代组：

```text
基本资料
出身 / 来历
当前身份 / 社会角色
性格 / 价值观 / 原则
能力 / 专长说明
局限 / 长期特征
长期目标 / 自我方向
```

起始/当前物品属于 `行囊 / Inventory`，不属于 Character Surface。长期目标属于 Character；当前未完成承诺/问题属于未来 `事务`。

### 11.4 Current Surface mother taxonomy

```text
概览
角色
重要经历
人物
事务
行囊
系统
地图
存档
```

这是一张 IA 母版，不代表九个 Tab 现在全部出现。

一个 Surface 只有在以下都成立时进入产品：

```text
real player question
+ real data owner / model-curated normalized material
+ player-safe projection
+ non-trivial product value
```

禁止为了 UI 完整度创建 fake HP/location/Inventory/Relationship/Faction/Quest 或空标签页。

当前已 grounded：

```text
概览
角色
重要经历
存档
```

其它 Surface 继续按真实 Domain / consumer evidence 拉出。

### 11.5 Player Status Host transition

MW-011 rich left `player_profile` 是已接受的阶段性过渡实现，不是长期 IA。

当右侧 Character Surface 成立后：

```text
identity / biography / background / personality / authored capability / limitation / goals
→ 迁入右侧 Character

left Player Status Host
→ 只保留 portrait + real live mechanic/status contributions
```

当前没有正式 portrait / mechanic contribution，因此 UI consumer 允许让左 Host collapse/narrow/hide；不得继续拿 biography、world/session metadata、recent actions 或 turn count 填空。

### 11.6 Visual Runtime

G6 re-entry audit：

```text
Runtime Asset Resolution = DEFERRED
portrait / scene / authored-map = DEFERRED
```

未来 re-entry 时仍保持：

```text
authored visual presentation
!= gameplay/world/location/knowledge authority

map image
!= topology/current location/travel/pathfinding/GIS
```

### 11.7 Internal Declarative UI Host

正确顺序：

```text
fixed real UI
→ multiple grounded Surface / mechanic consumers
→ repeated component patterns
→ Internal Declarative UI Host
→ bounded Action Intent
→ G8 external contract
```

`MW-013` 当前 **HOLD / NOT AUTHORIZED YET**。

不得因为 Character / Important Experiences 需要列表/分组就提前 re-authorize MW-013；先让真实手写 consumer 证明组件重复。

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
real mechanic state → player-facing System consumer + high-frequency left HUD contribution
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
MW-011 Player Host / ViewModel                 PRODUCT PASS / CLOSED
↓
Visual Runtime re-entry audit                  DONE / DEFER IMPLEMENTATION
↓
Character + Important Experiences semantics   FROZEN
↓
MW-014 model-driven information curation       ENGINEERING PASS / INTEGRATED
↓
Character + Important Experiences UI consumer CURRENT NEXT
↓
GPT Independent Review
↓
Owner UAT
↓
People Surface / Expansion mechanic-state consumer / next grounded Surface
↓
repeated patterns
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
   ├─ G6_SESSION_SHELL_INFORMATION_OWNERSHIP_DECISION.md
   ├─ G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md
   ├─ G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md
   ├─ G6_RPG_HOST_VIEWMODEL_V0_1_DECISION.md
   ├─ G6_PLAYER_CHARACTER_PROFILE_PROJECTION_V0_1_DECISION.md
   ├─ G6_VISUAL_RUNTIME_REENTRY_AUDIT_2026-09-06.md
   ├─ G6_ROUTE_CORRECTION_AFTER_MW011_UAT_2026-09-06.md
   ├─ G6_SURFACE_INFORMATION_ARCHITECTURE_DRAFT_V0_1.md
   └─ 声明式UIHost设计.md
```

历史 `zhangchenjia21-dot/the-world` / `sillytavern` 只作为产品证据 / 参考实现，不是当前 implementation authority。

新架构事实先更新本 Map；详细 trade-off / contract / migration / evidence 写 supporting doc。历史版本依赖 Git history，不并列多个 current。
