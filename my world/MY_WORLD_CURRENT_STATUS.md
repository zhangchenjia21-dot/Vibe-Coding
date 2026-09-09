---
title: my world｜当前状态
status: current-project-status
version: 17.22
created: 2026-08-26
updated: 2026-09-09
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: MW-027 Open Threads / 事务
current_owner: Codex
current_dispatch_state: AUTHORIZED / TASK SHAPED / NO IMPLEMENTATION RETURN YET
parent_task: G6 Package 3 Core Information Continuity
semantic_owner: GPT
owner_uat_required: deferred / later concentrated Product confirmation
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.5
active_uat_record: my world/docs/uat/G6_PACKAGE2_OWNER_UAT_U1.md@v1.6
active_architecture: my world/architecture/ui/G6_OPEN_THREADS_SURFACE_V1_0_DECISION.md@v1.0
active_task_packet: my-world/docs/tasks/MW-027_OPEN_THREADS_TASK.md
active_task_branch: mw-027-open-threads
formal_code_base: 5820c20b1150cd998b626e56fce79c023004b5ec
task_packet_commit: b687cfc29f65637424e8b05bbba6c50e332e0999
reviewed_implementation_main: 5820c20b1150cd998b626e56fce79c023004b5ec
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED

G6 RPG Core Closure + UAT Observability + Internal Dynamic UI ACTIVE
```

Current G6 flow:

```text
Package 0  Correction Train + Owner UAT          PRODUCT PASS / CLOSED
Package 1  UAT Observability / Debug Mode v0.1   PRODUCT PASS / CLOSED
MW-023     Gameplay Typography Readability        PRODUCT PASS / CLOSED
Package 2  Core Interaction Control              ENGINEERING COMPLETE / CORE OUTCOME ACCEPTED / PRODUCT CONFIRMATION DEFERRED
  MW-024   OOC / GM Guidance                     ENGINEERING PASS_WITH_NOTES / INTEGRATED
  MW-025   Character-guided Recommendations      ENGINEERING PASS_WITH_NOTES / INTEGRATED
  MW-026   Package-2 UAT Cleanup                  ENGINEERING PASS_WITH_NOTES / INTEGRATED
Package 3  Open Threads / 事务                    CURRENT / MW-027 TASK SHAPED + AUTHORIZED
Package 4  System / Public d20                   QUEUED
Package 5  factual Inventory                     QUEUED
Package 6  Internal Dynamic UI Host v0.1         QUEUED / CORE REQUIRED
Package 7  V0 Core Closure Reality Gate          QUEUED
```

## 2. Owner route instruction — Package 2 confirmation deferred

Owner explicitly instructed on 2026-09-09:

> **“UAT就以后再UAT吧，这次先跳过了”**

Formal interpretation:

- do not prepare the bounded Package-2 confirmation build now;
- do not mark Package 2 Product PASS without the missing experiential confirmation;
- retain all MW-024 / MW-025 / MW-026 engineering and prior exploratory-UAT evidence;
- use exact status:
  **`ENGINEERING COMPLETE / CORE OUTCOME ACCEPTED / PRODUCT CONFIRMATION DEFERRED`**;
- deferred OOC/Public-d20/recommendation spot evidence may be covered by a later concentrated UAT / Package 7 Reality Gate unless Owner asks sooner;
- the deferral is not a blocker for core development;
- proceed directly to Package 3.

Formal record:

`docs/uat/G6_PACKAGE2_OWNER_UAT_U1.md@v1.6`

## 3. Package 3 architecture — FROZEN / CURRENT

Frozen decision:

`architecture/ui/G6_OPEN_THREADS_SURFACE_V1_0_DECISION.md@v1.0`

Governance decision commit:

`c0ed3056130386cbc2f9d1e1d484c04002f0d1a1`

Product question:

> **“最近有哪些还没有真正结束、但值得我继续记住和跟进的事情？”**

Frozen semantics:

- `事务 / Open Threads` is a player-current unresolved-matters surface, not a Quest engine or task manager;
- the model decides whether a matter is worth remembering, whether it changes, and when it is resolved/expired/no longer relevant;
- ordinary turns may produce no Thread change;
- current-scene presence is neither necessary nor sufficient;
- current snapshot, not task history;
- full current replacement semantics: `open_threads=null` keeps, Array replaces, `[]` clears;
- no keyword/regex classifier, importance score, priority rule, event-type forest or turn threshold;
- one existing Information Curator call maintains Character + Important Experiences + People + Open Threads;
- no extra Provider call, new SQLite table, hidden World objective view or historical backfill;
- World Information Host adds `事务` between `人物` and `存档`;
- Debug Mode adds a read-only `threads` lane from player-safe before/after projection.

## 4. CURRENT — MW-027 Open Threads

Formal Code Base:

`my-world/main@5820c20b1150cd998b626e56fce79c023004b5ec`

Governance Base at Task Shape:

`Vibe-Coding/main@dbc83f67d0bacbf973c00d278fa760089f3152b3`

Task branch:

`mw-027-open-threads`

Task packet:

`docs/tasks/MW-027_OPEN_THREADS_TASK.md`

Task packet commit:

`b687cfc29f65637424e8b05bbba6c50e332e0999`

Required worktree:

`D:/AI/Projects/.worktrees/my-world/mw-027-open-threads`

Current dispatch meaning:

> **AUTHORIZED / TASK SHAPED / NO IMPLEMENTATION RETURN YET**

`current_owner: Codex` means Codex is the authorized production implementer for this Work Item. It is **not evidence that an agent process is currently running**.

Implementer return ceiling:

**READY FOR INDEPENDENT REVIEW**.

## 5. MW-027 required product result

The implementation should prove this vertical:

```text
accepted current role-action turn
→ existing Information Curator, same one semantic call
→ model decides Open Threads current snapshot change/no-change
→ existing information_curation durable owner
→ player-safe current Threads projection
→ 事务 Surface
→ Debug threads changed/no-change/failed
→ Save/reopen/Restore/Regenerate currentness
```

Expected player-facing navigation:

```text
概览 | 角色 | 重要经历 | 人物 | 事务 | 存档
```

v1.0 `事务` shows title + current summary + bounded details / empty state only.

Not in MW-027:

- Quest rewards/types/status engine;
- checkbox/manual completion/editing;
- search/filter/sort/priority controls;
- separate Threads model call;
- System / Inventory / Dynamic UI;
- generic Action Intent;
- player hide/edit preference;
- historical backfill;
- unrelated cleanup/refactor.

## 6. Review gates for MW-027

Independent Review must verify both engineering correctness and architecture fidelity.

Critical gates:

- old information-curation lived history remains readable with version-correct ID/currentness validation;
- new Thread writes follow accepted-prefix + parent-chain currentness;
- Restore/displaced future/stale callbacks cannot leak future Threads into current Timeline;
- no new SQLite owner or extra Provider call;
- no OOC lived curation;
- leaf UI consumes only L3 player-safe Threads projection;
- Debug compares safe before/after projection only;
- Character / Important Experiences / People remain intact;
- no Program semantic task rules are introduced.

Engineering PASS still requires GPT Independent Review after Codex returns. Owner Product UAT may remain deferred/combined and is not part of Codex's completion authority.

## 7. Next route after MW-027

Normal route after reviewed MW-027 integration:

```text
Package 4  System / Public d20
→ Package 5  factual Inventory
→ Package 6  Internal Dynamic UI Host v0.1
→ Package 7  V0 Core Closure Reality Gate
```

Do not insert discretionary typography, hide-preference, shell refactor, G3 debt, Context Orchestrator or other peripheral work unless a real blocker emerges or Owner explicitly changes route.

## 8. Deferred but approved

Player-side presentation hiding remains approved but deferred to Package 6 surface convergence:

- People-specific: `architecture/ui/G6_PEOPLE_CARD_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md`
- cross-surface: `architecture/ui/G6_MODEL_CURATED_SURFACE_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md`

Package-2 Product confirmation remains deferred, not waived.

## 9. Retained debt notes

- exact-baseline G3 Context assertions remain debt until relevant Context work;
- existing teardown/resource warnings remain non-blocking baseline evidence;
- layer-boundary findings remain bounded architecture debt;
- Application Shell decomposition remains evolutionary;
- long-session Context Orchestrator / general Structured Output Reliability remain G7.

## 10. Protected invariants

- Model Freedom First;
- Reversibility over prevention;
- free-form natural-language role action remains primary;
- World Truth != actor Knowledge != human-player disclosure;
- UI/Debug is projection, never second truth;
- Save / Restore / Regenerate currentness remains authoritative;
- OOC is guidance, not mutation;
- Character is evidence/model interpretation, not Program-enforced personality rules;
- Information Curator remains background / non-blocking and owns open semantic curation through the model, not Program heuristics.
