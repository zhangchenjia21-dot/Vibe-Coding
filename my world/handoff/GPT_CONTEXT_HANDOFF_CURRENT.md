---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-03
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-03M1 Multi-Actor Agency Cycle
current_execution_task: G5-03M1C02 Semantic-vs-Agency Currentness Separation
semantic_owner: GPT
current_execution_owner: KIMI
---

# my world｜GPT CONTEXT HANDOFF CURRENT

Refresh both GitHub `main` branches before authoritative work.

## Current state

```text
G5-01 World Turn / Semantic Materialization     PASS / CLOSED
G5-02 Knowledge Provenance                      PASS / CLOSED
G5-03 NPC / Faction Agency                      ACTIVE
G5-03M1 Multi-Actor Agency Cycle                CORRECTION REQUIRED
G5-03M1C01 Currentness + Timeline Isolation     PARTIAL PASS / CLOSED INTO C02
G5-03M1C02 Semantic-vs-Agency Currentness       ACTIVE — KIMI
G5-03M2 Stable Actor Materialization            NOT YET
G5-04 Event / Priority Evolution                NOT YET
```

Canonical multi-actor decision:

`my world/architecture/world/G5_MULTI_ACTOR_AGENCY_CYCLE_V0_2_DECISION.md`

C01 implementation HEAD:

`a28ccc7688ca44bce91589badb129f90736cc603`

C01 review:

`my-world/docs/g5_03/G5-03M1C01_INDEPENDENT_REVIEW.md`

Current packet:

`my-world/docs/tasks/G5-03M1C02_SEMANTIC_AGENCY_CURRENTNESS_SEPARATION_CORRECTION_TASK.md`

## Accepted architecture / C01 passes

Do not redesign:

```text
existing semantic-analysis request performs selection
→ 0..4 validated stable NPC candidates
→ separate concurrent actor-scoped executions
→ multiple sibling actions may durably commit in one Agency Cycle
```

C01 also correctly established production Restore invalidation, commit-time expected-head/source guards, current-hash actor Knowledge/Agency History filtering, same-turn stale cycle replacement and replay skip of already committed actors.

## C02 seam

C01 introduced an adjacent regression by treating semantic materialization and Agency handoff as the same currentness predicate.

Protected distinction:

```text
semantic source version still accepted
!=
Agency handoff still current
```

If Turn A remains an accepted entry with the same GM hash, valid G5-01 changes / G5-02 knowledge may still materialize even if the player already started or accepted Turn B.

But A's `agency_candidates` must be suppressed once A is no longer latest or foreground is active.

An actual regenerate/correction that changes A's GM hash remains stale for both semantic truth and Agency.

## Selection-only requirement

A valid current semantic response may contain no new change/knowledge but still select actors:

```json
{"changes":[],"knowledge_events":[],"agency_candidates":["stable-npc-id"]}
```

This must not create a fake semantic mutation, but the successful terminal result must carry exact `source_turn_index + source_gm_sha256 + agency_candidates` so Application can authenticate and start the Agency Cycle.

Same applies when all parsed knowledge events are dropped by the actor allowlist and no semantic/knowledge record remains.

Application's defensive latest/hash/foreground-idle validation stays in place.

## Required C02 proofs

When Kimi returns, inspect actual production code/tests for:

1. fast Turn B foreground does not erase unchanged accepted Turn A changes/knowledge;
2. Turn A candidates are suppressed after foreground advances;
3. selection-only current turn carries exact source hash and starts Agency without fake semantic mutation;
4. all-invalid knowledge + valid candidates still starts current Agency;
5. actual source hash replacement remains zero old semantic/Agency commit;
6. all C01/M1 Restore/head/replay/concurrency/knowledge-isolation tests stay green;
7. no real Provider call and `git diff --check` PASS.

## Provider rule

C02 makes **zero real Provider calls**. Parent G5-03 real proof remains `PENDING / EXTERNAL PROVIDER UNAVAILABLE`.

## Next intended slice

Only after C02 passes and G5-03M1 Engineering closes, shape:

`G5-03M2 Minimal Stable Actor Materialization / Registry Expansion`

Purpose: important non-Guaranteed named actors such as Cao Cao / Zhuge Liang can later enter the same multi-actor Agency Cycle with stable Game-local identity/material.

Temporary Kimi code-routing remains effective through 2026-09-06 23:59 (+08:00); correct in-flight Kimi work may finish after expiry.
