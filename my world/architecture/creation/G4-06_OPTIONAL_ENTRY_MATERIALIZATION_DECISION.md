# G4-06｜Optional Entry Materialization Decision

Status: CURRENT / FROZEN FOR G4-06
Date: 2026-08-30

## Decision

`GameCreationComposition.entry` remains optional in first-generation Final Create.

> **No Entry means no implicit temporal choice.**

Final Create must never invent a default/latest/nearest Entry or Character T0 profile merely because a World is historical or temporally scoped.

## Materialization rules

### A. Exact Entry selected

For the exact pinned World generation:

```text
initial World semantics
= top-level always-safe semantic_sections
+ exact selected Entry semantic_sections
```

For each exact pinned Character generation:

```text
exact profile binding exists for selected World/Entry
→ top-level always-safe sections + exact selected T0 profile sections

Character has zero profile coverage for selected World
→ top-level always-safe sections only / no_world_coverage

Character declares coverage for selected World but lacks exact Entry binding
→ hard temporal incompatibility / Final Create must fail before durable Game publication
```

Non-temporal/scenario Entries use the same exact Entry projection seam without acquiring historical meaning.

### B. No Entry selected

For the exact pinned World generation:

```text
initial World semantics
= top-level semantic_sections only
```

For every exact pinned Character generation:

```text
initial Character semantics
= top-level semantic_sections only
```

No Entry-scoped section or T0 profile is selected.

This is an explicit `none` composition choice, not a request for the system to infer a date/scenario.

## Invariants

- Entry remains `0..1`; G4-06 must not make it universally required.
- No `historical=true/false`, temporal mode switch, family restriction or hidden default Entry.
- No latest/nearest/later/full-life profile fallback.
- Exact Source generation pins are resolved from the frozen Composition; Final Create must not drift to a newer current generation.
- Source Package Total Content remains distinct from selected initial Game materialization.
- Exact Source generations stay immutable; only selected starting semantics are copied into Game-local canonical state.
- Source asset IDs are provenance, not Game-local World/Character entity IDs.
- Guaranteed NPC means the Character becomes part of the Game-local canonical cast; it does not mean opening-scene presence, current location, player knowledge or relationship.
- G4-06 does not call Provider or generate an AI Opening.

## Product consequence

Choosing no Entry can intentionally produce a less time-specific starting definition for a temporally scoped asset. That is acceptable for G4-06 correctness because it preserves the already-frozen optional Entry contract and avoids future-information leakage.

Whether this path is sufficiently rich/fun is a product-value question for G4-07 First Playable UAT, not a reason for Final Create to invent hidden temporal defaults.
