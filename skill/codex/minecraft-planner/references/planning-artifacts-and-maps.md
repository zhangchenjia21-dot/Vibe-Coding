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
settlement-capacity.json
sections/
assumptions.md
source-register.json
validation.json
```

At L0 / L1, if several significant settlement nodes are proposed, `settlement-capacity.json` or equivalent fields in `planning-objects.json` are strongly recommended.

---

## 2. Task-local planning IDs

Use stable IDs inside one planning packet, for example:

```text
REGION-01
NODE-01
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
5. coordinate / geometry provenance remains separate from narrative names;
6. at L0/L1, use `NODE-*` or `ANCHOR-*` for search-role objects that are not yet proven long-term settlements; promote to `SETTLEMENT-*` only when the settlement role is actually supported.

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

## 4. Planning Context fields

Planning artifacts should keep historical/evolution logic separate from current observation state.

Recommended fields:

```json
{
  "fabric_observation_state": "EXISTING_FABRIC_UNVERIFIED",
  "evolution_logic": "EXISTING_EVOLUTION",
  "maturity_state": "MATURE",
  "context_note": "..."
}
```

Do not encode “we did not inspect existing fabric” as `GREENFIELD`.

Useful observation values:

```text
NO_EXISTING_FABRIC_EXPECTED
EXISTING_FABRIC_OBSERVED
EXISTING_FABRIC_PARTIAL
EXISTING_FABRIC_UNVERIFIED
```

---

## 5. Settlement scale object

For important L0/L1 settlement nodes, record three distinct geometries / semantics when available:

```json
{
  "location_search_envelope": {},
  "built_fabric_capacity": {
    "area_range_blocks2": [12000, 25000],
    "confidence": "MEDIUM",
    "morphology": "compact_market_town",
    "drivers": []
  },
  "functional_hinterland": {
    "type": "RELATIONAL",
    "description": "..."
  }
}
```

Never use one circle / polygon ambiguously for all three.

If exact shapes are not justified, `built_fabric_capacity` may be an area range + scale-coded symbol rather than a literal polygon.

---

## 6. Authority / Evidence Register

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

## 7. Map hierarchy

Visual evidence should adapt to planning scale.

### L0 POLITY_TERRITORY

Recommended maps:

1. **Territorial Structure Map**
   - natural regions;
   - political / cultural regions if approved;
   - major nodes / anchors;
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

4. **Settlement Scale / Built-Fabric Map**
   - proposed node centers / search logic;
   - approximate built-fabric area range or scale-coded extent;
   - compact / dispersed / fragmented morphology;
   - capacity confidence;
   - clear visual distinction from catchment.

5. **Historical Growth Map**
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
- settlement built-fabric scale / capacity;
- expansion / interface with neighboring regions.

### L2 SETTLEMENT

Recommended maps:

1. Terrain / Constraint / Opportunity;
2. Anchor + Growth + Movement;
3. District / Density / Expansion;
4. approximate current / target built-fabric envelope;
5. current / inherited fabric when Existing Evolution.

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

## 8. Map readability requirements

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

For settlement scale maps, legend must explicitly distinguish:

```text
node / anchor point
location search envelope
built-fabric capacity envelope
functional hinterland / catchment
```

Avoid decorative medieval-style maps when they obscure planning evidence.

---

## 9. Built-fabric visualization

At L0/L1, important settlements should not all be identical dots.

Useful methods:

### Semi-transparent approximate envelope

Use when terrain and role support a rough local shape.

### Area-scaled symbol

Use when location is known broadly but exact shape is not. Scale the symbol to represent working built-fabric area and label the range.

### Fragmented cluster symbol

Use for mountain / island / terrace settlements where one continuous polygon would be misleading.

### Tooltip / side-table range

Use with HTML when map clutter would be high.

Do not draw the full search radius as if it were built town.

Do not draw catchment as urbanized area.

---

## 10. Layer semantics

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

## 11. Growth visualization

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

When built-fabric scale changes through time, show growth of extent only when supported as a planning hypothesis; do not make future maximum area look already built.

---

## 12. 3D and massing previews

Planner 3D evidence may show:

- overall block mass;
- settlement / district extent relative to terrain;
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

## 13. Sections

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

## 14. Planning packet markdown

`settlement-plan.md` should be readable without opening every JSON file.

Recommended order:

```text
Executive Premise
Authority / Evidence
Planning Context
Scale / Scope
Demand
Flows / Externalities
Anchors
Growth Sequence
Terrain Strategy
Movement / Commons
Settlement Hierarchy / Catchment
Settlement Capacity / Built-Fabric Scale
Morphology
Building Program
Architecture Kit Requirements
Recursive Planning Packages / Builder Packages
Implementation Sequence
Critic / Uncertainty
Map Index
```

Use concise causal traces rather than repeating all raw data.

---

## 15. Recursive package artifact

For L0–L3, `implementation-packages.json` should normally contain Planner→Planner packages with fields such as:

```text
upstream_fixed
downstream_to_resolve
downstream_adaptable
revision_triggers
capacity_hypothesis
```

Do not reuse Builder-specific `builder_adaptable` as the main child-planning schema.

For L4 / Builder-ready planning, switch to the Planner→Builder contract.

---

## 16. Version / revision

A plan revision should record:

- plan ID;
- revision;
- upstream source snapshot(s);
- previous revision;
- material changes;
- objects added / retired / split / merged;
- capacity changes where material;
- Gate results;
- review status.

Do not silently overwrite accepted plans without lineage.