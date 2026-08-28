---
title: my world｜架构总览
status: current-canonical-architecture-map
version: 2.0
created: 2026-08-26
updated: 2026-08-28
current_phase: G4
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜架构 CURRENT

## 0. 文档职责

本文件拥有 `my world` 的**当前架构地图与专题导航**。

它回答：系统现在如何分层、核心 owner / boundary 是什么、Primary Source 与 Game 如何连接、应用与 Game Session 如何分离、深入专题时去哪里读。

其它 Owner：

- 产品目的：`MY_WORLD_项目启动总纲_CURRENT.md`；
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`；
- 阶段 DAG：`MY_WORLD_总体规划路线图_CURRENT.md`；
- Current Task / PASS：`MY_WORLD_CURRENT_STATUS.md`；
- 开发路径复用经验：`experience/AI_RPG开发路径与阶段设计经验_v1.0_2026-08-28.md`。

正式文档结构：

> **Root is map; subfolders are depth.**

---

## 1. 产品 / 系统四层

```text
RPG Experience Layer
Application Main Menu / New Game / Game Library
+ in-game Player / Narrative / World surfaces
↓
The World Runtime
Application Lifecycle / Game / World / Timeline / Save / Conversation /
NPC / Faction / Knowledge / Relationship / Agent Context / World Evolution
↓
Engine Adapter
Godot UI / IO / Network / Assets / Lifecycle / Persistence binding
↓
Godot / Mature Game Foundation
Window / Control / 2D / Input / Font / Image / Audio / Packaging / Debug
```

原则：

> **Commodity Foundation, Owned Game Semantics.**
>
> **Engine-native, not engine-semantic-coupled.**

因此：

```text
World != SceneTree
NPC != Node
Save != Resource dump
Timeline != Scene history
Database row/table != automatic business owner
Source Asset != current Game World
Main Menu != Game truth
Source Library != Game Library
Application Lifetime != Game Session Lifetime
```

---

## 2. 第一代 Foundation / Infrastructure 冻结结果

当前采用：

```text
Host             Godot 4.7.2
Distribution     Standard / non-.NET Windows x64
Language         GDScript
Runtime          Godot same-process Runtime
Provider         DeepSeek deepseek-v4-pro
Config / Source  JSON/files where appropriate
Persistence      SQLite via 2shady4u/godot-sqlite v4.9 — ACCEPTED
```

G3 已 **PASS / CLOSED**。SQLite 第一代 authoritative persistence route 已通过真实 Windows fixture、transaction、migration、Save/Restore/Recovery、single-writer、backup/corruption recovery、real Provider continuity 与 Owner UAT。

继续保持：

- Domain 不依赖 Scene / Node / Resource 生命周期；
- Provider Adapter 极薄；
- Persistence storage 与业务 Domain / UI 分离；
- UI、Transcript、Markdown、Godot Resource 不作为 authoritative gameplay database；
- 不建设 IPC、通用 ORM、DI/EventBus 或 persistence framework forest。

---

## 3. Canonical Ownership

```text
Primary Source Assets
World Pack / Character Card / Expansion Pack
→ reusable authored pre-game source identity / immutable generation

Managed Source Library
→ installed Source inventory / validation / immutable generation retention

Game Creation Composition
→ one New Game's explicit selected Source generations + Entry + roles + settings

Application Lifecycle
→ Main Menu / Game Session open-close-switch / application quit

Game Domain
→ Game identity / current lifecycle / game-local provenance

World Domain
→ game-local authoritative World meaning/state

Timeline / Save / Recovery Domain
→ Timeline Node / Save Point / Recovery Checkpoint / Restore semantics

Conversation Domain
→ accepted player + GM conversation truth

Context Assembly
→ derived model-visible working set / request material

Persistence
→ SQLite representation / transaction / migration / backup / corruption recovery mechanics

Application UI
→ Main Menu / New Game / Game Library lifecycle intent projection

In-game UI
→ player-safe gameplay projection / intent dispatch
```

正式原则：

> **Persisted by SQLite != owned semantically by Persistence.**

Derived / Snapshot / Cache / Transcript 默认不可反向成为第二 live truth。

---

## 4. Primary Source Architecture

第一代正式主资产：

```text
Primary Source Assets
├─ World Pack
├─ Character Card
└─ Expansion Pack
```

三类资产共享最薄 identity seam：

```text
asset_id
asset_type
version
exact immutable generation / content fingerprint
```

但不使用一个巨大 universal asset schema 强迫三类内容拥有相同字段。

### 4.1 World Pack

拥有：

- 世界身份、作者版本、schema version；
- world / GM instructions；
- Source Lore；
- optional Entry / T0 seeds；
- authored asset declarations；
- 开局前世界材料。

World Source 不拥有游戏开始后的历史。

### 4.2 Character Card

Character Card 是 reusable Character Source，不是“玩家角色专用卡”。

第一代建局角色用途只有两个产品概念：

```text
Exactly 1 Player Character
0..N Guaranteed NPC Characters
```

Guaranteed NPC 的语义：

```text
selected exact Character Source
→ Final Create materializes a game-local canonical Character definition
→ character belongs to this Game's cast from T0
```

它不自动建立：

- opening placement；
- player-known state；
- relationship；
- shared scene；
- automatic current Context inclusion。

因此：

> **Guaranteed in Game != Guaranteed in Opening != Player Knows Character.**

### 4.3 Expansion Pack

Expansion 是额外机制 / GM / Runtime capability Source。

第一代数量：`0..N`。

必须区分：

```text
Expansion Source identity
!= selected Game binding
!= Program Runtime capability identity
!= current mechanic state
!= UI contribution
```

G4 只要求第一个真实 Expansion 产生可观察 Runtime effect；G6 才要求真实 mechanic state → Internal UI；G8 才外部化受控 UI contract。

---

## 5. Managed Source Library / Immutable Generation

第一代 asset-only New Game 要求先有 Managed Source Library。

```text
external/local source package
→ validate
→ install/publish immutable generation
→ Source Library inventory
→ New Game exact selection
```

Library 必须能表达：

```text
stable identity
+ semantic version / author version
+ exact generation fingerprint
+ current installed generation for future New Game
+ historical generation retained while any Game pins it
```

### 5.1 为什么 generation 必须 immutable

只保存 `pack_id + version` 但运行时继续读取被覆盖的外部目录，会导致旧 Game 的：

- Lore；
- Character definition；
- portrait；
- scene；
- authored map；
- Expansion behavior declaration

在 Source 更新后静默变化。

因此：

> **Existing Game pins exact Source generation, not mutable folder meaning.**

### 5.2 第一代不做历史版本 picker

Source Library 可以内部保存多个 generation，但 New Game UI 默认只展示当前安装版本。

玩家点击具体资产时 Program pin exact generation。已有 Game 继续引用旧 generation。

这样避免旧项目曾出现的 sibling-version selected-state 混淆，同时不牺牲旧 Game 可重复性。

---

## 6. Game Creation Composition

第一代固定 asset-only composition：

```text
Exactly 1 World Pack exact generation
+ selected Entry/T0
+ 0..N Expansion exact generations
+ Exactly 1 Player Character exact generation
+ 0..N Guaranteed NPC Character generations
+ Game display name
+ Protagonist Control Mode
+ optional opening supplement
```

Protagonist Control Mode：

```text
Full
Light
Narrative
```

默认推荐 Light。

### Selection authority

必须保持：

> **Chooser open / source mode / list visibility != selection.**

只有玩家点击一个具体 Source item 才写入 authoritative Composition。

Final Compatibility Review 是 Final Create 前的最后产品投影；它必须展示实际将被 pin 的 World / Entry / Character / Expansion / settings。

### 第一代明确不做

- no-World creation；
- no-Character player creation；
- free-text AI blank-world creation；
- Draft 直接进入 Game；
- Final Create 时自动 Publish Source；
- arbitrary World-specific form DSL；
- historical Source version picker；
- Expansion feature/module complex chooser。

---

## 7. Final Create / Materialization Boundary

```text
Source Library inventory
↓ explicit player selection
Game Creation Composition
↓ Final Review
Atomic Final Create
↓
Game-local Canonical Reality
↓
Runtime State
```

Final Create 必须是 Program-owned durable transaction flow，而不是 Wizard 每一步直接创建/改写 Game。

至少需要：

```text
creation identity / fingerprint
exact composition snapshot
creating state
independent Game identity
pinned source generations
game-local provenance
created state
```

要求：

- Provider calls = 0 during Final Create；
- double click / response loss exactly-once or replay-safe；
- same create identity → same Game；
- different intent → fail closed；
- Source Library never receives Runtime writeback。

### World materialization

World Source 形成本局 T0 reference / game-local world ancestry。

### Character materialization

Player Character → exactly one game-local player identity。

Guaranteed NPC → exactly one game-local canonical Character definition per selected exact Source。

### Expansion binding

G4 第一阶段可以为 none。第二阶段通过正式 binding 将 selected Expansion 接到 Program capability，并证明真实 observable effect。

---

## 8. Runtime-generated Reality

Source 只是惯性来源。

游戏进行中由模型/Runtime materialize 的 NPC / Place / Item / Event 可以拥有：

```text
game-local stable identity
provenance = runtime_generated
```

不要求所有新实体都映射回一个 Source card。

游戏中动态产生的角色与创建时 Guaranteed NPC 共用同一个 Game-local Character owner；区别在 provenance，不在 Runtime 物种。

---

## 9. Player-known Boundary

必须长期区分：

```text
Source exists
!= Game-local entity exists
!= Player knows entity
!= Runtime relevant
!= Model visible
```

Guaranteed NPC 被建局 materialize，也不自动成为玩家通讯录成员。

G5 Knowledge / Encounter owner 决定玩家何时认识、知道什么；G6 People Surface 只投影 player-safe knowledge。

---

## 10. Application / Game Session Lifecycle

G4-01 正式建立：

```text
Application Lifetime
!= Game Session Lifetime
```

目标：

```text
Launch EXE
→ Application READY / Main Menu

Continue or Select Game
→ open Game Session
→ enter in-game UI

Return to Main Menu
→ finish/cancel generation safely
→ flush required state / close Game-owned runtime resources
→ release Game-specific writer/connection ownership as designed
→ Application remains READY
```

Main Menu 不能只是盖在一个已经自动打开的 current Game 上面。

这条 seam 是后续 multi-Game 的前置条件。

---

## 11. Multi-Game / Game Library

G3 故意先证明 one-current-Game persistence。G4 才第一次有真实 multi-Game 产品需求。

Game Library 最少拥有：

- Game identity；
- player-safe display metadata projection；
- open / close / switch lifecycle；
- Continue / latest Game；
- legacy G3 Game adoption；
- no overwrite on New Game。

物理存储在 G4-04 开始前专项裁定：

```text
per-Game SQLite
vs
shared SQLite + game_id
```

判断依据优先：G3 evidence reuse、single-writer isolation、backup/recovery、迁移复杂度、Windows 文件生命周期和产品简单性。

不要从“未来可能有很多 Game”直接推导出数据库服务层。

---

## 12. Model / Runtime / Persistence Boundary

> **Model freedom first. Reversibility over prevention.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

模型可以广泛 author Narrative、行为、事件、新实体和后果。

Runtime 的强约束集中在：stable identity、atomic durability、Save/Restore correctness、filesystem/database integrity、secrets/OS authority 与可恢复性，而不是建立 Narrative 审查器。

G3 第一代 durable mutation 原子边界继续有效；G4 的 Source / Game Library / Final Create 不得绕过或重定义 G3 Owner。

---

## 13. Context Architecture

必须保持：

```text
Managed Source Library
!= Game Selected Source Set
!= Game-local Entity Set
!= Player-known Set
!= Runtime Relevant Set
!= Model Visible Working Set
```

以及：

```text
Runtime Relevant != Model Visible
```

G2-05 当前策略仍是最近 12 个完整 accepted Turns + current attempt。Context/messages 是 derived request material，不是 durable truth。

G4 First Playable A 要证明 World + Character Source 能形成足够具体的 Setup Context；G5 再负责真实 world semantic materialization、relevance 与 knowledge boundary。

目标：

```text
Game State / Event History / Source Library ↑↑↑
ordinary Turn model context ≈ bounded
```

---

## 14. Expansion Capability Layers

历史项目证明“Source / Manifest / Binding 都存在”仍可能没有真实玩法效果。因此 Expansion 必须逐层证明：

```text
G4
Expansion Source
→ exact selected binding
→ real observable Runtime/Context/mechanic effect

G5
Expansion semantics
→ durable world/mechanic state where applicable

G6
real mechanic state
→ Internal Declarative UI consumer

G8
proven internal capability
→ external authoring / UI declaration contract
```

不允许倒序：不能为了未来 Mod UI 在 G4 预造任意外部 UI schema。

---

## 15. Save / Timeline / Reversibility

```text
Cancel / Regenerate / latest correction
= 高频、低风险、靠近 Narrative

Save Point
= 玩家明确命名的长期恢复 intent/reference

Load / Restore
= 明确高影响操作，改变 active future

Recovery Checkpoint
= Runtime 自动保护 displaced current progress

Timeline Node
= Runtime durable recovery anchor

Physical Backup
= storage disaster recovery copy
```

核心不变量：

> **Save Point != Timeline Node.**
>
> **Recovery Checkpoint != Save Point.**
>
> **Physical Backup != Save Point / Recovery Checkpoint.**
>
> **Reversibility != frictionless arbitrary rewind.**

Multi-Game 不能破坏这些已关闭语义。

---

## 16. UI Host Architecture

### Application-level Product Surface

```text
Main Menu
New Game Wizard
Game Library
future Settings / authoring entry
```

拥有 navigation / lifecycle intent，不拥有 gameplay truth。

### In-game RPG Surface

```text
Player Host | Narrative Host | World Surface Host
```

Wide/maximized baseline 约 `18 / 60 / 22`；Player Host min ~250px，World Host min ~310px；空间不足时折叠侧 Host，Narrative 保持主区域。正文 readable width 约 <= 920px；默认 Maximized Window；1280×720 与 960×540 继续作为回归尺寸。

演化顺序：

```text
G2 fixed in-game Godot UI
→ G3 persistence/recovery integrated UI
→ G4 Application / New Game / Game Library
→ G5 real World projection
→ G6 Internal Declarative UI Host
→ G8 external declarative contract
```

---

## 17. G4 Current Execution Order

```text
G4-01 Application Shell / Main Menu + Game Session Lifecycle
→ G4-02 World Pack + Character Card Source Contracts
→ G4-03 Managed Local Source Library
→ G4-04 Multi-Game Lifecycle / Game Library Foundation
→ G4-05 Asset-only New Game Wizard
→ G4-06 Atomic Final Create + World/Character Materialization
→ G4-07 First Playable A — World + Character Owner UAT
→ G4-08 Expansion Pack v0.1 + Real Runtime Vertical
→ G4-09 First Playable B — Expansion Owner UAT
→ G4-10 Runtime Asset Resolution
→ G4-11 Two Primary Asset Families Reality Test
→ G4-GATE
```

旧的 `G4-01 World Pack v0.1` task packet 与前一版 `Main Menu-only` 路线都被本轮 Owner-approved review supersede；新的 G4-01 必须包含 Application / Game Session lifecycle seam，而不是只添加视觉菜单。

---

## 18. 业务模块内部 L3 → L0

```text
L3 外交层
↓
L2 流程层
↓
L1 器件层
↓
L0 公理层
```

向下跳层允许；向上依赖禁止；跨业务模块只通过对方公开 L3；Bootstrap 是 composition root；不为形式完整创建空层、空类或总线。

G4 预计会逐步出现 `source_library` / `game_library` / `game_creation` 等业务边界，但只有真实任务需要时才创建；禁止先造完整“Asset Platform Framework”。

---

## 19. 专题导航 / 维护规则

```text
architecture/
├─ foundation/
│  └─ Foundation架构决策_v1.0_2026-08-26.md
├─ persistence/
│  └─ 时间线存档与可逆性设计.md
└─ ui/
   └─ 声明式UIHost设计.md

experience/
├─ DSH经验继承矩阵_v1.0_2026-08-25.md
├─ AI_RPG开发路径与阶段设计经验_v1.0_2026-08-28.md
└─ 备选开发方向候选池_2026-08-28.md
```

`AI_RPG开发路径与阶段设计经验` 是未来同类项目优先复用的完整开发顺序/反模式/验证方法参考；旧项目文档继续保留历史证据，但未来新项目不应被迫重新拼读全部历史才能获得这些结论。

`备选开发方向候选池` 只记录尚未进入 CURRENT 的能力；Character Card / Expansion Pack / Protagonist Control Mode 已被本轮提升为 CURRENT，不再属于 deferred candidate。

新架构事实先更新本 Map；需要详细 trade-off / contract / migration / evidence 时再更新对应 supporting doc。Current Task 只看 `MY_WORLD_CURRENT_STATUS.md`。