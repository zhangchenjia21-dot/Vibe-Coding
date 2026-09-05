---
title: my world｜当前状态
status: current-project-status
version: 14.6
created: 2026-08-26
updated: 2026-09-05
phase: G5 World Semantics & GM Runtime
current_task: G5-07 / MW-010 Revision 2 Counterfactual + Knowledge Boundary Completion / Zcode
current_owner: ZCODE weekend implementation / GPT semantic-review lane
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
MW-005 Three Kingdoms Literary Style Primer ENGINEERING PASS — OWNER FOLLOW-UP POLISH/UAT DEFERRED
G5-05 Meaningful Choice / Mechanics Integration ENGINEERING COMPLETE — OWNER UAT DEFERRED / PROGRESSION AUTHORIZED
MW-006 Mechanics-Grounded World Consequence Vertical ENGINEERING PASS / CLOSED
MW-007 Mechanics Consequence Timeline Continuity ENGINEERING PASS / CLOSED
MW-008 Safe Markdown-Lite Narrative Rendering ENGINEERING PASS / CLOSED
G5-06 Runtime → UI Projection               ENGINEERING PASS / CLOSED
MW-009 Player-Safe Runtime Side Panels      ENGINEERING PASS / CLOSED
G5-07 World Product Tests                   ACTIVE
MW-010 G5 Living-World Integrated Reality Matrix REVISION 2 ACTIVE — IR#1 NOT PASS
G5-GATE                                     NOT YET
```

The project remains in G5. G5-06 is closed; G5-07 remains active because MW-010 IR#1 found two missing integrated-proof cases. No production defect has been established.

## 2. MW-010 IR#1 result

Candidate:

`my-world@78c05887600332b93a4d50cd1d63d639841fb2eb`

IR#1 verdict:

**NOT PASS — Revision 2 required.**

Formal review:

`my-world/docs/mw010/MW-010_INDEPENDENT_REVIEW_IR1.md`

Revision addendum:

`my-world/docs/tasks/MW-010_REVISION2_COUNTERFACTUAL_KNOWLEDGE_COMPLETION_ADDENDUM.md`

IR#1 accepted the candidate's overall architecture and zero-production-diff direction. The test already proves:

```text
quiet hold
independent NPC Agency + World Evolution advance
GM context vs player-safe disclosure asymmetry
Program-owned d20 → MW-006 grounding → G5-01 consequence
close/reopen reconstruction + no reroll
Save/Restore removal of Path-A semantic + Player-knowledge truth
alternate Path B currentness
```

Two blocking proof gaps remain.

### F01 — restored-away non-player truth

The frozen counterfactual scenario requires Path A after Save S to create its own Agency-or-Evolution truth and then prove Restore removes that Path-A-specific non-player truth from current state/GM context/player-safe consumers.

The IR#1 candidate instead drove Path A with `actors=[]` and `evolution=hold`; the Agency/Evolution truth checked after Restore was pre-S truth that correctly remained current. That does not prove restored-away non-player currentness isolation.

### F02 — NPC Knowledge → later Player disclosure

The frozen Knowledge Boundary requires one NPC-only post-T0 Knowledge Provenance fact that is hidden from the player-safe UI, followed later by Player Character Knowledge Provenance of the same/substantively related information, at which point disclosure becomes allowed.

The IR#1 candidate hid NPC Agency/Evolution correctly, but created no NPC-only `knowledge_events` record in the integrated timeline. Its Player knowledge facts were unrelated mechanics/Path-A facts. Hidden world truth is not equivalent to NPC Knowledge Provenance.

## 3. MW-010 Revision 2 route

Revision 2 keeps the same Work Item and branch lineage:

```text
Work Item: MW-010
Revision: 2
Next Review: IR#2
Implementer: Zcode + GLM-5.3-flash
Status: ACTIVE
```

Preferred result remains:

```text
production code diff = 0
```

Required completion:

1. after Save S, Path A creates a uniquely identifiable Agency or World Evolution truth; before Restore it is current/GM-visible but player-safe hidden; after Restore and alternate Path B it is no longer current;
2. the same timeline contains NPC-only Knowledge Provenance hidden from the player-safe panel, then later Player Character knowledge of related information that becomes visible; strongly prefer putting the later Player disclosure after Save S so Restore also proves the Player disclosure disappears while pre-S NPC-only provenance remains hidden.

If existing production architecture cannot express these cases, Zcode must STOP and report the owning prior capability. Do not repair prior capabilities under MW-010.

## 4. Owner progression decision remains unchanged

Owner requested that the project stop spending isolated cycles on side tasks and move through the G5 mainline. Remaining product-feel checks are batched into one later combined checkpoint.

Therefore:

```text
G5-05 = engineering complete; Owner product validation deferred to combined G5 test
MW-005 = R3 engineering complete; bounded style-weight Revision queued before combined UAT
MW-003 / MW-004 = open Owner-UAT items, not separate implementation tracks
G5-06 = engineering closed
G5-07 = active until MW-010 engineering proof completes
```

## 5. G5-07 frozen architecture

Canonical decision:

`my world/architecture/world/G5_WORLD_PRODUCT_TEST_MATRIX_V0_1_DECISION.md`

G5-07 is an integration reality proof, not another feature phase.

Required composed evidence covers:

```text
Player Absence / World Independence
Counterfactual Propagation via Save/Restore + alternate path
Independent Actor
Knowledge Boundary
Mechanics → living-world consequence
Save / reopen / Restore consistency
player-safe disclosure currentness
```

Automated tests may prove lifecycle/currentness/persistence/privacy composition. Only Owner real play may decide whether the resulting RPG actually feels alive, natural and coherent.

## 6. G5-05 engineering checkpoint

MW-006 + MW-007 establish:

```text
Program-owned Public d20 result
→ normal G5-01 semantic grounding
→ accepted free-form Narrative remains concrete consequence source
→ durable world consequence
→ Save / close / reopen / Continue / Restore coherent
```

The remaining product validation is deferred to the combined G5 Owner UAT:

```text
meaningful risky action
→ visible Public d20 result
→ natural GM consequence
→ later world remembers it
→ Save/reopen preserves it
```

Also confirm NO_CHECK remains natural and mechanics do not dominate ordinary play.

## 7. MW-005 bounded style-weight polish queued

MW-005 Revision 3 / IR#3 remains the current engineering baseline. Owner wants one modest additional style weight before the combined G5 UAT.

The queued implementation contract is:

`my-world/docs/tasks/MW-005_REVISION4_BOUNDED_STYLE_WEIGHT_POLISH_ADDENDUM.md`

It remains separate from MW-010 and must not alter Primer/source bytes, Source generation, mechanics/control, semantic or World Evolution authority.

## 8. Routing / worktree rule

Owner weekend override remains active through **2026-09-06 23:59 (+08:00)**:

```text
Zcode + GLM-5.3-flash → primary implementation owner for new code-changing work
GPT                    → semantics / architecture / task shaping / Independent Review
```

At 2026-09-07 00:00 (+08:00), absent a new Owner instruction, long-term Codex-backend / Kimi-frontend routing resumes automatically.

All task worktrees:

`D:/AI/Projects/.worktrees/my-world/<task-or-revision>`

Keep the active MW-010 worktree through IR#2. Do not delete/recreate it merely because Revision 2 is the same outcome lineage.

## 9. Remaining G5 route

```text
MW-010 Revision 2
→ GPT IR#2
→ if PASS: G5-07 engineering closeout
→ MW-005 bounded style-weight polish
→ one combined Owner G5 product checkpoint
→ G5-GATE verdict
→ G6 RPG Experience & Internal Declarative UI Host
```

Do not add another G5 feature vertical unless MW-010 or Owner UAT reveals a concrete blocker.
