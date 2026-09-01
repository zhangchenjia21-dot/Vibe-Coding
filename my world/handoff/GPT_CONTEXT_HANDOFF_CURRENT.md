---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-01
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-08 Expansion Pack v0.1 + First Real Runtime Vertical
current_execution_task: G4-08B Public d20 UI / Interaction Integration
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
G4-08B Public d20 UI Integration      ACTIVE — KIMI
G4-GATE                               NOT YET
```

---

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/architecture/source/G4_EXPANSION_V0_1_PUBLIC_D20_DECISION.md`

Implementation:

3. `AGENTS.md`
4. `docs/tasks/G4-08B_PUBLIC_D20_UI_INTEGRATION_TASK.md`
5. `docs/g4_08m1/G4-08M1_INDEPENDENT_REVIEW.md`
6. `docs/g4_08m1/G4-08M1C01_INDEPENDENT_REVIEW.md`
7. `src/行动判定/L3_外交层/行动判定公开接口.gd`
8. `src/ui/新游戏向导.gd`
9. `src/ui/叙事对话视图.gd`
10. `src/应用壳.gd`

---

## 3. Accepted backend mechanism

Final reviewed mechanism HEAD before UI task activation:

`d646427dfe3c4c6328809384e482cd1fdd2204a0`

Accepted:

- `expansion_pack.v0.1` third Source type through existing Managed Library;
- exact immutable Expansion generations;
- Composition explicit `0..N` exact selections;
- duplicate exact / capability-slot collision fail closed;
- Final Create exact Expansion provenance/rules/binding;
- Public d20 strict Proposal freeze before RNG;
- Program owns dice/total/outcome;
- CHECK_REQUIRED durable result + no-reroll retry/restart;
- NO_CHECK durable replay identity + lost-ACK recovery;
- real Han + Afterglow DeepSeek mechanism evidence;
- no-Expansion G4-07 regression;
- SQLite schema v4;
- no executable Source code.

Do not generically reopen M1/M1C01 during UI work.

---

## 4. Current G4-08B task

Packet:

`my-world/docs/tasks/G4-08B_PUBLIC_D20_UI_INTEGRATION_TASK.md`

Formal Code Base:

`d646427dfe3c4c6328809384e482cd1fdd2204a0`

Owner: **Kimi**. Reviewer: **GPT**.

Required UI product path:

```text
Wizard installed Expansion inventory
→ explicit 0..N selection / explicit none
→ Review exact Expansion projection
→ Final Create unchanged
→ Game-local capability-aware Narrative routing
→ stable action_id
→ ActionAdjudication L3 Host
→ public Program result
→ GM continuation
→ durable mechanic card redraw on Continue / Load
```

No Expansion preserves the existing G4-07 path.

---

## 5. Important UI semantics

### Wizard

- no auto-select first Expansion;
- use backend `set_expansion` / `confirm_expansion_none` as authority;
- slot collision must visually revert rejected toggle;
- no import UI in G4-08B.

### Player action

For Public d20 Sessions, UI must not call `conversation.begin_turn()` before the Host.

UI supplies a stable opaque action_id and retains it through retry until accepted/already accepted.

Provider failure after durable resolution must retry the same identity and never reroll.

On reopen, exactly one unresolved durable Public d20 action must gate new input and expose `重试行动`; multiple unresolved actions fail visibly rather than guessing.

### Regenerate

Public d20 accepted turns do not use the legacy post-accept generic Regenerate path in v0.1. Unaccepted failures use `重试行动`. No-Expansion Sessions preserve existing regenerate behavior.

### Mechanic card

UI only projects durable Program truth. It never rolls/recomputes/edits.

When `resolution_narrative` begins, show the already-durable result immediately in a transient mechanic surface. After acceptance, reconstruct the card in history using `accepted_turn_index`.

Continue / Load redraw must preserve/remove cards according to restored durable World+Conversation. NO_CHECK has no dice card.

---

## 6. After Kimi returns

GPT must refresh both `main` heads and review actual UI code/evidence.

Verify at minimum:

- Expansion Wizard inventory/select/none/review;
- no backend semantic edits;
- no-Expansion G4-07 regression;
- stable action_id lifecycle;
- checked action routes through accepted L3 Host;
- Program result visible before resolution narrative completion;
- retry reuses same action identity and does not reroll;
- reopen unresolved action recovery UI;
- accepted mechanic cards rebuild on Continue and disappear appropriately after Load to earlier state;
- real DeepSeek Han UI vertical and ordinary NO_CHECK turn;
- 1280×720 / 960×540 / maximized layout.

If PASS, proceed to G4-09 First Playable B and prepare the Owner production Source Library with the Public d20 package before Owner UAT.

---

## 7. Product route

```text
G4-08B Kimi
→ GPT IR
→ G4-09 First Playable B
→ Owner Source Library Public d20 bootstrap
→ Owner UAT B
→ remaining G4 gate
```

G4-08 is not yet Product PASS and G4-GATE is not yet complete.
