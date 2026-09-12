# planner-builder-handoff.md

## Purpose

This reference defines the contract between `minecraft-planner` and `minecraft-builder`.

The goal is to preserve settlement-scale causality without turning Planner into an architect or allowing Builder to silently erase upstream relationships.

---

## 1. Builder Design Package

Each downstream package should contain:

```text
Package ID
Scale / Scope
WHY / planning role
Upstream anchors / flows
Spatial envelope / parcel relation
Required adjacency
Required access
Shared-space / service relation
Program requirements
Historical / maturity note
Architecture Kit Requirements
Planner Fixed
Builder Adaptable
Dependencies
Known uncertainty
Implementation note
```

A Package may represent:

- one important building;
- a compound;
- a small building cluster;
- a bridge / gate / market structure;
- a terrain / circulation preparation scope;
- part of a larger Urban Ensemble.

Do not assume Package = one building.

---

## 2. PLANNER_FIXED

Use only for relationships whose violation would break the accepted settlement logic.

Typical examples:

- role in settlement hierarchy;
- required frontage orientation;
- must-adjoin / must-share-access relation;
- parcel / ensemble boundary;
- protected route;
- freight / service access side;
- shared courtyard relation;
- negative space that must remain open;
- approximate scale relationship to neighboring objects;
- location relative to an Anchor;
- public vs private threshold logic;
- growth age / inherited fabric relation;
- required use or program category.

Do not put details here simply because Planner has an opinion.

---

## 3. BUILDER_ADAPTABLE

Builder should retain authorship over:

- exact footprint inside approved envelope;
- room arrangement;
- exact section / floor heights;
- exact number of floors unless strategic relation requires a range;
- structure / tectonics;
- roof geometry;
- facade composition;
- opening geometry;
- detailed terrain interface;
- block palette;
- furniture / finishing;
- Minecraft Translation.

Builder may also refine minor route geometry when it preserves the upstream route role and connection.

---

## 4. UPSTREAM_PLANNING_ISSUE

Builder should return this status when accepted fixed constraints cannot all be satisfied without breaking planning logic.

Examples:

- parcel too small for required program after real terrain read;
- required service access conflicts with protected route;
- terrain makes both frontage and rear access impossible;
- two mandatory adjacencies contradict;
- planner-fixed courtyard has no drainage / usable ground;
- settlement package assumes a crossing that current world data disproves.

Builder should report:

```text
conflicting fixed constraints
observed evidence
why bounded adaptation is insufficient
minimum upstream decision required
```

Do not silently move the building, delete a lane or remove a shared yard.

---

## 5. Planner revision after upstream issue

Planner should:

1. preserve unaffected accepted relationships;
2. update only the smallest necessary upstream object(s);
3. explain the changed causal logic;
4. regenerate impacted packages;
5. increment planning artifact revision;
6. rerun Morphology / Handoff Gate as appropriate.

Do not rebuild the whole settlement plan for a local conflict unless the conflict reveals a systemic error.

---

## 6. Cross-package dependencies

Packages should record shared dependencies such as:

- terrain preparation;
- retaining wall system;
- shared lane;
- courtyard drainage;
- party wall / shared wall line;
- bridge / gate completion;
- water channel;
- shared Architecture Kit version;
- neighboring anchor completion.

This allows a large design unit to be built in bounded phases.

---

## 7. Design Unit ≠ Write Batch

A single accepted design unit can be larger than one safe world-write batch.

Example:

```text
Urban Ensemble design
→ terrain / drainage package
→ primary route package
→ anchor building package
→ secondary building packages
→ shared courtyard / service package
→ finishing / district review
```

The Planner may recommend dependency order, but Builder / implementation tooling owns exact write batching and repair strategy.

---

## 8. Growth Sequence ≠ Implementation Sequence

A Builder Package may represent a building historically older than another package but be constructed later in Minecraft because dependencies or safety require it.

Every handoff should keep both concepts separately labeled:

- `historical_growth_stage`
- `implementation_dependency`

Never infer historical age from Minecraft task order.

---

## 9. Handoff Gate checklist

Before declaring `HANDOFF_READY`, verify:

- every package has a clear WHY;
- boundaries are understandable;
- fixed constraints are few but meaningful;
- Builder has genuine architectural freedom;
- shared routes / courtyards / interfaces have explicit owners;
- no package depends on an undefined future object without a placeholder dependency;
- planning assumptions are visible;
- Architecture Kit requirements do not prescribe finished geometry;
- world-write authorization is not implied.

---

## 10. Recommended package JSON shape

```json
{
  "id": "PACKAGE-03",
  "scale": "URBAN_ENSEMBLE",
  "role": "mixed_frontage_cluster",
  "why": "gateway trade pressure + scarce frontage",
  "historical_growth_stage": "GROWTH-03",
  "planner_fixed": {
    "frontage_on": "ROUTE-01",
    "rear_service_access": "ROUTE-04",
    "shared_courtyard": "SPACE-02",
    "protected_open_space": ["SPACE-03"]
  },
  "builder_adaptable": [
    "exact_footprint",
    "plan_section",
    "structure",
    "roof",
    "facade",
    "palette"
  ],
  "dependencies": ["PACKAGE-01"],
  "uncertainty": []
}
```

This is an example contract, not a mandatory universal schema.
