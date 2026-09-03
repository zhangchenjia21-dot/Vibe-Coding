---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-03
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-02 Knowledge Provenance
current_execution_task: G5-02M1 Known-Actor Knowledge Provenance Spine
semantic_owner: GPT
current_execution_owner: KIMI
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
G5-01 World Turn / Semantic Materialization
                                      PASS / CLOSED
G5-01M1 Semantic Materialization Spine ENGINEERING PASS / CLOSED
G5-01M1C02 Restore Timeline Isolation CANCELLED / DO NOT EXECUTE
G5-02 Knowledge Provenance            ACTIVE
G5-02M1 Known-Actor Knowledge Spine   ACTIVE — KIMI
G5-03 NPC / Faction Agency            NOT YET
G5-04 Event / Priority Evolution      NOT YET
```

Do not start G5-03 before G5-02 Independent Review/closeout.

## 2. G5-01 closeout / Owner decision

Formal closeout:

`my-world/docs/g5_01/G5-01_CLOSEOUT.md`

Owner explicitly declined another dedicated G5-01 UAT, stating prior actual play already gave sufficient confidence that the world remembers player-choice consequences, and instructed the project to move directly to the next task.

Therefore G5-01 is PASS/CLOSED by explicit Owner progression decision after Engineering PASS.

The dedicated M1 real-provider attempt remains historically unproven because two bounded Kimi K3 ordinary-Narrative requests timed out before the semantic lane executed. Do not rewrite that evidence as PASS, but do not reopen G5-01 merely to manufacture retrospective proof.

The previously proposed `G5-01M1C02` Restore exact-replay correction is cancelled/do-not-execute. The edge requires same restored future turn index + byte-identical GM Narrative hash and needs later branch/result-reuse semantics if a real consumer ever exposes it.

## 3. Current G5-02 authority

Canonical decision:

`my world/architecture/world/G5_KNOWLEDGE_PROVENANCE_V0_1_DECISION.md`

Implementation packet:

`my-world/docs/tasks/G5-02M1_KNOWN_ACTOR_KNOWLEDGE_PROVENANCE_TASK.md`

Core distinction:

```text
Game / World Truth
!= actor knowledge
!= human-player disclosure
!= omniscient GM model context
```

Protected principle:

> **GM omniscience must not become actor omniscience.**

Current Source `disclosure: gm_reference` remains GM/reference metadata. It must not be interpreted as “all actors know this section.”

## 4. G5-02M1 bounded actor scope

Only post-T0 knowledge acquisition for stable current Game-local actors:

```text
player_character.local_character_id
+
guaranteed_npcs[*].local_character_id
```

No emergent/incidental NPC identity, Faction knowledge, group knowledge or generic entity graph.

Starting Source is not converted into a giant knowledge database; v0.1 makes post-T0 acquisition durable first.

## 5. Reuse one semantic request

Do not add a second Provider tax per accepted turn.

Extend the existing G5-01 auxiliary request conceptually to:

```json
{
  "changes": ["..."],
  "knowledge_events": [
    {
      "knower_id": "stable-local-id",
      "fact": "...",
      "basis": "witnessed|told|discovered|participated"
    }
  ]
}
```

Knowledge validation must be isolated so bad/absent knowledge data cannot invalidate an otherwise valid G5-01 `changes` result.

Unknown actor IDs never become durable knowledge authority.

## 6. Durable + Context semantics

Knowledge provenance lives inside the existing game-local world document under `living_world`, adjacent to G5-01 turn records. No SQLite schema migration/table.

A single accepted semantic response may produce changes only, knowledge only, both, or neither. If something durable exists, at most one atomic world mutation for that turn version.

First consumer is later GM Context:

- GM may still see broad world truth;
- add bounded Actor Knowledge Provenance;
- tell GM that post-T0 world truth is not automatically actor knowledge;
- no prose keyword checker/classifier/retry gate.

## 7. First proof target

Use a task-owned Game with Player Character + one Guaranteed NPC:

```text
Player alone discovers private fact F
→ Player gets durable knowledge provenance
→ NPC does not
→ later GM Context preserves asymmetry
→ later Narrative explicitly tells NPC F
→ NPC then gains durable provenance
→ Save/reopen preserves the boundary
```

Also prove:

- unknown actor IDs rejected;
- knowledge-only result can commit one world mutation;
- knowledge parse failure does not break valid G5-01 changes;
- stale GM-hash knowledge does not project;
- replay/reopen does not duplicate.

## 8. Provider validation

Standing authorization:

`my world/architecture/foundation/REAL_PROVIDER_VALIDATION_STANDING_AUTHORIZATION.md`

After offline gates are green, at most **one** task-owned real selected-Provider G5-02 attempt is authorized. Do not ask Owner again.

No fallback/hidden switch/second attempt.

If externally unavailable, commit/push reviewable implementation with:

`READY FOR INDEPENDENT REVIEW — REAL PROVIDER PROOF PENDING`.

## 9. Temporary routing

Through 2026-09-06 23:59 (+08:00):

```text
GPT        → semantics / architecture / governance / final Independent Review
Kimi       → primary code-changing owner; temporary Codex substitute
Grok Build → external research/evidence support only if useful
Owner      → Product UAT / verdict
```

Auto-expiry 2026-09-07 00:00 (+08:00); do not interrupt correct in-flight Kimi work solely due expiry.

## 10. Review focus when Kimi returns

Refresh both mains and inspect actual code/evidence. Verify especially:

1. only one semantic Provider request per accepted turn;
2. G5-01 changes remain backward-compatible when knowledge field is absent/invalid;
3. only stable Player/Guaranteed NPC local IDs can be persisted as knowers;
4. knowledge-only + changes+knowledge both use at most one atomic world mutation;
5. Context projects only committed/current-hash-matching knowledge and remains bounded;
6. no actor omniscience keyword/output gate is added;
7. Save/reopen preserves the asymmetry;
8. no Source migration, SQLite schema, G5-03 Agency, Faction knowledge, generic graph, UI or G6/G7 scope creep;
9. real Provider result is honest about success versus outage.

If G5-02M1 passes, close G5-02 and then shape G5-03 NPC / Faction Agency.
