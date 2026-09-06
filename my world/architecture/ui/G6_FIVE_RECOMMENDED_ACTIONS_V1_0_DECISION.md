---
title: my world｜G6 Five Recommended Actions v1.0 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-06
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
owner: Owner + GPT
trigger:
  - Owner explicitly deferred MW-018 Owner UAT and requested five recommended actions first, followed by combined UAT.
parent:
  - MY_WORLD_总体规划路线图_CURRENT.md
  - MY_WORLD_核心设计原则_CURRENT.md
  - architecture/ui/G6_SESSION_SHELL_INFORMATION_OWNERSHIP_DECISION.md
---

# G6 Five Recommended Actions｜FROZEN

## 1. Product purpose

After each successfully accepted GM Narrative, the Narrative Host should help the player answer:

> **“我现在可以做些什么？”**

The product provides **exactly five model-generated recommended actions** while preserving the existing unrestricted natural-language composer.

This feature is guidance, not a choice gate.

Protected rule:

> **Five recommended actions != five allowed actions.**

The player may always ignore every recommendation and type any natural-language action supported by the game.

## 2. User-facing behavior

When the latest GM Narrative becomes durably accepted:

```text
accepted GM Narrative
↓
background Action Recommender
↓
5 recommended action buttons
↓
player may click one
↓
that action is PREFILLED into the existing composer
↓
player may edit it freely
↓
existing Send / Ctrl+Enter path submits it
```

A recommendation click MUST NOT:

- immediately send a turn;
- bypass the normal composer;
- bypass Public d20/action adjudication when enabled;
- create authoritative world state;
- lock out free-form input.

Clicking a recommendation replaces the current composer text with that recommendation, focuses the composer and places the cursor for editing. The recommendation controls remain an optional drafting aid until the next foreground action begins.

## 3. Placement / presentation

Recommendations belong to the **Narrative Host**, immediately above or adjacent to the composer, not in World Information and not inside durable Narrative history.

Recommended v0.1 presentation:

```text
[当前 GM Narrative]

推荐行动
[行动一] [行动二]
[行动三] [行动四]
[行动五]

[自由输入你的行动…………………………] [发送]
```

Exact wrapping/grid layout may adapt to width, but:

- all five actions should be easy to scan;
- the recommendation area must not dominate the Narrative Host;
- no horizontal overflow at normal desktop/narrow supported layouts;
- long text should wrap or truncate safely with a useful tooltip if needed.

No numbering, icons, keyboard shortcuts, ranking badges, probability labels or “best choice” indicator are required in v0.1.

## 4. Recommendation semantics are model-owned

The model decides the content of the five suggestions.

Prompt semantics should encourage:

- concrete, immediately actionable player actions;
- meaningful variety in approach when context supports it;
- first-person / ready-to-edit natural-language phrasing;
- actions grounded only in what the player has actually seen/knows;
- no claimed guaranteed outcome;
- no hidden NPC/world information;
- no implication that the five items exhaust legal possibilities.

Program MUST NOT use a semantic classifier to enforce diversity, aggressiveness, social/exploration quotas, “best action” ranking or style categories.

Program may enforce only machine structure such as exact item count, string type, length and exact duplicate rejection.

## 5. Dedicated ephemeral recommendation call

v0.1 uses a **dedicated background Action Recommender model call** after an accepted GM Narrative.

Reason:

- changing the streaming GM Narrative into a JSON/envelope response would contaminate or complicate the authoritative raw Narrative path;
- World semantic analysis has internal/omniscient actor material not appropriate for player-facing recommendation generation;
- Information Curator is sequenced after semantic work and owns durable player-information maintenance, so coupling recommendations to it would add avoidable latency and responsibility mixing.

Therefore:

```text
GM Narrative accepted durably
├─ existing World semantic / Information Curator background work
└─ Action Recommender (parallel, independent, fail-soft)
```

This adds one bounded Provider request for a successful recommendation opportunity.

Use the currently configured Provider/model. No hidden Provider switch or fallback.

## 6. Player-safe input boundary

Action Recommender input is derived only from **player-visible accepted Conversation history**.

v0.1 does not need omniscient Runtime state to produce recommendations.

Allowed material:

- latest durably accepted GM Narrative;
- latest associated accepted Player action when present;
- a bounded recent prefix of earlier accepted Player/GM text for continuity.

Do not send merely for recommendation quality:

- raw World state;
- stable actor registry/profile material;
- NPC-private Knowledge;
- Agency private actions/plans;
- hidden World Evolution;
- Source-current content;
- GM-private semantic sections;
- information_curation storage objects;
- internal IDs/hashes/receipts.

A simple recency/byte bound is structural and allowed. Do not build Program semantic retrieval/ranking for v0.1.

## 7. Bounded response contract

Recommended machine response:

```json
{
  "actions": [
    "我……",
    "我……",
    "我……",
    "我……",
    "我……"
  ]
}
```

v0.1 structural contract:

- exact top-level shape;
- exactly 5 actions on success;
- every action is a non-empty string;
- each action <= 240 characters;
- exact duplicate strings are invalid;
- bounded whole response size;
- no model reasoning is persisted or shown.

If the response is malformed, incomplete, duplicated beyond the valid contract, oversized or otherwise invalid, the recommendation opportunity fails soft. Program does not invent/fill missing actions with heuristics.

## 8. Ephemeral, non-authoritative state

Recommended actions are **derived UI assistance**, not Game truth.

v0.1 does not persist recommendations into:

- World state;
- Conversation accepted truth;
- Timeline authoritative nodes;
- Save data;
- information_curation;
- Source.

They live in the current UI/session recommendation state and are always bound to an exact current accepted Conversation prefix/opportunity.

Consequences:

- recommendation wording may differ if regenerated after reopen/Restore;
- no recommendation text enters later GM Context unless the player actually submits it as their action;
- recommendation generation cannot mutate the Game.

## 9. Lifecycle / currentness

### Accepted GM completion

A recommendation opportunity begins only after the GM Narrative is durably accepted.

This includes:

- GM-only opening;
- normal player-authored turns;
- accepted replacement after Regenerate/correction.

### Foreground priority

When the player starts/submits a new foreground action before recommendations finish:

- clear current visible recommendations;
- cancel/invalidate any in-flight recommendation request for the previous state;
- do not delay the player action.

Foreground play always wins.

### Regenerate / correction

At the start of replacing the latest GM response, old recommendations are no longer offered as current guidance.

After the replacement becomes durably accepted, generate a new set bound to the new accepted prefix.

A failed/cancelled GM generation does not create a new recommendation set.

### Restore / reopen

Because recommendations are ephemeral, activation/reopen/Restore may issue at most one fresh recommendation request for the current latest accepted GM Narrative when the session is ready.

If there is no accepted GM Narrative, show no recommendations.

A stale pre-Restore callback must never publish into the restored state.

Rendering, resizing, clicking tabs and editing text do not trigger recommendation Provider calls.

## 10. Failure behavior

Recommendation failure must never block:

- existing composer typing;
- Send / Ctrl+Enter;
- GM Narrative;
- Save / Restore;
- World semantic work;
- Information Curator;
- People cards;
- Public d20.

UI may show a quiet muted unavailable/loading state, but no blocking error overlay is required.

Free-form input remains the complete fallback.

## 11. Relationship to G6-G Bounded Action Intent

The roadmap previously placed generic `bounded Action Intent` later, after repeated Host/UI vocabulary exists.

MW-019 does **not** authorize that generic platform early.

This feature uses one fixed first-party interaction:

```text
recommended action button
→ prefill existing composer text
```

It may be implemented directly through the existing Narrative Host contract.

Later G6-G may use this as one real consumer/evidence point when deciding whether a reusable bounded intent vocabulary is justified.

Do not create arbitrary callbacks, NodePath execution, declarative intent schema, filesystem/OS actions or generic action bus in MW-019.

## 12. MW-018 UAT sequencing

Owner explicitly chose:

```text
MW-018 Engineering PASS / integrated
→ defer standalone MW-018 Owner UAT
→ implement MW-019 Five Recommended Actions
→ review/integrate MW-019
→ prepare one fresh Owner build
→ combined Owner UAT for MW-018 + MW-019
```

MW-018 remains `OWNER UAT PENDING`; deferral is not Product PASS.

Defects found in combined UAT retain their own task lineage:

- People-card defects → MW-018 revision;
- recommendation defects → MW-019 revision.

## 13. Product acceptance direction

In real play, Owner should be able to observe:

```text
GM Narrative finishes
→ five useful recommendations appear without blocking free input
→ each item reads like a plausible next action, not hidden knowledge or guaranteed result
→ click one fills the composer but does not send
→ edit the text freely
→ Send follows the exact existing action path
→ starting another turn clears stale recommendations
→ regenerated/restored context never shows recommendations from the wrong history
```

The Owner should still feel:

> **“I can do anything I can express, and these five options are only inspiration.”**

That product feeling is required for PASS.

## 14. Explicit non-scope

MW-019 / v0.1 does not implement:

- traditional branching-choice-only gameplay;
- automatic submit on recommendation click;
- recommendation ranking/probabilities;
- semantic diversity validators;
- persistent recommendation history;
- player preference training for recommendations;
- hotkeys for choices;
- generic Bounded Action Intent platform;
- MW-013 Declarative UI Host;
- recommendation-driven world mutation;
- new SQLite tables;
- global UI polish unrelated to the recommendation area.
