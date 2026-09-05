---
title: my world｜当前状态
status: current-project-status
version: 13.7
created: 2026-08-26
updated: 2026-09-05
phase: G5 World Semantics & GM Runtime
current_task: MW-005 Three Kingdoms Literary Style Primer v0.1 — Active
current_owner: KIMI
parent_task: G4 Primary Source Assets & Local Game — owner-inserted content/runtime slice
semantic_owner: GPT
owner_uat_required: true
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G4-10 Runtime Asset Resolution              DEFERRED / MOVED TO G6

G5 World Semantics & GM Runtime             ACTIVE
G5-01 World Turn / Semantic Materialization PASS / CLOSED
G5-02 Knowledge Provenance                  PASS / CLOSED
G5-03 NPC / Faction Agency                  ENGINEERING PASS / CLOSED
MW-001 Runtime Narrative Actor Materialization PASS / CLOSED
G5-04 Event / Priority Evolution            ACTIVE — OWNER UAT PAUSED FOR MW-005
MW-002 Selective World Evolution Evaluator ENGINEERING PASS / CLOSED
MW-003 Visual Comfort Theme Pass            ENGINEERING PASS — OWNER UAT
MW-004 Minimal Player Agency Principle      IMPLEMENTED — OWNER UAT
MW-005 Three Kingdoms Literary Style Primer ACTIVE — KIMI
G5-GATE                                     NOT YET
```

The project has not advanced to G6. MW-005 is an Owner-inserted Three-Kingdoms-specific source-content/runtime integration experiment anchored to the closed G4 capability. It does not reopen G4, change G5-04 semantics, or authorize G5-05.

## 2. MW-005 identity / lineage

```text
Work Item: MW-005
Name: Three Kingdoms Literary Style Primer v0.1
Capability-Anchor: G4 Primary Source Assets & Local Game
Inserted-By: Owner
Triggered-By: G5-04 Owner UAT prose-quality observation
Revision: 1
Review-Round: 0
Implementation Base: 63262dfe52d9200115544bb0a1f2507795039e33
Governance Base: a900d8ec4a4b446b28bf68135c48b81c96c5da61
Owner: Kimi
Reviewer: GPT
Status: ACTIVE — KIMI
```

Task Packet:

`my-world/docs/tasks/MW-005_THREE_KINGDOMS_LITERARY_STYLE_PRIMER_TASK.md`

Approved Primer input:

`my-world/docs/tasks/inputs/MW-005_THREE_KINGDOMS_STYLE_PRIMER_V0_1.txt`

## 3. Product purpose

Owner UAT showed a gap between plausible Three Kingdoms world behavior and the prose voice. Remote political/military developments could read like a modern strategy-game briefing (`XX方向：...`) rather than entering the story through messenger, report, question/answer, scene and historical-social speech.

MW-005 tests a deliberately small model-friendly intervention: a bounded literary Style Primer derived from *Romance of the Three Kingdoms* examples, with high-risk historical referents neutralized.

Product rule:

> **Give the model better literary/historical exemplars without converting literary reference into world authority or a rigid writing protocol.**

## 4. Frozen authority boundary

```text
Literary Style Reference
= diction / syntax rhythm / titles / etiquette / dialogue form / narrative distance / historical-social register exemplar

!= current Game world truth
!= future canon
!= original-novel plot authority
!= NPC destiny constraint
!= Player/actor Knowledge
!= World Evolution causal input
!= mandatory chapter-novel format
```

Model Freedom First remains protected. Do not add mandatory `却说` / `且说`, forced half-classical Chinese, a modern-word blacklist, fixed output shape, parser/classifier gate, or retry protocol.

The user-provided EPUB is a local working source only. It must not be committed or installed as the Runtime corpus. MW-005 uses only the bounded approved Primer input.

## 5. Architecture direction

Current `world_pack.v0.2` already supports `semantic_sections` with open safe-token `section_type`. Preferred minimal carrier is:

```text
section_type = literary_style_reference
disclosure   = gm_reference
```

Kimi must first audit the real Three Kingdoms authoring package and all consumers of frozen World semantic sections.

Normal first-opening + ordinary GM Narrative are the intended v0.1 consumers. The literary reference must be projected under a clearly non-factual boundary. G5-04 `project_world_only()` must exclude it completely, and other causal/cognition consumers must not gain authority from it.

Source behavior remains generation-frozen:

```text
old Game
→ keeps old exact Source generation
→ no silent style injection

new Game after publication
→ freezes new Three Kingdoms exact generation
→ normal GM context receives Style Primer
```

No Source schema v0.3, retrieval/RAG, embeddings/vector DB, SQLite migration, generic style platform, or settings UI is authorized.

## 6. Review / UAT

Kimi return ceiling: `READY FOR INDEPENDENT REVIEW`.

GPT Independent Review must inspect actual code, Source generation publication evidence, tests and exact projector behavior. Automated tests prove authority separation/freeze semantics, not prose quality.

After Engineering PASS, Owner performs A/B UAT by creating a **new** Three Kingdoms game through `run-game.cmd` and judges whether:

- advisers/officials sound more naturally situated without everyone becoming ornate;
- regional intelligence enters narrative rather than repeated dashboard headings;
- battle/administrative prose feels less like modern strategic briefing;
- readability remains high;
- chapter-novel formulas do not become tics;
- no future novel plot leaks into current Game;
- MW-004 Player Agency behavior remains natural.

If overfitting appears, prefer reducing/tuning exemplars rather than adding more negative prompt constraints.

## 7. Existing open items

MW-004 remains `IMPLEMENTED — OWNER UAT`; it is not silently closed. Its protected principle remains:

> **The GM owns freedom to advance the world; the Player owns new meaningful choices for the protagonist.**

MW-003 remains `ENGINEERING PASS — OWNER UAT`; Owner's positive visual feedback is not silently converted into Product PASS.

MW-002 remains CLOSED. G5-04 remains ACTIVE and is paused only for this inserted product experiment. Do not start G5-05 before Owner closes G5-04.

## 8. Routing

Through 2026-09-06 23:59 (+08:00), Kimi owns code-changing implementation while GPT owns semantics/task shaping/Independent Review. MW-004's prior GPT direct edit was a one-item Owner exception only.

Gemini review remains CANCELLED / DO NOT EXECUTE.
