---
title: my world｜当前状态
status: current-project-status
version: 9.4
created: 2026-08-26
updated: 2026-09-02
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-09UATBC02A Public d20 Protocol Decoupling / Model-Freedom Correction
current_owner: CODEX
parent_task: G4-09UATB Owner Product UAT
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
G4-08 Expansion Pack v0.1             ACTIVE — gameplay value accepted, final gate pending
G4-08M1 Public d20 Mechanism          PASS / CLOSED
G4-08B Public d20 UI Integration      PASS / CLOSED
G4-09 First Playable B                ACTIVE pending Owner final verdict
G4-09P1 Owner UAT B Production Prep   PASS / CLOSED
G4-09R1 Runtime Model Settings v0.1   PASS / CLOSED
G4-09R1S0 Semantic Freeze             PASS / CLOSED
G4-09R1M1 Backend Mechanism           PASS / CLOSED
G4-09R1B1 Settings UI                 PASS / CLOSED AFTER CORRECTION-01
G4-09R1P1 Final Integration/Freshness PASS / CLOSED
G4-09UATBC01 Narrative Responsiveness PASS / CLOSED — streaming goal retained
G4-09UATB Owner Product UAT           HOLD — CORRECTION-02
G4-09UATBC02A d20 Protocol Decoupling ACTIVE — CODEX
G4-09UATBC02B Failure Visibility      HOLD — KIMI
G4-GATE                               NOT YET
```

Do not start G4-10 or G5 while correction-02 is active.

## 2. Owner findings preserved

The Owner has already accepted the Public d20 gameplay/mechanic itself. That product finding remains authoritative.

C01 fixed application-added whole-response narrative buffering. A later real focused retest then hit a new failure: an action ended in generic `行动未完成` with no narrative.

Independent diagnosis found two adjacent seams:

1. C01 coupled machine control and GM prose into one Provider response (`control JSON + narrative body`) and therefore made model formatting a blocking gameplay gate;
2. Public d20 UI did not surface the safe terminal failure reason even though a mapper already existed.

The mixed protocol is now superseded rather than patched with more parser exceptions.

## 3. Frozen runtime principles

Canonical decision:

`my world/architecture/foundation/G4_NARRATIVE_RESPONSIVENESS_V0_1_DECISION.md`

Principle:

```text
Model Freedom First
+
Visible Narrative First
+
Canonical Commit Behind a Turn Finalize Barrier
```

Implications:

- player-visible GM narrative must remain free-form natural language;
- do not require JSON/sentinel/exact-line framing in the narrative stream;
- do not add model-format gates merely to save Provider calls;
- hard gates are reserved for authoritative mechanics/persistence/capability integrity;
- Public d20 mechanics control is separated from narrative;
- the old hard requirement `NO_CHECK = exactly one Provider call` is superseded;
- isolated control output gets at most one bounded internal recovery attempt;
- if control still cannot be resolved, this action fails soft to the ordinary no-Expansion natural-language narrative path rather than dead-ending play;
- fail-soft degradation creates no fake d20 check and no fake successful-adjudication durable marker;
- CHECK_REQUIRED exact Program result remains durable before free-form result narrative;
- narrative still streams provisionally with no per-token canonical persistence;
- next action stays behind the Turn Finalize Barrier until required authoritative effects finalize.

Future G5/G6 character/event/world semantic calculations may run behind visible narrative when safe, but must converge before the next turn depends on them. This G4 work does not implement those systems or a generic background worker.

## 4. Current task｜G4-09UATBC02A

Owner: **Codex**

Packet:

`my-world/docs/tasks/G4-09UATBC02A_D20_PROTOCOL_DECOUPLING_TASK.md`

Required architecture:

```text
short isolated mechanics-control request
→ valid NO_CHECK or CHECK_REQUIRED

NO_CHECK
→ separate free-form narrative request
→ progressive visible narrative

CHECK_REQUIRED
→ Program RNG/outcome
→ durable exact check
→ separate free-form result narrative request
→ progressive visible narrative
```

Control formatting failure may not trap the player. One bounded internal recovery attempt is allowed; after that, degrade the action transparently to ordinary natural-language narrative without a d20 check.

No Provider fallback, no fake mechanic state, no narrative-format contract.

## 5. C02B after C02A

`my-world/docs/tasks/G4-09UATBC02B_PUBLIC_D20_FAILURE_VISIBILITY_TASK.md`

C02B remains HOLD until GPT closes C02A. Kimi will only surface safe terminal failure reasons and a non-blocking degradation notice; it must not redefine backend mechanics.

## 6. Correction-budget rule

This is correction-02. C01's streaming goal is retained, but its mixed protocol is superseded.

If the **decoupled control lane itself** still repeatedly fails at the same real-model seam after C02A, stop adding formatting patches and redesign the mechanic-control capability instead of spending correction-03 on special cases.

## 7. After correction

If C02A and C02B both pass GPT Independent Review:

```text
G4-09UATB ACTIVE — OWNER focused reliability/responsiveness retest
```

The Owner will not be asked to re-prove whether Public d20 gameplay is worthwhile.

Only after Owner final PASS may GPT close G4-09UATB, G4-09 First Playable B and G4-08 Expansion Pack v0.1, then inspect current roadmap authority before shaping G4-10.

Do not start G5 before G4-GATE.
