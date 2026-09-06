---
title: my world｜架构总览
status: current-canonical-architecture-map
version: 3.2
created: 2026-08-26
updated: 2026-09-07
current_phase: G6
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v3.1
---

# my world｜架构 CURRENT

## 0. 文档职责

本文件拥有 `my world` 当前架构地图与专题导航：系统分层、canonical owner、Source → Game-local Reality、Living World、Persistence/Timeline、Context、G6 Host/Surface，以及 **Internal Dynamic UI Host v0.1** 的正式位置与边界。

其它 authority：

- 产品：`MY_WORLD_项目启动总纲_CURRENT.md`
- 原则：`MY_WORLD_核心设计原则_CURRENT.md`
- Roadmap：`MY_WORLD_总体规划路线图_CURRENT.md`
- Current Task：`MY_WORLD_CURRENT_STATUS.md`

> **Root is map; subfolders are depth.**

---

## 1. 产品 / 系统四层

```text
RPG Experience Layer
Application Main Menu / New Game / Game Library
+ Player Status Host / Narrative Host / World Information Host
+ Internal Dynamic UI Host
↓
The World Runtime
Game / World / Timeline / Save / Conversation /
Character / NPC / Knowledge / Agency / Evolution / Mechanics /
Inventory / Information Curation / Context
↓
Engine Adapter
Godot UI / IO / Network / Assets / Lifecycle / Persistence binding
↓
Godot 4.7.2
```

核心：

> **Commodity Foundation, Owned Game Semantics.**
>
> **Engine-native, not engine-semantic-coupled.**

因此：

```text
World != SceneTree
NPC != Node
Save != Resource dump
Timeline != Scene history
SQLite table != semantic owner
Source Asset != current Game World
UI layout != gameplay truth
Dynamic UI declaration != gameplay truth
```

---

## 2. 技术基线

```text
Host             Godot 4.7.2 Standard / non-.NET Windows x64
Language         GDScript
Runtime          same-process Godot Runtime
Provider         current configured production Provider
Source           JSON manifest + package-local files where appropriate
Persistence      SQLite via godot-sqlite
Game topology    One Game = One SQLite
Source Library   managed immutable filesystem generations
```

继续不建设 IPC、通用 ORM、DI/EventBus 或 framework forest。

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
→ freezes exact selected projection into a new Game

Game / World / Character / gameplay Domains
→ Game-local lived authoritative reality

Conversation
→ accepted Player + GM Narrative truth

Knowledge / Agency / Evolution
→ bounded durable living-world semantics

Mechanics / Inventory owners
→ real mechanic and owned-item authoritative state

Information Curator model
→ semantic interpretation / curation for enabled player-information surfaces

Information Curation Runtime
→ normalized durable model-curated material + Timeline currentness

Timeline / Save / Recovery
→ reversible history / restore / protection semantics

Context Assembly
→ derived model-visible working set

In-game UI / Dynamic UI Host
→ player-safe projection + presentation only
```

正式原则：

> **Persisted by SQLite != owned semantically by Persistence.**
>
> **Canonical truth ownership != player information architecture.**
>
> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

Derived / Snapshot / Cache / Prompt / ViewModel / Dynamic UI tree 默认不可反向成为第二 live truth。

---

## 4. Source → Game-local → Runtime

```text
Reusable immutable Source
↓ explicit exact selection + Entry/T0
T0-scoped Source Projection
↓ Atomic Final Create
Game-local Canonical Reality
↓ current execution
Runtime State
↓ player-safe projection / model-driven curation / mechanic contribution
UI / Internal Dynamic UI Host
```

> **Source defines the starting reference; game-local reality owns lived history.**
>
> **Source provides inertia; actors create history.**

Stable Source identity 与 exact generation 永久区分；Existing Game 永久 pin exact generation；Source update 不得静默改旧 Game。

T0 quarantine 继续保护 future answer 不进入当前 Game/Context。

---

## 5. Game Creation

第一代固定：

```text
Exactly 1 World Pack
+ Entry/T0
+ 0..N Expansion
+ Exactly 1 Player Character
+ 0..N Guaranteed NPC
+ bounded settings
→ Compatibility Review
→ Atomic Final Create
```

Final Create 是 Program-owned durable transaction；Provider calls = 0。Guaranteed NPC 不自动意味着 opening appearance / same place / player knowledge / relationship。

---

## 6. Living World / Evolvable Semantics

Final Create 后允许 Source 未预见的新 NPC / Item / Event / semantic state，但必须：

- 不修改 Source；
- 不伪造 Source ancestry；
- 不产生第二 semantic owner；
- durable；
- Save / Restore reversible。

G5 已冻结：

```text
free-form accepted Narrative
→ best-effort semantic materialization
→ durable consequence

World Truth != actor Knowledge != human-player disclosure
stable NPC Agency
selective World Evolution / valid hold
Public d20 mechanics grounding
```

Narrative acceptance 不被 semantic / Knowledge / Agency / Evolution / curation / recommendation failure阻塞。

---

## 7. Persistence / Timeline / Reversibility

```text
Persistent World State
!= Save Point
!= Timeline Node
!= Recovery Checkpoint
!= Physical Backup
```

Restore 必须使 World + Conversation + Character/People/Open Threads + Inventory + mechanics + Dynamic UI projection 同时回到一致 current history。

Dynamic UI Host 不持久化第二份游戏事实；它只从 current safe projection / contribution 重建。

---

## 8. Context Architecture

必须保持：

```text
System Total State
!= Runtime Relevant Set
!= Model-visible Working Set
```

以及：

```text
Source total content
!= Game selected set
!= Game-local entity set
!= Player-known set
!= Runtime relevant set
!= model-visible set
```

> **Bounded context != starved context.**

G7 才系统化 Context Orchestrator / long-session retrieval；G6 UI / Dynamic UI 不得反向成为 Context authority。

---

## 9. G6 三 Host

```text
Player Status Host | Narrative Host | World Information Host
```

### Player Status Host
portrait + real live mechanic/status HUD only。没有真实 contribution 时允许收窄/隐藏；不拿 biography、turn count 或 fake HP 填空。

### Narrative Host
GM Narrative + natural-language composer，是视觉与交互中心。Five Recommended Actions 只能 prefill，不自动发送、不定义合法行动集。

### World Information Host
承载 grounded player information：概览 / 角色 / 重要经历 / 人物 / 事务 / 行囊 / 系统 / 地图 / 存档等。只有有 real owner + player-safe projection + product value 才出现。

---

## 10. Information Curation

已成立链：

```text
accepted Player + GM Narrative
+ bounded starting/current safe material
↓
Post-turn Information Curator
↓
bounded structured curation
↓
normalized durable currentness
↓
player-safe Character / Important Experiences / People projection
```

后续 Open Threads 与 richer information surfaces 优先扩展同一 curation family；Program 不用 keyword/score/rule-tree 模拟开放语义。

Curator 是 background maintenance，不是 Narrative Finalize Gate。

---

## 11. Core Mechanics / Inventory consumers

### 11.1 System / mechanics

```text
real mechanic owner
→ bounded player-safe mechanic contribution
→ System Surface / Dynamic UI Host
```

Shell 不认识通用 HP/Mana/Hunger/Money；只有真实 Expansion/mechanic owner 才能提供相应 state。

### 11.2 Inventory

Inventory 必须先有 authoritative Game-local owner / mutation contract：

```text
real owned item
→ accepted semantic/mechanic mutation
→ durable Inventory state
→ player-safe projection
→ Inventory Surface / Dynamic UI Host
```

Narrative 偶然提到不存在物品不得自动成为正式 Inventory。

---

## 12. Internal Dynamic UI Host v0.1｜ROUTE AUTHORIZED CORE

Owner 于 2026-09-07 明确：**动态 UI 是 V0 核心能力，必须在 Core Closure Reality Gate 前完成。**

正确顺序冻结为：

```text
Character / Important Experiences / People
+ Open Threads
+ System/Public d20
+ Inventory
↓
多个 production consumers 暴露重复 pattern
↓
Internal Dynamic UI Host v0.1
↓
V0 Core Closure Reality Gate
↓
更多信息 / mechanics consumers 复用
↓
G8 才考虑 external Declarative UI contract
```

### 12.1 v0.1 目标

统一承载真实 Game-local player-safe information / mechanic contributions，让不同实际游戏内容可以形成不同但一致的 RPG UI，而无需为每个内容类型重复手写整套页面。

### 12.2 v0.1 vocabulary 只能从真实重复模式抽象

候选仅限真实需要，例如：

- section / group；
- text / labeled field；
- card / list；
- collapsed / expanded region；
- bounded status / mechanic contribution；
- 已证明的 safe navigation。

### 12.3 禁止能力

v0.1 不允许：

- arbitrary GDScript callbacks；
- NodePath execution；
- OS/filesystem/network commands；
- Dynamic UI 直接 mutation authoritative state；
- renderer 接收 omniscient world_state 后自行过滤；
- generic Action Intent；
- external Source / Expansion 作者自由声明 UI。

### 12.4 MW-013 lineage

旧 `MW-013` 已完成过早期 Task Shaping，但形成时 consumer evidence 不足。

当前状态：

```text
MW-013 capability direction = ROUTE AUTHORIZED
old Task Packet             = STALE / DO NOT EXECUTE AS-IS
new executable Work         = only after Package 2–4 consumer evidence
```

届时必须重新 Task Shape；是否沿用 MW-013 executable identity 由 Task Identity / Lineage 审计决定，不能为了名称方便跳过重新设计。

---

## 13. Generic Action Intent / External UI

Generic Bounded Action Intent 继续 Deferred。MW-019 `prefill composer` 只是一个 fixed first-party consumer，不授权 command bus。

External Declarative UI 只能在 G8 基于 Package 5 的 production vocabulary 演化；不得设计第二套互不兼容 schema。

---

## 14. Current Core Execution Order

```text
Package 0  MW-018 + MW-019 Owner UAT
↓
Package 1  OOC + Character-guided Recommendations
↓
Package 2  Open Threads
↓
Package 3  System / Public d20 consumer
↓
Package 4  Inventory vertical
↓
Package 5  Internal Dynamic UI Host v0.1
↓
Package 6  V0 Core Closure Reality Gate
↓
G7 long-session / knowledge hardening
↓
G8 richer product / model ops / Source / Creator
↓
G9 release
```

当前 Task 以 `MY_WORLD_CURRENT_STATUS.md` 为准；路线授权不等于允许跳过 Task Shaping / Independent Review / Owner UAT。

---

## 15. Visual Runtime

Runtime Asset Resolution / portrait / scene / authored-map 仍 Deferred，直到真实第一方 visual demand 出现。

```text
visual presentation != gameplay/world/location/knowledge authority
map image != topology/current location/travel/pathfinding/GIS
```

---

## 16. Business-module layering

```text
L3 外交层
↓
L2 流程层
↓
L1 器件层
↓
L0 公理层
```

向下跳层允许；向上依赖禁止；跨模块通过公开 L3；Bootstrap 是 composition root；不为形式完整创建空层/空类/总线。

---

## 17. 专题导航

详细 product/architecture decisions 继续位于：

```text
architecture/foundation/
architecture/persistence/
architecture/source/
architecture/world/
architecture/ui/
```

G6 Dynamic UI 重新 Task Shape 时，必须读取 `architecture/ui/声明式UIHost设计.md`、既有 MW-013 evidence，以及届时 Package 2–4 的真实 consumer 实现；旧设计只作输入，不自动成为新 contract。