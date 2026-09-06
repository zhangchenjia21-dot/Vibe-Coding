---
title: my world｜当前状态
status: current-project-status
version: 15.5
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-011 Revision 2 Player Character Profile Surface
current_owner: ZCODE weekend implementation / GPT semantic-review lane
parent_task: G6 RPG Experience & Internal Declarative UI Host
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
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED
G5-GATE                                     PRODUCT PASS

G6 RPG Experience & Internal Declarative UI Host ACTIVE
MW-011 G6 RPG Host ViewModel Baseline       R1 ENGINEERING PASS / INTEGRATED; OWNER UI UAT NOT PASS
MW-011 Revision 2 Player Profile Surface    ACTIVE — ZCODE
MW-012 Zhang Chen Player Character Card     ENGINEERING PASS / INTEGRATED; CONTENT INGRESS CONFIRMED BY UAT
```

G5 remains closed. G6 remains active.

## 2. Current implementation baseline

Remote implementation main before MW-011 R2 task issuance was:

`my-world@6338af5665c5137d9a9528776e77a13ffb924ea6`

It contains both previously reviewed outcomes:

```text
7972ab74ccc1d5368f8ca32d4fd4fd83173aa04d  MW-011 R1 integrated
6338af5665c5137d9a9528776e77a13ffb924ea6  MW-012 R2 integrated
```

R1 review / integration records remain authoritative for their engineering boundaries.

## 3. MW-011 R1 Owner UAT result

The Owner tested the integrated playable build with Zhang Chen selected as Player Character.

Observed positive results:

- Narrative style/prose is acceptable to the Owner;
- Zhang Chen-specific Character content is clearly reaching GM context and influencing the opening Narrative;
- Player Host identity/profile and World Surface Overview/Save shell are present;
- Narrative remains the dominant center surface.

Observed product failure:

> despite the very detailed Zhang Chen Character Card, the left Player Host still shows essentially only identity/T0 context plus empty recent-action/session metadata at a fresh opening, leaving large dead space.

Therefore:

```text
MW-011 R1 Engineering = PASS
MW-011 R1 Owner Product UAT = NOT PASS
```

Formal UAT record:

`my-world/docs/mw011/MW-011_OWNER_UAT_R1_RESULT.md`

This is a same-outcome information-density defect, so task lineage continues as **MW-011 Revision 2**, not a new MW ID.

## 4. Root cause

Current data path is intentionally safe but too narrow:

```text
rich Character semantic_sections
→ frozen Character source_projection
→ MW-009 player-safe projection
   (identity/profile/world/entry/known_facts only)
→ MW-011 ViewModel
   (+ recent actions / Player-turn count)
→ Player Host
```

Character Card v0.2 semantic sections currently permit only `gm_reference` / `gm_private`. MW-011 R1 correctly did not expose those sections directly to the player. The missing product seam is an explicit player-facing Character profile projection.

Do not solve this by reclassifying every `gm_reference` section as player-visible or by letting UI parse raw Character Markdown.

## 5. MW-011 Revision 2 — ACTIVE

Canonical architecture:

`my world/architecture/ui/G6_PLAYER_CHARACTER_PROFILE_PROJECTION_V0_1_DECISION.md`

Executable addendum:

`my-world/docs/tasks/MW-011_REVISION2_PLAYER_CHARACTER_PROFILE_SURFACE_ADDENDUM.md`

Identity:

```text
Work Item: MW-011
Revision: 2
Review Round: IR#1 → IR#2
Name: Player Character Profile Projection + Player Host Surface
Implementer: Zcode + GLM-5.3-flash
Reviewer: GPT
Branch: mw-011-r2-player-character-profile-surface
Worktree: D:/AI/Projects/.worktrees/my-world/mw-011-r2
Status: ACTIVE — ZCODE
Return ceiling: READY FOR INDEPENDENT REVIEW
```

Required outcome:

```text
optional bounded Character player_profile
→ frozen Game-local selected Character projection
→ separate fail-closed Player Character Profile Projection
→ MW-011 presentation-only ViewModel
→ rich but bounded Player Host
```

R2 must preserve R1 and G5 safety boundaries. It must not expose raw `semantic_sections`, GM reference/private material, omniscient Runtime truth or latest Source Library bytes to an existing Game.

## 6. Player profile architecture

Character Card v0.2 gains one optional internal presentation field:

```text
player_profile:
  headline
  summary
  groups[]:
    group_id
    title
    items[]
```

This is authored presentation material, not gameplay/world authority. Existing v0.2 cards without it remain valid.

A new bounded domain projector reads only the Game-local frozen `player_profile`. MW-009 remains the owner of current Player-known facts and is not broadened to consume raw Character sections.

Old Games created from Character generations without the profile must remain unchanged and must not backfill from the current Source Library.

## 7. Zhang Chen R2 consumer requirement

The integrated MW-012 Character semantics remain protected and are not reopened.

MW-011 R2 may create a new Zhang Chen Source generation whose only semantic change is **none**: it adds a player-facing structured presentation profile faithfully derived from the already Owner-approved material.

Expected visible profile includes:

```text
24岁 · 现代穿越者
退役武警义务兵 / 985高校出身 / 历史与军事爱好者
背景
性格
能力
局限
初始目标
行为原则
随身物品
```

The Source package should normally increment from `0.1.0` to `0.1.1`, producing a new immutable generation fingerprint for future Games. Existing Games remain frozen on their prior generation.

## 8. G6 protected order

G6 remains consumer-first:

```text
Runtime projection
→ presentation-only ViewModel
→ real UI consumer
→ Runtime Asset Resolution only for actual visual consumers
→ portrait / scene / authored-map presentation
→ Character / Relationship / Inventory / Faction / Map / Save real surfaces
→ Expansion mechanic-state consumer
→ Internal Declarative UI Host v0.1
→ bounded Action Intent
→ responsive / Theme / navigation
→ Owner UAT / visual polish
```

MW-011 R2 is still a real consumer correction. It must not expand into a universal UI DSL, Character stat ontology, Mod schema or Creator.

## 9. G5 invariants still protected

- free-form Narrative remains primary and is not gated by semantic/Knowledge/Agency/Evolution extraction;
- World Truth != actor Knowledge != human-player disclosure;
- stable NPCs may act independently;
- World Evolution may hold or selectively advance;
- Public d20 remains program-owned mechanics grounding, not a second world truth;
- Save/reopen/Restore currentness remains authoritative;
- Literary Style Reference remains expression-only;
- raw accepted Narrative bytes remain authoritative; Markdown-lite remains disposable UI projection.

## 10. Routing

Owner weekend override remains active through **2026-09-06 23:59 (+08:00)**:

```text
Zcode + GLM-5.3-flash → primary implementation owner for new code-changing work
GPT                    → semantics / architecture / task shaping / Independent Review
```

At **2026-09-07 00:00 (+08:00)**, absent a new Owner instruction, routing returns to Codex-backend / Kimi-frontend according to the implementation seam.

Task worktree policy remains:

`D:/AI/Projects/.worktrees/my-world/<task-or-revision>`

Keep MW-011 R2 worktree through GPT Independent Review.

## 11. Immediate route

```text
Zcode implements MW-011 R2 from the frozen decision/addendum
→ push exact candidate
→ GPT IR#2 on actual diff/tests/evidence
→ integrate reviewed candidate
→ publish new Zhang Chen current generation
→ Owner creates a fresh Zhang Chen Game
→ Owner UI UAT on rich Player Host
→ continue next real G6 consumer/visual vertical
```
