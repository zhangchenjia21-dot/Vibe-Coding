# planner-builder-handoff.md

## Purpose

This reference defines the contract between `minecraft-planner` and `minecraft-builder` **after planning recursion has reached a Builder-ready scale**.

It does not define L0→L1→L2→L3→L4 planning recursion. For that use `recursive-planning-handoff.md`.

The goal is to preserve settlement-scale causality, rights, shared access and planning-level adaptation requirements without turning Planner into an architect / engineer or allowing Builder to erase upstream relationships.

---

## 1. When this contract applies

Use Planner→Builder handoff only when scope is concrete enough that Builder can design without another full planning scale in between:

- one important building;
- a compound;
- a small building cluster;
- a bridge / gate / market structure;
- a terrain / circulation preparation scope;
- part of an accepted Urban Ensemble.

Typical entry point is L4 `URBAN_ENSEMBLE`, though isolated low-density scopes may become Builder-ready from L2/L3 when relationships are already resolved.

Do **not** send national / regional unresolved nodes directly to Builder.

---

## 2. Builder Design Package

Each downstream package should contain as relevant:

```text
Package ID
Scale / Scope
WHY / planning role
Upstream anchors / flows
Spatial envelope / parcel relation
Required adjacency
Required access / access-right relation
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

A Package does not have to equal one building.

---

## 3. PLANNER_FIXED

Use only for relationships whose violation would break accepted settlement logic.

Typical examples:

- role in settlement hierarchy;
- required frontage orientation;
- must-adjoin / must-share-access relation;
- parcel / ensemble boundary;
- protected route / easement;
- freight / service access side;
- shared courtyard relation;
- negative space that must remain open;
- approximate scale relationship to neighbors;
- location relative to Anchor;
- public / common / private threshold logic;
- growth age / inherited fabric relation;
- required use / program category;
- planning-level mitigation requirement when accepted, such as “provide shared water interface”, “avoid known shallow void”, “retain stepped access”, or “preserve fallback lane”.

Do not freeze exact geometry merely because Planner has an opinion.

---

## 4. BUILDER_ADAPTABLE

Builder retains authorship over:

- exact footprint inside approved envelope;
- room arrangement;
- exact section / floor heights;
- exact floor count unless strategic relation requires range;
- structure / tectonics;
- roof geometry;
- facade composition;
- opening geometry;
- detailed terrain interface;
- exact well / cistern / retaining / bridge / drainage geometry when required at planning level;
- block palette;
- furniture / finishing;
- Minecraft Translation.

Builder may refine minor route / access geometry while preserving upstream role and rights.

---

## 5. Adaptation / mitigation boundary

Planner may hand down an accepted **relationship-level** requirement, but Builder chooses exact realization.

Example:

```text
PLANNER_FIXED:
- shared clean-water point must remain accessible from SPACE-02
- no dirty-service flow crosses this access

BUILDER_ADAPTABLE:
- exact well / cistern form
- depth / section after site evidence
- materials / structure
```

If several equivalent mitigation options remain unresolved at planning scale, do **not** force one into Builder package unless the Builder task itself is meant to compare them.

---

## 6. Capacity and Builder handoff

Builder receives only capacity relevant to current package.

Lower-scale planning should decompose settlement capacity into district / ensemble envelopes, building program, shared routes / spaces and phased packages.

Builder may challenge a local envelope when real site / terrain / ground evidence makes required program infeasible.

---

## 7. Rights / effective access in Builder handoff

When a planning relationship depends on shared / public / service access, make it explicit.

Examples:

- public passage must remain open;
- rear service easement shared by three parcels;
- loading access is pack-animal compatible but not intended for full freight convoy;
- commons edge cannot be privatized by one package.

Builder must not optimize geometry by silently closing a planning-fixed access relation.

---

## 8. UPSTREAM_PLANNING_ISSUE

Builder should return this when accepted fixed constraints cannot all be satisfied without breaking planning logic.

Examples:

- parcel too small after real terrain / ground read;
- service access conflicts with protected route / right;
- terrain makes both required frontage and rear access impossible;
- two mandatory adjacencies contradict;
- fixed courtyard has no usable ground;
- accepted mitigation proves disproportionate / physically infeasible at detailed design scale;
- actual footprint would materially exceed ensemble capacity.

Builder should report:

```text
conflicting fixed constraints
observed evidence
bounded adaptations considered
why Builder-level adaptation is insufficient
minimum upstream decision required
```

Do not silently move building, delete lane, remove shared yard or cancel shared access.

---

## 9. Planner revision after upstream issue

Planner should:

1. preserve unaffected relationships;
2. update smallest necessary upstream object(s);
3. explain changed causal logic;
4. regenerate impacted packages;
5. increment planning revision;
6. rerun relevant Gates.

Do not rebuild whole settlement for a local conflict unless systemic error is revealed.

---

## 10. Cross-package dependencies

Packages may share:

- terrain preparation;
- retaining / access system;
- shared lane / easement;
- courtyard drainage;
- party wall line;
- bridge / gate;
- water / storage interface;
- resilience fallback route / service point;
- Architecture Kit version;
- neighboring Anchor completion.

---

## 11. Design Unit ≠ Write Batch

One accepted design unit can be larger than one safe world-write batch.

Planner may recommend dependency order, but Builder / implementation tooling owns exact write batching and repair strategy.

---

## 12. Growth Sequence ≠ Implementation Sequence

Keep separately labeled:

- `historical_growth_stage`
- `implementation_dependency`

Never infer historical age from Minecraft task order.

---

## 13. Handoff Gate checklist

Before Builder-ready:

- planning recursion has reached sufficient scale;
- every package has clear WHY;
- boundary / envelope understandable;
- fixed constraints few but meaningful;
- Actor / rights / access relations are explicit where consequential;
- adaptation requirement is relationship-level, not exact engineering;
- Builder has genuine architectural / engineering freedom;
- shared routes / courts / interfaces have owners / access semantics;
- planning assumptions visible;
- Kit requirements do not prescribe finished geometry;
- capacity decomposed enough for bounded scope;
- world-write authorization not implied.

---

## 14. Recommended package JSON shape

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
    "protected_open_space": ["SPACE-03"],
    "shared_access_right": "PUBLIC / COMMON"
  },
  "builder_adaptable": [
    "exact_footprint",
    "plan_section",
    "structure",
    "local_mitigation_geometry",
    "roof",
    "facade",
    "palette"
  ],
  "dependencies": ["PACKAGE-01"],
  "uncertainty": []
}
```

Example only; not mandatory universal schema.
