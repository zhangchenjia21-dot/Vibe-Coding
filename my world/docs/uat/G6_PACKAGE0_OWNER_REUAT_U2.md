---
title: my world｜G6 Package 0 Focused Owner Re-UAT U2
status: OWNER UAT COMPLETE / ORIGINAL CORRECTIONS PRODUCT PASS / NEW UX CORRECTION REQUIRED
version: 1.1
created: 2026-09-08
updated: 2026-09-08
owner: Owner
semantic_owner: GPT
product_code_artifact: 5e5fd006fd17683ae811b17138df76a18b0b96aa
launch_command: D:\AI\Projects\my-world\run-game.cmd
supersedes_for_current_uat:
  - my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
---

# G6 Package 0｜Focused Owner Re-UAT U2

## 1. Final U2 verdict

Owner completed ordinary-play focused re-UAT on the frozen product artifact:

`5e5fd006fd17683ae811b17138df76a18b0b96aa`

Owner feedback:

1. main Narrative chat has no practically usable visible progress scrollbar; vertical navigation depends on mouse wheel and is cumbersome;
2. after exiting and continuing a Game, the main Narrative chat opens at the top instead of the latest progress; Continue should default to the latest/current bottom position;
3. other tested Package-0 correction outcomes are broadly PASS.

Formal product verdicts for the original correction train:

```text
MW-018 R1 People                 = PRODUCT PASS
MW-015 R1 Important Experiences = PRODUCT PASS
MW-019 R1 Recommendations        = PRODUCT PASS
MW-020 Context Budget            = ENGINEERING PASS_WITH_NOTES / no standalone Product gate / no new U2 continuity defect reported
```

Package 0 does **not** close yet because U2 found two new first-party Narrative Host usability defects. These are not regressions of the three original product outcomes and do not reopen MW-018 / MW-015 / MW-019.

## 2. New U2 findings

### U2-F01｜Main Narrative scrollbar is not practically visible/draggable

Observed product problem:

- the central/main conversation can become long;
- there is no useful visible progress scrollbar for direct position control;
- Owner must rely on mouse-wheel scrolling, which is cumbersome in a long-running text RPG.

Required direction:

- `NarrativeScroll` must expose a visible, comfortably draggable vertical scrollbar whenever content exceeds the viewport;
- keep this local to the Narrative Host rather than performing a global theme redesign;
- ordinary wheel/keyboard scrolling must continue to work.

This is a usability requirement, not visual polish.

### U2-F02｜Continue/reopen defaults to the top instead of latest progress

Observed product problem:

- exit Game/app;
- continue the existing Game;
- restored Narrative history appears at the top;
- Owner must manually scroll through prior history to reach the current/latest point.

Required direction:

```text
Continue / reopen existing Game
→ rebuild current accepted Conversation projection
→ after layout settles, default Narrative position to latest/current bottom
```

This default must not create a permanent forced-follow behavior. During an active session, if the Player deliberately scrolls upward to read history, ordinary incoming/render refreshes must continue respecting that reading position until the Player returns near the bottom or a full explicit history reconstruction requires a new current position.

No durable scroll-position persistence is required for this correction.

## 3. Source-level root-cause note

GPT source inspection after U2 found a direct implementation explanation consistent with Owner observations:

- `NarrativeScroll` uses the same theme environment in which the recommendation-area scrollbar previously required an explicit local width; the main Narrative scrollbar currently has no equivalent local width treatment;
- `redraw_from_conversation()` explicitly sets `_follow_scroll = true` and follows to the bottom, while `_initialize_session()` renders restored entries but does not perform the same reopen-bottom step.

This is sufficient for bounded Task Shaping; Codex must still confirm exact current code behavior before changing it.

## 4. Correction lineage

The two findings are one coherent new product outcome: **Narrative history navigation / reopen positioning**.

They are not recommendation-specific and therefore must not be placed under MW-019 revision lineage.

Shape a new flat Work ID:

`MW-021｜Narrative Scroll Navigation & Reopen Position`

One bounded task should cover both findings because they affect the same `NarrativeScroll` presentation seam and can be accepted together.

## 5. Protected behavior

The correction must preserve:

- accepted Conversation remains canonical; UI remains projection only;
- no new persistence/table for scroll position;
- no change to Save/Restore/Regenerate truth/currentness;
- no auto-scroll loop that prevents the Player from intentionally reading older history;
- no changes to Provider calls, Narrative generation, recommendation generation, Public d20 or world semantics;
- no global UI/theme redesign;
- free-form Player input remains primary.

## 6. Package 0 exit

Package 0 may close after MW-021 Engineering PASS/integration and a bounded confirmation that:

- a long main Narrative surface has a visible draggable vertical scrollbar;
- Continue/reopen lands at the latest/current Narrative position by default;
- deliberate manual upward reading remains possible without constant forced snap-back.

Because MW-018 / MW-015 / MW-019 are already Product PASS from U2, MW-021 does not require replaying the entire correction UAT unless its implementation materially touches unrelated product behavior.

## 7. Next package

After MW-021 closes Package 0, proceed immediately to:

**Package 1 — UAT Observability / Debug Mode v0.1**.
