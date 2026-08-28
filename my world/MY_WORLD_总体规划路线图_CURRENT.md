---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 2.0
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
- 哪些能力必须延后。

不重复维护：

- 产品定义：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 系统架构与专题导航：`MY_WORLD_架构_CURRENT.md`
- 当前 Task / PASS / UAT：`MY_WORLD_CURRENT_STATUS.md`

原则：

> **先跑通真实核心循环，再扩展外围能力。**
>
> **先让玩家有稳定产品入口，再让内容选择进入产品。**

---

## 1. 总体关键路径

```text
G1  Foundation & Project Bootstrap
↓
G2  AI Conversation Spine
↓
G3  Persistent Game / Save / Timeline Foundation
↓
G4  World Pack & Local Content Foundation
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
→ 主菜单
→ Continue 已有 Game / New Game
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

详细历史证据已下沉到 `architecture/foundation/` 与 Git history。

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

正式应用入口与生命周期。状态见 `MY_WORLD_CURRENT_STATUS.md`。

### G2-02｜Provider Adapter v0.1

DeepSeek `deepseek-v4-pro` 的薄 `stream / cancel / completion / failure` seam；不做 Provider platform。

### G2-03｜Narrative Conversation View + Host Slots

第一份真正改变玩家主路径的 UI：Player natural-language input、GM Narrative、real streaming、cancel、regenerate/retry、error recovery、长篇阅读友好的中央 Narrative，以及 `Player Host | Narrative Host | World Surface Host` 稳定布局。

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

## Tasks / Result

`G3-01 ... G3-07` 与 `G3-GATE`：**PASS / CLOSED**。

已证明 SQLite authoritative persistence、atomic durable mutation、accepted Conversation durability、reopen/resume、named Save、atomic Load/Restore、future-memory isolation、Recovery Checkpoint、single-writer、verified physical backup、staged corruption recovery 与真实 Provider continuation。

Arbitrary per-turn rewind / Timeline browser / backup browser 继续 Deferred。

---

# G4｜World Pack & Local Content Foundation

## Outcome

让产品从“只有一个正在自动打开的 Game”升级为真正的**多世界本地 AI RPG Host**：玩家先进入稳定主菜单，可以继续已有 Game 或创建新 Game；新 Game 从可复用 World Pack Source 与明确的建局选择形成独立 game-local reality。

Canonical chain：

```text
Application Main Menu
↓ New Game
World Pack Source Generation
+ Game Creation Composition
↓ materialize
Game-local Canonical Reality
↓ Runtime
Current Game World
```

核心边界：

> **World Pack Source != Game Creation Composition != Game-local Reality != Runtime State.**
>
> **Source 定义开始前的参考世界；游戏开始后，game-local reality 优先。**
>
> **Source provides inertia; actors create history.**

## Tasks

### G4-01｜Product Entry Shell / Main Menu

先建立应用级玩家入口，而不是先做临时 Pack selector。

第一代目标：

- 应用启动进入 Main Menu，而不是无条件直接进入当前 Game；
- `Continue`：继续 G3 已有 current Game，不破坏现有 persistence / recovery 语义；
- `New Game`：进入稳定的 New Game creation surface/host，后续 G4-03 在这里接真实 World Pack / Entry / protagonist 选择；
- `Quit`；
- 游戏内可安全返回 Main Menu，再继续当前 Game；
- existing startup failure / safe-backup recovery 仍然可发现、可用；
- 1280×720、960×540、Maximized Windows 产品路径真实验证。

本任务不实现 Pack discovery、不创建多 Game DB、不冻结 World Pack contract。它只建立**产品导航与生命周期壳**。

### G4-02｜World Pack Source v0.1 + Contract Reality Check

冻结并实现当前真实需要的 reusable Source contract / explicit-root loader：

- metadata / stable identity / author version / schema version；
- world / GM instructions；
- ordered source lore；
- initial character source seeds；
- authored map declaration；
- portrait / scene / map asset declarations；
- necessary mechanic declarations；
- optional `entry_points` / T0 Source seeds：`entry_id + display_name + authored source text`。

Entry Point 只描述“这一局可以从哪里/何种 T0 前提开始”，**不是 Opening Scenario 状态机**。不冻结 year/month/calendar/region/beat/precondition/branch DSL。

G4-02 关闭前必须做 **Contract Reality Check**：用两个 compact、差异明显的 Pack fixture（历史/低魔型 + 高魔型）真实编写并加载，验证 contract 没有强迫不同世界迁就单一早期 Schema。未发布阶段若 v0.1 设计错误，优先直接修正，不建立兼容层森林。

### G4-03｜Game Creation Composition v0.1 + New Game Flow

在 G4-01 的 New Game surface 中建立第一代建局选择语义。

v0.1 只冻结最小必要组合：

```text
selected World Pack exact generation
selected Entry / T0 seed
protagonist seed
```

`selected World Pack exact generation` 至少能唯一表达 pack identity + author version + exact content fingerprint/generation，不能只靠 display name 或文件夹名。

第一代先让玩家完成：

```text
New Game
→ 选择 World Pack
→ 选择 Entry（若 Pack 提供多个）
→ 定义/确认 protagonist seed
→ 创建 Game
```

历史上验证过的世界口径/profile、Expansion/mechanic 组合、主角操控模式等保留为候选，不因旧项目已有就全部塞入 v0.1；只有首个真实 Pack 建局证明必须时才增量加入。

### G4-04｜Source → Game-local Instance

从已确认 Composition 形成独立 Game-local reality。

必须持久化/绑定足够 provenance：

```text
pack_id
pack_version
exact source fingerprint/generation
selected entry
必要的 game-local Source ancestry
```

Source 后续更新不得静默改写已有 Game。剧情后续 runtime-generated NPC / Place / Item 可以只有 game-local identity 与 `runtime_generated` provenance，不要求伪造 Source ID。

Source character 被 materialize **不等于玩家已经认识他**，也不等于自动进入 UI/普通 Context；Player-known semantics 留给 G5/G6 的正式知识/Projection owner。

### G4-05｜Local Pack Library + Minimal Game Library

把 G3 的 one-current-Game 第一代约束升级为真正产品可用的本地 Game lifecycle：

- 本地 Pack discovery / install / load / selection；
- 多个独立 Game 共存；
- Main Menu 能列出/继续已有 Game；
- New Game 不覆盖已有 Game；
- 玩家可以从 Main Menu 在已有 Game 间切换；
- 不做在线商店、账户、云同步。

`每 Game 一个 SQLite` vs `共享 DB + game_id` 等物理形态在本任务开始前以最简单、可靠、可迁移 G3 current Game 的方案专项裁定；不要为了理论扩展性预建服务层。

### G4-06｜Asset Resolution

portrait / scene / authored map 通过 World Pack / game-local provenance 解析并由 Godot 真实加载，不写死核心工程。

本任务只解决资产定位、类型、缺失/fallback、Windows filesystem/export 路径与 UI 消费 seam；不升级成完整地图 gameplay/topology/travel system。

### G4-07｜Two-Pack Playable Reality Test

不再以“第二个 fixture 能 parse”作为 G4 最终证明。

至少使用两个差异明显的真实 compact World Pack：

```text
Pack A：历史/低魔型
Pack B：高魔/幻想型
```

分别完成：

```text
Main Menu
→ New Game
→ Pack / Entry / protagonist 选择
→ 创建独立 Game
→ 真实 DeepSeek 进入该世界继续互动
→ durable progression
→ exit / reopen / Continue
→ 两个 Game 可切换且互不污染
→ Source 修改/升级不静默改变已有 Game
→ portrait / scene / authored map refs 来自正确 Pack
```

G4-07 必须有 Owner UAT。Product success 不是只看到两个不同 `pack_id`，而是玩家能明确感觉“我真的进入了两个不同世界”。

## G4-GATE

G4-GATE 至少要求：

```text
稳定 Main Menu / New Game / Continue
+
两个真实 World Pack
+
两个独立 Game 可长期共存/切换
+
Source exact generation / game-local provenance 清楚
+
Source 更新不静默改写旧 Game
+
真实 Provider 可以在两个世界中继续游玩
+
Owner UAT PASS
```

G4 不要求现在完成 Character Card / Expansion Pack / Reference Library 的完整外部协议，也不要求 Creator、Opening Scenario Runtime、Map gameplay engine 或在线内容商店。

---

# G5｜World Semantics & GM Runtime

## Outcome

让世界真正“活起来”，同时保持模型创造力和有限工程复杂度。

## Tasks

### G5-01｜World Turn / GM Orchestration

```text
Player natural-language intent
+ relevant world context
→ Model / GM authors development
→ Runtime materializes durable consequences where needed
→ Narrative continues
```

### G5-02｜Knowledge Provenance

`World Truth != NPC Knowledge != Player Knowledge` 进入 Context/Projection，但不演化成无限 Narrative censor。

### G5-03｜NPC / Faction Agency

重要行动者拥有独立目标、风险、义务与 next move；`Off-screen != Inactive`。

### G5-04｜Event / Priority-driven World Evolution

按时间、重大事件、高影响 Actor/Faction、Agenda/Front 和因果关联选择需要推进的世界部分；不做每 NPC 每 tick 模型调用。

### G5-05｜Meaningful Choice / Mechanics Integration

骰子处理真正不确定性；身份、已知事实、人格底线与明显不可能性不能被万能 d20 抹平。

### G5-06｜Runtime → UI Projection

Character / Relationship / Item / Faction / World 等真实数据进入 Host Slots，UI 只读 player-safe projection。

### G5-07｜World Product Tests

至少包括 Player Absence、Counterfactual Propagation、Independent Actor 等测试，防止 **Protagonist Causal Monopoly**。

## G5-GATE

玩家不再是唯一因果源；世界有选择性自主演化且没有发展成全宇宙模拟器。

---

# G6｜RPG Experience & Internal Declarative UI Host

## Outcome

把已成立的 Runtime Truth 做成真正的 RPG 产品界面，并证明内部声明式 Host 能力。

## Tasks

- 三栏 RPG Experience 完整化；
- Player portrait/status/detail；
- Character / Relationship / Inventory / Faction / Map / Save 等真实 Surface；
- scene/portrait/map presentation；
- Internal Declarative UI Host v0.1；
- 小型 safe vocabulary：section/card/badge/meter/status_list/fact_list/action_list 等，仅按真实需求增长；
- Runtime projection → ViewModel → Host → Godot Control；
- bounded Action Intent；
- responsive / Theme / navigation；
- Owner UAT 与视觉 polish。

## G6-GATE

UI 明显增强游戏理解与沉浸；声明式能力改善可扩展性但没有让产品变成工程工具。

---

# G7｜Long-session Context & Performance

## Outcome

长局持续增长时，模型 working set、UI responsiveness 和 background work 仍可控。

## Tasks

- bounded Context Assembly；
- relevant subgraph / working set selection；
- deterministic background progression 与 model work 分离；
- TTFT / throughput / context size / persistence latency 的真实 long-play evidence；
- long-session recovery/performance test。

## G7-GATE

`Game State / History ↑↑↑` 时 ordinary Turn Context 不线性爆炸，游戏仍可玩。

---

# G8｜Mod / Authoring & External Declarative UI Contract

## Outcome

把 G4/G6 已证明的内容与 Host capability 外部化给 World Pack / Mod 作者。

顺序：

```text
proven internal capability
→ external schema / naming
→ validator / adapter
→ authoring helper / preview
```

## Tasks

- World Pack / Character / Expansion authoring contracts；
- external UI contribution schema；
- Surface ownership/dependency；
- safe component declarations；
- validation/migration；
- creator/authoring UX；
- second/third real pack proof。

复杂任意代码沙箱仍默认 Deferred。

## G8-GATE

Mod 能扩展内容和受控 UI，而不会获得任意 Runtime/OS 权限或制造第二事实源。

---

# G9｜Standalone Alpha / Release Validation

## Outcome

证明第一代可以作为独立长期 AI RPG 使用，而不是只通过单次演示。

覆盖：Windows build/startup/upgrade、long-play stability、Save/Load/Recovery、Context/performance、World Pack install/use、Provider failure、UI usability、Product Value UAT，以及与 The World / DSH simple baseline 的真实比较。

## G9-GATE

> **玩家愿意把它当一个独立 AI RPG 长期玩，而不是一个技术样品。**

---

## 文档 / 任务纪律

- 当前短周期状态只更新 `MY_WORLD_CURRENT_STATUS.md`；
- 架构结论只更新 `MY_WORLD_架构_CURRENT.md`，深度从其导航；
- 默认不因新 Stage 新建新的 status 文档；
- 正式 implementation/review 仍使用 repository-native Task Packet；
- 小 bug / polish 不必强行升级成完整架构事件；
- Product-facing Task 的 Engineering Acceptance 不替代 Owner Product UAT；
- 旧项目中有价值但当前未授权的能力统一进入 `experience/备选开发方向候选池_2026-08-28.md`，不得因为“以前做过”自动成为当前 Roadmap commitment。
