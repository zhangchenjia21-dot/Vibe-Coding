---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-03
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-03M1 Multi-Actor Agency Cycle
current_execution_task: G5-03M1C01 Agency Currentness + Timeline Isolation Correction
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
G5-03M1C01 Currentness + Timeline Isolation     ACTIVE — KIMI
G5-03M2 Stable Actor Materialization            NOT YET
G5-04 Event / Priority Evolution                NOT YET
```

Canonical multi-actor decision:

`my world/architecture/world/G5_MULTI_ACTOR_AGENCY_CYCLE_V0_2_DECISION.md`

Reviewed implementation HEAD:

`3b5d104682f33f594cf72178a754ef044ff97469`

Independent Review:

`my-world/docs/g5_03/G5-03M1_INDEPENDENT_REVIEW.md`

Current correction:

`my-world/docs/tasks/G5-03M1C01_AGENCY_CURRENTNESS_TIMELINE_ISOLATION_CORRECTION_TASK.md`

## Accepted M1 architecture

Do not redesign these passing semantics:

```text
existing semantic-analysis request
→ 0..4 validated stable NPC candidates
→ separate per-actor execution requests
→ actor requests may progress concurrently
→ multiple valid sibling actions may commit in one Agency Cycle
```

No one-NPC round-robin fallback.

## C01 blocking seam

C01 fixes one theme: **old/superseded world versions must not regain authority through asynchronous Agency work**.

Required fixes:

- a Turn A semantic result cannot start Agency after foreground has already begun/advanced Turn B;
- production Restore/progress switch automatically invalidates remaining uncommitted Agency work;
- each actor commit checks current source Conversation/hash and distinguishes sibling cycle head progression from unrelated head changes;
- selector/executor include only current-hash-matching actor Knowledge Provenance and Agency History;
- a new Narrative version at the same turn index replaces a stale Agency Cycle instead of merging into the old cycle hash;
- an actor already committed in a matching cycle is skipped on replay without a new execution request/mutation.

Preserve foreground freedom, actor knowledge isolation, concurrent execution, atomic world commits and fail-soft behavior.

## Provider rule

C01 makes **zero real Provider calls**. The parent real G5-03 feature proof remains `PENDING / EXTERNAL PROVIDER UNAVAILABLE` after its bounded attempt timed out before feature proof.

## Review when Kimi returns

Verify actual code/evidence for the six fixes above, plus regression coverage for G5-02/G5-01/G3/G4 and `git diff --check`. Do not accept evidence-only assertions where production wiring is the issue.

## Next intended slice

After C01 and M1 Engineering PASS, shape:

`G5-03M2 Minimal Stable Actor Materialization / Registry Expansion`

This is justified by the Owner's requirement that important non-Guaranteed named actors such as Cao Cao / Zhuge Liang can later enter the same multi-actor Agency Cycle.

Temporary Kimi code-routing remains effective through 2026-09-06 23:59 (+08:00); correct in-flight Kimi work may finish after expiry.
