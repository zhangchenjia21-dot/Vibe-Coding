---
title: my world｜当前状态
status: current-project-status
version: 8.5
created: 2026-08-26
updated: 2026-09-01
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-09R1M1 Runtime Model Settings / Multi-Provider Mechanism
current_owner: Codex
parent_task: G4-09R1 Runtime Model Settings v0.1
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
G4-08B Public d20 UI Integration      PASS / CLOSED
G4-08BC01 UI Projection / Fail-Loud   PASS / CLOSED
G4-09 First Playable B                ACTIVE
G4-09P1 Owner UAT B Production Prep   PASS / CLOSED
G4-09UATB Owner Product UAT           HOLD
G4-09R1 Runtime Model Settings v0.1   ACTIVE
G4-09R1S0 Semantic Freeze             PASS / CLOSED — GPT
G4-09R1M1 Backend Mechanism           ACTIVE — CODEX
G4-09R1B1 Settings UI                 HOLD — KIMI
G4-GATE                               NOT YET
```

Owner explicitly requested G4-09R1 before beginning UAT B. The previous UAT handoff is paused, not failed.

Canonical decision:

`my world/architecture/foundation/G4_RUNTIME_MODEL_SETTINGS_V0_1_DECISION.md`

Current implementation packet:

`my-world/docs/tasks/G4-09R1M1_RUNTIME_MODEL_SETTINGS_MECHANISM_TASK.md`

Planned UI packet:

`my-world/docs/tasks/G4-09R1B1_MODEL_SETTINGS_UI_TASK.md`

Current owner: **Codex**. Reviewer/semantic owner: **GPT**.

---

## 2. Owner-requested model settings freeze

The Main Menu will expose application-level runtime selection for:

```text
Models:
- DeepSeek V4 Pro
- DeepSeek V4 Flash
- Kimi K3
- Kimi K2.7

Context ceiling:
- 256K
- 1M

Requested reasoning:
- Low
- Medium
- High
- Max
```

Exact provider mapping:

```text
DeepSeek V4 Pro     → deepseek-v4-pro
DeepSeek V4 Flash   → deepseek-v4-flash
Kimi K3 / 256K      → k3-256k
Kimi K3 / 1M        → k3
Kimi K2.7 / 256K    → kimi-for-coding
```

Kimi K2.7 does not support 1M in this catalog and exposes fixed Thinking ON rather than the same graded effort selector.

For DeepSeek V4 and Kimi K3:

```text
Low → low
Medium → high
High → high
Max → max
```

Settings are application-local runtime preferences, not Source/Game canonical truth and not SQLite schema.

Credentials remain local environment-only:

```text
DEEPSEEK_API_KEY
KIMI_API_KEY
```

No automatic cross-provider fallback.

---

## 3. Current backend task｜G4-09R1M1

Codex must replace the hidden DeepSeek-only runtime routing with one validated closed profile seam used by all real Provider calls:

```text
First Opening
ordinary Narrative
Public d20 adjudication
Public d20 resolution narrative
retry/reopen calls
```

Backend owns:

- durable app-local settings/defaults;
- profile catalog and compatibility validation;
- fixed DeepSeek/Kimi endpoints and model-id derivation;
- reasoning requested→effective mapping;
- context ceiling metadata without pulling G7 forward;
- selected-provider credential routing / fail-loud;
- generic-enough OpenAI-compatible streaming seam for exactly these two Providers;
- `.env.example` and canonical launcher credential allowlist as required;
- real DeepSeek + Kimi evidence when local credentials/entitlement permit.

Real Kimi support may not be declared from stubs only. If local Kimi credential/entitlement is missing, Codex must report that exact blocker.

SQLite remains v4. Do not reopen Source, Final Create or Public d20 semantics.

---

## 4. Planned frontend task｜G4-09R1B1

Kimi remains HOLD until GPT Independent Review passes M1.

Then Kimi owns only:

- Main Menu `模型设置` entry;
- model/context/reasoning controls;
- backend-derived compatibility/disabled states;
- K2.7 256K-only + fixed-thinking explanation;
- Medium→effective High disclosure;
- non-secret DeepSeek/Kimi credential status;
- Save/Cancel/reopen/restart UX;
- responsive desktop layout.

UI never owns model ids, endpoints, request-body reasoning fields or secrets.

---

## 5. Context boundary

The 256K/1M selection is a runtime context ceiling/capability choice. Existing G2-05 conservative assembly may use less than the selected ceiling.

Do not introduce G7 summarization/retrieval/memory compression/tokenizer architecture here.

---

## 6. G4-09UATB hold

The previous DeepSeek-only Owner UAT instructions are stale while G4-09R1 is active.

Resume condition:

```text
G4-09R1M1 Codex
→ GPT Independent Review
→ G4-09R1B1 Kimi
→ GPT Independent Review
→ real Provider + Windows export freshness
→ refresh UAT B instructions
→ G4-09UATB ACTIVE — OWNER
```

G4-09 and G4-08 remain open. G4-GATE is NOT YET. Do not start G4-10 or G5.