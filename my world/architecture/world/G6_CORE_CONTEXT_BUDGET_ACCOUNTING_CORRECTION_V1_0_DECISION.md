---
title: my world｜G6 Core Context Budget Accounting Correction v1.0
status: FROZEN / CURRENT CORRECTION
version: 1.0
created: 2026-09-08
updated: 2026-09-08
phase: G6 RPG Core Closure
owner: Owner + GPT
triggered_by:
  - architecture/reviews/DEVELOPMENT_AUDIT_2026-09-08_ADOPTION_DECISION.md
work_item: MW-020
---

# G6 Core Context Budget Accounting Correction｜FROZEN

## 1. Proven defect

Current `WorldTurnContextProjector.project()` does not account for `MAX_PROJECTED_CHARS` from the actual assembled context exactly once.

Observed code path:

```text
semantic change blocks selected
→ projected_chars = sum(change blocks)
→ text = heading + joined change blocks
→ knowledge called with projected_chars
→ agency called with projected_chars + text.length()
→ evolution called with projected_chars + text.length()
```

This creates two opposite accounting errors:

1. headings / separators can be omitted from some earlier budget checks;
2. already assembled semantic-change text is later counted again through `projected_chars + text.length()`.

Real product consequence:

```text
NPC Agency / World Evolution is already durable and current
→ projected GM context still has physical room
→ accounting says budget is exhausted
→ the durable material is omitted
→ later GM may act as if a real world action/evolution never happened
```

This directly threatens the living-world core promise and must be corrected before the next Owner re-UAT.

## 2. Correct accounting rule

`MAX_PROJECTED_CHARS` remains exactly `16000`.

For every section appended to the World-turn GM context, budget consumption is defined by the **actual final assembled `context_text` characters**.

Formal rule:

```text
used budget before next section
= length of context_text already assembled
+ separator that will actually be inserted before the next non-empty section

candidate fits
iff
used + candidate_section.length <= MAX_PROJECTED_CHARS
```

Every visible heading, newline and inter-section separator counts exactly once.

The final successful projection must satisfy:

```text
context_text.length() <= MAX_PROJECTED_CHARS
```

Program must not maintain a second approximate counter whose meaning diverges from the text actually returned.

## 3. World-change section

The initial `Materialized World Changes` heading and separators are part of the same budget.

When choosing recent matching change blocks, the implementation must ensure the **actual rendered changes section** fits the limit, not merely the sum of block bodies.

Do not enlarge the limit to preserve prior accidental output volume.

## 4. Knowledge / Agency / Evolution

The existing semantic ordering remains:

```text
Materialized World Changes
→ Actor Knowledge Provenance
→ Independent Actor Actions
→ World Evolution Events
```

This correction does not change semantic priority or retrieval windows.

Each later section receives the exact remaining budget implied by the already assembled text. If a full section does not fit under the existing all-or-nothing section behavior, it remains omitted exactly as today; this task does not invent partial-line truncation or semantic ranking.

## 5. Protected semantics

Do not change:

- `MAX_PROJECTED_CHARS = 16000`;
- recent world-change turn limit;
- Knowledge event/actor ceilings;
- latest matching Agency-cycle policy;
- World Evolution recent-event policy;
- current accepted-hash filtering;
- stale/displaced-future rejection;
- `World Truth != actor Knowledge != human-player disclosure`;
- any durable World/Knowledge/Agency/Evolution storage;
- Narrative authority or Provider request lifecycle.

This is accounting correctness only.

## 6. No G7 pull-forward

MW-020 does **not** authorize:

- Context Orchestrator;
- semantic retrieval/ranking;
- summarization/recompression;
- embeddings/vector search;
- larger context windows;
- long-term memory redesign;
- caching/storage rewrite;
- new Provider calls;
- new SQLite tables;
- whole-file/world-context refactor beyond what is necessary to make accounting exact and testable.

## 7. Boundary acceptance

At minimum deterministic tests must prove:

1. **fits exactly** — a valid Agency section whose final assembled context ends exactly at the 16,000-character ceiling is included;
2. **one over** — the same class of section is omitted when final assembled output would exceed the ceiling by one character;
3. **no double-count** — a case equivalent to the audit reproduction includes an Agency action that physically fits even after substantial semantic-change text;
4. **heading/separator counted** — world-change/knowledge/agency/evolution section headings and real separators are included in accounting;
5. **final invariant** — every successful projection has `context_text.length() <= MAX_PROJECTED_CHARS`;
6. existing hash/currentness filtering and section ordering remain unchanged;
7. legitimate no-Agency / no-Evolution states remain quiet and valid.

Tests must use existing legitimate record constructors/validators rather than bypassing production contracts.

## 8. Product / gate effect

MW-020 is a correctness repair, not a new player-facing feature. Engineering PASS is sufficient for integration, but it is required before the fresh Package-0 Owner build because otherwise re-UAT of living-world behavior would be contaminated by a known context omission defect.

After MW-020 integration:

```text
prepare fresh Owner build
→ focused Package-0 re-UAT
→ only then Product verdicts for MW-018 R1 / MW-015 R1 / MW-019 R1
```
