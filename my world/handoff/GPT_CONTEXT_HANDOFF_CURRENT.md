---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-01
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-08B Public d20 UI / Interaction Integration
current_execution_task: G4-08BC01 UI Projection / Fail-Loud Correction
semantic_owner: GPT
current_execution_owner: Kimi
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4-07 First Playable A                PASS / CLOSED
G4-08 Expansion Pack v0.1             ACTIVE
G4-08S0 Expansion Semantic Freeze     PASS / CLOSED
G4-08M1 Public d20 Mechanism          PASS / CLOSED
G4-08M1C01 NO_CHECK Idempotency       PASS / CLOSED
G4-08B Public d20 UI Integration      CORRECTION REQUIRED
G4-08BC01 UI Projection / Fail-Loud   ACTIVE — KIMI
G4-GATE                               NOT YET
```

---

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/architecture/source/G4_EXPANSION_V0_1_PUBLIC_D20_DECISION.md`

Implementation:

3. `AGENTS.md`
4. `docs/g4_08b/G4-08B_INDEPENDENT_REVIEW.md`
5. `docs/tasks/G4-08BC01_UI_PROJECTION_FAIL_LOUD_CORRECTION_TASK.md`
6. `src/ui/叙事对话视图.gd`
7. `src/应用壳.gd`
8. `tests/g4_08b/公开D20界面整合测试.gd`

---

## 3. Accepted backend mechanism

M1 + M1C01 are **PASS / CLOSED** and are not reopened by this correction.

Accepted:

- Expansion third Source type / exact Managed Library generations;
- exact Composition 0..N;
- exclusive capability-slot compatibility;
- Final Create exact materialization/provenance;
- Proposal freeze before RNG;
- Program-owned dice / total / outcome;
- CHECK_REQUIRED durable no-reroll retry/restart;
- NO_CHECK durable replay identity / lost-ACK recovery;
- real Han + Afterglow Provider evidence;
- SQLite schema v4;
- no executable Source code.

---

## 4. Reviewed G4-08B implementation

Reviewed HEAD:

`3a20234d06c10904c220cd1a49bf29f6ad6769e7`

Broadly correct in shape:

- Wizard real Expansion inventory / selection;
- Review selected Expansion projection;
- no-Expansion G4-07 route preserved;
- Public d20 routes through ActionAdjudication L3;
- stable UI action_id / retry / unresolved-on-reopen;
- Program-owned mechanic values are only read/projection;
- Continue / Load durable card reconstruction exists;
- real Han DeepSeek UI vertical exists;
- protected backend paths unchanged.

Do not generically redo these seams.

---

## 5. Independent Review blockers

### A — mechanic-card lifecycle / order

Current live path:

```text
resolution_narrative begins
→ transient card appended directly to Entries
→ later Player + GM Conversation turn appears
→ accepted terminal does not remove/reposition transient
```

So first-play live history can be:

```text
card → Player → GM
```

Current full redraw is:

```text
Player → GM → card
```

Frozen correction target:

```text
Player → card → GM
```

for live, retry, reopen retry, Continue and Load/Restore.

Existing durable-check retry can also duplicate transient cards because both `request_assembled(stage=resolution_narrative)` and synchronous `start_action()` return can append the same check; failed attempt transient nodes also remain.

Invariant:

```text
one durable check_id → at most one visible mechanic projection at a time
```

### B — unsupported capability fail-loud

Current Shell behavior for `capability_slot=action_resolution` with unknown capability id only logs an error and mounts no Host. View then falls back to the legacy no-Expansion path.

Correction must surface a player-visible unsupported-rule state, gate input, and never call legacy Narrative Provider for that Game. Keep Game data intact.

### C — evidence gap

`_test_wizard_expansion_none_projection()` is currently `pass` although the evidence claims direct explicit-none Review coverage.

Add direct Wizard → Review `拓展 / 无` + frozen empty Expansion evidence.

Remove production debug probe output such as `PROBE card added`.

---

## 6. Current correction

Packet:

`my-world/docs/tasks/G4-08BC01_UI_PROJECTION_FAIL_LOUD_CORRECTION_TASK.md`

Formal Code Base:

`3a20234d06c10904c220cd1a49bf29f6ad6769e7`

Owner: **Kimi**. Reviewer: **GPT**. Correction budget: **correction-01**.

Protected backend paths remain off-limits.

---

## 7. After Kimi returns

GPT must refresh both current `main` heads and inspect actual correction code/evidence.

If all correction requirements pass:

```text
G4-08BC01 PASS / CLOSED
G4-08B PASS / CLOSED
→ G4-09 First Playable B
```

If the same card lifecycle / routing seam still fails, correction-02 must audit the neighboring UI state seam rather than pile on patches.

Do not start G4-09 before G4-08B closes.

---

## 8. Later product route

```text
G4-08BC01 Kimi
→ GPT IR
→ G4-09 First Playable B
→ Owner Source Library Public d20 bootstrap
→ Owner UAT B
→ remaining G4 gate
```

G4-08 is not yet Product PASS and G4-GATE is not yet complete.
