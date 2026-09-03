---
title: my world｜当前状态
status: current-project-status
version: 11.8
created: 2026-08-26
updated: 2026-09-03
phase: G5 World Semantics & GM Runtime
current_task: G5-03M1 Stable Guaranteed-NPC Independent Agency Vertical
current_owner: KIMI
parent_task: G5-03 NPC / Faction Agency
semantic_owner: GPT
owner_uat_required: false
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G3 Persistence / Save / Timeline      PASS / CLOSED
G4 Primary Source Assets & Local Game Creation
                                      PASS / CLOSED
G4-10 Runtime Asset Resolution        DEFERRED / MOVED TO G6
G4-11 Two Primary Asset Families      PASS / CLOSED
G4-11C01 Narrative Voice Soft Prompt  PASS / CLOSED
G4-GATE                               PASS

G5 World Semantics & GM Runtime       ACTIVE
G5-01 World Turn / Semantic Materialization
                                      PASS / CLOSED
G5-02 Knowledge Provenance            PASS / CLOSED
G5-02M1 Known-Actor Knowledge Spine   ENGINEERING PASS / CLOSED
G5-02M1C01 Actor Roster + Recent Knowledge Projection
                                      PASS / CLOSED
G5-02 real Provider vertical          HISTORICAL GAP / EXTERNAL PROVIDER UNAVAILABLE

G5-03 NPC / Faction Agency            ACTIVE
G5-03M1 Stable Guaranteed-NPC Agency  ACTIVE — KIMI
G5-04 Event / Priority Evolution      NOT YET
G5-GATE                               NOT YET
```

Do not start G5-04 before G5-03M1 Independent Review and GPT decides whether a second Faction slice is required for G5-03 closeout.

## 2. G5-02 closeout

Formal closeout:

`my-world/docs/g5_02/G5-02_CLOSEOUT.md`

Final review:

`my-world/docs/g5_02/G5-02M1C01_INDEPENDENT_REVIEW.md`

Accepted:

- same existing semantic-analysis request now receives exact stable actor name/local-ID roster;
- unknown/non-roster actors cannot become durable knowers;
- Knowledge parsing remains isolated from valid G5-01 `changes`;
- knowledge + changes share at most one atomic world mutation;
- bounded Actor Knowledge Context now retains newest matching knowledge rather than oldest forever;
- real-provider harness can no longer false-pass with empty knowledge;
- no SQLite schema/Source/UI/G5-03 scope was introduced by G5-02.

The single bounded G5-02 real selected-Provider attempt timed out in ordinary Narrative before feature execution. This remains an honest historical gap; no second attempt/fallback occurred. A separate G5-02 Owner UAT is not required because the first materially player-visible consumer is G5-03 actor agency.

## 3. Current G5-03 authority

Canonical decision:

`architecture/world/G5_STABLE_NPC_AGENCY_V0_1_DECISION.md`

Implementation packet:

`my-world/docs/tasks/G5-03M1_STABLE_NPC_INDEPENDENT_AGENCY_TASK.md`

Core product question:

> Can a stable Guaranteed NPC independently choose and undertake an action from its own Character Source + durable Knowledge Provenance, without explicit player prompting and without blocking the foreground Narrative loop?

Protected principle:

> **Source provides inertia; actors create history.**

## 4. G5-03M1 actor scope

First slice covers stable current Game-local Guaranteed NPCs only.

```text
guaranteed_npcs[*].local_character_id
```

Player Character is not autonomously controlled by this lane.

No incidental/emergent NPC identity, Faction identity/agency, settlement/group actor or universal entity graph in M1.

If multiple Guaranteed NPCs exist, M1 may use deterministic round-robin one-actor-per-source-turn **evaluation**. This is not the G5-04 pressure/priority scheduler.

## 5. Background agency contract

Protected ordering:

```text
Player action
→ free-form visible GM Narrative
→ durable Conversation acceptance
→ existing G5-01/G5-02 semantic lane terminal
→ optional background Stable-NPC agency evaluation
→ optional atomic agency world mutation
```

Agency is not a Turn Finalize Barrier.

The player may immediately begin the next action. If foreground Conversation starts while agency is queued/active, agency is cancelled/invalidated instead of delaying Narrative. Late callbacks after invalidation must not commit.

Provider/parse/hold/cancel failure is fail-soft: no fake mutation, no Narrative failure, no automatic retry, no fallback.

## 6. Actor-scoped cognition

Agency request for selected NPC may receive only bounded actor-local material:

- exact selected NPC ID/name;
- that NPC's exact Game-local Character Source projection / selected T0 profile;
- that NPC's own committed/current G5-02 Knowledge Provenance;
- that NPC's own recent durable agency history.

Do not pass full omniscient GM/world Context, Player private post-T0 knowledge or another NPC's private knowledge merely because the GM can see it.

This is the first direct consumer proving:

```text
GM knowledge != NPC knowledge
```

## 7. Durable agency shape

A valid `decision=act` may persist a bounded record under existing `world_state.living_world`, tied to:

`game_id + source turn + accepted GM hash + source world head + actor ID`.

Use existing `commit_world_mutation_durably(...)`; no SQLite table/schema.

A `hold` creates no mutation.

Later omniscient GM Context may receive bounded `Independent Actor Actions` as world truth. This does not automatically grant other actors knowledge and does not automatically disclose the hidden action to the human player.

## 8. Stale/race safety

Before agency commit, invalidate if:

- new foreground Conversation attempt starts;
- accepted Conversation advances;
- active world head changes;
- source GM hash changes;
- Save Restore / Recovery switches progress;
- session closes.

Use existing Conversation `attempt_started` and Runtime `restore_completed` or equivalent narrow seams. Best-effort transport cancellation alone is not sufficient; late callback must also fail closed.

## 9. Real Provider validation

Standing authorization applies.

After all deterministic/integration gates are green, G5-03M1 may spend at most **one** task-owned real selected-Provider agency request. Prefer stubbing unrelated Opening/Narrative/semantic prerequisites so the bounded call is spent on agency itself.

Feature-specific PASS requires valid `act` from the selected stable NPC, one durable agency record/effect, and later GM Context projection.

`hold`, malformed output, timeout or outage is inconclusive/pending. No loop-until-act, no fallback, no hidden model switch.

## 10. Temporary execution routing｜through 2026-09-06

```text
GPT        → semantics / architecture / task shaping / final Independent Review
Kimi       → primary code-changing implementation owner, temporarily substituting for Codex
Grok Build → external research / evidence support when useful
Owner      → Product UAT / explicit verdict
```

The override auto-expires at 2026-09-07 00:00 (+08:00). Correct in-flight Kimi work may finish under its issued packet.

## 11. Route

```text
Kimi G5-03M1 implementation
→ GPT Independent Review
→ decide from actual consumer whether a Faction slice is still required inside G5-03
→ only then move toward G5-04 Event / Priority Evolution
```

Visual runtime remains deferred to G6.
