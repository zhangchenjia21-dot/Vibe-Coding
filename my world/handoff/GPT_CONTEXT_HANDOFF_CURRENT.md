---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-03
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-01 Minimum Playable T0 + World Turn / Semantic Materialization
current_execution_task: G5-01M1 World Turn / Semantic Materialization Spine
semantic_owner: GPT
current_execution_owner: CODEX
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4 Primary Source Assets & Local Game Creation
                                      PASS / CLOSED
G4-10 Runtime Asset Resolution        DEFERRED / MOVED TO G6
G4-11 Two Primary Asset Families      PASS / CLOSED
G4-11C01 Narrative Voice Soft Prompt  PASS / CLOSED
G4-GATE                               PASS

G5 World Semantics & GM Runtime       ACTIVE
G5-01 Minimum Playable T0 + World Turn / Semantic Materialization
                                      ACTIVE
G5-01M1 Semantic Materialization Spine ACTIVE — CODEX
G5-02 Knowledge Provenance            NOT YET
G5-03 NPC / Faction Agency            NOT YET
G5-04 Event / Priority Evolution      NOT YET
```

Do not start G5-02 before G5-01 Independent Review + Owner product checkpoint.

## 2. G4 final truth

Owner confirmed the two G4-11 worlds materially feel different. Narrative prose voice convergence was a non-blocking quality finding.

G4-11C01 added one generic shared-GM soft creative instruction only. GPT Independent Review passed it:

`my-world/docs/g4_11/G4-11C01_INDEPENDENT_REVIEW.md`

Protected result:

```text
Narrative style is guidance, not an acceptance gate.
```

No Source migration, Provider change, style classifier, keyword gate or style-triggered reject/retry was introduced. Product effect will be observed opportunistically in the next suitable Owner UAT.

## 3. Visual deferral remains protected

```text
G4-10 Runtime Asset Resolution = DEFERRED / MOVED TO G6
```

Do not implement portrait / scene / authored-map runtime during G5. Visual presentation is not World/Character/Knowledge/Event authority.

## 4. G5-01 canonical decision

Read:

`my world/architecture/world/G5_WORLD_TURN_SEMANTIC_MATERIALIZATION_V0_1_DECISION.md`

Core transition:

> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

Ordering:

```text
free-form visible Narrative
→ durable Conversation acceptance
→ separate background semantic analysis
→ optional atomic World Turn mutation
```

Narrative must never contain G5 machine framing.

Semantic extraction is auxiliary/best-effort in v0.1. Analysis failure does not invalidate the accepted turn and does not block further play.

## 5. G5-01M1 task

Packet:

`my-world/docs/tasks/G5-01M1_WORLD_TURN_SEMANTIC_MATERIALIZATION_TASK.md`

Owner: **Codex**.

Required mechanism:

- trigger only from durably accepted ordinary player/GM turns;
- skip GM-only Opening in v0.1;
- one isolated selected-provider semantic request per new accepted turn;
- no Provider fallback / hidden model switch;
- valid non-empty `changes[]` → Program-owned World Turn → existing atomic `commit_world_mutation_durably(...)`;
- malformed/transport/empty semantic analysis → no mutation, accepted Conversation remains accepted;
- no automatic repair loop;
- later Context can project committed matching changes;
- stale record after regenerate/correction is excluded when its GM hash no longer matches current accepted Conversation;
- same accepted-content replay is idempotent;
- Save/Restore/Timeline remain coherent.

Conceptual current namespace:

```text
living_world
  schema_version = living_world.v0.1
  semantic_turns_by_index
    <turn_index>
      world_turn_id
      source_turn_index
      source_gm_sha256
      materialized_at
      changes[]
```

This is a turn-level consequence ledger, not a universal entity/fact ontology.

## 6. Existing seams to reuse

Current implementation already owns:

- exact T0 Source materialization;
- durable Conversation;
- Timeline/current head;
- `Current Game Session Runtime.commit_world_mutation_durably(...)`;
- Save/Restore/Recovery;
- one Game = one SQLite.

Do not create a second persistence owner/table unless a concrete blocker is returned to GPT.

## 7. G5-01 protected non-scope

Do not implement early:

- G5-02 Knowledge Provenance;
- G5-03 NPC/Faction autonomous agency;
- G5-04 Event/Priority evolution;
- universal graph/ontology;
- G6 UI/visual runtime;
- G7 long-session retrieval platform.

Do not modify `src/ui/**` or SQLite schema silently. Stop and return a boundary finding if such change proves necessary.

## 8. Independent Review focus after Codex returns

GPT must inspect actual code/evidence and especially verify:

1. accepted Narrative remains independent of semantic-analysis success;
2. production does not parse/control visible prose;
3. exactly one auxiliary semantic request per new accepted ordinary turn;
4. valid result commits one atomic world mutation;
5. invalid/transport analysis creates no fake mutation and no player-action failure;
6. correction/regenerate cannot project stale old semantic record;
7. replay does not duplicate World Turns;
8. Save/Restore/reopen preserve coherent Conversation + semantic snapshot;
9. Context projection is bounded and only uses committed matching records;
10. no Source/Provider-fallback/UI/G5-02+ scope creep;
11. real selected-provider proof is genuine if authorized;
12. Owner production state remains untouched.

## 9. After M1

If Independent Review passes:

```text
G5-01UAT short Owner checkpoint
```

Owner should establish one simple persistent consequence, continue/reopen, and judge whether the world reliably remembers it while Narrative remains free-form. The deferred narrative-voice soft-prompt effect may be observed in the same UAT, but is not a separate pass/fail gate.

After Owner PASS:

```text
G5-01 PASS / CLOSED
→ GPT shapes G5-02 Knowledge Provenance
```
