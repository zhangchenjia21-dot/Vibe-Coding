# planning-artifacts-and-maps.md

## Purpose

This reference defines lightweight machine-readable artifacts and visual planning evidence for `minecraft-planner`.

The objective is auditability and Owner readability, not building a GIS / economy / politics simulator.

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
surface-character.json
actors.json
metabolism.json
builder-design-packages.json
interface-baselines.json
sections/
assumptions.md
source-register.json
validation.json
```

Do **not** create every optional file by default. Add one only when the mechanism materially changes the plan.

At L0 / L1, if several significant settlement nodes are proposed, `settlement-capacity.json` or equivalent fields are strongly recommended.

If surface / substrate / land-cover materially affects planning, include equivalent evidence fields and an Owner-readable map layer.

If Actor / rights / bounded knowledge materially affects planning, represent it either inline or in `actors.json`.

At Builder-ready scale, `builder-design-packages.json` should follow the shared Planner–Builder contract and include local interface data when design freeze depends on it.

---

## 2. Task-local planning IDs

Useful IDs:

```text
REGION-01
ACTOR-01
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
INTERFACE-01
SERVICE-01
```

Rules:

1. IDs stable within plan revision where practical;
2. deleted IDs not silently reused;
3. split / merge records predecessor / successor when material;
4. task-local IDs do not become World Canon automatically;
5. geometry provenance remains separate from narrative names;
6. at L0/L1 use `NODE-* / ANCHOR-*` until long-term settlement role is supported;
7. do not create Actor / Interface / Service IDs for trivial relationships merely to satisfy schema.

---

## 3. Planning object minimum fields

Common example:

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

Optional v0.4+ fields when consequential:

```text
actor_refs
epistemic_state
rights / access_conditions
constraint_transformation
mitigation_options
residual_constraint
effective_access
stock_buffer
seasonal_pattern
resilience_role
site_value_drivers
demographic_driver
feedback_relations
boundary_semantic
interface_baselines
external_service_interfaces
resolve_before
```

Use only fields that explain spatial consequences.

Geometry may be point, centerline, polygon / mask, bounds when true geometry unavailable, or reference to authoritative object.

Do not use bbox as exact footprint when source geometry is irregular.

---

## 4. Planning Context fields

Recommended:

```json
{
  "fabric_observation_state": "EXISTING_FABRIC_UNVERIFIED",
  "evolution_logic": "EXISTING_EVOLUTION",
  "maturity_state": "MATURE",
  "context_note": "..."
}
```

Do not encode “not inspected” as `GREENFIELD`.

---

## 5. Actor / rights object

When Actor relations materially change planning, a lightweight object may contain:

```json
{
  "id": "ACTOR-01",
  "role": "merchant_group",
  "interests": ["reliable market access"],
  "rights": ["market use"],
  "constraints": ["no compulsory land-taking authority"],
  "cooperation_required": ["local landholders"],
  "knowledge": ["LOCALLY_KNOWN crossing"],
  "spatial_effects": ["prefers frontage near crossing"]
}
```

Do not infer detailed political institutions unless evidence / Canon supports them.

---

## 6. Settlement scale object

For important L0/L1 nodes, keep separate:

```json
{
  "location_search_envelope": {},
  "built_fabric_capacity": {
    "area_range_blocks2": [12000, 25000],
    "confidence": "MEDIUM",
    "morphology": "compact_market_town",
    "drivers": [],
    "adaptation_assumptions": [],
    "external_supply_dependency": [],
    "effective_access_summary": "..."
  },
  "functional_hinterland": {
    "type": "RELATIONAL",
    "description": "..."
  }
}
```

Never use one circle / polygon ambiguously for search, built fabric and catchment.

---

## 7. Constraint Transformation object

When an environmental / access condition could materially block or redirect planning, record:

```json
{
  "constraint": "weak surface-water evidence",
  "affected_activity": "permanent residence",
  "mitigation_options": ["well", "cistern", "carried supply"],
  "capability_required": "mature premodern ordinary works",
  "relative_burden": "LOW_TO_MODERATE",
  "residual_constraint": "large agricultural expansion remains weak",
  "confidence": "LOW"
}
```

Do not require exact engineering dimensions.

---

## 8. Effective Access object

When relevant:

```json
{
  "physical_access": "POSSIBLE / UNVERIFIED",
  "tenure_access": "UNRESOLVED",
  "political_permission": "UNRESOLVED",
  "security": "NORMAL / CONDITIONAL / UNRESOLVED",
  "seasonality": "YEAR_ROUND / SEASONAL / UNRESOLVED",
  "mode": ["pedestrian", "pack_animal"],
  "planning_result": "conditional regional corridor"
}
```

Do not call a route “usable” merely because its geometry is continuous.

---

## 9. Metabolism object

When stock / rhythm matters, use concise fields such as:

```json
{
  "flow": "grain inflow",
  "cadence": "seasonal",
  "consumption": "daily",
  "buffer_role": "shared + household storage",
  "peak": "harvest / market period",
  "failure_consequence": "temporary food stress",
  "resilience_response": "distributed stores"
}
```

No numerical inventory simulator is required.

---

## 10. Authority / Evidence Register

For important inputs record:

```text
source
snapshot / commit / hash
authority class
scale
freshness
uncertainty
used for which decisions
```

Keep direct observations separate from derived classes.

If a fact is relevant to historical sequence, optionally record epistemic state separately from evidence authority.

---

## 11. Map hierarchy

Visual evidence adapts to planning scale.

### L0 POLITY_TERRITORY

Recommended maps:

1. Territorial Structure
2. Flow / Effective Access Network
3. Settlement Hierarchy / Catchment
4. Settlement Scale / Built-Fabric
5. Historical Growth / Major Feedback
6. Surface / Land-Cover — when materially important
7. Rights / political-access layer — only when it materially changes network structure

### L1 REGIONAL_SYSTEM

Recommended:

- terrain / resource / raw constraints;
- surface / substrate / land-cover when consequential;
- regional settlement network;
- effective access / conditional corridors;
- flow / stock / processing chain when relevant;
- settlement hierarchy / catchment;
- built-fabric scale / capacity;
- adaptation / fallback relation where it materially changes interpretation.

### L2 SETTLEMENT

Recommended:

1. Terrain / Raw Constraint / Opportunity;
2. Surface / Ground Character;
3. Existing / inherited fabric;
4. Anchor + Growth + Movement + major infrastructure;
5. District / Density / Expansion;
6. approximate built-fabric envelope;
7. commons / rights / effective-access layer when consequential.

### L3 DISTRICT

Recommended:

- existing condition;
- route / service / commons;
- block / parcel / frontage;
- ground constraints / adaptation relation;
- rights / easement / shared access when consequential;
- site-value / subdivision pressure where it helps explain morphology;
- L4 package boundaries.

### L4 URBAN_ENSEMBLE

Recommended:

- high-resolution parcel plan;
- frontage / access / service diagram;
- shared courtyard / negative space;
- local terrain / drainage / adaptation context;
- Builder Package map;
- Builder-facing local interface slice / protected clear envelope when design freeze depends on it;
- sections / simple massing when useful.

---

## 12. Map readability requirements

Every important map should provide enough context:

- title;
- planning scale / mode;
- north / orientation;
- world coordinates / bounds;
- legend;
- scale indication when practical;
- source snapshot / revision;
- uncertainty / provisional layers;
- clear separation between Observed / Derived / Canon / Proposal.

For settlement scale maps distinguish node / anchor, search envelope, built-fabric capacity and catchment.

For effective-access maps distinguish confirmed / conditional / unresolved access rather than drawing all lines as roads.

For Builder-facing interface maps, distinguish planning centerline, corridor envelope, protected clear envelope, threshold search segment and adjustable edge when those semantics differ.

Avoid decorative maps that obscure evidence.

---

## 13. Built-fabric visualization

At L0/L1 important settlements should not all be identical dots.

Useful methods:

- approximate envelope;
- area-scaled symbol;
- fragmented cluster symbol;
- side-table / tooltip range.

Do not draw search radius as town or catchment as urbanized area.

---

## 14. Surface / adaptation visualization

When ground character materially changes planning, Owner should see what kind of ground candidates occupy.

When mitigation materially changes the conclusion, maps / side notes may distinguish:

```text
raw constraint
candidate adaptation / mitigation area
residual constraint / protected ground
```

Do not make a conceptual mitigation layer look like built infrastructure.

---

## 15. Growth visualization

Do not default to concentric rings.

Growth may be corridor / ridge / valley / terrace / shoreline / multi-core / bridgehead / parcel-infill / satellite driven.

Where important, show feedback such as:

```text
new crossing → traffic concentration → frontage growth
```

Do not make future maximum area look already built.

---

## 16. 3D and sections

Planner 3D may show settlement / district extent, terrain stepping, street enclosure, courtyard proportions, skyline hierarchy and local adaptation context.

Do not prematurely lock facade / roof ornament / windows / palette / furniture / exact engineering.

Sections are useful for slope, terrace, retaining, riverbank, crossing, stacked street, shallow ground relation or dense frontage.

At Builder handoff, a local section baseline may be appropriate when threshold / lane / shared-ground continuity cannot be understood from plan alone. It should control relationship and clearance, not design the final structure.

Detailed structural section remains Builder work.

---

## 17. Planning packet markdown

Recommended order:

```text
Executive Premise
Authority / Evidence
Planning Context
Actors / Rights / Bounded Knowledge (when consequential)
Demand
Flows / Stocks / Rhythms / Externalities
Terrain + Surface
Constraint Transformation / Effective Access (when consequential)
Anchors
Growth + Feedback
Movement / Commons
Settlement Hierarchy / Catchment
Settlement Capacity / Built-Fabric Scale
Morphology / Site Value / Parcel / Density
Building Program
Architecture Kit Requirements
Recursive Planning Packages / Builder Packages
Implementation Sequence
Critic / Uncertainty
Map Index
```

Use concise causal traces rather than copying all raw data.

---

## 18. Recursive package artifact

For L0–L3, `implementation-packages.json` should normally include:

```text
upstream_fixed
downstream_to_resolve
downstream_adaptable
revision_triggers
capacity_hypothesis
```

When consequential, also pass:

```text
actor_rights_assumptions
epistemic_unknowns
mitigation_assumptions
effective_access_conditions
metabolic_dependencies
resilience_requirements
feedback_dependencies
```

Do not reuse Builder-specific fields as child-planning schema.

---

## 19. Builder Design Package artifact｜v0.5

When planning recursion reaches Builder-ready scope, prefer `builder-design-packages.json` or equivalent machine-readable objects.

Minimum fields should follow `../shared/minecraft-planner-builder-contract.md` and normally include:

```text
package_id / revision
recipient = minecraft-builder
builder_handoff_readiness
WHY / role
design_context
planning_causal_context
spatial_envelope
boundary_semantic
program_requirements
PLANNER_FIXED
BUILDER_ADAPTABLE
dependencies
known_uncertainty + resolve_before
world_write_authorization = false
```

### Interface Baselines

If a planning-fixed cross-scope physical interface affects the next Builder stage, embed it or place it in `interface-baselines.json` and reference it directly.

Recommended artifact fields:

```json
{
  "interface_id": "IF-LANE-04-P01",
  "source_object": "LANE-04",
  "source_revision": "P04-r1",
  "role": "common pedestrian frontage",
  "local_geometry": {"type": "corridor_envelope", "geometry": {}},
  "nominal_width": 2,
  "minimum_clear_requirement": 2,
  "height_or_section_baseline": {},
  "adjustment_envelope": {},
  "rights_access_semantic": "COMMON_EASEMENT",
  "coordination_owner": "BDP-00",
  "resolve_before": "BEFORE_DESIGN_FREEZE"
}
```

Do not store only an object ID if Builder cannot resolve the local geometry without opening the full parent plan.

### Boundary semantic

If voxel legality depends on a continuous edge, record one of:

```text
CELL_CENTER_MASK
FULL_VOXEL_INSIDE
CONTINUOUS_BOUNDARY_WITH_TOLERANCE
NEGOTIABLE_EDGE
REFERENCE_ONLY
```

Where relevant also persist edge tolerance / adjustment strip.

### External service interfaces

If water / drainage / waste / goods / fuel / shared retaining crosses package boundaries, record location or direct ref, status, responsibility and `resolve_before`.

The artifact must distinguish “Builder reserves an interface” from “Builder owns the public system”.

---

## 20. Version / revision

Record:

- plan ID / revision;
- upstream snapshots;
- previous revision;
- material changes;
- objects added / retired / split / merged;
- capacity changes;
- relevant evidence / access / Actor / mitigation changes;
- Builder package revision when emitted;
- Interface Baseline revisions when material;
- Gate results;
- review status.

If a fixed interface changes materially, mark dependent Builder packages / designs stale for fidelity review rather than silently overwriting their baseline.

Do not silently overwrite accepted plans without lineage.
