---
title: my world｜当前状态
status: current-project-status
version: 13.1
created: 2026-08-26
updated: 2026-09-04
phase: G5 World Semantics & GM Runtime
current_task: MW-002 Selective World Evolution Evaluator
current_owner: KIMI
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
G5-04 Event / Priority Evolution            ACTIVE
MW-002 Selective World Evolution Evaluator ACTIVE — KIMI
G5-GATE                                     NOT YET
```

Current G5-04 canonical decision:

`architecture/world/G5_SELECTIVE_WORLD_EVOLUTION_V0_1_DECISION.md`

Current repository-native Task Packet:

`my-world/docs/tasks/MW-002_SELECTIVE_WORLD_EVOLUTION_EVALUATOR_TASK.md`

## 2. G5-03 closeout remains protected

G5-03 is closed on proven stable-NPC Agency + Stable Actor Registry/Materialization.

Do not add a generic Faction-agency platform merely for symmetry. The current Faction decision remains:

`architecture/world/G5_03_AGENCY_CLOSEOUT_AND_FACTION_DEFERRAL_V1_0_DECISION.md`

A concrete later Faction consumer may pull the smallest required collective identity/Knowledge/agency semantics. MW-002 does not create that consumer: aggregate faction-related consequences may remain world events.

Parent real G5-03 Provider proof remains honestly:

`PENDING / EXTERNAL PROVIDER UNAVAILABLE`

Do not switch Provider merely to manufacture PASS evidence.

## 3. G5-04 Product Definition

G5-04 is now frozen around one product problem:

> **The world must be able to move without direct Player causation, but must not manufacture an event every turn.**

Protected framing:

```text
World Independence + Player Spotlight
Persistent != Fully Simulated
Evaluation opportunity != event obligation
```

Historical reference audit confirmed both requirements:

- strong-model world progression across days/months/years is valuable;
- `event → event → event` pacing damages downtime, free activity, relationships and Life Loop play.

Therefore `hold` is a first-class correct evaluator result.

## 4. MW-002 execution identity

```text
Work Item: MW-002
Name: Selective World Evolution Evaluator
Capability-Anchor: G5-04
Revision: 1
Review-Round: 0
Owner: Kimi
Reviewer: GPT
```

`MW-002` is a new independent Outcome, so it receives a new flat Work Item ID under:

`governance/TASK_IDENTITY_AND_LINEAGE_V1_0.md`

## 5. Frozen runtime ordering

Do not reopen G5-03 sequencing:

```text
ordinary accepted Player Narrative
↓
existing semantic lane
↓
existing standalone Agency Selector / optional actor cycle
↓
Agency opportunity truly terminal
↓
World Evolution Evaluator gets one best-effort opportunity
↓
hold
OR
advance at most one causally-ready world event
↓
optional single durable World mutation
↓
next ordinary GM continuation Context can consume the event
```

The Player turn is a safe scheduling opportunity, **not causal authority** for the event.

Opening-only GM generation creates no MW-002 opportunity. No offline/wall-clock evolution is introduced.

## 6. World-event semantics

World Evolution owns concrete developments that need not be one stable NPC's intentional choice, for example:

- weather / environment / natural process;
- aggregate conflict/front movement;
- institutional/economic/social process;
- deadline ripening;
- disaster/accident;
- chain reaction from prior Player/NPC/world consequences.

Intentional stable-NPC action remains G5-03 Agency territory.

One evaluator opportunity may advance at most one event. This is a v0.1 safety bound, not a universal simulation rule.

## 7. Priority is model-owned

Do not build:

- numeric priority/urgency score;
- fixed `every N turns` event cadence;
- persistent pressure queue;
- generic Quest/Thread scheduler;
- random-event generator as the primary system;
- universal event/entity/Faction ontology.

The model decides whether any **already-supported causal process** deserves progression now, and may hold.

Program owns only bounded response validation, identity, currentness, durability and foreground safety.

## 8. Evaluator context boundary

MW-002 evaluator receives bounded GM-level world causality material:

1. frozen Game-local T0 **World-only** projection from the opened Game;
2. latest accepted Player action + GM Narrative;
3. recent current-hash semantic world changes;
4. recent current-hash Agency actions/effects;
5. recent current-hash prior World Evolution events.

Do not include Actor Knowledge Provenance or Character-private Source material.

Do not consult mutable Source Library current during gameplay. The frozen T0 World projection is needed so early-game pressures are not starved before many post-T0 changes exist.

## 9. Durable / currentness semantics

Keep `living_world.v0.1`; no SQLite migration/table.

A Program-owned event record binds:

```text
world_evolution_id
opportunity_turn_index
opportunity_gm_sha256
evolution_base_head_id
materialized_at
event
effects[]
```

Opportunity turn/hash is scheduling/currentness metadata, not Player causation.

Matching committed event replay is idempotent. `hold`/failure creates no fake durable marker and is not auto-retried in the same runtime opportunity.

Foreground Player attempt, Restore/progress switch, shutdown or unrelated head change invalidates uncommitted evolution work. Late callbacks remain inert.

Regenerate/correction leaves stale Timeline history physically intact but excludes stale event records from current Context by exact accepted hash.

## 10. First real consumer / disclosure boundary

The first real consumer is the existing next-GM continuation Context.

Extend World Turn Context with bounded current `World Evolution Events` and explicit guidance:

- omniscient GM current-world fact;
- not automatic Player knowledge;
- not automatic actor knowledge;
- GM decides when/how scene, information flow and pacing make it relevant.

Do not force an immediate visible event announcement and do not auto-create Knowledge Provenance.

MW-002 does not add evolution truth to actor execution requests; actor execution remains actor-local and Knowledge-scoped.

## 11. Minimal G5-03 observational seam

MW-002 may add one Scheduler `opportunity_finished`-equivalent signal so the Application can wake World Evolution only after the whole Agency opportunity reaches terminal.

This signal must carry frozen opportunity turn/hash and may not change:

- dirty ownership/consumption;
- semantic-terminal wake;
- selector 0..4 validation;
- actor concurrency;
- actor-private inputs;
- foreground invalidation;
- retry semantics.

## 12. Validation / route

Development is focused-first under `tests/g5_04/`.

After focused green run only minimal directly affected regressions:

- G5-01 semantic/context;
- G5-02 Knowledge boundary/context;
- G5-03 Scheduler/Cycle;
- G4-07 continuation/FirstOpening Context consumer;
- one G3-04 Save/Restore;
- `git diff --check`.

Real Provider calls = 0 for deterministic Engineering Acceptance.

Route:

```text
MW-002 implementation — Kimi
→ GPT actual-code Independent Review
→ if Engineering PASS: Owner G5-04 UAT
→ only Owner Product PASS may close G5-04
→ then G5-05
```

Owner UAT is mandatory because automated tests cannot prove the desired pacing balance.

UAT must include both:

1. a quiet/Life Loop situation where `hold` is the right result and no artificial escalation appears;
2. a genuinely ripe world pressure that advances outside direct Player causation and later enters GM Narrative naturally.

## 13. Protected non-scope

Do not prebuild:

- universal simulation engine;
- persistent pressure database;
- priority scoring platform;
- generic Faction identity/shared-Knowledge/agency;
- universal entity/relationship/event ontology;
- UI;
- SQLite schema solely for G5-04;
- offline simulation;
- Public d20/mechanics redesign;
- G5-05 work.

## 14. Routing

Through 2026-09-06 23:59 (+08:00): GPT owns semantics/architecture/task shaping/review; Kimi owns code-changing implementation.

Kimi's maximum return for MW-002 is:

`READY FOR INDEPENDENT REVIEW`
