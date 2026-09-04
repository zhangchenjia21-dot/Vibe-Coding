---
title: my world｜当前状态
status: current-project-status
version: 12.9
created: 2026-08-26
updated: 2026-09-04
phase: G5 World Semantics & GM Runtime
current_task: MW-001 Runtime Narrative Actor Materialization
current_owner: KIMI
parent_task: G5-03 Stable Actor / Agency line
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
G5-03M2A Registry Foundation                ENGINEERING PASS / CLOSED
MW-001 Runtime Narrative Actor Materialization ACTIVE — KIMI
  Capability Anchor                         G5-03
  Legacy Planning Ref                       G5-03M2B
G5-04 Event / Priority Evolution            NOT YET
G5-GATE                                     NOT YET
```

Parent real G5-03 Provider proof remains honestly:

`PENDING / EXTERNAL PROVIDER UNAVAILABLE`

Do not switch Provider merely to manufacture PASS evidence.

## 2. Stable Actor product requirement

Current canonical rule:

```text
Guaranteed Source NPC
+ automatic Source-backed NPC
+ creation-authored Game-local NPC without a Card
+ runtime Narrative-materialized NPC without a Card
→ one Game-local stable actor system with Program-owned identity
```

A Character Card is a material source, not an identity prerequisite.

Canonical decision:

`architecture/world/G5_STABLE_ACTOR_REGISTRY_AND_MATERIALIZATION_V0_2_DECISION.md`

Historical `G5_STABLE_NPC_MATERIALIZATION_V0_1_DECISION.md` remains superseded.

## 3. M2A closeout

`G5-03M2A Stable Actor Registry Foundation` is now **ENGINEERING PASS / CLOSED** after GPT Independent Review IR#2.

Implementation review confirmed the foundation supports:

- first-intent automatic exact-profile Source-backed stable NPC snapshot;
- creation-authored no-Card stable NPC ingress;
- strict bounded raw-string actor material;
- Program-owned local IDs;
- honest Source-backed vs Game-local material families;
- unified stable registry/material/roster helpers;
- G5-02/G5-03 consumers using exact local identity;
- existing-Game compatibility with no mutable Source retrofit;
- accepted-hash currentness contract for future runtime actors;
- Save/reopen/Restore preservation of the opaque World snapshot containing stable actor records.

IR#2 evidence is in the implementation repository:

`docs/g5_03/G5-03M2A_INDEPENDENT_REVIEW_IR2.md`

M2A remains a legacy task identity for historical continuity; it is not renamed retroactively.

## 4. Current executable Work Item — MW-001

Current repository-native packet:

`my-world/docs/tasks/G5-03M2B_RUNTIME_NARRATIVE_ACTOR_MATERIALIZATION_TASK.md`

Identity:

```text
Work Item: MW-001
Name: Runtime Narrative Actor Materialization
Capability-Anchor: G5-03
Legacy Planning Ref: G5-03M2B
Revision: 1
Review-Round: 0
Owner: Kimi
Reviewer: GPT
```

This is the first new flat Work Item minted under:

`governance/TASK_IDENTITY_AND_LINEAGE_V1_0.md`

The roadmap position remains G5-03; execution identity is MW-001; review/correction history will live in Revision / Review-Round metadata rather than recursive suffixes.

## 5. MW-001 frozen implementation direction

MW-001 adds runtime Narrative actor ingress on top of the closed M2A registry foundation.

Required semantics:

- extend the **existing** post-Narrative semantic lane with optional `new_actor_candidates`;
- no additional mandatory Provider call;
- candidate field is independently fail-soft and cannot invalidate otherwise valid `changes` / `knowledge_events`;
- semantic model owns open-ended continuity relevance judgment; no keyword/score/first-mention gate;
- candidate carries bounded descriptive `display_name` / `profile_text` only;
- Program, not model, mints deterministic exact `local_character_id`;
- runtime actor origin binds `source_turn_index + source_gm_sha256`;
- model sees the current exact stable roster and is told not to repropose already-stable actors;
- no display-name authoritative dedupe;
- valid runtime actors are added through the **same single semantic durable mutation** as changes/knowledge;
- actor-only semantic materialization is valid without fake changes;
- same accepted-version replay, including actor-only replay, must not duplicate actors;
- stale runtime-origin actors remain physically historical but are filtered out of current roster/Agency by accepted hash;
- materialization grants no automatic Knowledge or action;
- after semantic commit, the existing semantic-terminal Agency wake may see/select the actor in the same player-turn opportunity;
- old semantic `agency_candidates` must not be reactivated; standalone Agency Scheduler v0.3 remains authoritative.

## 6. Protected semantics / scope ceiling

Do not:

- require Character Cards for runtime actors;
- use display names as identity;
- let models mint final actor IDs;
- invent Source provenance for no-Card actors;
- read mutable Source current during ordinary gameplay / semantic materialization / Continue / Save / Restore / Agency;
- gate accepted visible Narrative on actor extraction;
- add another mandatory actor-discovery Provider call;
- grant Knowledge merely from registry membership;
- introduce a universal entity/faction simulation ontology;
- add UI;
- add SQLite table/schema/migration solely for actor materialization;
- change Multi-Actor Agency v0.3 dirty / foreground / 0..4 / concurrency semantics;
- implement Faction agency or G5-04;
- change Public d20/mechanics.

## 7. Validation policy

Keep MW-001 focused-first.

Development:

- deterministic `tests/g5_03m2b/` only.

After focused green, one minimal affected pass:

- directly affected G5-01/G5-02 semantic/parser tests;
- one relevant G5-03 Scheduler/Cycle regression;
- one G3-04 Save/Restore regression;
- `git diff --check`.

No unrelated full-project/UI/G4/Public-d20 matrix without a concrete reason.

Use deterministic semantic stubs; real Provider calls are not required for MW-001 acceptance.

## 8. Next

```text
MW-001 implementation
→ GPT actual-code Independent Review
→ if PASS: G5-03M2 complete
→ GPT determines whether any separate Faction-agency Outcome is still required for G5-03 closure
→ only after G5-03 sequencing is explicitly closed may G5-04 start
```

## 9. Routing

Through 2026-09-06 23:59 (+08:00): GPT owns architecture/task shaping/review; Kimi owns code-changing implementation. Correct in-flight Kimi work may finish after expiry.
