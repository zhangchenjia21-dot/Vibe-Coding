---
title: my world｜当前状态
status: current-project-status
version: 5.4
created: 2026-08-26
updated: 2026-08-28
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-02 World Pack + Character Card Source Contracts v0.1
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. 文档职责

本文件只拥有当前执行状态：Current Phase、Current Task、已完成 Gate、Owner UAT 结论、当前 blocker / waiting。

其它事实 owner：

- 产品目的：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 架构地图：`MY_WORLD_架构_CURRENT.md`
- 阶段 / Task DAG：`MY_WORLD_总体规划路线图_CURRENT.md`
- 开发路径经验：`experience/AI_RPG开发路径与阶段设计经验_v1.0_2026-08-28.md`

---

## 2. 当前状态

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G2-GATE                               PASS
G3 Persistent Game / Save / Timeline PASS / CLOSED
G3-GATE                               PASS
G4-01 Application Shell / Lifecycle  PASS / CLOSED

Current Phase                         G4 — Primary Source Assets & Local Game Creation
Current Task                          G4-02 — World Pack + Character Card Source Contracts v0.1
G4-GATE                               NOT YET
```

---

## 3. G3｜PASS / CLOSED

G3-01..G3-07 已完成对应 Engineering / Independent Review / Owner UAT。G3-07 Owner UAT（2026-08-28）：**PASS**。

G3-GATE：**PASS**。

第一代 persistence backbone 已成立：

```text
SQLite authoritative persistence
+ atomic durable World/Timeline mutation
+ accepted Conversation durability
+ current Game reopen/resume
+ named Save / atomic Load / Restore
+ future-memory isolation
+ internal Recovery Checkpoint / reciprocal Recover
+ immutable internal Timeline branching
+ single-writer process safety
+ SQLite-native verified physical backup
+ staged corruption recovery / quarantine
+ real Provider continuation after Restore / Recover
```

明确 Deferred：任意 Turn 一键 rewind、Timeline browser、backup browser。

---

## 4. G4 Route｜Owner-approved v3

2026-08-28 经过第二轮跨项目历史开发步骤审计与 Owner 讨论，G4 再次收敛。

本轮不是增加更多功能，而是修正开发顺序与第一代产品范围：

1. 第一代只有 asset-only New Game 主路径；
2. World Pack / Character Card / Expansion Pack 升级为 Primary Source Trio；
3. Character Card 不限于玩家角色；建局为 `Exactly 1 Player + 0..N Guaranteed NPC`；
4. Expansion 数量 `0..N`，允许 none；
5. 第一次资产试玩只验证 World + Character；通过后才加入真实 Expansion；
6. Main Menu 任务必须包含 `Application Lifetime != Game Session Lifetime`，不能只画视觉菜单；
7. Managed Source Library 必须先于 New Game Wizard；
8. Multi-Game / Game Library 必须在正式 New Game 前成立；
9. Source generation immutable，existing Game exact pin；
10. First Playable A / B 在 G4 中途分别进行 Owner UAT，不等到整个 G4 最后才第一次试玩。

新 G4 Task DAG：

```text
G4-01 Application Shell / Main Menu + Game Session Lifecycle
↓
G4-02 World Pack + Character Card Source Contracts v0.1
↓
G4-03 Managed Local Source Library v0.1
↓
G4-04 Multi-Game Lifecycle / Game Library Foundation
↓
G4-05 Asset-only New Game Wizard v0.1
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

所有旧的 G4-01 World Pack task packet / Main-Menu-only handoff 均 **SUPERSEDED BEFORE EXECUTION**。

---

## 5. 第一代 New Game 产品冻结

正式路径：

```text
Main Menu
→ New Game
→ Exactly 1 World Pack
→ Entry / T0
→ Expansion Pack 0..N（可 none）
→ Exactly 1 Player Character Card
→ 0..N Guaranteed NPC Character Cards
→ Minimal Settings
→ Compatibility Review
→ Atomic Final Create
→ Game-local Reality
→ real AI GM Opening
```

Minimal Settings 当前批准：

- Game display name；
- Protagonist Control Mode：Full / Light / Narrative；
- optional opening supplement。

默认推荐 Light Delegation。

第一代不支持：

- 无 World Pack 建局；
- 无 Character Card 的玩家角色建局；
- AI 从空白自由文本直接生成世界并开局；
- Draft / arbitrary external file 直接成为 Game Source；
- historical Source version picker；
- complex Expansion feature/module chooser；
- Creator 在 G4 进入关键路径。

---

## 6. Character Card 当前语义

Character Card 是 reusable Character Source。

第一代建局：

```text
Exactly 1 Player Character
0..N Guaranteed NPC Characters
```

Guaranteed NPC：

> 被玩家明确选择后，从 Final Create 起就是本局 canonical cast 的一部分。

但：

```text
Guaranteed in Game
!= Opening appearance
!= same scene
!= player-known
!= relationship
!= automatic context inclusion
```

具体出现时间/地点/关系由 G5 世界因果与 Runtime 决定。

---

## 7. Expansion 当前语义

第一代允许 `0..N` Expansion Pack。

开发顺序冻结：

```text
First Playable A
World + Character
→ Owner UAT

then

Expansion v0.1
→ exact binding
→ real observable Runtime effect
→ First Playable B
→ Owner UAT
```

不接受 `Source / Manifest / Binding 存在` 就宣称 Expansion 已工作。

G4 不建设 generic external Expansion UI framework；真实 mechanism UI 留给 G6，external declarative UI contract 留给 G8。

---

## 8. G4-01｜PASS / CLOSED

正式名称：

> **G4-01 — Application Shell / Main Menu + Game Session Lifecycle**

Implementation commit：`d193268be10082df15ce4d8f610de6ebdc7e58ac`。

2026-08-28 Independent Review：**PASS**。

2026-08-28 Owner UAT：**PASS**。

已正式证明：

```text
Application Launch
→ Main Menu READY without Game DB open/lock
→ Continue
→ current Game Session
→ Return to Main Menu
→ Provider/View/Runtime/SQLite/writer cleanup
→ Application remains READY
→ Continue again
→ same durable Game truth
```

同时保留 G3 Save / Load / Recovery、single-writer、corruption recovery 与 responsive Windows product path。

结论：**G4-01 PASS / CLOSED**。

---

## 9. Current Task｜G4-02

正式名称：

> **G4-02 — World Pack + Character Card Source Contracts v0.1**

Outcome：建立第一代两个低复杂度 Primary Source contract，并通过真实差异内容做 Contract Reality Check，为 G4-03 Managed Source Library 提供可验证输入边界。

必须保持：

```text
World Pack != Character Card
stable Source identity != exact immutable generation
Source != Game-local Reality != Runtime State
```

Shared identity seam 只允许最薄共同部分：

```text
asset_id
asset_type
version
exact immutable generation / content fingerprint
```

World Pack 至少表达：world / GM instructions、ordered Source Lore、0..N lightweight Entry/T0 seed、authored portrait/scene/map declarations 与必要 Source material。

Character Card 至少表达：stable/display identity、public profile、GM/private Source profile、portrait reference、player-character eligibility；不得携带 live location/current relationship/current injury/current knowledge 等 Runtime state。

Contract Reality Check 必须使用至少两个风格/规则显著不同的 compact World Source，以及足以覆盖 Player Character / Guaranteed NPC 用途的 Character Source，证明 contract 不是只对单一示例成立。

本任务不做 Managed Source Library、Game Library、New Game selector、Final Create、Expansion Pack 或 Runtime Asset Resolution。

Implementation Owner：**Codex**。

正式 Task Packet：

`my-world/docs/tasks/G4-02_WORLD_CHARACTER_SOURCE_CONTRACTS_TASK.md`

Task Packet commit：`60a0139c8e7facc019fc63bd6593ef2000261284`。

当前状态：**ISSUED — waiting Codex implementation → READY FOR INDEPENDENT REVIEW**。

---

## 10. 当前核心约束

- `Commodity Foundation, Owned Game Semantics.`
- `Model freedom first. Reversibility over prevention.`
- `Narrative richness over artificial brevity.`
- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Source provides inertia; actors create history.`
- `Off-screen != Inactive.`
- `World Truth != NPC Knowledge != Player Knowledge.`
- `Context stays bounded, not starved.`
- UI / Transcript / Prompt / Cache 不是 authoritative truth。
- `Application Lifetime != Game Session Lifetime`。
- `Source Library != Game Library`。
- `Source stable identity != exact immutable generation`。
- `World / Character / Expansion Source != Game-local Reality != Runtime State`。
- `Guaranteed NPC != Opening NPC != Player-known NPC`。
- `Expansion binding != real gameplay effect`。

---

## 11. 当前 waiting

```text
Blocking: NONE KNOWN
G4-01: PASS / CLOSED — Engineering + Independent Review + Owner UAT PASS
Current: G4-02 World Pack + Character Card Source Contracts v0.1
Implementation Owner: Codex
Formal G4-02 Task Packet: ISSUED — docs/tasks/G4-02_WORLD_CHARACTER_SOURCE_CONTRACTS_TASK.md
Packet commit: 60a0139c8e7facc019fc63bd6593ef2000261284
Waiting: Codex implementation → READY FOR INDEPENDENT REVIEW
G4-03+: HOLD until G4-02 closes
G4-GATE: NOT YET
```