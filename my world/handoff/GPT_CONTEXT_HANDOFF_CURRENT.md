---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-04
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-03 NPC / Faction Agency
current_execution_task: G5-03M2 Stable NPC Materialization / Registry Expansion
semantic_owner: GPT
current_execution_owner: KIMI
---

# my world｜GPT CONTEXT HANDOFF CURRENT

Refresh both `main`s before authoritative work.

## Current state

```text
G5-01 World Turn / Semantic Materialization     PASS / CLOSED
G5-02 Knowledge Provenance                      PASS / CLOSED
G5-03 NPC / Faction Agency                      ACTIVE
G5-03M1 Multi-Actor Agency v0.3                 ENGINEERING PASS / CLOSED
G5-03M1R01C02 Dirty Opportunity Consumption     PASS / CLOSED
G5-03M1R02 Semantic-Terminal Wake Ownership     PASS / CLOSED
G5-03M2 Stable NPC Materialization              ACTIVE — KIMI
G5-04 Event / Priority Evolution                NOT YET
```

Gemini review remains cancelled. Flow remains Kimi implementation → GPT actual-code Independent Review.

## M1 closure

R02 implementation `d56ff094885c334a791c17429d76a1e21b7fd92d` is PASS.

Accepted Agency flow:

```text
ordinary accepted turn
→ semantic changes + knowledge
→ one dirty Agency opportunity
→ semantic settles
→ standalone selector over post-semantic current world
→ 0..4 stable actors
→ separate concurrent actor-scoped Agency executions
→ 0..N durable actions
```

Protected: player foreground never waits; selector omniscience does not become actor omniscience; actor execution receives only own frozen Source/Knowledge/history; sibling commits serialize; unrelated foreground/Restore invalidates uncommitted work; no same-opportunity retry; replay does not duplicate.

Parent real G5-03 Provider proof remains `PENDING / EXTERNAL PROVIDER UNAVAILABLE`.

## Current M2

Canonical decision:

`my world/architecture/world/G5_STABLE_NPC_MATERIALIZATION_V0_1_DECISION.md`

Implementation packet:

`my-world/docs/tasks/G5-03M2_STABLE_NPC_CREATION_SNAPSHOT_AND_REGISTRY_EXPANSION_TASK.md`

M2 reuses Final Create's existing Guaranteed-NPC identity/materialization template:

```text
Program-owned local_character_id
+ exact Source provenance
+ frozen Character T0 source_projection
```

On first creation-intent build, snapshot additional Character Cards whose existing T0 contract returns `exact_profile` for the selected exact World+Entry. Exclude Player and explicit Guaranteed asset IDs, deterministically sort, and store them under optional Game-local `stable_npcs[]`.

Same creation ID retry/resume reuses frozen `initial_setup`; never rescan later Source current. Existing Games missing `stable_npcs` remain valid with no retrofit or Source lookup.

Unified runtime semantics:

```text
stable_npc_records = guaranteed_npcs + stable_npcs
actor_roster       = Player + stable_npc_records
Agency eligibility = stable_npc_records only
```

Registry membership grants identity/material only, not knowledge or automatic actions.

Do not use display-name matching, model-minted IDs, runtime Source current, new SQLite schema/table, universal entity platform, Faction agency, or G5-04.

## Validation policy

M2 is deterministic and makes zero real Provider calls.

Use focused-first validation. After M2 focused green, run one affected pass only:

- G4-06 Final Create / creation integration;
- G5-03 focused;
- G5-02 focused;
- one relevant G3 Save/Restore suite;
- `git diff --check`.

Do not restore the prior full-project regression matrix absent a concrete reason.

## Next review focus

When Kimi returns, inspect actual code/evidence for:

1. exact-profile-only automatic snapshot;
2. Player/Guaranteed asset exclusion and deterministic IDs/order;
3. creation-intent retry stability despite Source-current changes;
4. no Source lookup for existing/ordinary runtime Games;
5. unified actor roster and Agency pool correctness;
6. automatic stable-NPC actor execution uses its own frozen Source/Knowledge/history;
7. no knowledge granted merely by registry inclusion;
8. Save/reopen/Restore stability;
9. no Faction/G5-04/schema/UI scope creep.

After M2 review, decide whether any Faction-agency slice remains necessary before G5-03 closure.
