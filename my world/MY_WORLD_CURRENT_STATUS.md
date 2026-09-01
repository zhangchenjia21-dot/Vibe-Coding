---
title: my world｜当前状态
status: current-project-status
version: 8.1
created: 2026-08-26
updated: 2026-09-01
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-08B Public d20 UI / Interaction Integration
current_owner: Kimi
parent_task: G4-08 Expansion Pack v0.1 + First Real Runtime Vertical
semantic_owner: GPT
owner_uat_required: false
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Current

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G2-GATE                               PASS
G3 Persistent Game / Save / Timeline PASS / CLOSED
G3-GATE                               PASS
G4-01 Application Shell / Lifecycle  PASS / CLOSED
G4-02R1 Source semantic re-audit      PASS / CLOSED
G4-03 Managed Local Source Library    PASS / CLOSED
G4-04 Multi-Game / Game Library       PASS / CLOSED
G4-05 Asset-only New Game Wizard      PASS / CLOSED
G4-06 Atomic Final Create             PASS / CLOSED
G4-07 First Playable A                PASS / CLOSED
G4-07A Opening Runtime                PASS / CLOSED
G4-07B Playable UI Integration        PASS / CLOSED
G4-07UAT01 Owner Launch Freshness     PASS / CLOSED
G4-08 Expansion Pack v0.1             ACTIVE
G4-08S0 Expansion Semantic Freeze     PASS / CLOSED
G4-08M1 Public d20 Mechanism          PASS / CLOSED
G4-08M1C01 NO_CHECK Idempotency       PASS / CLOSED
G4-08B Public d20 UI Integration      ACTIVE — KIMI
G4-GATE                               NOT YET
```

Current formal execution packet:

`my-world/docs/tasks/G4-08B_PUBLIC_D20_UI_INTEGRATION_TASK.md`

Formal Code Base:

`d646427dfe3c4c6328809384e482cd1fdd2204a0`

Primary execution owner: **Kimi**.  
Reviewer / semantic owner: **GPT**.

---

## 2. G4-08M1 final result｜PASS / CLOSED

Initial M1 review found one correction-01 blocker on Expansion-enabled `NO_CHECK` stable-action replay. Codex corrected it without widening architecture.

Final review records:

- `my-world/docs/g4_08m1/G4-08M1_INDEPENDENT_REVIEW.md`
- `my-world/docs/g4_08m1/G4-08M1C01_INDEPENDENT_REVIEW.md`

Accepted backend mechanism now proves:

```text
exact Expansion Source
→ Managed Library
→ exact Composition selection
→ capability-slot compatibility
→ Final Create exact materialization
→ Program-owned Public d20 Host
→ strict adjudication before RNG
→ Program RNG / total / outcome
→ durable CHECK_REQUIRED or durable NO_CHECK action identity
→ retry / restart exactly once
→ real DeepSeek continuation
```

Both Provider branches are stable-action replay safe:

```text
CHECK_REQUIRED → never rerolls same action
NO_CHECK       → never replays accepted action through Provider
```

SQLite remains schema v4. No executable Source support was added. Real Han + Afterglow mechanism evidence remains valid.

---

## 3. Current task｜G4-08B UI / interaction integration

Purpose:

> Project the accepted Expansion mechanism into the real player path without moving mechanism truth into UI.

Required product integration:

### New Game

- Wizard reads installed Expansion generations from existing Source inventory;
- selection is explicit `0..N`, never auto-select first item;
- explicit none remains valid;
- backend `set_expansion` / compatibility remains authority;
- Review displays actual selected Expansion name/version.

### Runtime

No Expansion:

```text
preserve accepted G4-07 Narrative path unchanged
```

Public d20 selected:

```text
Player action
→ stable UI-owned action_id
→ accepted ActionAdjudication L3 seam
→ NO_CHECK normal narrative
   or
→ CHECK_REQUIRED Program result
→ public mechanic projection
→ GM continuation
```

UI does not call `conversation.begin_turn()` before the d20 Host and never computes dice truth.

### Retry / reopen

- failed/cancelled action with durable resolution reuses exact action_id/text;
- no reroll because of Provider failure;
- unresolved durable action after reopen must be surfaced as `重试行动`, not silently forgotten;
- Public d20 accepted turns do not use legacy generic Regenerate in v0.1.

### Mechanic card

Show Program-owned accepted check values, including intent, DC, modifier/reasons, stance, raw rolls, selected roll, total, outcome and failure stakes.

The public result should become visible when resolution narrative begins, before GM continuation finishes. Historical accepted cards must rebuild on Continue / Load from durable Game-local state. NO_CHECK has no dice card.

---

## 4. Protected boundaries

G4-08B must not redesign:

- Source / Managed Library;
- Composition backend semantics;
- Final Create;
- Public d20 rules / RNG / durable identity;
- persistence schema;
- Provider protocol;
- G5 world consequences.

If a required UI-neutral L3 projection is missing, Kimi reports the blocker rather than editing lower-layer mechanism ownership.

Player-facing resource import remains deferred to G8.

---

## 5. Next progression

```text
G4-08B UI/integration — Kimi
→ GPT Independent Review
→ G4-09 First Playable B
→ prepare Owner production Source Library with Public d20 exact package
→ Owner UAT B
→ remaining G4 gate work
```

G4-08 is not yet Product PASS. Do not start G5 before the remaining G4 route / G4-GATE are complete.
