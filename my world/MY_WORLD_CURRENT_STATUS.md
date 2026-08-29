---
title: my world｜当前状态
status: current-project-status
version: 6.1
created: 2026-08-26
updated: 2026-08-29
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-05 Asset-only New Game Wizard v0.1 — REWORK / G4-05R1
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
G4-04 Multi-Game / Game Library      PASS / CLOSED
G4-04 Storage Design Gate             PASS — per-Game SQLite

Current Phase                         G4 — Primary Source Assets & Local Game Creation
Current Task                          G4-05 — Asset-only New Game Wizard v0.1 — REWORK
Active Correction                     G4-05R1 — Historical Real-Asset Fidelity Correction
G4-GATE                               NOT YET
```

---

## 3. Closed foundation summary

G3 persistence backbone 已成立：SQLite authoritative persistence、atomic durable mutation、accepted Conversation durability、Save/Restore/Recovery、single-writer、verified physical backup、staged corruption recovery 与 real Provider continuation。

G4-01 建立 `Application Lifetime != Game Session Lifetime`。

G4-02 建立 World Pack / Character Card v0.1 contract、真实 filesystem validation 与 content-sensitive exact generation fingerprint。

G4-03 建立 Managed Local Source Library：external mutable package → validate → staged copy/revalidate → append-only immutable generation → explicit current metadata → restart-stable inventory/exact lookup。

G4-04 建立 Multi-Game / Game Library foundation：

```text
One Game = One SQLite
Game Library metadata != gameplay truth
Main Menu boot does not open Game DB
Continue/select resolves exact existing DB
→ Runtime open
→ DB internal game_id cross-check
→ only then current commit
A close/release before B open
legacy G3 current-game.sqlite adopts in place
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
G4-04 Multi-Game Lifecycle / Game Library Foundation — CLOSED
↓
G4-05 Asset-only New Game Wizard v0.1 — REWORK / G4-05R1 ACTIVE
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

G4-06+ HOLD until G4-05 closes。

---

## 5. 第一代 New Game 产品冻结

```text
Main Menu
→ New Game
→ Exactly 1 World Pack
→ Entry / T0: 0..1 from chosen World
→ Expansion Pack: 0..N（G4-05 当前允许 none；真实 Expansion contract/runtime 属于 G4-08）
→ Exactly 1 Player Character Card
→ 0..N Guaranteed NPC Character Cards
→ Game display name
→ Protagonist Control Mode: Full / Light / Narrative
→ optional opening supplement
→ Compatibility Review
→ Atomic Final Create (G4-06)
→ independent Game-local Reality
→ real AI GM Opening
```

Selection authority：

> **Chooser open / list visibility / mode != authoritative selection. Only explicit click on a concrete Source item selects an exact generation.**

第一代不支持 no-World / no-Character / blank-world direct create / arbitrary Draft direct-to-Game / historical Source version picker / complex Expansion chooser / Creator critical path。

---

## 6. G4-04｜PASS / CLOSED

Implementation commit：`67decaa23903803d34d97e6ea04adeeab0d7fe53`。

2026-08-29 Independent Review：**PASS**。Task Packet `owner_uat_required: false`；无需 Owner UAT。

Review 确认：per-Game SQLite；existing-only open；record/DB identity cross-check；current after Runtime READY；A-close-before-B-open；same-Game writer block + different-Game isolation；B recovery 不改 A；legacy 原位 adoption 且保留 World/Conversation/Save/Recovery；metadata failure/restart fail-loud；G3/G4-01 与 Windows GUI/export lifecycle evidence PASS；无 SQLite schema / G4-05 / Source pin / Provider 泄漏。

结论：**G4-04 PASS / CLOSED**。

---

## 7. Current Task｜G4-05 — REWORK

正式名称：

> **G4-05 — Asset-only New Game Wizard v0.1**

Original implementation candidate：`145c3e1192b443f6284da7f36aee74619adad5bf`。

2026-08-29 Independent Review：**REWORK**。

### 已通过审查的部分

当前 implementation 已证明以下主路径语义，未发现 P0/P1：

```text
Managed Source Library current inventory
→ no implicit first-row selection
→ explicit exact World / Entry / Player / NPC selection
→ honest Expansion none
→ minimal settings
→ deterministic Compatibility Review
```

并证明：

- Composition 保存 `asset_type + asset_id + version + generation_fingerprint`；
- World 变化清除旧 Entry；
- Player 必须 `player_character_supported=true`；
- same exact Character 不可同时 Player/NPC；
- selected generation X 后 current 变 Y，Composition 不静默漂移；
- Review 重新 exact lookup X，tamper/missing fail-loud；
- 完整 Wizard→Review 不创建 Game SQLite、不改 Game Library、不调用 Provider；
- Cancel 回到 `MENU_READY / Session ABSENT`；
- relevant G4-01/G4-03/G4-04 regression 与 Windows GUI evidence PASS。

### P1 阻断项｜Historical real-asset fidelity

原 Task Packet 明确要求历史资产作为 **primary real-content / complexity pressure**，并禁止为了方便把真实 lore 改写成 synthetic summary。

当前转换包虽然来自固定 historical snapshot，且 2 World + 6 Character 均能通过 G4-02 validate、G4-03 install/exact lookup，但实际内容被高度摘要化：大型 World 的多层世界资料被压缩为少量 instructions/lore；Character 原卡中的能力/局限、决策逻辑、关系/自主性、语言表现、知识边界、T0/历史使用边界等大量稳定 GM-useful 语义被压缩成简短 summary/background + traits/drives。

因此现有 evidence 只能证明：

> **根据真实资产写出的 compact fixture 能通过 current contract**

还不能证明：

> **current Source contract + Managed Library + Wizard 能承载真实资产的 substantive content/complexity**

这会绕过本轮真实资产 Reality pressure 的目的，所以 G4-05 不得关闭。

Active correction：

`my-world/docs/tasks/G4-05R1_REAL_ASSET_FIDELITY_CORRECTION_TASK.md`

Correction base：`145c3e1192b443f6284da7f36aee74619adad5bf`。

Correction 只修真实资产内容保真、section-level mapping / omission audit 与 fidelity evidence；不得推倒已通过的 Wizard/Composition 主结构，也不得开始 G4-06。

Task Packet `owner_uat_required: false`；G4-05 完整 Owner UAT 仍按 DAG 留在 G4-07 First Playable A。

---

## 8. Real-asset reality policy

Synthetic compact fixtures 继续用于 deterministic failure/unit tests，但不能作为真实资产 Reality evidence 的替代品。

历史真实资产 evidence source：

```text
repo: zhangchenjia21-dot/sillytavern-assets
snapshot: 4a5364a042e41f4c8a69621fc4467956a78703c0
```

G4-05 mandatory real families：

```text
World: 世界包/汉末三国_天下未定_World_Pack_v0.2.3.md
Characters: 人物卡/汉末三国/...

World: 世界包/埃瑟维亚_诸界余辉_World_Pack_v0.1.3.md
Characters: 人物卡/诸界余辉/...
```

必须迁移的是**真实内容事实与复杂性**，不是旧 schema。G4-05R1 要把属于 current Source owner 的 substantive GM-useful sections 实质保留，并逐项记录 mapping / omission reason。

World 至少审计真实的 world operation/T0/history/social/institutional/geographic/material/knowledge/GM-use semantics；Character 至少审计 identity、personality contradictions、abilities/limitations、behavior logic、relationship/autonomy、language/expression、knowledge boundary、T0/historical-use guidance。

若 current v0.1 真有重要稳定 Source 概念无法合理表达，必须 `BLOCKED` 并报告最小 contract gap；不得用摘要规避，也不得建立 production legacy importer/compatibility framework。

G4-07 First Playable A 必须主要使用真实有产品价值的 World/Character 输入，而不是继续只靠 Agent 自创 compact fixtures。

> **Migrate real content/complexity, not legacy schema debt.**

---

## 9. 当前核心约束

- `Commodity Foundation, Owned Game Semantics.`
- `Model freedom first. Reversibility over prevention.`
- `Narrative richness over artificial brevity.`
- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Source provides inertia; actors create history.`
- `Off-screen != Inactive.`
- `World Truth != NPC Knowledge != Player Knowledge.`
- `Context stays bounded, not starved.`
- `Application Lifetime != Game Session Lifetime`。
- `Source Library != Game Library`。
- `One Game = One SQLite`。
- `Game Library metadata != gameplay truth`。
- `Source stable identity != exact immutable generation`。
- `Source Generation != Game Creation Composition != Game-local Reality != Runtime State`。
- `Guaranteed NPC != Opening NPC != Player-known NPC`。
- `Expansion binding != real gameplay effect`。

---

## 10. 当前 waiting

```text
Blocking: P1 — G4-05 historical real-asset fidelity
G4-01..G4-04: PASS / CLOSED
Current: G4-05 Asset-only New Game Wizard v0.1 — REWORK
Implementation candidate reviewed: 145c3e1192b443f6284da7f36aee74619adad5bf
Independent Review: REWORK — Wizard/Composition semantics accepted; real-asset conversion fidelity insufficient
Active correction: G4-05R1 Historical Real-Asset Fidelity Correction
Correction packet: docs/tasks/G4-05R1_REAL_ASSET_FIDELITY_CORRECTION_TASK.md
Historical source: sillytavern-assets@4a5364a042e41f4c8a69621fc4467956a78703c0
Implementation Owner: Codex
Waiting: G4-05R1 correction implementation → READY FOR INDEPENDENT REVIEW
G4-06+: HOLD until G4-05 closes
G4-GATE: NOT YET
```
