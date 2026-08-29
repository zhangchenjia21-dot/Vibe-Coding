---
title: my world｜当前状态
status: current-project-status
version: 5.8
created: 2026-08-26
updated: 2026-08-29
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-04 Multi-Game Lifecycle / Game Library Foundation
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. 文档职责

本文件只拥有当前执行状态：Current Phase、Current Task、已完成 Gate、Owner UAT 结论、当前 blocker / waiting。

其它事实 owner：

- 产品目的：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 架构地图：`MY_WORLD_架构_CURRENT.md`
- 专项架构裁定：`architecture/`
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
G4-02 World + Character Contracts    PASS / CLOSED
G4-03 Managed Local Source Library   PASS / CLOSED

Current Phase                         G4 — Primary Source Assets & Local Game Creation
Current Task                          G4-04 — Multi-Game Lifecycle / Game Library Foundation
G4-04 Storage Design Gate             PASS — per-Game SQLite
G4-GATE                               NOT YET
```

---

## 3. Closed foundation summary

G3 persistence backbone 已正式成立：

```text
SQLite authoritative persistence
+ atomic durable World/Timeline mutation
+ accepted Conversation durability
+ current Game reopen/resume
+ named Save / atomic Load / Restore
+ Recovery Checkpoint / reciprocal Recover
+ immutable internal Timeline branching
+ single-writer process safety
+ SQLite-native verified physical backup
+ staged corruption recovery / quarantine
+ real Provider continuation after Restore / Recover
```

G4-01 已建立：

```text
Application Lifetime != Game Session Lifetime
Launch → Main Menu → explicit Continue → Game Session
Game → Return → complete cleanup → Main Menu
```

G4-02 已建立 World Pack / Character Card v0.1 contracts、真实 filesystem validation 与 content-sensitive exact generation fingerprint。

G4-03 已建立 Managed Local Source Library：

```text
external mutable package
→ G4-02 validate
→ staged contract-owned copy
→ exact-generation revalidation
→ append-only managed generation
→ explicit current metadata
→ restart-stable inventory / exact lookup
```

---

## 4. G4 Route｜Owner-approved v3

```text
G4-01 Application Shell / Main Menu + Game Session Lifecycle — CLOSED
↓
G4-02 World Pack + Character Card Source Contracts v0.1 — CLOSED
↓
G4-03 Managed Local Source Library v0.1 — CLOSED
↓
G4-04 Multi-Game Lifecycle / Game Library Foundation — CURRENT
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

G4-05+ HOLD until G4-04 closes。

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
→ Game display name
→ Protagonist Control Mode: Full / Light / Narrative
→ optional opening supplement
→ Compatibility Review
→ Atomic Final Create
→ independent Game-local Reality
→ real AI GM Opening
```

第一代不支持 no-World / no-Character / blank-world direct create / arbitrary Draft direct-to-Game / historical Source version picker / complex Expansion chooser / Creator critical path。

---

## 6. Current architecture locks relevant to G4-04

长期保持：

```text
Source Library != Game Library
Source Generation != Game-local Reality != Runtime State
Application Lifetime != Game Session Lifetime
Game Library metadata != gameplay truth
```

2026-08-29 G4-04 Storage Design Gate 已正式裁定：

> **One Game = One SQLite database.**

Supporting architecture decision：

`architecture/persistence/G4-04_MULTI_GAME_STORAGE_TOPOLOGY_DECISION.md`

Decision commit：`15118de0edd2d6a179c047d2c409c9819be29924`。

选择依据：当前 Runtime 已接受 explicit database path；writer lock、backup、recovery 都按 DB path 隔离。shared SQLite + `game_id` 会重写 G3 已验证的 one-current verification、backup/recovery blast radius、writer ownership 与大量 persistence query scope，因此第一代拒绝。

Legacy G3 `user://my-world/current-game.sqlite` 默认优先 non-destructive adopt in place；新 Games 目标采用独立 per-Game storage。G4-04 不授权 production SQLite schema migration。

---

## 7. G4-03｜PASS / CLOSED

正式名称：

> **G4-03 — Managed Local Source Library v0.1**

Implementation commit：`b227ff9043a25b3ebf7581eb340f3e2f9006a919`。

2026-08-29 Independent Review：**PASS**。

Task Packet `owner_uat_required: false`；无需 Owner UAT。

Review 确认：

- publish chain 为 G4-02 validate → staging → staged fingerprint revalidation → append-only final generation → atomic current metadata；
- duplicate exact install idempotent；
- same version / different fingerprint 可并存；
- external 修改/删除不改变 managed generation；
- restart 新进程恢复 current inventory 与 retained exact generation；
- copy interruption / fingerprint race / current commit failure / retry / stale staging 均有失败语义；
- managed missing/tamper fail-loud，不信 fingerprint 目录名、不从 external 自动修复；
- tests 使用 task-owned root，未触碰 Owner production Source Library；
- 未启动 G4-04、Game pin、SQLite Game schema 或 Provider。

结论：**G4-03 PASS / CLOSED**。

---

## 8. Current Task｜G4-04

正式名称：

> **G4-04 — Multi-Game Lifecycle / Game Library Foundation**

Outcome：在不重写 G3 persistence 的前提下，让多个独立 Game coexist，并让 Application 通过 Game Library metadata 选择 exact existing Game database path，再复用 G4-01 Session lifecycle 打开/关闭/切换。

必须至少成立：

```text
multiple independent Games coexist
+ per-Game SQLite
+ Game Library player-safe metadata
+ Continue/latest Game
+ select/open/switch
+ legacy G3 current-game.sqlite non-destructive adoption
+ missing/corrupt Game fail-loud
+ one writable Game Session at a time
+ new Game cannot overwrite existing Game
```

Game Library record 必须在 open 时与数据库内部 `game_id` 交叉验证；metadata 不是 gameplay truth。

Implementation Owner：**Codex**。

正式 Task Packet：

`my-world/docs/tasks/G4-04_MULTI_GAME_GAME_LIBRARY_FOUNDATION_TASK.md`

Task Packet commit：`5c5ae75a4010a3b0b420e0a8aa2f89cb43b68d0e`。

当前状态：**ISSUED — waiting Codex implementation → READY FOR INDEPENDENT REVIEW**。

---

## 9. Real-asset reality policy

Synthetic compact fixtures 继续用于 deterministic contract/failure tests，但不能作为长期唯一 Reality evidence。

G4-04 不再创建 Source fixture，因为 Source content 不是本任务变量。

G4-05/06 开始应把历史真实资产的**内容与复杂性**按 current Source contract 重新封装，而不是迁移旧 schema；G4-07 First Playable A 必须主要使用真实有产品价值的 World/Character 进行 Reality/UAT。

> **Migrate real content/complexity, not legacy schema debt.**

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
- `One Game = One SQLite`（G4 first generation）。
- `Game Library metadata != gameplay truth`。
- `Source stable identity != exact immutable generation`。
- `Guaranteed NPC != Opening NPC != Player-known NPC`。
- `Expansion binding != real gameplay effect`。

---

## 11. 当前 waiting

```text
Blocking: NONE KNOWN
G4-01: PASS / CLOSED
G4-02: PASS / CLOSED
G4-03: PASS / CLOSED — implementation b227ff9; IR PASS; Owner UAT not required
Current: G4-04 Multi-Game Lifecycle / Game Library Foundation
Storage Design Gate: PASS — per-Game SQLite
Architecture decision: architecture/persistence/G4-04_MULTI_GAME_STORAGE_TOPOLOGY_DECISION.md
Formal G4-04 Task Packet: ISSUED — docs/tasks/G4-04_MULTI_GAME_GAME_LIBRARY_FOUNDATION_TASK.md
Packet commit: 5c5ae75a4010a3b0b420e0a8aa2f89cb43b68d0e
Implementation Owner: Codex
Waiting: Codex implementation → READY FOR INDEPENDENT REVIEW
G4-05+: HOLD until G4-04 closes
G4-GATE: NOT YET
```
