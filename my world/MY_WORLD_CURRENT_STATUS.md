---
title: my world｜当前状态
status: current-project-status
version: 12.7
created: 2026-08-26
updated: 2026-09-04
phase: G5 World Semantics & GM Runtime
current_task: G5-03M2A Stable Actor Registry Foundation
current_owner: KIMI
parent_task: G5-03M2 Stable Actor Registry + Materialization
semantic_owner: GPT
owner_uat_required: false
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
G5-03 NPC / Faction Agency                  ACTIVE
G5-03M1 Multi-Actor Agency v0.3             ENGINEERING PASS / CLOSED
G5-03M2 Stable Actor Registry               ACTIVE
G5-03M2A Registry Foundation                ACTIVE — KIMI
G5-03M2B Runtime Narrative Materialization  PLANNED / REQUIRED AFTER M2A PASS
G5-04 Event / Priority Evolution            NOT YET
G5-GATE                                     NOT YET
```

## 2. Owner product correction

The prior M2 scope was too narrow because it treated pre-authored Character Cards as the only way to become a stable NPC.

Current product requirement:

```text
Guaranteed Source NPC
+ Source-backed NPC not manually enabled
+ Game-local NPC authored/enabled without a Character Card
+ NPC created naturally during runtime Narrative
→ all can become stable Game-local actors
```

A Character Card is a material source, not an identity prerequisite.

The current canonical decision is:

`architecture/world/G5_STABLE_ACTOR_REGISTRY_AND_MATERIALIZATION_V0_2_DECISION.md`

Historical `G5_STABLE_NPC_MATERIALIZATION_V0_1_DECISION.md` is superseded.

## 3. M2 execution split

To avoid another oversized implementation packet, M2 is mandatory but split into two reviewable slices.

### M2A — current

`my-world/docs/tasks/G5-03M2A_STABLE_ACTOR_REGISTRY_FOUNDATION_TASK.md`

M2A implements:

- automatic exact-profile Source-backed stable NPC snapshot at first new-Game intent;
- optional creation-time `game_local_npcs` for no-Card NPCs;
- Program-owned local IDs;
- honest Source-backed vs Game-local actor material;
- unified stable registry/material/roster helpers;
- G5-02/G5-03 consumers understand both material families;
- existing Game compatibility and no runtime Source retrofit;
- current-hash helper semantics prepared for runtime-origin actors.

### M2B — mandatory next after M2A Independent Review PASS

`my-world/docs/tasks/G5-03M2B_RUNTIME_NARRATIVE_ACTOR_MATERIALIZATION_TASK.md`

M2B will extend the **existing** background semantic-analysis lane with optional independently fail-soft `new_actor_candidates` and allow a continuity-relevant Narrative-created person to receive a Program-owned durable Game-local identity without any Character Card.

No extra mandatory Provider call is added. Narrative acceptance remains independent of actor-extraction success.

M2 is not complete until both M2A and M2B PASS.

## 4. Protected semantics

- display name is never authoritative identity;
- model never mints final actor IDs;
- no fake Source provenance for no-Card actors;
- runtime Narrative actor identity is bound to exact accepted turn/hash so regenerate/correction can make stale history inert;
- stable registry membership grants no automatic knowledge and no automatic action;
- Agency v0.3 scheduling/order remains unchanged;
- no new SQLite table/migration, universal actor simulator, Faction agency, G5-04, UI, or mechanics expansion in M2.

## 5. Validation policy

Keep both M2 slices focused-first.

M2A uses zero real Provider calls. Develop against the M2A focused suite; after green run one affected pass over Final Create, G5-02, G5-03, one relevant Save/Restore suite, and `git diff --check`.

Do not restore the prior full-project regression matrix absent a concrete reason.

Parent real G5-03 feature proof remains honestly:

`PENDING / EXTERNAL PROVIDER UNAVAILABLE`

## 6. Next

M2A Independent Review PASS
→ M2B Runtime Narrative Actor Materialization
→ GPT decides whether a remaining Faction-agency slice is necessary before G5-03 closure
→ only then G5-04.

## 7. Routing

Through 2026-09-06 23:59 (+08:00): GPT owns architecture/review; Kimi owns code-changing implementation. Correct in-flight Kimi work may finish after expiry.
