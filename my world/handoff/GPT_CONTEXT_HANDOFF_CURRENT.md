---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-08-30
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-07 First Playable A
current_execution_task: G4-07A First Playable Opening Runtime
semantic_owner: GPT
current_execution_owner: Codex
owner_uat_required: true
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。不是 Product / Architecture / Status 的替代权威。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current route

```text
G4-06 Atomic Final Create      PASS / CLOSED
↓
G4-07 First Playable A         ACTIVE — Owner UAT gate
↓
G4-07A Opening Runtime         ACTIVE — Codex
↓ GPT Independent Review
G4-07B Playable UI Integration HOLD — Kimi
↓ GPT Independent Review
G4-07 Owner UAT
```

Engineering PASS does not equal Product PASS.

---

## 2. Read first

Governance:

1. `AGENTS.md`
2. `my world/MY_WORLD_项目启动总纲_CURRENT.md`
3. `my world/MY_WORLD_核心设计原则_CURRENT.md`
4. `my world/MY_WORLD_架构_CURRENT.md`
5. `my world/MY_WORLD_总体规划路线图_CURRENT.md`
6. `my world/MY_WORLD_CURRENT_STATUS.md`
7. `my world/AGENT_EXECUTION_ROUTING_CURRENT.md`
8. `my world/architecture/creation/G4-06_OPTIONAL_ENTRY_MATERIALIZATION_DECISION.md`

Implementation:

9. `AGENTS.md`
10. `docs/tasks/G4-07A_FIRST_PLAYABLE_OPENING_RUNTIME_TASK.md`
11. `docs/g4_06/G4-06_ATOMIC_FINAL_CREATE_IMPLEMENTATION_EVIDENCE.md`
12. `docs/g4_06/G4-06IR01_PROCESS_RESTART_EVIDENCE.md`
13. `src/最终建局/L3_外交层/原子最终建局公开接口.gd`
14. `src/runtime/当前游戏会话运行时.gd`
15. `src/context/上下文组装器.gd`
16. current `src/provider/**` public/request seams
17. `src/persistence/L3_外交层/世界持久化公开接口.gd`
18. G2 real Provider tests
19. G3 reopen/Conversation durability tests
20. G4-04 existing-only Game lifecycle tests

Do not reread the entire historical repository unless evidence requires it.

---

## 3. Agent routing

Canonical:

`my world/AGENT_EXECUTION_ROUTING_CURRENT.md`

```text
GPT        → Meaning / architecture / governance / task shaping / Independent Review
Codex      → backend / mechanism implementation
Kimi       → frontend / UI / interaction implementation
Grok Build → search / external research / evidence discovery
```

Task fit comes before quota availability.

G4-07A is backend/runtime/context/provider-heavy → Codex.
G4-07B is frontend/application interaction-heavy → Kimi after G4-07A IR PASS.

---

## 4. G4-06 closure truth

Accepted commits:

```text
implementation     1457ca18c4ef19fd5757844820630649ea85fe6b
evidence           383481631cd3de3c4b9fd2cc47eef911961d8373
real process IR01  39d7300790b2b067b12630f4d1efd4fd51b6d126
```

GPT Independent Review: **PASS / CLOSED**.

Confirmed:

- exact frozen Composition re-review before durable create side effects;
- immutable creating intent fixes creation/Game/root/local identities and selected setup;
- same identity/same payload converges to one Game;
- same identity/changed payload conflicts;
- different creation identities allow distinct Games from identical Composition;
- One Game = One SQLite;
- DB identity/root verified before Game Library registration;
- wrong-existing identity is preserved and fails loud;
- exact Source pin survives current-generation drift;
- Source tamper fails before creation side effects;
- Han early materialization excludes future/unselected bytes;
- no-Entry remains explicit null/top-level-only;
- non-temporal scenario route works;
- local Character IDs are not Source asset IDs;
- Guaranteed NPC does not imply opening presence/location/player knowledge/relationship;
- no Provider/AI Opening during Final Create;
- production SQLite schema remains v4.

IR01 specifically proved the four crash windows through **three distinct Godot/OS process IDs per case**. Existing DB SHA-256 remained stable across recovery, so valid truth was not destructively rebuilt.

Do not reopen G4-06 without new P0 evidence or explicit Owner decision.

---

## 5. Source and Game-local runtime boundary

Source v0.2-r2 remains frozen.

Temporal quarantine is optional authored capability.

At Final Create:

```text
selected Entry
→ World top-level + exact Entry
→ Character top-level + exact matching profile when authored

no Entry
→ World top-level only
→ Character top-level only
```

No latest/nearest/later/full-life fallback and no hidden historical mode.

Critical G4-07 runtime rule:

> **After create, the durable Game-local materialized setup/current World is runtime semantic truth.**

Do not reconstruct Opening context from Wizard memory or `SourceLibrary.current`. Provenance remains pinned for audit, but mutable Source current cannot rewrite an existing Game.

---

## 6. Current task｜G4-07A First Playable Opening Runtime

Packet:

`docs/tasks/G4-07A_FIRST_PLAYABLE_OPENING_RUNTIME_TASK.md`

Packet commit:

`8d3bc6e7557c9687141e02a5e554ee90959c2a68`

Formal Code Base:

`39d7300790b2b067b12630f4d1efd4fd51b6d126`

Repository activation commit:

`0a8c8aa0477b92be85634bea833824502ed12a97`

Owner: **Codex**  
Reviewer: **GPT**  
Return ceiling: **READY FOR INDEPENDENT REVIEW**  
Parent Product gate: **G4-07 Owner UAT required**

---

## 7. G4-07A required meaning

Target:

```text
G4-06 created Game
→ existing-only open
→ durable Game-local setup/current truth
→ bounded-but-rich first Opening Context
→ real DeepSeek GM Opening
→ accepted Conversation durable exactly once
→ close/reopen same Game
→ continue from durable Conversation + World truth
```

Important invariants:

- Opening context comes from durable Game truth, never mutable Wizard state;
- do not re-resolve Source current to reconstruct created semantics;
- missing/wrong/corrupt Game fails loud; never silently create a new Game;
- first Opening is a GM turn, not a synthetic persisted Player prompt;
- provider failure/cancel before acceptance leaves zero accepted Opening and permits clean retry;
- once first Opening is accepted/durable, reopen must not auto-generate a duplicate Opening;
- Han early-start Provider-visible context must exclude later/future material;
- no-Entry Game stays no-Entry;
- Guaranteed NPC canonical existence does not force scene-one presence/player familiarity;
- Context is bounded but cannot collapse rich authored semantics into one-line summaries;
- reuse G2 Provider stack and G3 Conversation/persistence ownership;
- no second Provider stack or transcript store;
- schema stays v4 unless task returns BLOCKED before migration;
- do not implement G4-07B UI or G5/G7 broad systems.

Required real Provider engineering evidence: at least Han + Afterglow created through production G4-06.

---

## 8. Independent Review after Codex return

Refresh both repos first. Do not trust report text alone.

Inspect at least:

1. actual production diff and task matrix;
2. test Game is created through production G4-06, not a synthetic empty fixture shortcut;
3. session open is existing-only and preserves exact `game_id`;
4. first Opening Context is assembled from durable Game-local root/current setup;
5. no read of mutable Source current supplies semantic content after create;
6. Han selected early semantics are present and known later markers absent from Provider-visible payload;
7. no-Entry remains no-Entry;
8. Guaranteed NPC is not forcibly placed/known by policy;
9. real DeepSeek requests occurred for Han + Afterglow without committed secrets;
10. streaming/cancel/failure use existing Provider semantics;
11. failed/cancelled attempt leaves accepted Conversation empty;
12. accepted first GM Opening persists exactly once;
13. close/reopen retains that turn and does not auto-generate another first Opening;
14. continuation context includes durable accepted history;
15. existing G2/G3/G4 regressions remain meaningful;
16. physical schema remains v4 or task correctly returned BLOCKED before migration.

If PASS, activate G4-07B for Kimi. Do **not** close G4-07 parent yet.

---

## 9. G4-07 parent UAT

Only after G4-07A + G4-07B engineering/IR PASS should Owner play the real vertical.

Owner judges:

- Narrative richness;
- Character individuality;
- anti-convergence;
- Context not starved;
- Han and Afterglow feel materially different;
- the product genuinely feels like an AI RPG;
- Save/exit/reopen/Continue preserves confidence and continuity.

No automated score can substitute for this product judgment.
