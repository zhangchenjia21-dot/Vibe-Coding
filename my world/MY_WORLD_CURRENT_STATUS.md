---
title: my world｜当前状态
status: current-project-status
version: 11.6
created: 2026-08-26
updated: 2026-09-03
phase: G5 World Semantics & GM Runtime
current_task: G5-02M1 Known-Actor Knowledge Provenance Spine
current_owner: KIMI
parent_task: G5-02 Knowledge Provenance
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
G5-01M1 Semantic Materialization Spine ENGINEERING PASS / CLOSED
G5-01M1C02 Restore Timeline Isolation CANCELLED / DO NOT EXECUTE
G5-01 dedicated real Provider vertical HISTORICAL GAP — Provider unavailable; not rewritten as PASS

G5-02 Knowledge Provenance            ACTIVE
G5-02M1 Known-Actor Knowledge Spine   ACTIVE — KIMI
G5-03 NPC / Faction Agency            NOT YET
G5-04 Event / Priority Evolution      NOT YET
G5-GATE                               NOT YET
```

Do not start G5-03 before G5-02M1 Independent Review and G5-02 closeout.

## 2. G5-01 closeout

Formal closeout:

`my-world/docs/g5_01/G5-01_CLOSEOUT.md`

The Owner explicitly waived a new dedicated G5-01 UAT because prior actual play already provided sufficient product confidence that the world remembers consequences of player choices, and directed the project to proceed directly to the next task.

Therefore:

```text
G5-01 dedicated additional UAT
WAIVED BY OWNER

G5-01 product progression
ACCEPTED / CLOSED
```

This does not fabricate the earlier dedicated real-Provider vertical as PASS. Two bounded Kimi K3 attempts timed out during ordinary Narrative before the feature-specific semantic lane executed. That remains a historical evidence gap only and does not reopen G5-01.

The previously proposed C02 exact-replay Restore correction is cancelled. It addresses a rare exact turn-index + byte-identical GM-hash replay edge and would require a later consumer-driven branch/reuse decision to solve correctly. Do not implement it now.

## 3. Current G5-02 semantic decision

Canonical decision:

`architecture/world/G5_KNOWLEDGE_PROVENANCE_V0_1_DECISION.md`

Core distinction:

```text
Game / World Truth
!= actor knowledge
!= human-player disclosure
!= omniscient GM model context
```

Protected principle:

> **GM omniscience must not become actor omniscience.**

Current Source `disclosure: gm_reference` is GM/reference visibility, not automatic actor knowledge.

## 4. G5-02M1 scope

Implementation packet:

`my-world/docs/tasks/G5-02M1_KNOWN_ACTOR_KNOWLEDGE_PROVENANCE_TASK.md`

Owner: **KIMI** under the temporary execution routing through 2026-09-06.

v0.1 tracks only **post-T0 knowledge acquisition** for actors that already have stable Game-local IDs:

```text
Player Character
+
Guaranteed NPCs
```

Do not build identity for incidental/emergent NPCs or Factions in G5-02M1.

G5-02M1 extends the existing one-per-turn semantic-analysis request; it must **not add a second Provider call per accepted turn**.

Conceptually:

```json
{
  "changes": ["durable world consequence"],
  "knowledge_events": [
    {
      "knower_id": "local-character-id",
      "fact": "post-T0 fact this actor now has grounds to know",
      "basis": "witnessed|told|discovered|participated"
    }
  ]
}
```

Knowledge validation failure must not invalidate an otherwise valid G5-01 `changes` result.

## 5. Durable / Context semantics

Knowledge provenance belongs inside the existing game-local world snapshot under `living_world`, adjacent to G5-01 turn records. No SQLite schema/table migration.

First consumer is later ordinary GM Context:

- GM may still receive broad World/Source truth;
- Context additionally projects a bounded actor-knowledge provenance section;
- GM is instructed not to make actors speak/plan/react from post-T0 facts merely because GM knows them;
- this is soft semantic guidance, not a Narrative output classifier/gate.

Do not create a universal Knowledge Graph, inference engine, rumor network, false-belief system, confidence scores or Faction knowledge in v0.1.

## 6. Required first vertical

At minimum prove:

```text
Player Character + Guaranteed NPC A
→ Player alone discovers private fact F
→ durable knowledge provenance says Player knows F
→ NPC A does not
→ later Context preserves the asymmetry
→ later accepted turn explicitly tells NPC A
→ NPC A then gains provenance for F
→ Save/reopen preserves knowledge state
```

Unknown returned actor IDs must never become durable authority.

## 7. Provider authorization / outage rule

Canonical standing decision:

`architecture/foundation/REAL_PROVIDER_VALIDATION_STANDING_AUTHORIZATION.md`

After offline/integration gates are green, G5-02M1 may make **at most one** task-owned real selected-Provider attempt. It is already authorized; do not pause to ask Owner again.

No fallback or hidden model switch.

If the selected Provider is unavailable:

```text
real G5-02 vertical = PENDING / EXTERNAL PROVIDER UNAVAILABLE
→ commit/push reviewable implementation/tests/evidence
→ GPT Independent Review proceeds
```

## 8. Temporary execution routing｜through 2026-09-06

Canonical decision:

`architecture/foundation/TEMPORARY_EXECUTION_ROUTING_2026-09-03_TO_2026-09-06.md`

```text
GPT        → semantics / architecture / task shaping / final Independent Review
Kimi       → primary code-changing implementation owner, temporarily substituting for Codex
Grok Build → external research / evidence support when useful
Owner      → Product UAT / explicit product verdict
```

The override auto-expires at 2026-09-07 00:00 (+08:00). Correct in-flight Kimi work may finish under its issued packet.

## 9. Route

```text
G5-02M1 Kimi implementation
→ GPT Independent Review
→ G5-02 closeout
→ shape G5-03 NPC / Faction Agency
```

Do not implement G5-03 autonomous action early.

Visual runtime remains deferred to G6.
