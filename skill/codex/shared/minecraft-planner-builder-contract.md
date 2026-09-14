# Minecraft Planner–Builder Contract v1.0

## Purpose

This file is the shared interface contract between `minecraft-planner` and `minecraft-builder`.

It exists so that Planner can emit a bounded, causal design package and Builder can consume it without either:

- rereading the entire upstream planning archive;
- guessing missing public / shared geometry;
- silently changing planning-fixed relationships;
- or treating Planner output as a finished architectural blueprint.

Canonical division of authorship:

> **Planner owns why buildings relate to each other and what relationships must survive. Builder owns the exact architecture and physical realization.**

This contract is the source of truth for the interface itself. Planner-specific planning logic remains in `minecraft-planner`; architectural design logic remains in `minecraft-builder`.

---

## 1. Progressive-disclosure context model

A Builder Design Package should expose three context layers.

### `DESIGN_CONTEXT`

Builder should normally read this directly.

Examples:

- civilization / regional architectural family;
- users / actors;
- site / terrain condition;
- maturity / historical stage;
- package role.

### `PLANNING_CAUSAL_CONTEXT`

Builder must understand this before design freeze.

Examples:

- WHY this package exists;
- relevant flows / rhythms / externalities;
- public / common / private rights;
- frontage / shared-space / service relationships;
- planning-level adaptation requirements;
- causal relation to neighboring packages.

### `REFERENCE_CONTEXT`

Only read when needed to resolve a concrete design question.

Examples:

- upstream Canon source;
- parent planning object;
- supporting terrain evidence;
- larger regional rationale.

Do not require every Builder scope to load the complete L0→L4 planning chain.

---

## 2. Builder Design Package minimum contract

A Builder Design Package should contain, when relevant:

```text
package_id
package_revision
recipient = minecraft-builder
scope / scale
builder_handoff_readiness

WHY / planning_role
design_context
planning_causal_context
upstream_anchors_flows
spatial_envelope
boundary_semantic
required_adjacency
required_access
shared_space_service_relations
program_requirements
historical_growth_stage
architecture_kit_requirements

PLANNER_FIXED
BUILDER_ADAPTABLE
INTERFACE_BASELINES
EXTERNAL_SERVICE_INTERFACES

dependencies
known_uncertainty
upstream_issue_protocol
source_refs
world_write_authorization
```

Not every package needs every field, but every planning-fixed relationship that crosses the current Builder scope must be resolvable through this package or one of its direct immutable references.

---

## 3. Builder handoff readiness

Use the following package-level readiness semantics.

### `CONCEPT_DESIGN_READY`

Enough information exists for bounded Architecture Design exploration, but one or more planning-fixed external interfaces are not yet sufficient for final design freeze.

Builder may develop Plan / Section / Tectonics while preserving a clear interface HOLD.

### `DESIGN_FREEZE_READY`

The package is sufficiently specified for Builder to freeze Architecture Design, subject to uncertainties whose `resolve_before` occurs later, such as world write or operation.

Planning-fixed cross-scope interfaces are locally resolvable.

### `INCOMPLETE_HANDOFF`

The package lacks information required even to preserve its planning-fixed semantics during the requested design step.

Builder must report the missing minimum data rather than reread full upstream planning or invent it.

Readiness is not world-write authorization.

---

## 4. `PLANNER_FIXED`

Use only for relations whose violation would break accepted planning causality.

Typical examples:

- package / parcel role;
- frontage orientation or functional face;
- required public / common / service access;
- protected negative space;
- shared courtyard relation;
- capacity / household ceiling;
- public-private threshold logic;
- service-side requirement;
- growth / inherited-fabric relation;
- planning-level mitigation requirement;
- cross-package interface that must remain open.

Planner should freeze the **relationship**, not architectural geometry that Builder is supposed to author.

---

## 5. `BUILDER_ADAPTABLE`

Builder normally owns:

- exact footprint within the legal envelope;
- room arrangement;
- exact Plan + Section;
- exact floor count / heights unless constrained by planning relation;
- structure / tectonics;
- roof / facade / openings;
- detailed terrain interface;
- exact well / cistern / retaining / drainage / bridge geometry;
- local threshold geometry;
- block palette;
- furniture / finishing;
- Minecraft Translation.

Builder may move or reshape an interface locally only inside an explicitly supplied adjustment envelope and only while preserving planning semantics.

---

## 6. Interface Baseline｜跨 Scope 的固定物理接口必须可解析

Any physical interface that is both:

1. relevant to the current Builder scope, and
2. planning-fixed or required for design freeze,

must be represented by an `Interface Baseline`.

Examples:

- public lane beside a parcel;
- shared courtyard edge;
- bridge landing;
- common stair / retaining interface;
- service alley;
- clean-water access point;
- parcel-to-road threshold band;
- party-wall or shared-wall line;
- shared drainage / delivery interface.

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

### 6.1 Local slice, not full upstream plan

Do not copy an entire regional route into every building package.

Provide only the local slice needed by the Builder scope, or an immutable object reference + revision that can be dereferenced directly.

### 6.2 Geometry must mean something

`local_geometry` must declare whether it represents:

- planning centerline;
- corridor envelope;
- protected clear envelope;
- edge / boundary;
- search segment;
- section control line;
- point interface.

A route ID plus “2 blocks wide” is not enough if Builder must prove that a threshold does not obstruct it.

### 6.3 Nominal width ≠ minimum clear width

Keep separate when consequential:

```text
nominal_width
minimum_clear_requirement
```

Nominal planning width may include shoulders / tolerance; minimum clear width is the relationship that must actually survive detailed design.

### 6.4 Height / section baseline

If a shared interface depends on elevation, include enough local information to resolve:

- relevant ground / design elevation;
- grade / step / landing relation;
- section control points;
- whether Builder may alter local grade;
- maximum or qualitative adjustment range.

Planner need not engineer the final construction.

---

## 7. Minecraft Boundary Semantic｜连续规划边界必须说明如何离散化

When Planner gives a polygon or curved / diagonal boundary that Builder will translate to blocks, specify one of the following or an equivalent project-local semantic:

- `CELL_CENTER_MASK` — a voxel column belongs to the scope when its column center is inside the planning geometry;
- `FULL_VOXEL_INSIDE` — the whole occupied voxel footprint must remain inside the continuous boundary;
- `CONTINUOUS_BOUNDARY_WITH_TOLERANCE` — continuous edge controls, with explicit tolerance / negotiation strip;
- `NEGOTIABLE_EDGE` — edge may move within an explicit adjustment envelope;
- `REFERENCE_ONLY` — geometry is locational context and must not be treated as a legal build mask.

Where needed also state:

```text
edge_tolerance
adjustment_strip
vertical_boundary_semantic
```

Builder must not invent a discretization rule for a planning-fixed edge.

---

## 8. External Service Interfaces｜建筑内部系统离开地块后谁负责

When a building system crosses its parcel / package boundary, identify the relationship and ownership.

Examples:

- rainwater outfall;
- clean-water delivery / storage interface;
- waste pickup;
- fuel delivery;
- shared drainage;
- loading / unloading;
- public lighting / gate control;
- retaining / slope system shared across parcels.

Recommended fields:

```text
service_id
service_type
interface_location_or_ref
status
upstream_or_downstream_owner
builder_responsibility
neighbor_or_public_responsibility
allowed_local_adjustment
resolve_before
uncertainty
```

Useful `builder_responsibility` values include:

- `RESERVE_INTERFACE_ONLY`
- `DESIGN_LOCAL_CONNECTION`
- `CO-DESIGN_INTERFACE`
- `FULL_SCOPE_OWNER`

A Builder must not route water, waste, freight or other service into public space merely because no external interface was supplied.

---

## 9. Known uncertainty and `resolve_before`

Uncertainty should not be flattened into one blocker.

Each consequential unresolved item should state when it must be resolved, for example:

- `BEFORE_CONCEPT_DESIGN`
- `BEFORE_DESIGN_FREEZE`
- `BEFORE_WORLD_WRITE`
- `BEFORE_PERMANENT_OCCUPATION`
- `BEFORE_OPERATION`

Examples:

- exact foundation capacity may be `BEFORE_DESIGN_FREEZE` or `BEFORE_WORLD_WRITE` depending on design dependency;
- land-use permission may allow concept design but remain `BEFORE_WORLD_WRITE`;
- seasonal supply may allow architecture design but block `BEFORE_PERMANENT_OCCUPATION`.

---

## 10. Builder Planner Context Intake

When a Builder Design Package exists, Builder should perform this before Architectural Intent.

1. Read the target BDP and direct interface dependencies.
2. Compile an `Inherited Planning Intent` containing:
   - WHY / role;
   - users / actors;
   - program;
   - flows / rhythms;
   - rights / public-private relationships;
   - site / envelope;
   - PLANNER_FIXED;
   - BUILDER_ADAPTABLE;
   - Interface Baselines;
   - External Service Interfaces;
   - Architecture Kit requirements;
   - unresolved conditions and `resolve_before`.
3. Verify that every planning-fixed external interface required for the requested design stage is resolvable.
4. If not, return `INCOMPLETE_HANDOFF` with the minimum missing information.
5. Do **not** compensate by reading the full parent plan unless the task explicitly authorizes that broader context.

Builder may read direct immutable references supplied by the BDP. This is contract resolution, not uncontrolled upstream fishing.

---

## 11. Planning Fidelity Gate

Before Architecture Design is frozen, Builder verifies that the design still expresses the inherited planning logic.

Check, where applicable:

- WHY / role is still legible in the architecture;
- required program has not been silently replaced;
- PLANNER_FIXED frontage / adjacency / access survives;
- shared / public space is not privatized;
- rights / easements are not erased by geometry;
- package / parcel capacity is not exceeded without upstream revision;
- terrain / mitigation relation remains consistent;
- required flows have real thresholds / routes;
- Interface Baselines are satisfied;
- External Service Interfaces are reserved or connected according to responsibility;
- planning uncertainty has not been converted into invented fact;
- Builder still retains genuine architectural authorship.

Gate results:

- `PASS` — design may freeze / proceed;
- `FAIL_BUILDER_DESIGN` — fix architecture inside Builder authority and rerun;
- `INCOMPLETE_HANDOFF` — missing upstream interface / boundary / responsibility data;
- `UPSTREAM_PLANNING_ISSUE` — supplied planning-fixed relations are mutually incompatible or infeasible after bounded Builder adaptation.

---

## 12. Upstream issue protocol

When Builder cannot satisfy the contract, report:

```text
package / interface IDs
conflicting or missing relation
observed evidence
bounded adaptations attempted
why Builder-level adaptation is insufficient
minimum upstream information or decision required
which packages / interfaces become stale if changed
```

Builder must not silently:

- enlarge a parcel;
- increase household / program capacity;
- close a shared route;
- occupy common land;
- move a planning anchor;
- invent a public drainage / water / service route;
- or replace required program with an easier one.

Planner should revise the smallest causal object necessary and republish affected interface revisions.

---

## 13. Versioning, immutable refs and staleness

Every Builder-ready package should identify:

```text
package_revision
source_plan_revision
interface_revision(s)
```

If an interface changes materially, downstream designs depending on the old revision become `STALE_FOR_FIDELITY_REVIEW` until rechecked.

Prefer immutable object refs / commit-pinned refs for archived regression or accepted planning packages.

---

## 14. World-write boundary

Planner handoff never implies construction authorization.

Default:

```text
world_write_authorization = false
```

Builder may perform Architecture Design and evidence generation while world writes remain false.

Rights, supply, foundation, collision or other uncertainties can have later `resolve_before` stages; they must not be silently promoted to resolved merely because a design exists.

---

## 15. Compact JSON example

```json
{
  "id": "BDP-01",
  "package_revision": "r2",
  "recipient": "minecraft-builder",
  "builder_handoff_readiness": "DESIGN_FREEZE_READY",
  "why": "repair household serving west-side transfer flow",
  "spatial_envelope": {"ref": "PARCEL-01"},
  "boundary_semantic": "CELL_CENTER_MASK",
  "planner_fixed": [
    "keep common frontage passage open",
    "preserve one complete household + repair program"
  ],
  "builder_adaptable": [
    "exact footprint",
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
      "role": "public pedestrian frontage",
      "local_geometry": {"type": "corridor_envelope", "ref": "..."},
      "nominal_width": 2,
      "minimum_clear_requirement": 2,
      "adjustment_envelope": "local threshold may move inside parcel only",
      "rights_access_semantic": "COMMON_EASEMENT",
      "coordination_owner": "BDP-00",
      "resolve_before": "BEFORE_DESIGN_FREEZE"
    }
  ],
  "world_write_authorization": false
}
```

The schema is intentionally compact. Add fields only when they materially change design, responsibility, or verification.
