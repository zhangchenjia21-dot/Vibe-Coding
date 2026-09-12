# planning-artifacts-and-maps.md

## Purpose

This reference defines lightweight machine-readable artifacts and visual planning evidence for `minecraft-planner`.

The objective is auditability and Owner readability, not building a GIS platform.

---

## 1. Minimum artifact set

Recommended default outputs:

```text
settlement-plan.md
planning-objects.json
growth-sequence.json
building-program.json
implementation-packages.json
maps/
```

Optional when useful:

```text
sections/
assumptions.md
source-register.json
```

---

## 2. Task-local planning IDs

Use stable IDs inside one planning packet, for example:

```text
REGION-01
SETTLEMENT-01
ANCHOR-01
ROUTE-01
SPACE-01
DISTRICT-01
BLOCK-01
PARCEL-01
PROGRAM-01
PACKAGE-01
```

Rules:

1. IDs are stable within the plan revision where practical;
2. deleted IDs should not be silently reused inside the same planning lineage;
3. split / merge should record predecessor / successor when material;
4. task-local IDs do not automatically become World Canon IDs;
5. coordinate / geometry provenance remains separate from narrative names.

---

## 3. Planning object minimum fields

Use only fields relevant to the object.

Common fields:

```json
{
  "id": "ANCHOR-01",
  "type": "anchor",
  "scale": "SETTLEMENT",
  "authority": "DESIGN_PROPOSAL",
  "geometry": {},
  "drivers": [],
  "relations": [],
  "historical_stage": "GROWTH-01",
  "why": "...",
  "uncertainty": [],
  "source_refs": []
}
```

Geometry may be:

- point;
- centerline;
- polygon / mask;
- bounds only when true geometry is not yet available;
- reference to an existing authoritative spatial object.

Do not use bbox as if it were exact footprint when source geometry is irregular.

---

## 4. Authority / Evidence Register

For important inputs record:

```text
source
snapshot / commit / hash if available
authority class
scale
freshness
uncertainty
used for which decisions
```

If the project already has a terrain / Atlas evidence contract, reuse it instead of inventing parallel semantics.

---

## 5. Map hierarchy

Visual evidence should adapt to planning scale.

### L0 POLITY_TERRITORY

Recommended maps:

1. **Territorial Structure Map**
   - natural regions;
   - political / cultural regions if approved;
   - major settlement nodes;
   - frontier / sparse / strategic areas;
   - major resource / environmental constraints.

2. **Flow Network Map**
   - major land / river / sea corridors;
   - selected important food / ore / timber / trade / military / pilgrimage flows;
   - gateways and chokepoints.

3. **Settlement Hierarchy / Catchment Map**
   - primary / regional / specialized centers;
   - market / service relationships;
   - qualitative hinterlands.

4. **Historical Growth Map**
   - early cores;
   - later corridors;
   - frontier incorporation;
   - secondary centers;
   - obsolete / residual systems where important.

### L1 REGIONAL_SYSTEM

Recommended maps:

- terrain / resource / constraint;
- regional settlement network;
- flow / processing chain;
- settlement hierarchy / catchment;
- expansion / interface with neighboring regions.

### L2 SETTLEMENT

Recommended maps:

1. Terrain / Constraint / Opportunity;
2. Anchor + Growth + Movement;
3. District / Density / Expansion;
4. current / inherited fabric when Existing Evolution.

### L3 DISTRICT

Recommended maps:

- existing condition;
- route / service / commons;
- block / parcel / frontage;
- infill / subdivision / shared space;
- Builder Package boundaries.

### L4 URBAN_ENSEMBLE

Recommended:

- high-resolution parcel plan;
- frontage / access / service diagram;
- shared courtyard / negative space;
- Builder Package map;
- 1–2 street / terrain sections where elevation matters;
- simplified 3D / massing / player-height view if useful.

---

## 6. Map readability requirements

Every important planning map should provide enough context to interpret it:

- title;
- planning scale / mode;
- north / orientation;
- coordinate reference or world coordinates;
- bounds / extent;
- legend;
- scale indication when practical;
- source snapshot / revision;
- major uncertainty or provisional layers;
- clear visual separation between observed evidence and design proposal.

Avoid decorative medieval-style maps when they obscure planning evidence.

---

## 7. Layer semantics

Recommended visual distinction:

```text
Observed / existing
Derived / interpreted evidence
Approved Canon constraint
Planning proposal
Planning assumption / provisional
Protected / locked
Future / optional
```

Exact colors / styles are not prescribed. The important requirement is legibility and legend consistency.

---

## 8. Growth visualization

Do not default to concentric rings.

Growth may be:

- corridor-based;
- ridge / valley-based;
- fragmented around buildable terraces;
- shoreline-linear;
- multi-core;
- gate / bridgehead-based;
- parcel-by-parcel infill;
- satellite cluster absorption.

Use arrows, stage overlays, separate panels or animation / HTML when clearer.

---

## 9. 3D and massing previews

Planner 3D evidence may show:

- overall block mass;
- street enclosure;
- terrain stepping;
- courtyard proportions;
- skyline relationships;
- relative building hierarchy;
- visual corridor / landmark relation.

Planner 3D should **not** prematurely lock:

- facade details;
- exact roof ornament;
- window families;
- material palette;
- furniture;
- architectural finishing.

Use primitive / low-detail massing when possible so downstream authorship remains visible.

---

## 10. Sections

Sections are useful when morphology depends on:

- slope;
- terrace;
- retaining walls;
- riverbank;
- ridge / valley crossing;
- stacked streets;
- dense frontage on uneven terrain.

A Planner section should emphasize:

- ground profile;
- route levels;
- parcel / building envelopes;
- retaining / drainage relation;
- public / service space;
- relative massing.

Detailed structural section remains Builder work.

---

## 11. Planning packet markdown

`settlement-plan.md` should be readable without opening every JSON file.

Recommended order:

```text
Executive Premise
Authority / Evidence
Scale / Context
Demand
Flows / Externalities
Anchors
Growth Sequence
Terrain Strategy
Movement / Commons
Morphology
Building Program
Architecture Kit Requirements
Builder Packages
Implementation Sequence
Critic / Uncertainty
Map Index
```

Use concise causal traces rather than repeating all raw data.

---

## 12. Version / revision

A plan revision should record:

- plan ID;
- revision;
- upstream source snapshot(s);
- previous revision;
- material changes;
- objects added / retired / split / merged;
- Gate results;
- review status.

Do not silently overwrite accepted plans without lineage.
