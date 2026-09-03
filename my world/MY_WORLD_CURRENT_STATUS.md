---
title: my world｜当前状态
status: current-project-status
version: 12.0
created: 2026-08-26
updated: 2026-09-03
phase: G5 World Semantics & GM Runtime
current_task: G5-03M1C01 Agency Currentness + Timeline Isolation Correction
current_owner: KIMI
parent_task: G5-03M1 Multi-Actor Agency Cycle
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
G5-03M1 old single-NPC task                 SUPERSEDED / DO NOT EXECUTE
G5-03M1 Multi-Actor Agency Cycle            CORRECTION REQUIRED
G5-03M1C01 Currentness + Timeline Isolation ACTIVE — KIMI
G5-03M2 Stable Actor Materialization        NOT YET
G5-04 Event / Priority Evolution            NOT YET
G5-GATE                                     NOT YET
```

Do not start G5-03M2/G5-04 before C01 Independent Review PASS.

## 2. Reviewed G5-03M1 implementation

Kimi implementation/evidence HEAD:

`3b5d104682f33f594cf72178a754ef044ff97469`

Formal review:

`my-world/docs/g5_03/G5-03M1_INDEPENDENT_REVIEW.md`

Result:

```text
CORRECTION REQUIRED / NOT ENGINEERING PASS YET
```

Accepted architecture:

```text
one existing semantic-analysis request
→ 0..4 validated stable NPC candidates
→ isolated concurrent actor executions
→ several sibling actor actions may durably commit in one Agency Cycle
```

The Owner correction against one-NPC-per-turn round-robin is preserved.

## 3. Blocking currentness findings

The multi-actor mechanism is sound, but asynchronous current-version/timeline isolation is incomplete.

Required correction:

1. **late semantic handoff** — Turn A semantic completion must not start Agency after foreground already began/advanced Turn B;
2. **Restore wiring** — production Restore/progress-switch callback must invalidate active uncommitted Agency work; the current focused test manually invalidates and therefore does not prove product wiring;
3. **commit-time guard** — every actor commit must verify current source Conversation/hash and distinguish same-cycle sibling head progression from unrelated head change;
4. **actor-local memory filtering** — selector/executor must exclude stale Knowledge Provenance and stale Agency History whose source GM hash no longer matches current accepted Conversation;
5. **same-turn replacement** — a new Narrative version at the same turn index must replace a stale agency cycle, not merge new action data into the old cycle hash;
6. **replay behavior** — already committed actor action must actually be skipped on replay/consideration; identity equality alone is not sufficient proof.

These are one seam:

> **Old world versions must never regain authority through asynchronous Agency work.**

## 4. Current correction packet

`my-world/docs/tasks/G5-03M1C01_AGENCY_CURRENTNESS_TIMELINE_ISOLATION_CORRECTION_TASK.md`

The correction must remain focused. Preserve:

- same-request Agency Selection;
- 0..4 multi-actor candidates;
- concurrent per-actor execution;
- serialized atomic commits;
- sibling head progression;
- fail-soft hold/malformed/provider failure;
- foreground player input never waits;
- no automatic Player/other-actor knowledge grant;
- no SQLite/UI/Faction/G5-04 scope.

## 5. Provider status

G5-03M1 real feature proof remains:

```text
PENDING / EXTERNAL PROVIDER UNAVAILABLE
```

The bounded parent attempt timed out during ordinary Narrative before feature-specific Agency proof.

**C01 must make zero real Provider calls.** Use deterministic/stubbed race, replacement and Restore proofs only.

## 6. G5-03M2 remains justified but blocked behind C01

Owner's Red-Cliffs example established that Guaranteed NPC cannot be the permanent Agency pool. After M1 Engineering PASS, the intended next slice remains:

```text
G5-03M2 Minimal Stable Actor Materialization / Registry Expansion
```

Purpose: allow important non-Guaranteed named actors such as Cao Cao / Zhuge Liang to become stable Game-local actors when sufficient canonical identity/material exists.

Do not implement M2 inside C01.

## 7. Temporary execution routing

Through 2026-09-06 23:59 (+08:00):

```text
GPT        → semantics / architecture / task shaping / final Independent Review
Kimi       → primary code-changing implementation owner
Grok Build → research/evidence support when useful
Owner      → Product UAT / explicit verdict
```

Auto-expiry 2026-09-07 00:00 (+08:00); correct in-flight Kimi work may finish.

## 8. Route

```text
Kimi G5-03M1C01
→ GPT Independent Review
→ if PASS, close G5-03M1 Engineering
→ shape G5-03M2 Stable Actor Materialization
```

Visual runtime remains deferred to G6.
