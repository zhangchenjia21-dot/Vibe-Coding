---
title: my world｜架构总览
status: current-canonical-architecture-map
version: 1.3
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
玩家看到、阅读和操作的游戏体验
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

Foundation 原始证据：`architecture/foundation/Foundation架构决策_v1.0_2026-08-26.md`。

Persistence 深度设计：`architecture/persistence/时间线存档与可逆性设计.md`。

---

## 3. Canonical Ownership

重要对象先区分业务 owner 与 durable storage responsibility：

```text
Reusable Source / World Pack
→ pre-game authored reference material / source identity

Game Domain / lifecycle
→ Game identity / active-game semantics

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

UI
→ player-safe projection / intent dispatch
```

正式原则：

> **Persisted by SQLite != owned semantically by Persistence.**

Derived / Snapshot / Cache / Transcript 默认不可反向成为第二 live truth。

---

## 4. Reusable Source → Game-local Reality

长期事实模型：

```text
Reusable Source Assets
World Pack / Character / Expansion
↓ new game / bind
Game-local Canonical Reality
↓ current runtime
Runtime State
```

核心边界：

> **World Pack Source != Game-local Instance != Runtime World State.**
>
> **Source 定义开始前的参考世界；游戏开始后，game-local reality 优先。**
>
> **Source provides inertia; actors create history.**

因此：

- Source 提供 T0 前材料、作者意图与惯性；
- 开局后 game-local reality 权威；
- Source 更新不得静默改写已有 Game；
- 模型可创造 NPC / 地点 / 物品 / 事件与开放式后果；
- durable 内容需要 stable identity / provenance；
- UI 只投影当前真相；
- World Pack 内容不能因“可扩展”而自动获得 Runtime / OS 任意代码执行权。

G4 当前顺序：

```text
G4-01 World Pack Source contract v0.1
→ G4-02 Source → Game-local Instance
→ G4-03 Pack Discovery / Install / Load
→ G4-04 Asset Resolution
→ G4-05 Second Pack Fixture
```

G4-01 只冻结 reusable Source contract / explicit-root loader：metadata、world instructions、Source lore、initial character Source seeds、authored map declaration、portrait/scene/map asset declarations、必要 mechanic declarations。它不冻结 G5 NPC/Faction/Knowledge/Relationship schema，不执行 mechanic code，不定义 external declarative UI contract，也不把 Source 写进当前 Game。

---

## 5. Model / Runtime / Persistence 边界

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

G3 已关闭 stable mutation identity / replay-safe semantics、Save/Restore/Recovery、crash/backup 等基础能力。G4 Source loading 默认不得绕过或重定义这些 owner。

---

## 6. Context 架构

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

## 7. Save / Timeline / Reversibility 架构

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

## 8. UI Host 架构

长期桌面骨架：

```text
Player Host | Narrative Host | World Surface Host
```

Wide/maximized baseline 约 `18 / 60 / 22`；Player Host min ~250px，World Host min ~310px；空间不足时折叠侧 Host，Narrative 保持主区域。正文 readable width 约 <= 920px；默认 Maximized Window；1280×720 与 960×540 继续作为回归尺寸。

演化顺序：

```text
G2    fixed Godot UI + stable Host Slots
↓
G3–G5 real Domain / player-safe projections
↓
G6    Internal Declarative UI Host
↓
G8    external World Pack / Mod declarative UI contract
```

> **Definition declares what should be expressed; Host owns how it is rendered.**

G4 World Pack 不因内容包能力而提前获得 external UI schema；该能力仍留到 G8。

详细设计：`architecture/ui/声明式UIHost设计.md`。

---

## 9. 业务模块内部 L3 → L0

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

G4-01 可采用一个小型 `src/world_pack/` production module；不要为了四层命名形式创建空壳层。

---

## 10. 专题导航 / 维护规则

```text
architecture/
├─ foundation/
│  └─ Foundation架构决策_v1.0_2026-08-26.md
├─ persistence/
│  └─ 时间线存档与可逆性设计.md
└─ ui/
   └─ 声明式UIHost设计.md

experience/
└─ DSH经验继承矩阵_v1.0_2026-08-25.md
```

G4-01 先通过 Task Packet / implementation evidence 冻结最小 Source contract；只有产生需要长期维护的 trade-off / migration / public contract 深度时，再在 `architecture/world-pack/` 建 supporting doc，并从本 Map 导航。不要为了阶段编号机械新建文档。

新架构事实先更新本 Map；需要详细 trade-off / contract / migration / evidence 时再更新对应 `architecture/<domain>/` supporting doc。Current Task 只看 `MY_WORLD_CURRENT_STATUS.md`，不要新建每阶段顶层 CURRENT 文件。
