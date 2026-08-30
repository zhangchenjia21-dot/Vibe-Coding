---
title: my world｜Optional Temporal Source Scope Decision
status: current-architecture-decision
owner: GPT
created: 2026-08-30
updated: 2026-08-30
scope: G4 Source semantics
---

# G4｜Optional Temporal Source Scope Decision

## 1. Decision

T0-scoped Source / Post-T0 Canon Quarantine is an **optional Source capability**, not a mandatory mode for every World Pack or Character Card.

It exists when an authored Source genuinely has multiple starting temporal cuts and material outside the selected cut would leak future/canon answers into the current Game.

Typical use:

- historical settings with multiple selectable historical T0s;
- fictional/canonical settings with multiple authored temporal cuts where later canon must not contaminate an earlier start.

It is **not** required merely because an asset is a World Pack or Character Card.

## 2. No universal historical flag

Do not introduce a global `historical=true/false`, `temporal_mode`, family classification, or similar hard ontology just to decide whether quarantine applies.

The capability is expressed by authored structure and real need:

- World Entry-scoped `semantic_sections` when some material is only valid for a selected Entry/T0;
- Character `t0_profiles` + exact bindings when the Character genuinely changes across selectable T0s.

If that structure is absent, no temporal matrix is invented.

## 3. Non-temporal / single-state assets

A Character with no temporal-scope requirement may keep its complete reusable starting semantics in top-level `semantic_sections` and omit `t0_profiles` entirely.

For such a Character:

- no profile fallback is needed because there is no profile matrix;
- the temporal mechanism must not hard-block it merely because no binding exists;
- later Compatibility policy may still evaluate other explicit product constraints, but not an invented same-family or historical rule.

A World with no temporal-scope requirement may keep its reusable semantics top-level. Entries may still exist for scenario/opening/location choice without implying a historical future-quarantine matrix; Entry-scoped sections are used only when authored meaning actually differs by Entry.

## 4. Fixed-T0 multi-entry assets

A setting such as `world.ashtervia.afterglow` may have several Entries at the same authored T0. Multiple Entry bindings to one Character profile are valid, but this does not mean every future Afterglow-like asset must use Character T0 profiles.

If a future non-historical Character has no meaningful temporal variation, it may use top-level semantics only.

## 5. Historical assets

For assets such as Han-era historical World/Characters, multiple selectable historical T0s create a real future-information hazard. In that case the existing v0.2-r2 rules remain mandatory:

- exact selected World Entry projection;
- exact Character T0 profile projection;
- no latest/nearest/later/full-life fallback;
- closed per-World temporal coverage when the Character declares bindings to that World;
- post-T0 Source material excluded from ordinary Runtime visibility while still participating in the exact generation fingerprint.

## 6. Product invariant

The game must support both at the same time:

```text
Temporal-scoped Source
→ selective T0 projection / future quarantine

Non-temporal Source
→ ordinary rich Source semantics without artificial T0 machinery
```

The program must infer neither mode from genre, asset family, display name, year text, or pretrained knowledge.

**Use the narrow capability only where the authored Source needs it.**
