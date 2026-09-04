---
title: my world｜当前状态
status: current-project-status
version: 12.8
created: 2026-08-26
updated: 2026-09-04
phase: G5 World Semantics & GM Runtime
current_task: G5-03M2A Stable Actor Registry Foundation — Revision 2 correction
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
G5-03M2A Registry Foundation                CORRECTION REQUIRED — REVISION 2 — KIMI
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

### M2A — current / Revision 2 correction

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

GPT Independent Review IR#1 inspected actual code/diff/test source/evidence and found the overall architecture substantially correct, but M2A is not yet PASS. Two bounded findings must be corrected:

1. `game_local_npcs.display_name/profile_text` must enforce raw string type instead of accepting non-string values through `String(...)` coercion;
2. the focused persistence proof must actually invoke the production Restore path and prove creation-time stable registry records / Program-owned IDs are preserved.

These findings do not change the M2A Outcome or architecture. The same legacy task remains active as **Revision 2**; no recursive `C01/R02` task ID is created.

Review evidence lives in the implementation repository:

`docs/g5_03/G5-03M2A_INDEPENDENT_REVIEW_IR1.md`

### M2B — mandatory next after M2A Independent Review PASS

`my-world/docs/tasks/G5-03M2B_RUNTIME_NARRATIVE_ACTOR_MATERIALIZATION_TASK.md`

M2B will extend the **existing** background semantic-analysis lane with optional independently fail-soft `new_actor_candidates` and allow a continuity-relevant Narrative-created person to receive a Program-owned durable Game-local identity without any Character Card.

No extra mandatory Provider call is added. Narrative acceptance remains independent of actor-extraction success.

M2 is not complete until both M2A and M2B PASS.

## 4. Task identity / lineage governance

Owner approved the cross-project dual-coordinate rule:

`governance/TASK_IDENTITY_AND_LINEAGE_V1_0.md`

```text
Roadmap / Capability Anchor
!= Executable Work Item ID
!= Revision / Review Lineage
```

Same-Outcome implementation defects stay on the same Work Item/task and advance `Revision` / `Review-Round`; genuinely distinct new outcomes, prerequisites, architecture seams, or Owner-inserted goals receive a new short flat Work Item ID with lineage metadata.

Legacy/in-flight IDs are not renamed for aesthetics. G5-03M2A therefore remains the same legacy task during Revision 2. New recursive suffix chains are prohibited going forward.

## 5. Protected semantics

- display name is never authoritative identity;
- model never mints final actor IDs;
- no fake Source provenance for no-Card actors;
- runtime Narrative actor identity is bound to exact accepted turn/hash so regenerate/correction can make stale history inert;
- stable registry membership grants no automatic knowledge and no automatic action;
- Agency v0.3 scheduling/order remains unchanged;
- no new SQLite table/migration, universal actor simulator, Faction agency, G5-04, UI, or mechanics expansion in M2.

## 6. Validation policy

Keep both M2 slices focused-first.

For M2A Revision 2:

- update the focused suite for strict raw-string rejection;
- add direct production Restore proof for registry/ID preservation;
- after focused green, rerun only directly affected Final Create validation / Save-Restore regressions unless production code outside those seams changes;
- run `git diff --check`;
- real Provider calls remain 0.

Do not restore the prior full-project regression matrix absent a concrete reason.

Parent real G5-03 feature proof remains honestly:

`PENDING / EXTERNAL PROVIDER UNAVAILABLE`

## 7. Next

M2A Revision 2 correction
→ GPT Independent Review IR#2
→ if PASS, M2B Runtime Narrative Actor Materialization
→ GPT decides whether a remaining Faction-agency slice is necessary before G5-03 closure
→ only then G5-04.

## 8. Routing

Through 2026-09-06 23:59 (+08:00): GPT owns architecture/review; Kimi owns code-changing implementation. Correct in-flight Kimi work may finish after expiry.
