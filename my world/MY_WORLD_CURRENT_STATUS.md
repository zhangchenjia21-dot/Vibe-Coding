---
title: my world｜当前状态
status: current-project-status
version: 14.3
created: 2026-08-26
updated: 2026-09-05
phase: G5 World Semantics & GM Runtime
current_task: MW-008 Safe Markdown-Lite Narrative Rendering / Zcode; MW-005 Revision 3 Engineering PASS awaiting Owner UAT; G5-05 engineering completion evidence ready for Owner UAT
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
MW-005 Three Kingdoms Literary Style Primer ENGINEERING PASS — OWNER UAT (Revision 3 / IR#3)
G5-05 Meaningful Choice / Mechanics Integration ENGINEERING COMPLETION EVIDENCE READY — OWNER UAT
MW-006 Mechanics-Grounded World Consequence Vertical ENGINEERING PASS / CLOSED
MW-007 Mechanics Consequence Timeline Continuity ENGINEERING PASS / CLOSED
MW-008 Safe Markdown-Lite Narrative Rendering ACTIVE — ZCODE
G5-GATE                                     NOT YET
```

The project has not advanced to G6.

G5-04 is Product PASS / CLOSED. G5-05 has completed its two engineering verticals (MW-006 + MW-007) and now waits for Owner product validation in real play.

MW-005 remains a separate Owner-inserted source-content/runtime integration outcome anchored to G4. Revision 3 passed GPT IR#3 after changing only runtime salience/placement of the existing Primer; the next gate is Owner prose UAT in a new Three Kingdoms Game.

MW-008 is a separate presentation task triggered by literal Markdown visible in GM Narrative UI. It does not reopen G2 and must not change MW-005 semantics, raw Narrative truth, mechanics or world-state projection.

## 2. MW-005 Revision 3 result

```text
Work Item: MW-005
Name: Three Kingdoms Literary Style Primer v0.1
Capability-Anchor: G4 Primary Source Assets & Local Game
Revision: 3
Review-Round: IR#3
Implementation SHA: a52236c5ec55bf07a727b4e07c4ef63572b18555
Reviewer: GPT
Status: ENGINEERING PASS — OWNER UAT
```

Revision 3 was triggered because Owner UAT after IR#2 found that the Primer was technically present but the visible prose shift remained too weak. The correction deliberately kept Primer/source bytes and Source generation unchanged and changed only request-time salience.

IR#3 verified the actual production diff and final-request tests. The result is:

```text
Factual Game / World / Character / World-Turn / mechanics material
→ first

Literary Style Reference
→ derived request-only material
→ exactly once
→ late Narrative-only anchor
→ concise positive voice cue
```

Narrative consumers that receive the anchor:

```text
first opening
ordinary continuation
Public d20 resolution narrative
Public d20 NO_CHECK narrative
Public d20 degraded narrative
```

Consumers that remain style-free:

```text
Public d20 control / control_recovery
G5-04 project_world_only()
G5-01 semantic analysis
```

Protected authority boundary:

```text
Literary Style Reference
= diction / syntax rhythm / address / etiquette / dialogue / narrative distance / information-delivery exemplar

!= Game world truth
!= future canon
!= original-novel plot authority
!= NPC destiny
!= Player/actor Knowledge
!= World Evolution causal input
!= mechanics-control authority
!= semantic-consequence authority
!= mandatory output protocol
```

Approved Source generation remains immutable:

`58966f73dfade50b0aa7536aad38a8840e614016975e8beba0735f7dd14ab443`

If Owner still cannot perceive a meaningful prose difference after Revision 3, do **not** add another prompt-salience layer. The next MW-005 revision should revisit Primer content/selection strategy.

Review record:

`my-world/docs/mw005/MW-005_INDEPENDENT_REVIEW_IR3.md`

## 3. G5-05 engineering completion state

MW-006 established:

```text
Program-owned Public d20 result
→ bounded grounding in the normal G5-01 semantic opportunity
→ accepted free-form Narrative remains the source of concrete consequence
→ durable world consequence through the existing mutation seam
```

MW-007 then proved with zero production code changes that mechanics-grounded consequences remain coherent through:

```text
Save → close → reopen → Continue
and
pre-action Save → later mechanics/Narrative/consequence → Restore
```

No second mechanics truth, hardcoded outcome-effect map, reroll protocol, Narrative gate/retry, SQLite schema or generic Timeline platform was added.

G5-05 now needs Owner UAT only. Suggested path:

```text
meaningful risky action
→ visible Public d20 result
→ natural GM consequence
→ later world remembers it
→ Save/reopen preserves it
```

Also confirm NO_CHECK remains natural, not every action becomes a check, and mechanics do not dominate free-form Narrative.

If this feels right, G5-05 can be Product-closed without another speculative mechanics backend task.

## 4. MW-008 Safe Markdown-Lite Narrative Rendering

```text
Work Item: MW-008
Name: Safe Markdown-Lite Narrative Rendering
Capability-Anchor: G2 Narrative Conversation View / presentation
Inserted-By: Owner UAT observation during G5
Implementer: Zcode + GLM-5.3-flash
Reviewer: GPT
Revision: 1
Review-Round: 0
Status: ACTIVE — ZCODE
```

Task Packet:

`my-world/docs/tasks/MW-008_SAFE_MARKDOWN_LITE_NARRATIVE_RENDERING_TASK.md`

Primary product rule:

> **Render presentation semantics at the UI boundary; preserve authored Narrative bytes everywhere else.**

Initial whitelist only:

```text
**text**  → bold
*text*    → italic
---       → thematic separator when it is the whole trimmed line
```

Raw GM text in Conversation, persistence, Timeline and future Provider context must remain unchanged. Rendering is disposable UI projection only.

Security boundary:

```text
[color=red] / [url] / [img] / arbitrary Godot BBCode
→ literal text
→ never interpreted merely because RichTextLabel supports BBCode
```

Streaming chunk boundaries must be handled correctly. A transient current-GM view buffer is allowed if necessary, but it must not become a second durable history store.

MW-008 explicitly does **not** authorize full CommonMark/GFM, HTML, links/images, Markdown prompting, a Narrative output gate, typography redesign or G5-06 world-state UI work.

## 5. Worktree hygiene / routing

Owner weekend routing override remains active through **2026-09-06 23:59 (+08:00)**:

```text
Zcode + GLM-5.3-flash → primary implementation owner for new code-changing work
GPT                    → semantics / architecture / task shaping / Independent Review
```

At 2026-09-07 00:00 (+08:00), absent a new Owner instruction, long-term Codex-backend / Kimi-frontend routing resumes automatically.

Owner worktree rule:

```text
all new my-world task worktrees
→ D:/AI/Projects/.worktrees/my-world/<task-or-revision>
```

Revision 3 evidence confirms the old top-level MW-006/MW-007/baseline task worktrees were inspected and safely removed, then the active R3 worktree was created under `.worktrees`. After IR#3, MW-008 may clean the R3 worktree only after verifying clean+pushed+no unknown user work, then create:

`D:/AI/Projects/.worktrees/my-world/mw-008`

Registered worktrees must be removed with `git worktree remove`; never blindly delete them from Explorer.

## 6. Other open product/debt items

MW-004 remains `IMPLEMENTED — OWNER UAT`; protected principle:

> **The GM owns freedom to advance the world; the Player owns new meaningful choices for the protagonist.**

MW-003 remains `ENGINEERING PASS — OWNER UAT`; positive visual feedback is not silently converted into Product PASS.

A pre-existing G3-04 assertion is stale after MW-004 because it treats the literal phrase `Current Game Context` in GM instructions as proof of raw persisted-context leakage. Repair separately; do not fold it into MW-005, G5-05 or MW-008.
