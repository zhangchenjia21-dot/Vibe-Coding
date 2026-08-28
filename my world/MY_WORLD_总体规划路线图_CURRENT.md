---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 3.0
created: 2026-08-25
updated: 2026-08-28
current_phase: G4
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜总体规划路线图 CURRENT

## 0. 文档职责

本文件只拥有：

- G1–G9 阶段顺序；
- 每阶段核心 Outcome；
- 主要 Task DAG；
- Stage Gate；
- 哪些能力必须延后；
- 每个阶段为什么按这个顺序做。

不重复维护：

- 产品定义：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 系统架构与专题导航：`MY_WORLD_架构_CURRENT.md`
- 当前 Task / PASS / UAT：`MY_WORLD_CURRENT_STATUS.md`
- 可供未来项目复用的开发路径方法论：`experience/AI_RPG开发路径与阶段设计经验_v1.0_2026-08-28.md`

总原则：

> **先跑通真实核心循环，再扩展外围能力。**
>
> **先建立玩家产品入口，再建立内容选择。**
>
> **先证明 World + Character 能创建并开始一局，再加入 Expansion。**
>
> **真实需求 → 最小能力 → 第一消费者 → Owner UAT → 第二消费者 → 再抽象协议。**

---

## 1. 总体关键路径

```text
G1  Foundation & Project Bootstrap
↓
G2  AI Conversation Spine
↓
G3  Persistent Game / Save / Timeline Foundation
↓
G4  Primary Source Assets & Local Game Creation
↓
G5  World Semantics & GM Runtime
↓
G6  RPG Experience & Internal Declarative UI Host
↓
G7  Long-session Context & Performance
↓
G8  Mod / Authoring & External Declarative UI Contract
↓
G9  Standalone Alpha / Release Validation
```

第一条真正产品脊柱：

```text
启动应用
→ Main Menu
→ Continue / Asset-only New Game
→ 进入 AI GM 自然语言互动
→ 世界产生 durable change
→ 退出 / 重开仍是同一 Game
→ 明确 Save
→ 继续产生未来
→ 明确 Load / Restore
→ 世界 + Context 一致恢复
→ 被回滚未来不泄漏
→ 继续新的当前进度
```

任意历史 Turn 一键 Rewind **不是第一代关键路径**。

---

# G1｜Foundation & Project Bootstrap

## Outcome

证明 Godot 4.7.2 能作为独立 Host，并冻结第一代最小技术边界。

## Result

`G1-01 ... G1-06` 与 `G1-GATE`：**PASS / CLOSED**。

已证明 Windows-local Godot/Git、中文长文本/输入、真实 Provider streaming/cancel、UI 非冻结、本地 IO、filesystem images、Windows export/exported EXE，以及第一代 Godot/GDScript/same-process 技术选择。

历史经验：Foundation 只证明真正影响产品纵向的能力；不要把 Foundation 阶段变成通用 App Framework 建设。

---

# G2｜AI Conversation Spine

## Outcome

建立第一个真正值得体验的 AI RPG Narrative 主循环：

```text
打开游戏
→ 输入自然语言
→ AI GM 真 streaming Narrative
→ 连续多回合
→ Cancel / Regenerate / Retry
→ Provider failure 后继续
```

## Tasks

### G2-01｜Application / Game Shell

正式应用入口与生命周期的第一代壳。

### G2-02｜Provider Adapter v0.1

DeepSeek `deepseek-v4-pro` 的薄 `stream / cancel / completion / failure` seam；不做 Provider platform。

### G2-03｜Narrative Conversation View + Host Slots

Player natural-language input、GM Narrative、real streaming、cancel、regenerate/retry、error recovery、长篇阅读友好的中央 Narrative，以及 `Player Host | Narrative Host | World Surface Host` 稳定布局。

### G2-04｜Turn / Conversation Domain v0.1

冻结 Player Turn、GM Turn、Conversation Entry、Generation State，以及 Retry / Regenerate / latest-turn correction 的最小正式语义。Transcript 不等于 Timeline。

### G2-05｜Context Assembly v0.1

system/GM instructions + 当前 Conversation working set + 当前最小 game context；不做复杂 retrieval/long-memory platform。

### G2-06｜第一轮 Owner Playtest

连续真实游玩，与 The World / DSH 简单基线比较 Narrative、自由度、交互成本与“是否想继续玩”。

## G2-GATE

Engineering：多回合、stream/cancel/retry、failure recovery、UI response 可靠。

Product Value：作为 AI RPG 对话核心，已经值得继续玩，而不是工程 demo。

---

# G3｜Persistent Game / Save / Timeline Foundation

## Outcome

建立长期世界的 durable backbone，让 Save / Load / Restore 成为可靠原生能力，同时保证未来记忆隔离。

必须区分：

```text
Game
World State
Timeline
Save Point
Conversation
Agent Context
UI Preference
```

核心产品边界：

> **Save Point != Timeline Node.**
>
> **Reversibility != frictionless arbitrary rewind.**

## Result

`G3-01 ... G3-07` 与 `G3-GATE`：**PASS / CLOSED**。

已证明 SQLite authoritative persistence、atomic durable mutation、accepted Conversation durability、reopen/resume、named Save、atomic Load/Restore、future-memory isolation、Recovery Checkpoint、single-writer、verified physical backup、staged corruption recovery 与真实 Provider continuation。

历史经验：Persistence 必须先把 current Game 语义做稳，再在真正出现多个 Game 的产品需求时升级 multi-Game；不要一开始同时造 Game Library、Cloud、Timeline browser 和 Backup browser。

---

# G4｜Primary Source Assets & Local Game Creation

## Outcome

让产品从“只有一个自动打开的 Game”升级为真正的**本地多世界 AI RPG Host**，并冻结第一代唯一 New Game 路径：

```text
Managed Source Library
→ World Pack + Character Card + optional Expansion Pack
→ explicit Game Creation Composition
→ atomic Final Create
→ independent Game-local Reality
→ real AI GM play
```

第一代**不支持无 World Pack / 无 Character Card / 空白 AI 世界直接建局**。这不是长期否定，而是早期产品收缩：先把正式资产组合建局做到可靠、可理解、可恢复。

### G4 核心对象

```text
Primary Source Assets
├─ World Pack
├─ Character Card
└─ Expansion Pack

Managed Source Library
!= Game Library

Source Generation
!= Game-local Reality
!= Runtime State
```

### 第一代 Character Card 用途

```text
Exactly 1 Player Character Card
0..N Guaranteed NPC Character Cards
```

Guaranteed NPC 的含义是“从建局开始属于本局 canonical cast”，不是“第一幕必须出现”，也不是“玩家已经认识”。

### 第一代 Expansion 数量

`0..N`。Expansion 选择步骤存在，但允许玩家明确选择 none。

---

## G4-01｜Application Shell / Main Menu + Game Session Lifecycle

### 目标

不只是画一个主菜单，而是正式拆开：

```text
Application Lifetime
!=
Game Session Lifetime
```

产品路径：

```text
Application Launch
→ Main Menu READY
├─ Continue
├─ New Game
└─ Quit

Continue
→ open selected/current Game Session

Return to Main Menu
→ close/cleanup Game Session
→ Application remains READY
```

### 验收重点

- 启动不再无条件进入 Game；
- Continue 复用 G3 reopen/resume truth；
- in-game → Main Menu → Continue 正确；
- corruption / safe-backup recovery 仍明显可发现；
- 不在 Main Menu 维护第二份 Game truth；
- Windows Maximized / 1280×720 / 960×540 实测；
- product-facing Owner UAT。

### 非范围

不做真实 Source selector、多 Game storage、Settings framework、商店、云服务。

---

## G4-02｜World Pack + Character Card Source Contracts v0.1

### 目标

先建立两类相对低复杂度、以文字/设定为主的正式 Source contract，并用两个差异世界做 Reality Check。

### Shared identity seam

三类 Primary Source 最终共享最薄 identity：

```text
asset_id
asset_type
version
exact immutable generation / content fingerprint
```

但 World / Character / Expansion 各自拥有自己的语义合同，不建立万能 `AssetV1` 巨大 Schema。

### World Pack v0.1

至少表达：

- metadata / identity / schema version；
- world / GM instructions；
- ordered Source Lore；
- `0..N` lightweight Entry / T0 seed；
- authored portrait / scene / map declarations；
- 必要的 World Source material。

Entry 不是 Beat graph / Scenario DSL。

### Character Card v0.1

至少表达：

- stable identity / display identity；
- public profile；
- GM/private Source profile；
- portrait reference；
- `player_character_supported` 或等价 eligibility；
- 不包含 live location / current relationship / current injury / current knowledge 等 Runtime state。

### Contract Reality Check

使用历史/低魔型与高魔/幻想型 compact Source 真实编写并加载。若 v0.1 设计错误，在未发布阶段直接修，不建兼容层森林。

---

## G4-03｜Managed Local Source Library v0.1

### 目标

New Game Wizard 只能消费正式本地 Source，因此 Library 必须先于 Wizard 成立。

### 必须证明

```text
install / discover
→ validate
→ publish immutable Source generation into managed local library
→ inventory current install
```

Source Library 必须保留：

```text
stable identity
+ version
+ exact immutable generation
```

已有 Game pin 的旧 generation 不能因安装新版本被覆盖或删除。

第一代 New Game UI 默认只展示当前安装版本，不提供历史版本 picker；Program 在选择时 pin exact generation。

Draft / arbitrary external file / Creator working copy 不能直接成为 Final Game Source。

---

## G4-04｜Multi-Game Lifecycle / Game Library Foundation

### 目标

正式结束 G3 的 one-current-Game 产品约束。

### 必须解决

- 多个独立 Game 共存；
- active/open Game selection；
- Continue / switch；
- New Game 不覆盖已有 Game；
- G3 legacy current Game 的安全 adoption/migration；
- single-writer、backup、corruption recovery 在 multi-Game 形态下继续成立；
- Game Library 与 Source Library 分离。

### 设计 Gate

任务开始前专项比较：

```text
每 Game 一个 SQLite
vs
共享 SQLite + game_id
```

只选择最简单、可靠、能继承 G3 证据的方案，不为了理论规模预建服务层。

---

## G4-05｜Asset-only New Game Wizard v0.1

### 第一代固定流程

```text
New Game
→ World Pack: Exactly 1
→ Entry/T0: 0..1 selected from chosen World
→ Expansion Pack: 0..N；第一轮可 none
→ Player Character Card: Exactly 1
→ Guaranteed NPC Character Cards: 0..N
→ Minimal Settings
→ Compatibility Review
→ Final Create
```

### Minimal Settings

至少包括：

- Game display name；
- Protagonist Control Mode：Full / Light / Narrative；
- optional opening supplement。

默认推荐 Light Delegation。

### Selection authority

必须冻结：

> **打开 chooser / mode != authoritative selection。只有玩家点击具体资产才构成选择。**

Final Review 至少显示：

- exact World；
- selected Entry；
- exact Player Character；
- selected Guaranteed NPC Characters；
- Expansion none / selected set；
- control mode；
- 其它最小本局参数。

第一代不做 Source 历史版本 chooser、复杂 Expansion feature/module toggle tree、World-specific arbitrary form DSL。

---

## G4-06｜Atomic Final Create + World/Character Game-local Materialization

### 目标

Final Create 是一次明确、可恢复、可重放的产品事务，而不是 Wizard 各步骤边选边改 Game。

### 必须证明

```text
exact Composition
→ Program-derived create identity / fingerprint
→ persist creating intent
→ create independent Game
→ pin exact Source generations
→ materialize World + Character game-local definitions
→ establish Setup Context ancestry
→ created
```

要求：

- Final Create Provider calls = 0；
- double click / response loss 不创建第二个 Game；
- exact same create request 可安全 replay same Game；
- mismatched intent fail closed；
- Source 更新不改变已创建 Game；
- Runtime 不反写 Source Library。

### Character materialization

- Player Character → exactly one game-local player identity；
- selected Guaranteed NPC → game-local canonical Character definition；
- Guaranteed NPC 不自动成为 player-known，不自动 placement 到 opening scene。

---

## G4-07｜First Playable A：World + Character

### 目的

在引入真实 Expansion 前，先证明资产建局核心产品纵向成立。

### Reality Path

```text
真实 World Pack
+ Exactly 1 real Player Character Card
+ 0..N real Guaranteed NPC Character Cards
+ Expansion = none
→ Main Menu / New Game
→ Compatibility Review
→ Final Create
→ real DeepSeek Opening
→ continuous free play
→ Save
→ exit / reopen / Continue
→ Owner UAT A
```

### Gate question

> **只凭正式 World + Character，我们能不能可靠创建并开始一局真正想玩的游戏？**

如果这一关失败，先修建局、Source、Context 或 Opening；不要同时把 Expansion 复杂性压进来。

---

## G4-08｜Expansion Pack v0.1 + First Real Runtime Vertical

### 目标

在 First Playable A 已成立的基础上加入第三类 Primary Source。

### Expansion Source v0.1

最少表达：

- stable identity / exact generation；
- bounded mechanic / GM instructions；
- optional config；
- dependencies；
- required Program capability / runtime binding declaration。

### 第一代边界

- `0..N`；
- 不做任意 GDScript / DLL / shell code execution；
- 不做复杂 feature/module UI 勾选树；
- 不做 generic external UI contribution schema；
- Publish / install != Runtime activation；
- Source module identity != Program runtime capability identity。

### 真实 Runtime effect

必须至少证明：

```text
Expansion selected
→ exact binding
→ formal Runtime / Context / mechanic behavior actually changes
```

不接受 `manifest/binding exists` 就宣称 Expansion 已工作。

---

## G4-09｜First Playable B：Add Real Expansion

### Reality Path

```text
已通过的 World + Character 组合
+ 1 real Expansion
→ New Game
→ exact binding
→ real DeepSeek play
→ observable Expansion effect
→ Save / reopen / Continue
→ Owner UAT B
```

### Gate question

> **拓展包是否真的给已经成立的游戏增加玩法，而不是只增加一条数据库记录？**

完整机制专用 UI 留给 G6；外部 Mod UI contract 留给 G8。

---

## G4-10｜Runtime Asset Resolution

### 目标

让 portrait / scene / authored map 不再写死于核心工程，并且绑定 exact Source generation。

必须覆盖：

- type-safe local path resolution；
- pack-root escape 防护；
- missing/fallback；
- Windows filesystem / export 路径；
- Godot real load；
- old Game pin old generation asset；
- Source update 不静默替换旧 Game 的视觉资产。

不升级成完整地图 topology / travel / GIS / procedural map system。

---

## G4-11｜Two Primary Asset Families Reality Test

### 目的

证明前面的设计不是第一个世界的特例。

至少两组差异明显 Primary Source：

```text
Family A：历史 / 低魔
World + Character + optional Expansion

Family B：高魔 / 幻想
World + Character + optional Expansion
```

分别完成：

```text
Main Menu
→ New Game
→ exact Source selection
→ independent Game
→ real DeepSeek
→ durable progression
→ Save / reopen / switch
→ correct portrait / scene / map generation
→ Source newer generation installed
→ existing Game unchanged
```

Owner 必须能明显感到进入的是两个不同世界，而不是只看到两个不同 `asset_id`。

---

## G4-GATE

至少要求：

```text
Application / Game Session 生命周期分离
+
Managed Source Library
+
World Pack + Character Card 正式 Source
+
0..N Expansion Pack 正式 Source
+
asset-only New Game Wizard
+
Multi-Game Library
+
Atomic Final Create / exact provenance
+
World + Character Owner UAT A PASS
+
Expansion Owner UAT B PASS
+
Runtime asset resolution
+
Two-family real Provider proof
+
Owner UAT PASS
```

G4 不要求 Creator、Reference Library、Opening Scenario Runtime、Map gameplay engine、任意外部 UI 插件、在线内容商店或无资产建局。

---

# G5｜World Semantics & GM Runtime

## Outcome

让 G4 创建出的资产世界真正“活起来”，同时保持模型创造力和有限工程复杂度。

## G5-01｜Minimum Playable T0 + World Turn / Semantic Materialization

G4 只需证明真实 AI Opening 能消费正确 Source/Composition；G5 开始负责把需要长期存在的新 NPC / Place / Item / Event 等变成 authoritative game-local reality。

必须避免旧项目的“Creation 成功但世界只有一个空 Scene”的失败：第一次真正进入世界时应有足够具体的 playable situation，而 Narrative 不应长期依赖 phantom interactables。

### 其余 Tasks

- G5-02 Knowledge Provenance：`World Truth != NPC Knowledge != Player Knowledge`；
- G5-03 NPC / Faction Agency：重要行动者拥有独立目标、风险、义务与 next move；
- G5-04 Event / Priority-driven World Evolution：不做每 NPC 每 tick；
- G5-05 Meaningful Choice / Mechanics Integration：骰子处理真正不确定性；
- G5-06 Runtime → UI Projection；
- G5-07 World Product Tests：Player Absence / Counterfactual Propagation / Independent Actor，防止 Protagonist Causal Monopoly。

## G5-GATE

玩家不再是唯一因果源；世界有选择性自主演化；G4 selected Guaranteed NPC 可以成为真正行动者；Expansion 的机制语义进入正式世界，而没有发展成全宇宙模拟器。

---

# G6｜RPG Experience & Internal Declarative UI Host

## Outcome

把已成立的 Runtime Truth 做成真正的 RPG 产品界面，并证明内部声明式 Host 能力。

## Tasks

- 三栏 RPG Experience 完整化；
- Player portrait/status/detail；
- Character / Relationship / Inventory / Faction / Map / Save 等真实 Surface；
- scene/portrait/map presentation；
- Expansion mechanic state 的真实 UI consumer；
- Internal Declarative UI Host v0.1；
- 小型 safe vocabulary：section/card/badge/meter/status_list/fact_list/action_list 等，仅按真实需求增长；
- Runtime projection → ViewModel → Host → Godot Control；
- bounded Action Intent；
- responsive / Theme / navigation；
- Owner UAT 与视觉 polish。

## G6-GATE

UI 明显增强游戏理解与沉浸；至少一个真实 Expansion/UI consumer 证明 Internal Host 是被需求拉出来的，而不是预造平台。

---

# G7｜Long-session Context & Performance

## Outcome

长局持续增长时，模型 working set、UI responsiveness 和 background work 仍可控。

## Tasks

- bounded Context Assembly；
- relevant subgraph / working set selection；
- deterministic background progression 与 model work 分离；
- TTFT / throughput / context size / persistence latency 的真实 long-play evidence；
- Source Library 大、Game history 长时仍不把全部资产塞入 Prompt；
- long-session recovery/performance test。

## G7-GATE

`Game State / History ↑↑↑` 时 ordinary Turn Context 不线性爆炸，游戏仍可玩，且 Context bounded 不等于 Narrative starved。

---

# G8｜Mod / Authoring & External Declarative UI Contract

## Outcome

只在 G4/G6 已证明内部能力后，把内容与 Host capability 外部化给作者。

顺序：

```text
proven internal capability
→ external schema / naming
→ validator / adapter
→ Creator / authoring helper / preview
```

## Tasks

- World Pack Creator；
- Character Card Creator；
- Expansion Pack Creator；
- Draft / Published Source 分离；
- Import / revision / append-only publish；
- AI-assisted authoring：typed scope / visible ChangeSet / Undo；
- external UI contribution schema；
- Surface ownership/dependency；
- validation/migration；
- second/third real authored asset proof。

复杂任意代码沙箱仍默认 Deferred。

## G8-GATE

作者能创作和发布正式 Source，Mod 能扩展内容和受控 UI，而不会获得任意 Runtime/OS 权限或制造第二事实源。

---

# G9｜Standalone Alpha / Release Validation

## Outcome

证明第一代可以作为独立长期 AI RPG 使用，而不是只通过单次演示。

覆盖：Windows build/startup/upgrade、long-play stability、Save/Load/Recovery、Context/performance、Source install/use、multi-Game、Provider failure、UI usability、Product Value UAT，以及与 The World / DSH simple baseline 的真实比较。

到 G9 设计前重新评估是否拆成：

```text
Alpha Stabilization
→ Windows Packaging / Release
```

旧项目曾证明产品 Alpha 与 Windows Release 是两种不同风险；但当前不提前增加阶段编号。

## G9-GATE

> **玩家愿意把它当一个独立 AI RPG 长期玩，而不是一个技术样品。**

---

## 10. 跨阶段开发纪律

每个高价值产品阶段默认采用：

```text
Freshness / historical preflight
→ failure / contract matrix
→ minimal production vertical
→ automated validation
→ real Provider / real Windows evidence when relevant
→ Independent Review
→ Owner UAT when product-facing
→ Decision Propagation
→ next task
```

历史项目已经反复证明以下模式高风险：

- 先造完整平台，再等玩法来消费；
- Creator 先于“使用资产创建游戏”；
- parser/schema PASS 就替代真实建局/真实试玩；
- binding/proof module 存在就宣称机制真正工作；
- Owner UAT 推到阶段最后，导致整段路线返工；
- 用 stable display identity 猜 exact Source version；
- Source 更新静默改变旧 Game；
- 在 first consumer 前同时设计所有未来扩展类型。

详细复用说明见：`experience/AI_RPG开发路径与阶段设计经验_v1.0_2026-08-28.md`。

---

## 11. 文档 / 任务纪律

- 当前短周期状态只更新 `MY_WORLD_CURRENT_STATUS.md`；
- 架构结论只更新 `MY_WORLD_架构_CURRENT.md`，深度从其导航；
- 默认不因新 Stage 新建新的 status 文档；
- 正式 implementation/review 使用 repository-native Task Packet；
- 小 bug / polish 不必强行升级成完整架构事件；
- Product-facing Task 的 Engineering Acceptance 不替代 Owner Product UAT；
- 旧项目中有价值但当前未授权的能力统一进入 `experience/备选开发方向候选池_2026-08-28.md`；
- 已经被 Owner 提升为 CURRENT 的 Character Card / Expansion / Control Mode 不再被候选池描述为“当前不做”。