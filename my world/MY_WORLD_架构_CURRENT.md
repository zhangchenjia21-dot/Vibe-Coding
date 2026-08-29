---
title: my world｜架构总览
status: current-canonical-architecture-map
version: 2.1
created: 2026-08-26
updated: 2026-08-29
current_phase: G4
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜架构 CURRENT

## 0. 文档职责

本文件拥有 `my world` 的**当前架构地图与专题导航**。

它回答：系统如何分层、核心 owner / boundary 是什么、Primary Source 如何进入某一局、Application 与 Game Session 如何分离，以及深入专题时去哪里读。

其它 Owner：

- 产品目的：`MY_WORLD_项目启动总纲_CURRENT.md`；
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`；
- 阶段 DAG：`MY_WORLD_总体规划路线图_CURRENT.md`；
- Current Task / PASS：`MY_WORLD_CURRENT_STATUS.md`。

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
Database row/table != automatic business owner
Source Asset != current Game World
Main Menu != Game truth
Source Library != Game Library
Application Lifetime != Game Session Lifetime
```

---

## 2. 技术基线

```text
Host             Godot 4.7.2
Distribution     Standard / non-.NET Windows x64
Language         GDScript
Runtime          Godot same-process Runtime
Provider         DeepSeek deepseek-v4-pro
Source           JSON manifest + package-local files where appropriate
Persistence      SQLite via 2shady4u/godot-sqlite v4.9
Game topology    One Game = One SQLite
Source Library   managed immutable filesystem generations
```

继续保持：

- Domain 不依赖 Scene / Node / Resource 生命周期；
- Provider Adapter 极薄；
- Persistence storage 与业务 Domain / UI 分离；
- UI、Transcript、Prompt、Cache、Markdown、Godot Resource 不自动成为 authoritative gameplay truth；
- 不建设 IPC、通用 ORM、DI/EventBus 或 persistence framework forest。

---

## 3. Canonical Ownership

```text
Primary Source Assets
World Pack / Character Card / Expansion Pack
→ reusable authored pre-game source + exact immutable generations

Managed Source Library
→ installed Source inventory / validation / immutable generation retention

Game Creation Composition
→ one New Game's explicit exact selections + Entry/T0 + roles + settings

Application Lifecycle
→ Main Menu / Game Session open-close-switch / application quit

Game Domain
→ Game identity / current lifecycle / game-local provenance

World / Character / other gameplay Domains
→ game-local authoritative lived reality

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

Derived / Snapshot / Cache / Transcript / Prompt 默认不可反向成为第二 live truth。

---

## 4. Primary Source Architecture｜v0.2-r2

第一代正式主资产：

```text
Primary Source Assets
├─ World Pack
├─ Character Card
└─ Expansion Pack
```

三类资产只共享最薄 identity / exact-generation seam；不得为了统一而建立巨大 universal asset schema。

正式长期分层：

```text
Reusable immutable Source
↓ exact selection + selected Entry/T0
T0-scoped Source Projection
↓ Final Create
Game-local Canonical Reality
↓ current execution
Runtime State
```

核心不变量：

> **Source defines the starting reference; game-local reality owns lived history.**
>
> **Source provides inertia; actors create history.**

### 4.1 Rich semantic sections

World / Character v0.2-r2 采用：

```text
thin identity / catalog metadata
+ ordered rich semantic_sections
+ disclosure = gm_reference | gm_private
+ package-local UTF-8 Markdown/TXT content files
+ exact fingerprint over every declared package byte
```

`section_type` 是开放 semantic hint，不是封闭 ontology。

真实长篇 World / Character prose、表格、技能/法术/行为/关系/知识边界可以继续作为 first-class authored Source bytes；不得为了 parser 便利压缩成 `summary/traits/background/drives`，也不得反向造“万能 78 字段 Character schema”。

### 4.2 World Pack

World Pack 可以拥有：

- identity / version / schema version / display metadata；
- `catalog_summary`；
- compact world / GM instructions；
- top-level always-safe rich semantic sections；
- `entries[]`，每个 Entry 可拥有自己的 rich semantic sections；
- authored asset declarations。

World selected projection：

```text
top-level always-safe semantic sections
+
exact selected Entry semantic sections
```

其它 Entry 的 bytes 仍属于 exact generation / fingerprint，但不因此成为当前 Game 的普通 Runtime 可见内容。

### 4.3 Character Card

Character Card 是 reusable Character Source，不是“玩家角色专用卡”。

可拥有：

- identity / version / display metadata；
- `catalog_summary`；
- top-level always-safe rich semantic sections；
- zero or more `t0_profiles[]`；
- optional portrait；
- explicit `player_character_supported`。

每个 T0 profile 可以显式绑定一个或多个 `(world_asset_id, entry_id)`，并拥有自己的 rich semantic sections。

Character selected projection：

```text
top-level always-safe semantic sections
+
exact matching T0 profile semantic sections
```

禁止：

```text
latest profile fallback
nearest-year fallback
later-profile fallback
complete-life biography fallback
same-family guessing
```

如果 Character 对某 World 已声明 profile coverage，则：

```text
exact Entry binding exists → temporally compatible
selected Entry binding missing → hard temporal incompatibility
```

如果对该 World 完全没有 profile coverage，则不得因为“不同家族”自动 hard-block；只保留可区分的 no-exact-profile / always-safe-only 状态，让后续真实 Compatibility consumer 决定产品呈现。

### 4.4 Fixed-T0 multiple Entry binding

同一个 T0 profile 可以绑定同 T0 的多个 World Entry。

Binding 表示 authored starting compatibility，不表示：

- current location；
- opening appearance；
- current relationship；
- recommended-scene score；
- 当前契约/任务。

### 4.5 Disclosure != Knowledge

`gm_reference` = 普通 GM-facing authored reference。  
`gm_private` = explicit backstage/secret Source truth。

二者都不自动等于 player-known / Character-known。

Character knowledge 必须由 Game-local evidence / Knowledge ownership 产生，而不是从 Source disclosure 推导。

---

## 5. T0-scoped Source / Post-T0 Canon Quarantine

正式不变量：

> **Do not show the model a post-T0 answer and then ask it to forget that answer.**

必须区分：

```text
Immutable Source Package Total Content
!= Selected T0 Source Projection
!= Game-local Canonical Reality
!= Runtime Relevant Set
!= Model-visible Working Set
```

T0 quarantine 是**信息 authority / Context boundary**，不是 Narrative state machine。

它隔离 post-T0 authored future answer，但必须保留截至 T0 已真实形成的：

- 人格惯性与矛盾；
- 已发生经历及影响；
- 能力、局限、专业风格；
- 关系历史与社会位置；
- 当时合理知识与来源；
- 制度、资源、地理、文化与冲突压力；
- 开放目标、风险与 deliberate blanks。

> **Quarantine future answers; preserve present depth.**

不增加 convergence force，也不增加 divergence force。当前因果自然重现原历史是允许的；关键前提改变后，原历史没有特殊收敛权重。

Canonical decision：

`architecture/source/G4_T0_SCOPED_SOURCE_AND_POST_T0_CANON_QUARANTINE_DECISION.md`

---

## 6. Managed Source Library / Immutable Generation

```text
external/local source package
→ validate
→ staged verified publish
→ immutable generation
→ Source Library inventory
→ New Game exact selection
```

必须长期区分：

```text
stable Source identity
!= exact immutable generation
```

Exact generation fingerprint 覆盖所有 declared bytes，包括：

- canonical manifest；
- top-level rich section files；
- 所有 Entry-scoped section files；
- 所有 Character T0 profile section files；
- authored assets；
- optional portrait when declared。

正式不变量：

> **Fingerprint coverage != Runtime visibility.**

一个未选 Entry、later profile 或 `gm_private` 文件即使不进入当前 projection，bytes 变化仍必须改变 generation fingerprint。

已有 Game pins exact Source generation；Source update 不得静默改变旧 Game。

第一代 New Game UI 不提供 historical generation picker；Library 可内部保留历史 generation 以服务旧 Game exact resolution。

---

## 7. Game Creation Composition

第一代固定 asset-only composition：

```text
Exactly 1 World Pack exact generation
+ Entry/T0 0..1
+ Expansion 0..N (G4-05 current honest implementation remains none-only)
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

默认 Light。

Selection authority：

> **Chooser open / focus / list visibility != selection.**

只有显式点击具体 Source item 才写 authoritative Composition，并 pin exact generation。

World 改变时必须清除 dependent Entry。Player Character 必须满足 explicit eligibility；同一 exact Character 不得同时作为 Player + Guaranteed NPC。

Guaranteed NPC 只表示 Final Create 后属于本局 canonical cast；不自动表示 opening appearance、same scene、player knowledge、relationship 或 automatic current Context inclusion。

---

## 8. Atomic Final Create / Materialization Boundary

```text
Source Library inventory
↓ explicit exact selection
Game Creation Composition
↓ Compatibility Review
Atomic Final Create
↓
Game-local Canonical Reality
↓
Runtime State
```

Final Create 必须是 Program-owned durable transaction flow，不允许 Wizard 每一步直接创建/改写 Game。

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

G4-06 才实现这一边界；G4-02R1M1 不提前做 materialization。

---

## 9. Game-local Evolvable Semantics

正式原则：

> **Source schema is not the possibility ceiling of the Living World. Game-local semantic structure is evolvable.**

Final Create 之后：

```text
exact immutable Source ancestry
+
stable Program-owned game-local kernel
+
evolvable Game-local semantics
```

Program-owned kernel 至少概念上保护：

- game-local stable identity；
- object/entity type；
- source provenance / exact generation ancestry when source-backed；
- lifecycle / timeline ownership；
- durable mutation identity / integrity。

模型/Runtime 可以让本局产生 Source 未预见的新长期语义，例如新的誓言、创伤、政治伦理、学派、制度或其它真正由 lived history 产生的 meaning。

但：

- 不修改原始 Source；
- 不修改 global Source contract；
- 不允许模型 `ALTER TABLE` 或创建任意 production schema；
- 已有 Location / Relationship / Knowledge / Injury / Inventory / Faction / Timeline 等 canonical Domain 时，必须使用对应 Domain，不创建 duplicate generic truth；
- local semantic evolution 必须 durable、Save/Restore/Timeline reversible。

Canonical decision：

`architecture/source/G4_GAME_LOCAL_EVOLVABLE_SEMANTICS_DECISION.md`

---

## 10. Runtime-generated Reality / Knowledge Boundary

Runtime-created NPC / Place / Item / Institution / Event 可以只有：

```text
game-local stable identity
provenance = runtime_generated
```

不要求伪造 Source ancestry。

必须长期区分：

```text
Source exists
!= Game-local entity exists
!= Player knows entity
!= Runtime relevant
!= Model visible
```

Guaranteed NPC 被建局 materialize 也不自动成为玩家通讯录成员。

Knowledge / Encounter owner 决定玩家何时知道什么；UI 只投影 player-safe knowledge。

---

## 11. Application / Game Session Lifecycle

G4-01 已正式建立：

```text
Application Lifetime
!= Game Session Lifetime
```

目标：

```text
Launch EXE
→ Application READY / Main Menu

Continue or Select Game
→ open exact Game Session
→ enter in-game UI

Return to Main Menu
→ safely finish/cancel generation
→ flush/close Game-owned resources
→ release Game-specific writer/connection ownership
→ Application remains READY
```

Main Menu 不能只是盖在自动打开的 current Game 上。

---

## 12. Multi-Game / Game Library｜G4-04 FROZEN

G4-04 已正式裁定：

> **One Game = One SQLite.**

Managed Game path：

`user://my-world/games/<game_id>/game.sqlite`

Historical G3 Game path：

`user://my-world/current-game.sqlite`

Game Library / Application index 只拥有 application-level metadata / discovery，不拥有 gameplay truth。

必须保持：

```text
Application index metadata != gameplay truth
Application Lifetime != Game Session Lifetime
Game A DB != Game B DB
```

Existing-only open 必须做 Game identity cross-check；current Game commit 只能在 Runtime ready 后发生；切换 Game 时先关闭 A 再打开 B；legacy Game 可被原地 adoption，不要求复制进 managed path。

G4-04 已 PASS / CLOSED；Source v0.2-r2 不得重新打开这项 topology 决策。

Canonical decision：

`architecture/persistence/G4-04_MULTI_GAME_STORAGE_TOPOLOGY_DECISION.md`

---

## 13. Model / Runtime / Persistence Boundary

> **Model freedom first. Reversibility over prevention.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

模型可以广泛 author Narrative、NPC 行为/动机、事件、新实体、新语义与后果。

Runtime 强约束集中在：

- stable identity；
- permission / authority；
- atomic durability；
- Save / Restore / Timeline；
- filesystem/database integrity；
- secrets / OS authority；
- crash/retry/recovery correctness。

Runtime 不应膨胀成 Narrative 审查委员会。

---

## 14. Context Architecture

必须保持：

```text
Managed Source Library
!= Game Selected Source Set
!= T0-scoped Source Projection
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

G4-02R1M1 只需要提供安全的 exact selected Source projection seam；不在该任务中预造完整通用 Context Retrieval platform。

G4 First Playable A 必须证明 rich World + Character T0 projection 实际形成具体、非贫血的 GM Setup Context；G5 再继续拉出更完整 world relevance / knowledge / long-lived semantic retrieval。

---

## 15. Expansion Capability Layers

Expansion 第一代数量仍是 `0..N`，但 G4-05 当前 honest implementation 是 none-only，真实 Expansion contract/runtime 在 G4-08 才进入。

演化顺序：

```text
G4-08
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

不允许为了未来 Mod/Creator 在当前 G4 提前造 universal protocol / arbitrary UI schema。

---

## 16. Save / Timeline / Reversibility

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

Game-local semantic evolution同样必须进入 Timeline/Save/Restore，而不是游离在其外。

---

## 17. UI Host Architecture

Application-level surface：

```text
Main Menu
New Game Wizard
Game Library
future Settings / authoring entry
```

拥有 navigation / lifecycle intent，不拥有 gameplay truth。

In-game surface：

```text
Player Host | Narrative Host | World Surface Host
```

演化顺序：

```text
G2 fixed in-game Godot UI
→ G3 persistence/recovery integrated UI
→ G4 Application / New Game / Game Library
→ G5 real World projection
→ G6 Internal Declarative UI Host
→ G8 external declarative contract
```

详细 UI host 设计继续由 `architecture/ui/` supporting docs 拥有。

---

## 18. G4 Current Execution Order

```text
G4-01 Application Shell / Main Menu + Game Session Lifecycle
  PASS / CLOSED
↓
G4-02 original World/Character v0.1 engineering
  HISTORICAL PASS
↓
G4-02R1 semantic re-audit + v0.2-r2 real-asset full-fidelity correction
  SEMANTIC/FIDELITY PASS / FROZEN
↓
G4-02R1M1 Source v0.2-r2 mechanism correction
  CURRENT — CODEX
↓
GPT Independent Review
↓
G4-05 Asset-only New Game Wizard
  resume closure only if mechanism/fidelity regressions PASS
↓
G4-06 Atomic Final Create + World/Character Materialization
↓
G4-07 First Playable A — World + Character Owner UAT
↓
G4-08 Expansion Pack v0.1 + First Real Runtime Vertical
↓
G4-09 First Playable B — Expansion Owner UAT
↓
G4-10 Runtime Asset Resolution
↓
G4-11 Two Primary Asset Families Reality Test
↓
G4-GATE
```

G4-03 Managed Source Library 与 G4-04 Multi-Game/Game Library 已 **PASS / CLOSED**，不是被跳过；它们的已验证工程继续作为当前纵向基础。

Current Task 只看 `MY_WORLD_CURRENT_STATUS.md`。

---

## 19. 业务模块内部 L3 → L0

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

当前 Source mechanism 应 repair/extend existing Source layers，而不是另造 parallel loader stack。

---

## 20. 专题导航

```text
architecture/
├─ foundation/
│  └─ Foundation架构决策_v1.0_2026-08-26.md
├─ persistence/
│  ├─ 时间线存档与可逆性设计.md
│  └─ G4-04_MULTI_GAME_STORAGE_TOPOLOGY_DECISION.md
├─ source/
│  ├─ G4_SOURCE_SEMANTIC_OWNERSHIP_AND_REAUDIT_DECISION.md
│  ├─ G4_GAME_LOCAL_EVOLVABLE_SEMANTICS_DECISION.md
│  └─ G4_T0_SCOPED_SOURCE_AND_POST_T0_CANON_QUARANTINE_DECISION.md
└─ ui/
   └─ 声明式UIHost设计.md
```

Implementation-repo Source contract / real-fixture authority：

```text
my-world/docs/source/World Pack与Character Card合同v0.2_SEMANTIC_FREEZE.md
my-world/docs/source/G4-02R1_T0_SCOPED_SOURCE_CONTRACT_ADDENDUM.md
my-world/docs/source/G4-02R1_T0_CHARACTER_INDIVIDUALITY_ADDENDUM.md
my-world/docs/source/G4-02R1_FIXED_T0_MULTI_ENTRY_CHARACTER_BINDING_CLARIFICATION.md
my-world/docs/source/G4-02R1_REAL_ASSET_V0_2_R2_MIGRATION_SPEC.md
my-world/docs/source/G4-02R1_HAN_FAMILY_JOINT_FULL_FIDELITY_AUDIT.md
my-world/docs/source/G4-02R1_AFTERGLOW_FAMILY_JOINT_FULL_FIDELITY_AUDIT.md
my-world/docs/source/G4-02R1_CROSS_FAMILY_PACKAGE_SHAPE_STABILITY_DECISION.md
```

新架构事实先更新本 Map；需要详细 trade-off / contract / migration / evidence 时再更新 supporting doc。历史版本依赖 Git history / archive，不并列多个 current。
