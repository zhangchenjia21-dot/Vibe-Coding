---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-03
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-03 NPC / Faction Agency
current_execution_task: G5-03M1 Stable Guaranteed-NPC Independent Agency Vertical
semantic_owner: GPT
current_execution_owner: KIMI
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4 Primary Source Assets & Local Game Creation   PASS / CLOSED
G5 World Semantics & GM Runtime                  ACTIVE
G5-01 World Turn / Semantic Materialization     PASS / CLOSED
G5-02 Knowledge Provenance                      PASS / CLOSED
G5-02M1 / C01                                   PASS / CLOSED
G5-03 NPC / Faction Agency                      ACTIVE
G5-03M1 Stable Guaranteed-NPC Agency             ACTIVE — KIMI
G5-04 Event / Priority Evolution                NOT YET
```

Do not start G5-04 before G5-03M1 Independent Review and GPT decides whether a later Faction slice is required for G5-03 closeout.

## 2. G5-02 final state

Final review:

`my-world/docs/g5_02/G5-02M1C01_INDEPENDENT_REVIEW.md`

Closeout:

`my-world/docs/g5_02/G5-02_CLOSEOUT.md`

Accepted semantics:

```text
Game / World Truth
!= actor knowledge
!= human-player disclosure
!= omniscient GM Context
```

The same existing G5-01 semantic-analysis request now receives the stable Player/Guaranteed-NPC name + exact local-ID roster and may emit bounded post-T0 `knowledge_events` adjacent to `changes`.

Knowledge failure does not invalidate valid changes or accepted Narrative; unknown actor IDs are discarded; one semantic result uses at most one atomic world mutation; later GM Context gets bounded recent Actor Knowledge Provenance.

The single bounded G5-02 real-provider attempt timed out before feature execution and remains a historical external-provider gap. Do not retry merely to rewrite history; G5-03 is the first product consumer of the boundary.

## 3. Current G5-03 authority

Canonical decision:

`my world/architecture/world/G5_STABLE_NPC_AGENCY_V0_1_DECISION.md`

Task packet:

`my-world/docs/tasks/G5-03M1_STABLE_NPC_INDEPENDENT_AGENCY_TASK.md`

Core product question:

> Can one stable Guaranteed NPC act independently from its own Source + durable Knowledge Provenance rather than from omniscient GM knowledge, without blocking the player's foreground loop?

## 4. M1 scope

Only stable `guaranteed_npcs[*].local_character_id` actors.

No autonomous Player Character, incidental/emergent NPC identity, Faction identity/agency, universal actor graph or G5-04 priority scheduler.

If multiple Guaranteed NPCs exist, one actor per source turn may be selected deterministically/round-robin for **evaluation**. The selected NPC may still return `hold`.

## 5. Foreground ordering

```text
Player action
→ visible free-form GM Narrative
→ durable Conversation acceptance
→ existing semantic lane terminal
→ optional background NPC agency evaluation
→ optional agency world mutation
```

Agency must not become a Turn Finalize Barrier.

If the player starts the next Conversation attempt, queued/active agency is cancelled/invalidated. A late callback after invalidation cannot commit. Provider/parse/hold/cancel failure is fail-soft, with no retry loop or fallback.

## 6. Actor-scoped cognition

The selected NPC agency request may see only:

- its exact stable ID/name;
- its exact Game-local Character Source projection / selected T0 material;
- its own current committed G5-02 Knowledge Provenance;
- its own bounded recent agency history.

It must not receive full omniscient GM/world Context or another actor's private post-T0 knowledge as a shortcut.

This is the direct consumer that makes G5-02 meaningful.

## 7. Durable agency semantics

A valid `decision=act` writes one bounded agency record into existing `living_world` using the existing atomic world mutation seam.

Identity must bind game + source turn/hash + source head + actor.

A `hold` writes no fake mutation.

Later GM Context may project bounded Independent Actor Actions as world truth. Those actions/effects do not automatically become all actors' knowledge and do not automatically equal human-player disclosure.

No SQLite schema/table.

## 8. Race safety

Agency result is stale and may not commit if, after request start:

- foreground Conversation attempt starts/advances;
- active world head changes;
- source accepted GM hash changes;
- Restore/Recovery switches progress;
- session closes.

Use existing Conversation `attempt_started` + Runtime `restore_completed` or equivalent narrow seams. Late callbacks must be guarded even if transport cancellation exists.

## 9. Required proofs

At minimum inspect/verify when Kimi returns:

1. actor-scoped request contains selected NPC Source + own knowledge and excludes Player/other-NPC private knowledge;
2. valid `act` makes one atomic durable agency mutation and later GM Context sees it;
3. `hold`/malformed/wrong actor produce zero mutation;
4. no Guaranteed NPC produces no agency request;
5. multi-NPC selection is one-per-turn and deterministic/fair;
6. foreground next-turn start invalidates active agency and late callback cannot commit;
7. Restore invalidates active agency and late callback cannot commit;
8. replay/reopen does not duplicate and preserves Context;
9. no UI/SQLite schema/Source/G5-04+/Faction scope creep.

## 10. Real Provider rule

After offline/integration gates are green, at most one task-owned real selected-Provider **agency** request is allowed under standing authorization.

Prefer stubbed unrelated prerequisites so the real call targets agency itself.

Feature-specific PASS requires a valid stable-actor `act`, durable agency record/effect and later GM Context projection.

A `hold`, malformed response, timeout or outage is pending/inconclusive. No retry-until-act, fallback or hidden provider switch.

## 11. Temporary routing

Through 2026-09-06 23:59 (+08:00):

```text
GPT        → semantics / architecture / governance / final Independent Review
Kimi       → code-changing implementation owner
Grok Build → external research/evidence support only if useful
Owner      → Product UAT / explicit verdict
```

Auto-expiry: 2026-09-07 00:00 (+08:00). Correct in-flight Kimi work may finish under its issued packet.

## 12. After Kimi returns

Refresh both mains and inspect actual code/evidence, not self-report.

If M1 passes, GPT decides from the proven consumer whether parent G5-03 needs a second Faction identity/agency slice before closeout. Do not automatically jump to G5-04 from memory.
