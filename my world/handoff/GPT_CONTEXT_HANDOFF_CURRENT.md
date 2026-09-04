---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-04
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-03M2 Stable Actor Registry + Materialization
current_execution_task: G5-03M2A Stable Actor Registry Foundation
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
G5-03M2 Stable Actor Registry                   ACTIVE
G5-03M2A Registry Foundation                    ACTIVE — KIMI
G5-03M2B Runtime Narrative Materialization      PLANNED / REQUIRED AFTER M2A PASS
G5-04 Event / Priority Evolution                NOT YET
```

Gemini review remains cancelled. Flow remains Kimi implementation → GPT actual-code Independent Review.

## Owner correction that governs M2

Do not equate “stable NPC” with “pre-authored Character Card”. The Game must be able to keep and later act through characters created locally inside the Game.

Required stable NPC origins:

```text
1. Guaranteed Source NPC (existing)
2. automatic Source-backed NPC not explicitly enabled
3. creation-authored Game-local NPC without Character Card
4. runtime Narrative-materialized NPC without Character Card
```

Canonical:

`my world/architecture/world/G5_STABLE_ACTOR_REGISTRY_AND_MATERIALIZATION_V0_2_DECISION.md`

Historical source-only v0.1 decision and packet are superseded.

## Current M2A

Execute:

`my-world/docs/tasks/G5-03M2A_STABLE_ACTOR_REGISTRY_FOUNDATION_TASK.md`

M2A builds only the registry foundation:

- Final Create optional additive `game_local_npcs`, missing -> `[]`;
- automatic exact-profile Source-backed stable NPC snapshot;
- Program-owned local IDs for both Source-backed and no-Card creation-authored NPCs;
- Source-backed records keep exact provenance + frozen T0 projection;
- no-Card records keep honest `game_local_material` and never fake Source provenance;
- unified helpers for stable NPC enumeration, actor material and actor roster;
- G5-02 and G5-03 consumers understand both material families;
- existing Games without `stable_npcs` remain valid and never retrofit from Source current;
- helper API already supports accepted turn->GM hash currentness for future runtime-origin records.

Do not implement semantic `new_actor_candidates` in M2A.

## Mandatory M2B after M2A review

Preplanned packet:

`my-world/docs/tasks/G5-03M2B_RUNTIME_NARRATIVE_ACTOR_MATERIALIZATION_TASK.md`

M2B is not optional. It will reuse the existing post-Narrative semantic-analysis call and add optional independently fail-soft `new_actor_candidates`.

Protected runtime path:

```text
accepted Narrative establishes a continuity-relevant new person
→ existing semantic lane optionally describes candidate material
→ Program mints authoritative local_character_id
→ same semantic durable mutation records actor
→ origin binds source_turn_index + source_gm_sha256
→ semantic terminal wake runs existing Agency selector
→ newly materialized NPC may become independently active immediately
```

Malformed/absent actor extraction must never invalidate Narrative, valid changes, or valid knowledge. No extra mandatory Provider call.

Runtime-origin actors are visible only when their origin turn/hash still matches current accepted Conversation, so regenerate/correction makes erased actors inert without deleting history.

## Stable actor boundaries

- display names are labels, never authoritative IDs;
- model never mints final IDs;
- no fake Source provenance;
- registry membership grants identity/material only, not automatic knowledge/action;
- no runtime Source-current lookup;
- no universal entity platform;
- no Faction/G5-04/UI/Public-d20/SQLite scope in M2.

## M2A review focus

When Kimi returns, inspect actual code/evidence for:

1. optional `game_local_npcs` backward compatibility;
2. exact-profile Source snapshot and Player/Guaranteed exclusion;
3. no-Card actor gets Program ID + Game-local material with no fake provenance;
4. duplicate display names remain distinct identities;
5. retry/resume freezes both Source-backed and authored actors;
6. old Games do not consult Source;
7. unified roster/Agency eligibility and actor-material resolution;
8. actor-private Knowledge/history isolation;
9. synthetic runtime-origin current-hash filtering helper behavior;
10. Save/reopen/Restore and slim regression evidence.

After M2A PASS, activate M2B. M2 is not complete before M2B PASS.

Parent real G5-03 Provider proof remains `PENDING / EXTERNAL PROVIDER UNAVAILABLE`.
