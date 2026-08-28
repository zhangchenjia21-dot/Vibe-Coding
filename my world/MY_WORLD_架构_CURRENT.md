---
title: my world｜架构总览
status: current-canonical-architecture-map
version: 1.4
created: 2026-08-26
updated: 2026-08-28
current_phase: G4
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜架构 CURRENT

## 0. 文档职责

本文件拥有 `my world` 的**当前架构地图与专题导航**。

它回答：系统现在如何分层、核心 owner / boundary 是什么、深入专题时去哪里读。产品目的看 `MY_WORLD_项目启动总纲_CURRENT.md`；跨阶段原则看 `MY_WORLD_核心设计原则_CURRENT.md`；阶段 DAG 看 `MY_WORLD_总体规划路线图_CURRENT.md`；Current Task / PASS 看 `MY_WORLD_CURRENT_STATUS.md`。

正式文档结构：

> **Root is map; subfolders are depth.**

---

## 1. 产品 / 系统四层

```text
RPG Experience Layer
Application Main Menu / New Game / Continue
+ in-game Player / Narrative / World surfaces
↓
The World Runtime
Game / World / Timeline / Save / Conversation / NPC / Faction /
Knowledge / Relationship / Agent Context / World Evolution
↓
Engine Adapter
把领域语义连接到 Godot UI / IO / Network / Assets / Lifecycle / Persistence binding
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
World Pack Source != current Game World
Main Menu != Game truth
```

Main Menu 属于 Application-level product surface：它负责表达 Game lifecycle intent（Continue / New Game / switch / quit），不持有第二份 Game、Pack 或 Save 真相。

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

Foundation 原始证据：`architecture/foundation/Foundation架构决策_v1.0_2026-08-26.md`。

Persistence 深度设计：`architecture/persistence/时间线存档与可逆性设计.md`。

---

## 3. Canonical Ownership

重要对象先区分业务 owner 与 durable storage responsibility：

```text
Reusable Source / World Pack
→ pre-game authored reference material / source identity / source generation

Game Creation Composition
→ one New Game's explicit player-selected source generation / entry / protagonist seed

Game Domain / lifecycle
→ Game identity / active-game semantics / open-close-switch semantics

World Domain
→ game-local authoritative World meaning/state

Timeline / Save / Recovery Domain
→ Timeline Node / Save Point / Recovery Checkpoint / Restore semantics

Conversation Domain
→ accepted player + GM conversation truth

Context Assembly
→ derived model-visible working set / request material

Persistence
→ SQLite representation
→ transaction / migration / backup / corruption recovery mechanics

Application UI
→ Main Menu / New Game / Continue lifecycle intent projection

In-game UI
→ player-safe gameplay projection / intent dispatch
```

正式原则：

> **Persisted by SQLite != owned semantically by Persistence.**

Derived / Snapshot / Cache / Transcript 默认不可反向成为第二 live truth。

---

## 4. Reusable Source → Composition → Game-local Reality

长期事实模型正式升级为：

```text
Reusable Source Assets
World Pack / future Character / future Expansion
↓ select exact source generation
Game Creation Composition
↓ materialize / bind
Game-local Canonical Reality
↓ current runtime
Runtime State
```

核心边界：

> **World Pack Source != Game Creation Composition != Game-local Instance != Runtime World State.**
>
> **Source 定义开始前的参考世界；游戏开始后，game-local reality 优先。**
>
> **Source provides inertia; actors create history.**

### Source Generation / Provenance

新 Game 不能只记一个人类可读 pack 名称。至少需要能唯一定位创建时使用的 Source generation：

```text
stable pack_id
+ author-controlled pack_version
+ exact content fingerprint / generation identity
```

物理实现可以简单，但产品语义必须保证：Source 后续编辑/升级不会静默改写旧 Game。

### Game Creation Composition v0.1

第一代只冻结当前真实需要：

```text
selected World Pack exact generation
selected Entry / T0 seed
protagonist seed
```

世界口径/profile、Expansion/mechanic 组合、主角操控模式等过去真实使用过的选项不被否定，但当前只作为候选方向；等真实 New Game UAT 证明需要后再进入正式 Composition。

### Entry Point / T0 Source Seed

World Pack v0.1 可以提供 `0..N` 个轻量 Entry：

```text
entry_id
human display name
authored source text / seed
```

Entry 只回答“这一局可以从怎样的 T0 前提开始”。它不是 Scenario Runtime，也不提前冻结 year/month/calendar/region/beat/precondition/branch 等通用 DSL。

### Runtime-generated reality

Source 只是惯性来源。游戏进行中由模型/Runtime 合法 materialize 的 NPC / Place / Item / Event 可以只有 game-local stable identity 与 `runtime_generated` provenance，不要求每个实体都伪造 Source ancestry。

### Player-known boundary

Source 中存在 Character、甚至 Character 已进入 game-local reality，**都不等于玩家已经认识他**。未来人物资料 Surface / Context 必须由 Player Knowledge / Encounter evidence 投影，不能把 Source catalog 当玩家通讯录。

---

## 5. G4 Product Entry / Multi-Game lifecycle

G3 为了先证明 persistence，第一代 intentionally 使用一个 `current-game.sqlite` 路径与单 current Game 体验。G4 第一次真正需要多个 World Pack / 多个独立新局，因此现在才升级 Game lifecycle，而不是在 G3 提前平台化。

正确产品入口：

```text
Application Start
↓
Main Menu
├─ Continue existing Game
├─ New Game
│   ↓
│   World Pack / Entry / protagonist composition
│   ↓
│   Create independent Game
└─ Quit
```

G4-01 先建立 Main Menu / navigation shell；G4-05 才把真实 Local Pack Library 与 Minimal Game Library 接入。

Main Menu 的第一代原则：

- 不复制 Game state；
- Continue 复用 G3 已关闭的 reopen/resume truth；
- startup failure / safe-backup recovery 不能因为增加 Main Menu 被藏掉；
- 从 Game 返回 Main Menu 必须先完成正确 runtime cleanup；
- New Game UI 是 Composition 的产品入口，不把 Source contract 与 UI widget 写死成同一层；
- 多 Game 物理存储形态等到 G4-05 依据现有 SQLite 证据专项裁定。

---

## 6. Model / Runtime / Persistence 边界

> **Model freedom first. Reversibility over prevention.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

模型可以广泛 author Narrative、行为、事件、新实体和后果。Runtime 的强约束集中在 stable identity、atomic durability、Save/Restore correctness、filesystem/database integrity、secrets/OS authority 与可恢复性，而不是建立 Narrative 审查器。

第一代 durable mutation 原子边界：

```text
required authoritative World changes
+ new Timeline Node / mutation metadata
+ required checkpoint/recovery material when applicable
+ Game.active_head reference
→ one SQLite transaction
→ COMMIT 后 publish success
```

失败 rollback；不得由 UI / Transcript / Context 补成“看起来成功”。

G3 已关闭 stable mutation identity / replay-safe semantics、Save/Restore/Recovery、crash/backup 等基础能力。G4 Source / Composition / Game Library 不得绕过或重定义这些 owner。

---

## 7. Context 架构

必须保持：

```text
Asset Library
!= Game Enabled Set
!= Runtime Relevant Set
!= Model Visible Working Set
```

以及：

```text
Runtime Relevant != Model Visible
```

G2-05 第一代策略：最近 12 个完整 accepted Turns + current attempt。Context/messages 是 derived request material，不是 durable truth；Restore 后未来 Context 必须失效/重建，不能泄漏被回滚 future。

World Pack Source 也**不是自动 Model Context**。G4/G5 后续必须经过 game-local materialization、relevance/knowledge 等正式边界后，才决定哪些材料进入模型 working set。

目标仍是：

```text
Game State / Event History ↑↑↑
ordinary Turn model context ≈ bounded
```

---

## 8. Save / Timeline / Reversibility 架构

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

G3-GATE 已 PASS。Arbitrary per-turn rewind、Timeline browser、backup browser 继续 Deferred。

SQLite / Timeline / backup / migration 深度：`architecture/persistence/时间线存档与可逆性设计.md`。

---

## 9. UI Host 架构

UI 正式分成两个层次。

### 9.1 Application-level Product Surface

```text
Main Menu
New Game flow
Continue / Game Library
future Settings / authoring entry as needed
```

G4 开始建立这层。它拥有导航与 lifecycle intent，不拥有 gameplay truth。

### 9.2 In-game RPG Surface

长期桌面骨架：

```text
Player Host | Narrative Host | World Surface Host
```

Wide/maximized baseline 约 `18 / 60 / 22`；Player Host min ~250px，World Host min ~310px；空间不足时折叠侧 Host，Narrative 保持主区域。正文 readable width 约 <= 920px；默认 Maximized Window；1280×720 与 960×540 继续作为回归尺寸。

演化顺序：

```text
G2    fixed in-game Godot UI + stable Host Slots
↓
G3    persistence/recovery integrated into real UI
↓
G4    Application Main Menu + New Game / Game lifecycle surfaces
↓
G5    real World Domain / player-safe projections
↓
G6    Internal Declarative UI Host
↓
G8    external World Pack / Mod declarative UI contract
```

> **Definition declares what should be expressed; Host owns how it is rendered.**

G4 World Pack 不因内容包能力而提前获得 external UI schema；该能力仍留到 G8。

详细设计：`architecture/ui/声明式UIHost设计.md`。

---

## 10. G4 current execution order

```text
G4-01 Product Entry Shell / Main Menu
→ G4-02 World Pack Source v0.1 + two-shape Contract Reality Check
→ G4-03 Game Creation Composition v0.1 + New Game Flow
→ G4-04 Source → Game-local Instance
→ G4-05 Local Pack Library + Minimal Game Library
→ G4-06 Asset Resolution
→ G4-07 Two-Pack Playable Reality Test
→ G4-GATE
```

旧的 `G4-01 World Pack v0.1` task packet 在执行前被本路线 supersede；World Pack Source 工作顺延为 G4-02，且新增 lightweight Entry/T0 seed 与双形态 contract reality check。

---

## 11. 业务模块内部 L3 → L0

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

G4-02 可采用一个小型 `src/world_pack/` production module；G4-01 产品导航应复用现有 Application/Game Shell，不另建平行 App framework。

---

## 12. 专题导航 / 维护规则

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
└─ 备选开发方向候选池_2026-08-28.md
```

`experience/备选开发方向候选池_2026-08-28.md` 记录旧项目中有价值、但当前不授权实施的能力与 revisit trigger。它不是 Roadmap，也不能自动提升为 Task。

新架构事实先更新本 Map；需要详细 trade-off / contract / migration / evidence 时再更新对应 `architecture/<domain>/` supporting doc。Current Task 只看 `MY_WORLD_CURRENT_STATUS.md`，不要新建每阶段顶层 CURRENT 文件。
