---
title: my world｜当前状态
status: current-project-status
version: 13.3
created: 2026-08-26
updated: 2026-09-04
phase: G5 World Semantics & GM Runtime
current_task: G5-04 Event / Priority-driven World Evolution — Owner UAT
current_owner: OWNER
parent_task: G5-04 Event / Priority-driven World Evolution
semantic_owner: GPT
owner_uat_required: true
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G4-10 Runtime Asset Resolution              DEFERRED / MOVED TO G6

G5 World Semantics & GM Runtime             ACTIVE
G5-01 World Turn / Semantic Materialization PASS / CLOSED
G5-02 Knowledge Provenance                  PASS / CLOSED
G5-03 NPC / Faction Agency                  ENGINEERING PASS / CLOSED
G5-03M1 Multi-Actor Agency v0.3             ENGINEERING PASS / CLOSED
G5-03M2 Stable Actor Registry               ENGINEERING PASS / CLOSED
G5-03M2A Registry Foundation                ENGINEERING PASS / CLOSED
MW-001 Runtime Narrative Actor Materialization PASS / CLOSED
G5-04 Event / Priority Evolution            ACTIVE — OWNER UAT
MW-002 Selective World Evolution Evaluator ENGINEERING PASS / CLOSED
G5-GATE                                     NOT YET
```

Current G5-04 canonical decision:

`architecture/world/G5_SELECTIVE_WORLD_EVOLUTION_V0_1_DECISION.md`

MW-002 final packet:

`my-world/docs/tasks/MW-002_SELECTIVE_WORLD_EVOLUTION_EVALUATOR_TASK.md`

Independent Reviews:

- `my-world/docs/g5_04/MW-002_INDEPENDENT_REVIEW_IR1.md` — CORRECTION REQUIRED
- `my-world/docs/g5_04/MW-002_INDEPENDENT_REVIEW_IR2.md` — ENGINEERING PASS / CLOSED

## 2. MW-002 final identity / lineage

```text
Work Item: MW-002
Name: Selective World Evolution Evaluator
Capability-Anchor: G5-04
Revision: 2
Review-Round: IR#2
Triggered-By: MW-002 IR#1
Revision-2 Implementation: 5459c4f8a1d92928129c1d3217a40c6622522496
Revision-2 Evidence: 001106f2c790e70935ea4e54eb721b4aef9e07b4
Verdict: ENGINEERING PASS / CLOSED
```

Same Outcome correction remained on the same flat Work Item under:

`governance/TASK_IDENTITY_AND_LINEAGE_V1_0.md`

Do not mint suffix identities for historical correction lineage.

## 3. IR#2 result

GPT refreshed both authoritative `main`s and independently inspected the actual Revision-2 correction diff, production seams, focused test source and committed evidence.

Revision-2 compare from reviewer propagation head `d4332dd4233fd90a3dbbae382ba199c5fedc2535` to Kimi evidence head `001106f2c790e70935ea4e54eb721b4aef9e07b4` is exactly 2 commits / 5 files, limited to:

- Application Shell foreground invalidation composition;
- Agency Scheduler immediate-terminal handling;
- Game-local World-only projector;
- G5-04 focused tests;
- evidence.

IR#1 findings are closed:

- **F01 PASS** — Public-d20 `request_assembled` invalidates Agency + World Evolution before Provider `start_stream`, closing the pre-Conversation foreground window.
- **F02 PASS** — Agency `already_committed` is now a clean immediate terminal: runtime cleanup, exactly one frozen turn/hash `opportunity_finished`, one evolution wake, later dirty opportunity remains viable.
- **F03 PASS** — World Evolution T0 baseline is now truly World-only; arbitrary opening supplement/control mode/Game settings no longer enter causal input.
- **F04 PASS** — production active-evaluator → Restore → invalidation → late callback → zero commit is directly proven.

No architecture restart or new engineering slice is required before Product UAT.

## 4. Engineering evidence

Kimi's committed Revision-2 evidence reports:

- G5-04 focused: **144 PASS / 0 FAIL**;
- G5-03 Scheduler/Cycle regression: **0 FAIL**;
- G4-08M1 Public-d20 regression: **0 FAIL**;
- G4-07A continuation/context regression: **0 FAIL**;
- G3-04 Save/Restore regression: **PASS**;
- `git diff --check`: clean;
- real Provider calls: **0**.

The reviewed final commit has no external CI status. GPT's reviewer environment has no Godot executable, so GPT did not independently rerun Godot. The IR verdict is based on direct actual-code inspection plus committed deterministic test evidence.

## 5. Frozen G5-04 Product Definition

> **The world must be able to move without direct Player causation, but must not manufacture an event every turn.**

```text
World Independence + Player Spotlight
Persistent != Fully Simulated
Evaluation opportunity != event obligation
```

`hold` is a first-class correct result.

Frozen order:

```text
ordinary accepted Player Narrative
→ semantic lane
→ existing Agency Selector / optional actor cycle
→ Agency opportunity truly terminal
→ World Evolution Evaluator
→ hold OR at most one causally-ready world event
→ optional durable mutation
→ next GM continuation may consume the event
```

The Player turn is a safe scheduling opportunity, not causal authority for the event. Opening-only GM generation creates no opportunity. No offline/wall-clock evolution exists.

## 6. Protected MW-002 semantics

Do not reopen absent a concrete regression/new consumer:

- dedicated evaluator after the whole Agency opportunity terminal;
- first-class `hold`, no fake marker/mutation, no same-opportunity auto-retry;
- one-event ceiling;
- priority judgment remains model-owned, not Program scoring/keywords/cadence;
- deterministic Program-owned event/mutation/node identity bound to exact Game + turn/hash + base head;
- additive `living_world.v0.1`; no SQLite migration/table;
- durable replay idempotence;
- exact accepted-hash currentness;
- foreground / Public-d20 control start / Restore / shutdown / unrelated head change invalidate uncommitted work;
- late callbacks cannot commit obsolete work;
- frozen Game-local T0 evaluator baseline is World-only;
- no `opening_supplement`, protagonist control mode, Character-private Source or Actor Knowledge Provenance in evaluator causal input;
- no mutable Source Library current lookup during gameplay;
- first real consumer is next GM continuation Context;
- event truth is omniscient GM world fact, not automatic Player/actor knowledge;
- no automatic G5-02 Knowledge creation or forced visible event announcement;
- stable-NPC intentional actions remain G5-03 Agency;
- no generic Faction actor/shared-Knowledge/agency platform, persistent pressure queue, numeric priority platform, random-event engine, universal simulator/event ontology, UI or offline simulation.

## 7. G5-03 closeout remains protected

G5-03 remains ENGINEERING PASS / CLOSED.

Current Faction deferral:

`architecture/world/G5_03_AGENCY_CLOSEOUT_AND_FACTION_DEFERRAL_V1_0_DECISION.md`

Parent real G5-03 Provider proof remains honestly:

`PENDING / EXTERNAL PROVIDER UNAVAILABLE`

Do not switch Provider merely to manufacture evidence.

## 8. Current gate — Owner G5-04 UAT

Engineering PASS is necessary but insufficient because G5-04 changes world pacing/product feel.

Owner UAT must counterpose at least two scenarios:

### A — Quiet / Life Loop

Choose a situation where no world process is causally ripe.

Expected Product behavior:

- evaluator can genuinely `hold`;
- no artificial escalation/event is manufactured merely because a turn completed;
- downtime, free activity, relationships and Life Loop retain breathing room.

### B — Genuine ripe world pressure

Choose a T0 or accumulated current world pressure that should mature even without direct Player causation.

Expected Product behavior:

- one credible consequence advances;
- the consequence is durable;
- later GM Narrative can surface/use it naturally when scene, information flow and pacing make sense;
- it does not feel like a forced random encounter or scheduler-generated escalation.

Only Owner Product PASS may close G5-04.

## 9. Route from here

```text
MW-002 ENGINEERING PASS / CLOSED
→ Owner G5-04 UAT
→ Owner Product PASS / correction verdict
→ only after G5-04 closes: shape/start G5-05
```

Do not start G5-05 before the Owner verdict.

## 10. Routing

Through 2026-09-06 23:59 (+08:00): GPT owns semantics/architecture/task shaping/review; Kimi owns code-changing implementation. Correct in-flight Kimi work may finish after expiry.