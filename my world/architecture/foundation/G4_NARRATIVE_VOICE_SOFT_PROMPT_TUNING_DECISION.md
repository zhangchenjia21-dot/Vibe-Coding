---
title: my world｜G4 Narrative Voice Soft Prompt Tuning Decision
status: current-canonical-micro-decision
created: 2026-09-03
updated: 2026-09-03
phase: G4 closeout
owner: OWNER + GPT
---

# G4 Narrative Voice Soft Prompt Tuning｜Canonical Decision

## 1. Owner product finding

G4-11 Owner Reality UAT established that the Han / 刘备 and Afterglow / 莉维娅 play experiences are materially different RPG worlds.

Owner also reported a narrower quality issue:

> 两个不同的世界的叙述文风太过相似。

Classification:

```text
World / Character / reality differentiation    PASS
Narrative voice differentiation                NON-BLOCKING QUALITY FINDING
```

This finding does not reopen G4-11 product value or invalidate the two-family reality result.

## 2. Root cause framing

Current Host behavior intentionally pushes both worlds toward clear modern Chinese narrative:

- one shared generic GM instruction;
- Han Source prefers readable modern Chinese over forced pseudo-classical language;
- Afterglow Source also prefers readable modern Chinese over decorative fantasy prose.

Therefore some prose convergence is expected even when the underlying world semantics differ.

Do not classify this as a Provider defect by default.

## 3. Authorized minimal correction

Owner authorized exactly one low-cost soft-prompt tuning pass before G4 closeout.

Preferred seam:

```text
shared Host GM creative instruction
+
existing Current Game Context
→ allow world-specific language texture to emerge naturally
```

The intended instruction is:

> 让叙述的语言质感自然服从当前 World、Character 与场景：不要把不同世界统一成同一种通用 RPG / 网文旁白；优先让词汇域、句法节奏、观察重点、人物称谓、对话礼法、制度语言与比喻来源从当前 Game Context 自然长出，同时保持清晰、长期可读。不要为了显得不同而机械堆砌古语、奇幻形容词、固定标签或固定模板。

This is a creative preference only.

## 4. Model Freedom boundary

The following remain forbidden:

```text
style prompt
→ generated text
→ keyword/style classifier
→ reject / retry / block
```

Do not add:

- output-format requirements;
- mandatory style vocabulary;
- genre keyword gates;
- prose similarity thresholds;
- output scoring/classification;
- automatic retry/regeneration for style;
- second-model style judging;
- world-specific hardcoded templates.

Canonical invariant:

> **Narrative style is guidance, not an acceptance gate.**

## 5. Why Host prompt, not Source mutation

Changing World Pack `gm_instructions` would create new Source generations and would not automatically affect existing Games pinned to older generations.

This finding is a lightweight cross-world presentation quality issue, not a change to semantic World truth.

Therefore the first correction intentionally uses the shared Host prompt so both existing and future Games can benefit without Source migration/versioning.

No Source schema or exact-generation semantics change is authorized.

## 6. UAT policy

No standalone Owner UAT is required for this micro-fix.

Engineering acceptance only verifies:

- prompt projection to Opening and ordinary continuation;
- no new gates/retry/protocol behavior;
- no Source/Provider/persistence change;
- focused regressions.

Actual prose-quality benefit will be observed in the next suitable Owner/Product UAT. If the soft prompt has little visible effect, do not iterate repeatedly during G4. Record the remaining issue for later Narrative/RPG Experience polish.

## 7. Route

```text
G4-11UAT Owner Reality Test        PASS / CLOSED
G4-11C01 Narrative Voice Tuning    ACTIVE — CODEX
G4-GATE                            HOLD pending C01 engineering review only
```

After C01 Independent Review PASS:

```text
G4-11 PASS / CLOSED
→ G4-GATE PASS
→ G4 CLOSED
→ shape G5-01 Minimum Playable T0 + World Turn / Semantic Materialization
```

C01 must not grow into a narrative-style subsystem.
