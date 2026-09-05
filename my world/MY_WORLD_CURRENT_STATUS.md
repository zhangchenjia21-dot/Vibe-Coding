---
title: my world｜当前状态
status: current-project-status
version: 13.9
created: 2026-08-26
updated: 2026-09-05
phase: G5 World Semantics & GM Runtime
current_task: MW-005 Revision 2 control-lane style exclusion correction; G5-05 active after MW-006 IR#1
current_owner: KIMI correction / GPT semantic-review lane
parent_task: G5 World Semantics & GM Runtime
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
G5-04 Event / Priority Evolution            PRODUCT PASS / CLOSED
MW-002 Selective World Evolution Evaluator ENGINEERING PASS / CLOSED
MW-003 Visual Comfort Theme Pass            ENGINEERING PASS — OWNER UAT
MW-004 Minimal Player Agency Principle      IMPLEMENTED — OWNER UAT
MW-005 Three Kingdoms Literary Style Primer CORRECTION REQUIRED — REVISION 2 / KIMI
G5-05 Meaningful Choice / Mechanics Integration ACTIVE
MW-006 Mechanics-Grounded World Consequence Vertical ENGINEERING PASS / CLOSED
G5-GATE                                     NOT YET
```

The project has not advanced to G6. MW-005 remains an Owner-inserted Three-Kingdoms-specific source-content/runtime integration experiment anchored to the closed G4 capability. It runs independently from G5-05 and does not reopen G4.

G5-04 is closed: Owner explicitly completed UAT on 2026-09-05 and authorized the next step. G5-05 is active. MW-006 is the first closed engineering vertical under G5-05; closing MW-006 does **not** close the whole G5-05 capability.

## 2. MW-005 identity / lineage

```text
Work Item: MW-005
Name: Three Kingdoms Literary Style Primer v0.1
Capability-Anchor: G4 Primary Source Assets & Local Game
Inserted-By: Owner
Triggered-By: G5-04 Owner UAT prose-quality observation
Revision: 2
Review-Round: IR#1 completed — CORRECTION REQUIRED
Implementation Base: 63262dfe52d9200115544bb0a1f2507795039e33
Revision-1 reviewed main: f7e347ed3b97bed8a036ad9c225aaa28f7e249fe
Revision-1 implementation SHA: 97dbbbc36288129b4b21ac5556e5dd9378be5850
Governance Base: a900d8ec4a4b446b28bf68135c48b81c96c5da61
Owner: Kimi
Reviewer: GPT
Status: CORRECTION REQUIRED — REVISION 2 / KIMI
```

Canonical parent Task Packet:

`my-world/docs/tasks/MW-005_THREE_KINGDOMS_LITERARY_STYLE_PRIMER_TASK.md`

Revision-2 correction addendum:

`my-world/docs/tasks/MW-005_REVISION2_CONTROL_LANE_STYLE_EXCLUSION_ADDENDUM.md`

IR#1 record:

`my-world/docs/mw005/MW-005_INDEPENDENT_REVIEW_IR1.md`

Approved Primer input:

`my-world/docs/tasks/inputs/MW-005_THREE_KINGDOMS_STYLE_PRIMER_V0_1.txt`

## 3. MW-005 product purpose

Owner UAT showed a gap between plausible Three Kingdoms world behavior and the prose voice. Remote political/military developments could read like a modern strategy-game briefing (`XX方向：...`) rather than entering the story through messenger, report, question/answer, scene and historical-social speech.

MW-005 tests a deliberately small model-friendly intervention: a bounded literary Style Primer derived from *Romance of the Three Kingdoms* examples, with high-risk historical referents neutralized.

Product rule:

> **Give the model better literary/historical exemplars without converting literary reference into world authority or a rigid writing protocol.**

## 4. MW-005 frozen authority boundary

```text
Literary Style Reference
= diction / syntax rhythm / titles / etiquette / dialogue form / narrative distance / historical-social register exemplar

!= current Game world truth
!= future canon
!= original-novel plot authority
!= NPC destiny constraint
!= Player/actor Knowledge
!= World Evolution causal input
!= Public-d20 mechanics/control authority
!= mandatory chapter-novel format
```

Model Freedom First remains protected. Do not add mandatory `却说` / `且说`, forced half-classical Chinese, a modern-word blacklist, fixed output shape, parser/classifier gate, or retry protocol.

The user-provided EPUB is a local working source only. It must not be committed or installed as the Runtime corpus. MW-005 uses only the bounded approved Primer input.

## 5. MW-005 architecture direction / IR#1 correction

Current `world_pack.v0.2` already supports `semantic_sections` with open safe-token `section_type`. The accepted minimal carrier remains:

```text
section_type = literary_style_reference
disclosure   = gm_reference
```

Revision 1 correctly established the bounded Source generation, frozen old/new Game behavior, an explicit non-factual literary boundary, and complete exclusion from G5-04 `project_world_only()`.

IR#1 found one blocking consumer leak: the shared normal projector also feeds Public-d20 `control` / `control_recovery`, so literary examples currently enter the request that proposes whether a check is required and its DC/modifier/stance/stakes. Prompt text saying the examples are non-causal is not sufficient; v0.1 requires a consumer boundary.

Revision 2 must preserve normal factual Game/Player/Character context for d20 control while excluding only `literary_style_reference` there. Narrative consumers continue to receive the Primer:

```text
first opening                         INCLUDE
ordinary GM narrative                 INCLUDE
Public d20 resolution narrative       INCLUDE
Public d20 NO_CHECK/degraded narrative INCLUDE
Public d20 control/control_recovery   EXCLUDE
G5-04 project_world_only()            EXCLUDE
```

Do not use `project_world_only()` as the d20-control fix because the mechanics control lane may still need normal Player/Character/Game factual context. Do not build a generic context-policy platform.

The Source bytes are not defective. Production Source current has already advanced to immutable generation:

`58966f73dfade50b0aa7536aad38a8840e614016975e8beba0735f7dd14ab443`

Revision 2 does **not** require republishing or minting another Source generation. Owner prose A/B UAT waits for IR#2.

## 6. G5-04 closeout

Owner completed G5-04 UAT and authorized progression on 2026-09-05.

Canonical product rule remains:

> **World Independence + Player Spotlight.**

`hold` remains a first-class correct result; independent World Evolution is not required every turn and does not automatically seize foreground Narrative or grant Player/Actor Knowledge.

Repository closeout:

`my-world/docs/g5_04/G5-04_CLOSEOUT.md`

Status:

```text
G5-04 = PRODUCT PASS / CLOSED
MW-002 = ENGINEERING PASS / CLOSED
```

## 7. G5-05 / MW-006

Owner authorized Zcode + GLM-5.3-flash as a task-local implementation override for the first G5-05 vertical.

```text
Work Item: MW-006
Name: Mechanics-Grounded World Consequence Vertical
Capability-Anchor: G5-05 Meaningful Choice / Mechanics Integration
Revision: 1
Review-Round: IR#1
Implementation SHA: adb3ca45c2e869c7685915de18664ee3ce7e6f39
Reviewer: GPT
Status: ENGINEERING PASS / CLOSED
```

Core authority split:

```text
existing Program-owned Public d20 result
→ request-time grounding for the normal G5-01 semantic opportunity
→ accepted Narrative still determines concrete scene consequences
→ existing semantic mutation seam materializes supported durable consequences
```

Protected boundaries:

- no `SUCCESS/FAILURE → fixed world effect` table;
- no second mechanics truth;
- no reroll;
- no Narrative gate/retry protocol;
- NO_CHECK receives no fake mechanics block;
- no direct raw-mechanics injection into G5-04;
- no Actor Knowledge shortcut;
- no SQLite migration;
- MW-005 literary examples must not become mechanics-control authority.

IR record:

`my-world/docs/mw006/MW-006_INDEPENDENT_REVIEW_IR1.md`

The MW-006 Task Packet is a documented **post-implementation governance backfill** because GPT failed to propagate the intended packet before implementation. IR#1 accepted the Owner-authorized conversation launch instruction as Revision 1's authoritative executable contract.

G5-05 remains ACTIVE after MW-006; the Work Item closes only this first vertical.

## 8. Review / UAT open items

MW-005 is `CORRECTION REQUIRED — REVISION 2 / KIMI`. After Kimi returns the narrow consumer correction, GPT performs IR#2. Only after Engineering PASS should Owner create a **new** Three Kingdoms Game and perform prose A/B UAT.

MW-004 remains `IMPLEMENTED — OWNER UAT`; it is not silently closed. Its protected principle remains:

> **The GM owns freedom to advance the world; the Player owns new meaningful choices for the protagonist.**

MW-003 remains `ENGINEERING PASS — OWNER UAT`; Owner's positive visual feedback is not silently converted into Product PASS.

A pre-existing G3-04 regression assertion is stale after MW-004 because it treats the literal phrase `Current Game Context` in GM instructions as proof that raw persisted context leaked into Provider messages. MW-006 did not introduce that failure; repair it separately rather than folding it into MW-005 or MW-006.

## 9. Routing

Through 2026-09-06 23:59 (+08:00), Kimi is the default code-changing implementer while GPT owns semantics/task shaping/Independent Review. Owner may issue task-local overrides; MW-006's Zcode + GLM-5.3-flash implementation override is complete.

MW-004's prior GPT direct edit remains a one-item Owner exception only.

Gemini review remains CANCELLED / DO NOT EXECUTE.