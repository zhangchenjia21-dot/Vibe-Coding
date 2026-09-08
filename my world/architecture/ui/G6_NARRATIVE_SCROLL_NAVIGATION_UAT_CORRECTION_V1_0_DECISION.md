---
title: my world｜G6 Narrative Scroll Navigation UAT Correction v1.0
status: FROZEN / CURRENT CORRECTION
version: 1.0
created: 2026-09-08
updated: 2026-09-08
owner: Owner + GPT
triggered_by: my world/docs/uat/G6_PACKAGE0_OWNER_REUAT_U2.md
---

# Narrative Scroll Navigation UAT Correction｜FROZEN

## 1. Product problem

Focused Owner UAT confirmed the original Package-0 corrections broadly pass, but exposed two central Narrative Host usability defects:

1. long main conversation history lacks a practically visible/draggable vertical scrollbar;
2. Continue/reopen of an existing Game rebuilds history at the top instead of the latest/current Narrative position.

These are one bounded navigation/reopen-position outcome.

## 2. Required behavior

### A. Visible main Narrative scrollbar

When `NarrativeScroll` content exceeds its viewport:

- a vertical scrollbar must be visible and directly draggable;
- the scrollbar must have a practical local width/hit target;
- wheel/keyboard scrolling remains available;
- no global theme redesign is required.

When content does not overflow, normal ScrollContainer behavior may keep the bar unnecessary/hidden.

### B. Continue/reopen defaults to current/latest bottom

On binding/reopening an existing current Game:

```text
durable accepted Conversation
→ rebuild Narrative visual projection
→ wait until the rebuilt layout has a valid scroll range
→ position NarrativeScroll at the current/latest bottom
```

The Player should immediately see the latest accepted progress rather than the first historical message.

### C. Manual historical reading remains respected

This correction must not create an unconditional auto-scroll loop.

During ordinary active play:

- if the Player is already near the bottom, existing follow-latest behavior may continue;
- if the Player deliberately scrolls upward, normal incremental rendering must not continuously snap them back down;
- once the Player returns near the bottom, normal follow-latest behavior may resume.

A full Continue/reopen is a new session projection and therefore defaults to latest bottom.

## 3. No persistent scroll-position owner

Do not add SQLite/file/session persistence merely to remember visual scroll position.

This correction defines a startup/reopen default, not durable UI state.

## 4. Protected boundaries

Do not change:

- Conversation truth or accepted bytes;
- Save/Restore/Regenerate semantics;
- Provider/model calls;
- world semantics / Agency / Evolution / Information Curator;
- recommendation generation or click semantics;
- Public d20;
- free-form Player input;
- global theme architecture.

UI remains projection only.

## 5. Acceptance

Engineering must prove at minimum:

1. overflowing long Narrative history exposes a visible draggable main vertical scrollbar with practical width;
2. Continue/reopen of a history long enough to overflow lands at the bottom/latest accepted content after layout settles;
3. a Player manually scrolling upward during an active session is not forcibly snapped back by ordinary incremental rendering;
4. returning near the bottom re-enables normal follow-latest behavior;
5. short/non-overflowing history remains usable;
6. no Conversation/World/Timeline/persistence mutation occurs merely from scroll behavior.

## 6. Package 0 relation

MW-018 R1 / MW-015 R1 / MW-019 R1 already received Owner Product PASS in U2.

This correction receives new Work ID `MW-021` because it is a distinct Narrative Host usability outcome. Package 0 closes after MW-021 Engineering PASS/integration plus bounded Owner confirmation; a full repeat of the prior UAT is unnecessary unless implementation scope expands materially.
