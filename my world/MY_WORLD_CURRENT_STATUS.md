---
title: my world｜当前状态
status: current-project-status
version: 17.11
created: 2026-08-26
updated: 2026-09-08
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: MW-021 Narrative Scroll Navigation & Reopen Position
current_owner: Codex
parent_task: G6 Core Closure Package 0
semantic_owner: GPT
owner_uat_required: bounded confirmation after integration
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.3
owner_uat_record: my world/docs/uat/G6_PACKAGE0_OWNER_REUAT_U2.md
active_task_packet: my-world/docs/tasks/MW-021_NARRATIVE_SCROLL_NAVIGATION_TASK.md
active_task_branch: mw-021-narrative-scroll-ux
active_correction_decision: architecture/ui/G6_NARRATIVE_SCROLL_NAVIGATION_UAT_CORRECTION_V1_0_DECISION.md
formal_product_code_base: 5e5fd006fd17683ae811b17138df76a18b0b96aa
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED

G6 RPG Core Closure + UAT Observability + Internal Dynamic UI ACTIVE
```

Package 0 state after Owner Re-UAT U2:

```text
MW-018 R1 Known / Off-screen People          PRODUCT PASS
MW-015 R1 Sparse Important Experiences       PRODUCT PASS
MW-019 R1 Recommendation UX                  PRODUCT PASS
MW-020 Core Context Budget Accounting        ENGINEERING PASS_WITH_NOTES / INTEGRATED
MW-021 Narrative Scroll Navigation           CURRENT / CODEX
↓
bounded Owner confirmation of two scroll behaviors
↓
Package 0 close
```

Package 1 UAT Observability / Debug Mode v0.1 remains first after Package 0 closes.

## 2. U2 final verdict

Formal record:

`my world/docs/uat/G6_PACKAGE0_OWNER_REUAT_U2.md@v1.1`

Owner judged the original correction outcomes broadly PASS and reported two new Narrative Host usability findings:

1. main chat lacks a practically visible/draggable vertical progress scrollbar, forcing mouse-wheel navigation;
2. Continue/reopen rebuilds the main chat at the top instead of the latest/current progress.

These do not reopen MW-018 / MW-015 / MW-019. They form a new bounded outcome under MW-021.

## 3. MW-021 — CURRENT

Frozen authority:

`architecture/ui/G6_NARRATIVE_SCROLL_NAVIGATION_UAT_CORRECTION_V1_0_DECISION.md`

Task Packet:

`my-world/docs/tasks/MW-021_NARRATIVE_SCROLL_NAVIGATION_TASK.md`

Task branch:

`mw-021-narrative-scroll-ux`

Formal product-code base:

`5e5fd006fd17683ae811b17138df76a18b0b96aa`

Task-packet/main preparation commit is governance/task metadata only; Codex must preserve the formal product-code base and report exact Starting HEAD.

Required outcome:

```text
long Narrative history
→ visible draggable vertical scrollbar

Continue / reopen existing Game
→ restore accepted Narrative projection
→ after layout settles, default to latest/current bottom
```

Protected behavior:

- deliberate manual upward reading is respected during active play;
- returning near bottom resumes follow-latest;
- no persistent scroll-position owner;
- no Conversation/World/Timeline/persistence changes;
- no Provider call;
- no recommendation/People/Experiences/Debug/Dynamic UI scope expansion;
- no global theme redesign.

Return ceiling: `READY FOR INDEPENDENT REVIEW`.

## 4. Source-level shaping evidence

Current Narrative view already contains near-bottom follow logic:

- `_on_narrative_scroll_changed()` disables follow when Player scrolls upward;
- `_follow_scroll_if_needed()` only moves to latest when follow is enabled;
- `redraw_from_conversation()` explicitly resets follow and follows to bottom;
- `_initialize_session()` renders restored entries but lacks the equivalent reopen-to-bottom step.

Main `NarrativeScroll` also lacks the local practical vertical-bar width treatment already used by the recommendation-local scrollbar.

Codex must verify current code before changing it; materially different root cause requires STOP/report.

## 5. Package 0 closure

After MW-021 Engineering PASS/integration, Owner does only a bounded confirmation:

1. long main chat exposes a visible/draggable vertical scrollbar;
2. Continue/reopen lands at latest progress;
3. manually scrolling upward does not cause constant forced snap-back.

No full repeat of People / Important Experiences / Recommendations UAT is required unless MW-021 expands outside its frozen scope.

If these pass, Package 0 closes and GPT immediately Task-Shapes Package 1 UAT Observability / Debug Mode v0.1.

## 6. Protected project invariants

- Model Freedom First;
- free-form Player natural-language action remains primary;
- `World Truth != actor Knowledge != human-player disclosure`;
- UI is projection, not second truth;
- Save / Restore / Regenerate currentness remains authoritative;
- no generic framework pulled forward solely for a UX correction.
