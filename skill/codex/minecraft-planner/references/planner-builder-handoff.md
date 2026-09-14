# planner-builder-handoff.md｜v0.5

## Purpose

This reference defines how `minecraft-planner` emits Builder-ready packages **after planning recursion has reached a scale that no longer needs another full Planner layer**.

The shared interface source of truth is:

`../../shared/minecraft-planner-builder-contract.md`

This file specializes that shared contract from the **Planner / emitter** side.

It does not define L0→L1→L2→L3→L4 recursion. For that use `recursive-planning-handoff.md`.

The goal is to preserve planning causality, rights, effective access, shared-space logic, interface geometry and uncertainty without turning Planner into an architect / engineer or forcing Builder to reread the entire upstream plan.

---

## 1. When this contract applies

Use Planner→Builder handoff only when scope is concrete enough that Builder can design without another full planning scale in between:

- one important building;
- a compound;
- a small building cluster;
- a bridge / gate / market structure;
- a terrain / circulation preparation scope;
- part of an accepted Urban Ensemble.

Typical entry point is L4 `URBAN_ENSEMBLE`, though isolated low-density scopes may become Builder-ready directly from L2/L3 if their planning relationships are already sufficiently resolved.

Do **not** send national / regional unresolved nodes directly to Builder.

---

## 2. Builder Design Package｜不是一份建筑说明书

Each package should contain as relevant:

```text
Package ID
Package revision
Recipient = minecraft-builder
Scale / Scope
Builder handoff readiness

WHY / planning role
Design Context
Planning Causal Context
Upstream anchors / flows
Spatial envelope / parcel relation
Boundary semantic
Required adjacency
Required access / access-right relation
Shared-space / service relation
Program requirements
Historical / maturity note
Architecture Kit Requirements

PLANNER_FIXED
BUILDER_ADAPTABLE
INTERFACE_BASELINES
EXTERNAL_SERVICE_INTERFACES

dependencies
known uncertainty + resolve_before
upstream issue protocol
source refs
world_write_authorization = false
```

A Package does not have to equal one building.

Use progressive disclosure: give Builder the current causal context and direct interface dependencies; keep wider L0–L3 material as references instead of forcing a full reread.

---

## 3. Builder handoff readiness

### `CONCEPT_DESIGN_READY`

Builder can perform bounded Architecture Design, but one or more planning-fixed external interfaces still block final design freeze.

Use this only when the unresolved interface does not prevent Builder from exploring architecture without breaking planning semantics.

### `DESIGN_FREEZE_READY`

Builder can freeze Architecture Design because every planning-fixed cross-scope interface needed for that freeze is locally resolvable.

Uncertainties may remain when their `resolve_before` is later, such as world write or permanent occupation.

### `INCOMPLETE_HANDOFF`

The package lacks information needed for the requested Builder stage. Do not label such a package Builder-ready merely because the WHY and parcel polygon are present.

---

## 4. `PLANNER_FIXED`

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
- planning-level mitigation requirement, such as “provide shared water interface”, “avoid known shallow void”, “retain stepped access”, or “preserve fallback lane”.

Do not freeze exact architecture merely because Planner has an opinion.

> **If a PLANNER_FIXED relation crosses the Builder package boundary, it must be resolvable through an Interface Baseline or direct immutable reference.**

---

## 5. `BUILDER_ADAPTABLE`

Builder retains authorship over:

- exact footprint inside approved envelope;
- room arrangement;
- exact Plan + Section;
- exact floor heights / count unless planning relation truly constrains a range;
- structure / tectonics;
- roof geometry;
- facade composition;
- opening geometry;
- detailed terrain interface;
- exact well / cistern / retaining / bridge / drainage geometry;
- local threshold design;
- block palette;
- furniture / finishing;
- Minecraft Translation.

Builder may refine a local interface only inside the supplied adjustment envelope and only while preserving upstream role / rights.

---

## 6. Interface Baseline｜v0.5 的核心补强

MP-I01 showed that a route ID plus nominal width is not sufficient when Builder must prove a threshold / landing does not obstruct public circulation.

For every cross-package physical relation that is planning-fixed and relevant to design freeze, provide a local `Interface Baseline`.

Recommended fields:

```text
interface_id
role
source_object
source_revision / immutable_ref
status
local_geometry
coordinate_semantic
height_or_section_baseline
nominal_width
minimum_clear_requirement
adjustment_envelope
rights_access_semantic
flow_or_service_semantic
coordination_owner
resolve_before
uncertainty
```

### 6.1 Local slice, not complete parent plan

Only provide the local geometry / section needed by the current Builder scope, or a direct immutable object ref + revision.

Builder should not need to open the complete parent district plan just to find the lane beside one doorway.

### 6.2 Geometry semantic

State whether the object is:

- centerline;
- corridor envelope;
- protected clear envelope;
- edge / boundary;
- point interface;
- threshold search band;
- section control line.

### 6.3 Nominal width vs required clear width

If design depends on real clearance, distinguish:

```text
nominal_width
minimum_clear_requirement
```

Do not ask Builder to infer that “nominal 2 blocks” necessarily means exactly 2 clear blocks after steps, doors and edge treatment.

### 6.4 Height / section

If the interface is topographically sensitive, provide enough local control to establish threshold continuity:

- relevant existing / planning ground elevations;
- grade / landing relation;
- section control points;
- allowed local adjustment.

Planner does not need to design the retaining wall, stair or pavement construction.

---

## 7. Minecraft Boundary Semantic｜不要让 Builder 猜斜边怎么落格

When a continuous polygon or diagonal boundary becomes a voxel design envelope, state its discretization semantic.

Allowed examples:

- `CELL_CENTER_MASK`
- `FULL_VOXEL_INSIDE`
- `CONTINUOUS_BOUNDARY_WITH_TOLERANCE`
- `NEGOTIABLE_EDGE`
- `REFERENCE_ONLY`

When needed also provide:

```text
edge_tolerance
adjustment_strip
vertical_boundary_semantic
```

This applies especially when parcel capacity accounting already uses an exact column mask. A continuous polygon alone is not enough if world-write legality depends on individual block columns.

---

## 8. External Service Interfaces｜跨出地块以后是谁的工作

When a required service leaves the Builder package, identify its external relation and responsibility.

Examples:

- rainwater outfall;
- clean-water delivery / storage point;
- waste pickup;
- fuel / goods delivery;
- shared drainage;
- loading interface;
- retaining / slope system shared with public ground.

Recommended fields:

```text
service_id
service_type
interface_location_or_ref
status
builder_responsibility
public_or_neighbor_responsibility
coordination_owner
allowed_local_adjustment
resolve_before
uncertainty
```

Builder responsibilities may be:

- `RESERVE_INTERFACE_ONLY`
- `DESIGN_LOCAL_CONNECTION`
- `CO-DESIGN_INTERFACE`
- `FULL_SCOPE_OWNER`

Do not make Builder invent a public drain, water route or waste terminal because the parcel-level architecture needs one.

---

## 9. Adaptation / mitigation boundary

Planner may hand down an accepted **relationship-level** adaptation requirement, while Builder chooses exact realization.

Example:

```text
PLANNER_FIXED:
- shared clean-water point must remain accessible from SPACE-02
- dirty-service flow may not block that access

BUILDER_ADAPTABLE:
- exact well / cistern form
- depth / section after site evidence
- structure / materials
```

If multiple equivalent mitigation options remain unresolved at planning scale, do not force one into Builder package unless the Builder task itself is meant to compare them.

---

## 10. Capacity and Builder handoff

Builder receives only capacity relevant to the current package.

Lower-scale planning should have decomposed settlement capacity into:

- parcel / ensemble envelopes;
- program;
- shared routes / spaces;
- phased packages.

Builder may challenge a local envelope when real Site / terrain / ground evidence makes required program infeasible.

Capacity ranges that are merely exploratory should say so; do not turn a planning test range into a mandatory building footprint.

---

## 11. Rights / effective access

When a relationship depends on shared / public / service access, make it explicit.

Examples:

- public passage must remain open;
- rear service easement shared by multiple parcels;
- loading route is pack-animal compatible but not intended for full freight convoy;
- commons edge cannot be privatized by one package.

Every such cross-package fixed relation should point to an Interface Baseline with rights / access semantics.

Builder must not optimize geometry by silently closing it.

---

## 12. Uncertainty and `resolve_before`

Do not collapse every unknown into a universal blocker.

For each consequential item, say when it must be resolved:

- `BEFORE_CONCEPT_DESIGN`
- `BEFORE_DESIGN_FREEZE`
- `BEFORE_WORLD_WRITE`
- `BEFORE_PERMANENT_OCCUPATION`
- `BEFORE_OPERATION`

This allows concept design to proceed while preventing premature construction or occupation claims.

---

## 13. `UPSTREAM_PLANNING_ISSUE` vs `INCOMPLETE_HANDOFF`

### `INCOMPLETE_HANDOFF`

Use when Planner has not supplied the information Builder needs to preserve a planning-fixed relation at the requested stage.

Examples:

- public lane must remain open but no local lane geometry / immutable ref exists;
- boundary is fixed but voxel discretization semantic is absent;
- parcel drainage must connect to public system but no external service owner / interface is defined.

### `UPSTREAM_PLANNING_ISSUE`

Use when the information exists but accepted fixed constraints cannot all be satisfied after bounded Builder adaptation.

Builder should report:

```text
package / interface IDs
missing or conflicting relation
observed evidence
bounded adaptations considered
why Builder-level adaptation is insufficient
minimum upstream information / decision required
```

Do not silently move building, delete lane, remove shared yard, enlarge parcel, increase household count or invent public infrastructure.

---

## 14. Planner revision after issue

Planner should:

1. preserve unaffected relationships;
2. revise the smallest necessary planning object / interface;
3. explain changed causal logic or newly supplied baseline;
4. increment relevant package / interface revision;
5. mark affected downstream packages stale when material;
6. rerun Handoff Gate.

Do not rebuild the whole settlement for a local interface correction unless a systemic error is revealed.

---

## 15. Cross-package dependencies

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

If a dependency physically touches the Builder scope, provide a local Interface Baseline or direct immutable ref.

---

## 16. Design Unit ≠ Write Batch

One accepted design unit can be larger than one safe world-write batch.

Planner may recommend dependency order, but Builder / implementation tooling owns exact write batching and repair strategy.

---

## 17. Growth Sequence ≠ Implementation Sequence

Keep separately labeled:

- `historical_growth_stage`
- `implementation_dependency`

Never infer historical age from Minecraft task order.

---

## 18. Builder Handoff Gate｜v0.5

Before a package is declared Builder-ready, verify:

- planning recursion has reached sufficient scale;
- WHY / role is explicit;
- Design Context and Planning Causal Context are sufficient;
- boundary / envelope is understandable;
- Minecraft boundary semantic is explicit where voxel legality depends on it;
- fixed constraints are few but meaningful;
- Actor / rights / access relations are explicit where consequential;
- every planning-fixed cross-scope physical interface needed by the next Builder stage is resolvable;
- relevant Interface Baselines include local geometry / section / clear-width semantics when needed;
- external service responsibilities are explicit when architecture depends on them;
- adaptation requirement is relationship-level, not exact engineering;
- Builder retains genuine architectural / engineering freedom;
- planning assumptions and unresolved conditions carry `resolve_before`;
- Kit requirements do not prescribe finished geometry;
- capacity is decomposed enough for bounded design scope;
- package / interface revision lineage is present;
- world-write authorization is not implied.

If fixed external interfaces are missing, mark `CONCEPT_DESIGN_READY` at most, or `INCOMPLETE_HANDOFF` if even concept design cannot preserve planning semantics.

---

## 19. Recommended JSON shape

```json
{
  "id": "BDP-01",
  "package_revision": "r2",
  "recipient": "minecraft-builder",
  "builder_handoff_readiness": "DESIGN_FREEZE_READY",
  "scale": "URBAN_ENSEMBLE",
  "why": "repair household serving west transfer flow",
  "design_context": {
    "regional_family": "east-domain mountain settlement",
    "users": ["repair household"]
  },
  "planning_causal_context": {
    "flow": "west unload -> hand-carried repair -> east light load",
    "rights": "common frontage passage must remain open"
  },
  "spatial_envelope": {"ref": "PARCEL-01"},
  "boundary_semantic": "CELL_CENTER_MASK",
  "planner_fixed": [
    "one complete household + repair program",
    "common frontage passage remains open"
  ],
  "builder_adaptable": [
    "exact_footprint",
    "plan_section",
    "structure",
    "roof",
    "facade",
    "palette"
  ],
  "interface_baselines": [
    {
      "interface_id": "IF-LANE-04-P01",
      "source_object": "LANE-04",
      "source_revision": "P04-r1",
      "role": "common pedestrian frontage",
      "local_geometry": {"type": "corridor_envelope", "ref": "LOCAL-SLICE-01"},
      "nominal_width": 2,
      "minimum_clear_requirement": 2,
      "adjustment_envelope": "threshold may move inside parcel, lane may not be narrowed",
      "rights_access_semantic": "COMMON_EASEMENT",
      "coordination_owner": "BDP-00",
      "resolve_before": "BEFORE_DESIGN_FREEZE"
    }
  ],
  "known_uncertainty": [
    {
      "id": "C-RIGHTS",
      "state": "UNRESOLVED",
      "resolve_before": "BEFORE_WORLD_WRITE"
    }
  ],
  "world_write_authorization": false
}
```

Example only; keep schemas lightweight and task-relevant.
