---
title: my world｜G6 Five Recommended Actions UAT Correction v1.0
status: FROZEN / CURRENT CORRECTION
version: 1.0
created: 2026-09-08
updated: 2026-09-08
phase: G6 RPG Core Closure
owner: Owner + GPT
triggered_by: my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
supersedes_when_conflicting:
  - architecture/ui/G6_FIVE_RECOMMENDED_ACTIONS_V1_0_DECISION.md
---

# Five Recommended Actions UAT Correction｜FROZEN

## 1. Owner UAT finding

Package 0 Owner play confirmed that the first MW-019 implementation works mechanically but its product presentation and recommendation shape are wrong in four ways:

1. recommendation buttons contain long full-action prose, so the area is crowded and hard to scan;
2. the player wants to choose a broad direction first, then receive the detailed editable action text in the existing composer;
3. a five-item response can read like one multi-step plan split into five sentences rather than five independently selectable next actions;
4. recommendation/composer controls are visually too small and cramped for comfortable long-form play.

This is a same-outcome MW-019 Revision, not a new independent feature.

## 2. Corrected user interaction

The revised interaction is:

```text
accepted GM Narrative
↓
one bounded Action Recommender call
↓
5 paired recommendations
  short label + detailed draft
↓
UI shows only the five short labels
↓
Player clicks one label
↓
that item's detailed draft replaces PlayerInput
↓
focus + caret for editing
↓
normal Send / Ctrl+Enter remains authoritative
```

A click never sends automatically.

The detailed draft is generated in the same recommendation call and held only in ephemeral recommendation state. Clicking does **not** trigger a second Provider call.

This keeps recommendation interaction immediate, bounded and fail-soft.

## 3. Short label vs detailed draft

Each recommendation has two player-safe texts:

```text
label
→ quick scan-level direction
→ tells the player broadly what they would do
→ no long rationale / multi-sentence detail

draft
→ concrete editable natural-language action
→ may contain the detail needed to actually submit the intended action
```

Example shape only:

```text
label: 混入流民打听黄巾余部

draft: 我先混进附近流民中……
```

The model owns the wording. Program does not generate a label by truncating/summarizing a draft and does not expand a label into a draft heuristically.

## 4. Five standalone alternatives

The five recommendations must be understood as:

> **five independently selectable alternative next actions, not five sequential steps or five sentences from one plan.**

Model-facing semantics should make clear:

- each item can stand alone as the player's next submitted action;
- selecting one does not assume the other four happen before/after it;
- different approaches are welcome when the context supports them;
- no fixed tactical/social/exploration/category quota exists;
- none is ranked as the best/legal/only option.

Program MUST NOT implement a semantic diversity classifier, similarity score, category quota, aggressiveness balance, ranking system or plan-step detector.

Program may reject only machine-level exact duplicates/invalid structure.

## 5. Revised ephemeral response contract

The existing recommendation lane remains one dedicated player-safe background call.

The revised first-party machine response is conceptually:

```json
{
  "actions": [
    {"label":"简短方向","draft":"详细可编辑行动草稿"},
    {"label":"简短方向","draft":"详细可编辑行动草稿"},
    {"label":"简短方向","draft":"详细可编辑行动草稿"},
    {"label":"简短方向","draft":"详细可编辑行动草稿"},
    {"label":"简短方向","draft":"详细可编辑行动草稿"}
  ]
}
```

Structural v0.1 correction limits:

- exact one-key top-level object remains acceptable;
- exactly 5 action objects on success;
- each action object has exactly `label` + `draft`;
- both values are non-empty strings after trim;
- `label` <= 48 Unicode characters;
- `draft` <= 400 Unicode characters;
- exact duplicate labels are invalid;
- exact duplicate drafts are invalid;
- bounded whole response may increase only as narrowly required for five detailed drafts;
- no model reasoning is stored or displayed.

These are payload/display bounds, not semantic limits.

Recommendations remain ephemeral and non-authoritative; no new SQLite table or Save truth.

## 6. Player-safe input remains unchanged

R1 does not authorize new omniscient inputs.

The Action Recommender continues to consume only bounded accepted player-visible Conversation material under the current MW-019 input/currentness rules.

Do not add merely for this revision:

- raw World state;
- stable actor private profiles;
- NPC-private Knowledge;
- Agency/Evolution private state;
- Source-current hidden material;
- Information Curation storage;
- IDs/hashes/receipts.

Character-guided recommendations are an already-approved **later Package 2** outcome and are explicitly not pulled into this revision.

## 7. Readability correction

This revision includes a bounded Narrative Host readability correction because the existing recommendation UI is materially too small/cramped for Owner play.

Target behavior:

- short labels are comfortably readable without displaying the full draft inside the button;
- controls have clearly larger text, height and padding than the current compact 12/13px, ~28px-button presentation;
- ordinary desktop layouts prefer a comfortable one/two-column arrangement rather than squeezing five options into tiny controls merely to save vertical space;
- narrow layout may wrap vertically rather than shrink text to preserve density;
- PlayerInput/composer text and related primary controls receive the minimum sizing adjustment needed for comfortable reading/editing;
- no horizontal overflow or clipped essential label text at supported widths.

Implementation should use the existing theme/layout system where practical rather than introducing one-off global styling architecture.

This is not final visual-art polish and does not authorize a global UI redesign.

## 8. Lifecycle invariants remain

All existing MW-019 lifecycle protections remain:

- accepted Narrative triggers opportunity;
- free-form Player input always remains available;
- foreground Send/d20 interrupts stale recommendation work;
- Regenerate/correction clears old recommendations;
- Restore/reopen may request one current fresh set;
- stale callbacks cannot publish;
- rendering/editing/resizing/clicking does not start Provider work;
- recommendation failure never blocks normal play.

## 9. Explicit non-scope

R1 does not implement:

- Character/personality-guided recommendations;
- accepted action → Character evolution feedback;
- OOC / GM Guidance;
- Context budget correction;
- Debug Mode / richer failure diagnostics;
- Structured Output general reliability framework;
- fence-stripping / hidden retries / Provider fallback;
- semantic recommendation ranking/scoring/classification;
- recommendation persistence/history;
- Dynamic UI Host;
- global visual redesign.

## 10. Product acceptance direction

Focused Owner re-UAT must verify:

1. the five visible recommendation controls can be scanned quickly;
2. each visible label communicates the broad intended action without full prose crowding the UI;
3. clicking one immediately fills a useful detailed editable draft and never sends;
4. the five suggestions feel like five actual alternatives rather than one plan split into steps;
5. free-form action still feels primary and unrestricted;
6. the recommendation/composer area is materially easier to read and use at normal desktop size;
7. no stale recommendation appears after foreground action / Regenerate / Restore.

Product PASS remains Owner-only after the complete Package 0 correction train and context-budget correctness fix are installed for focused re-UAT.
