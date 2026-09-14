# recursive-planning-handoff.md

## Purpose

This reference defines the contract for **Planner → Planner** recursion across planning scales:

```text
L0 POLITY_TERRITORY → L1 REGIONAL_SYSTEM
L1 REGIONAL_SYSTEM → L2 SETTLEMENT
L2 SETTLEMENT → L3 DISTRICT
L3 DISTRICT → L4 URBAN_ENSEMBLE
```

It is intentionally different from `planner-builder-handoff.md`.

v0.4 expands the handoff so lower scales inherit not only geometry / capacity questions, but also relevant Actor rights, bounded-knowledge uncertainty, mitigation assumptions, effective-access conditions, metabolism and resilience dependencies.

---

## 1. Why recursive handoff exists

A national plan should not design every street.

A regional plan should not design every parcel.

A settlement plan should not decide every building roof.

Higher-scale planning should hand down:

> constraints + causal relations + unresolved questions + evidence boundaries

rather than finished lower-scale geometry.

Core rule:

> **Higher scales constrain lower scales; they do not replace them.**

---

## 2. Planning Package fields

A recursive Planning Package should contain as relevant:

```text
Package ID
Parent Plan / Revision
Parent Scale
Child Scale
Scope / search geometry
WHY / role
Upstream anchors / flows
UPSTREAM_FIXED
DOWNSTREAM_TO_RESOLVE
DOWNSTREAM_ADAPTABLE
Capacity hypothesis
Evidence / source refs
Known uncertainty
Cross-package dependencies
Revision triggers
Expected outputs
World-write authorization = false
```

v0.4 optional-but-important fields when consequential:

```text
actor / rights assumptions
epistemic / discovery state
mitigation assumptions / constraint-transformations
effective-access conditions
stock / buffer / seasonal dependency
resilience requirement / fallback relation
site-value / demographic pressure
feedback relations
```

Do not include these mechanically when they do not affect spatial reasoning.

---

## 3. UPSTREAM_FIXED

Only place a relation here if breaking it would invalidate accepted parent-scale causality.

Examples:

### L0 → L1

- region role in national exchange;
- political / territorial boundary;
- strategic gateway relationship;
- broad settlement hierarchy;
- major cross-region food / ore / authority flow;
- major access-right or commons relation;
- approximate capacity relationship;
- strategic resilience dependency.

### L1 → L2

- settlement role in regional system;
- required relation to route / harbor / pass / market;
- regional catchment relation;
- production / transfer role;
- approximate capacity range;
- cross-settlement dependency;
- important Actor / access relation if role depends on it.

### L2 → L3

- district role / anchor relation;
- major settlement movement route;
- protected commons / infrastructure space;
- broad density / frontage logic;
- district capacity / expansion relationship;
- shared rights / easement / access where essential.

### L3 → L4

- block / parcel relation;
- required frontage / service route;
- shared courtyard / loading interface;
- protected lane / negative space;
- program relationship between building groups;
- critical shared access / rights.

Do not freeze details merely because the parent Planner imagined them.

---

## 4. DOWNSTREAM_TO_RESOLVE

This field lists questions the child Planner **must investigate or decide**, not simply inherit.

Common examples:

- current existing fabric;
- exact site within search envelope;
- water source / well / storage feasibility;
- harbor / ford / bridge viability;
- terrain / surface continuity;
- current road / path inheritance;
- local land tenure / easement / public access;
- Actor authority / cooperation where consequential;
- whether key information was historically known / discovered;
- ordinary mitigation feasibility and residual constraint;
- effective accessibility for relevant transport modes;
- local settlement capacity;
- catchment competition;
- actual resource access;
- stock / buffer / seasonal peak needs;
- resilience fallback if failure consequence is high;
- district / parcel morphology;
- local institutional expression.

An upstream plan that leaves no meaningful questions for the child scale is probably over-designed.

---

## 5. DOWNSTREAM_ADAPTABLE

The child Planner may modify these without parent revision, provided UPSTREAM_FIXED remains true.

Examples:

- precise node location inside search logic;
- exact route corridor among equivalent options;
- local mitigation method among equivalent bounded solutions;
- internal settlement hierarchy;
- number of local villages / hamlets;
- district boundaries;
- local capacity distribution;
- parcel / frontage evolution;
- lower-scale Architecture Kit requirements;
- resilience implementation form when parent only fixes the need for fallback.

Adaptive freedom prevents the parent plan from becoming a frozen masterplan.

---

## 6. Capacity handoff

For an important settlement / node, hand down separately:

```text
location_search_envelope
built_fabric_capacity_hypothesis
functional_hinterland_relation
```

When relevant also include:

```text
surface_character_summary
adaptation_assumptions
external_supply_dependency
effective_access_conditions
stock / seasonal dependency
resilience dependency
```

The child Planner should:

1. refresh local evidence;
2. test ordinary mitigation before treating a raw constraint as fatal;
3. refine or reject capacity range;
4. preserve strategic role if evidence permits;
5. trigger parent revision only when role and feasible effective capacity become incompatible.

Example:

```text
Parent L1:
regional service node
working built fabric 5k–9k
surface water unverified

Child L2 finds:
no nearby surface source, but ordinary well / cistern is plausible at LOW–MODERATE intervention burden

Result:
keep settlement role;
record water infrastructure as local planning requirement;
reduce agricultural assumption if necessary;
do not automatically collapse residence to zero.
```

---

## 7. Constraint / mitigation handoff

A parent may pass a raw constraint without fixing its solution.

Recommended pattern:

```text
raw_constraint: steep approach
parent_implication: heavy freight must not cross settlement core
mitigation_space: switchback / staged unloading / retaining / alternative approach
child_freedom: choose bounded solution based on detailed terrain
revision_trigger: no period-appropriate bounded mitigation can preserve required flow
```

This keeps human adaptation inside recursive planning without stealing Builder geometry.

---

## 8. Effective-access handoff

Do not hand down “route exists” if only physical geometry was tested.

When relevant distinguish:

```text
physical_access
tenure / legal access
political permission
security
seasonality
transport-mode compatibility
```

If a parent route depends on unresolved passage rights, say so.

---

## 9. Bounded-knowledge handoff

When historical discovery matters, pass the epistemic boundary.

Example:

```text
Planner evidence: mineralized area exists
parent historical stage: UNDISCOVERED
child task: model plausible discovery / exploitation sequence before route stabilization
```

Do not let a lower-scale Planner retroactively use future knowledge to justify earlier form.

---

## 10. Metabolism / resilience handoff

Where material, pass:

- replenishment cadence;
- peak / seasonal use;
- storage / buffer role;
- failure consequence;
- required fallback relation.

Do not force numerical stock simulation.

Example:

```text
regional market receives seasonal grain
→ settlement needs buffer/storage capacity
→ child decides distributed vs shared storage morphology
```

---

## 11. Revision triggers

A Planning Package should say what evidence is strong enough to challenge its parent.

Typical triggers:

- proposed crossing remains infeasible **after proportionate mitigation is considered**;
- effective access fails because rights / security / mode cannot be reconciled;
- ordinary water adaptation cannot support proposed scale;
- existing fabric conflicts with parent assumption;
- resource / throughput evidence differs drastically;
- political Canon changes;
- two fixed relationships cannot both hold;
- capacity collapses by an order of magnitude;
- important Anchor disappears / moves;
- a critical resilience dependency cannot be satisfied.

Do **not** trigger parent revision merely because a natural convenience is absent if bounded local adaptation can solve it.

---

## 12. UPSTREAM_PLANNING_ISSUE

Use when child scale cannot satisfy accepted parent logic with bounded local adaptation.

Report:

```text
parent package / fixed relation
new evidence
raw constraint / access / rights issue
mitigation options considered
why local adaptation is insufficient
smallest parent object(s) requiring revision
possible branches
```

Do not silently ignore the parent relation.

Do not automatically rebuild the whole parent plan.

---

## 13. Parent revision protocol

When upstream issue is valid:

1. preserve unaffected parent objects;
2. revise the smallest causal chain;
3. record predecessor / successor IDs where material;
4. update affected flows / capacities / packages;
5. increment plan revision;
6. rerun relevant Critic / Gates;
7. mark downstream packages stale if assumptions changed.

This is planning lineage, not destructive overwrite.

---

## 14. Cross-package dependencies

Examples:

- two regions depend on same crossing;
- multiple settlements depend on one market hierarchy;
- two districts share a freight route;
- political commons requires multi-region access;
- distributed storage / alternate water points provide shared resilience.

Record dependencies so local optimization does not break the larger system.

---

## 15. No Builder leakage

Planner → Planner packages must not use Builder-specific freedom as their main schema.

Wrong at L0→L1:

```text
builder_adaptable:
- roof
- facade
- palette
```

Correct:

```text
DOWNSTREAM_TO_RESOLVE:
- exact settlement site
- local capacity
- rights / access
- water adaptation
- internal network

DOWNSTREAM_ADAPTABLE:
- node location within search logic
- local mitigation among equivalent options
- secondary settlement pattern
```

Builder-specific fields appear only when planning reaches Builder-ready scope.

---

## 16. Child-scale output expectation

### L0 → L1

Need refined regional network, local evidence, effective access, capacity refinement, cross-region interfaces, L2 packages.

### L1 → L2

Need settlement site / morphology, actors / anchors / major movement, current fabric, adaptation / supply closure as needed, district formation, L3 packages.

### L2 → L3

Need district morphology, block / route / frontage / rights relation, capacity / density distribution, L4 packages.

### L3 → L4

Need parcel group / ensemble relation, shared space / service / access logic, concrete Builder package boundaries.

---

## 17. Recursive Handoff Gate

Before parent `HANDOFF_READY`, verify:

- recipient scale is correct;
- UPSTREAM_FIXED contains only meaningful causal constraints;
- DOWNSTREAM_TO_RESOLVE contains real questions;
- DOWNSTREAM_ADAPTABLE preserves authorship;
- capacity is separated from search / catchment;
- Actor / rights / epistemic / mitigation / access / metabolism dependencies are passed only when consequential;
- uncertainty is visible;
- revision triggers consider bounded mitigation before parent rollback;
- cross-package dependencies are explicit;
- no lower-scale geometry is prematurely frozen;
- no direct Builder authorization is implied;
- world-write remains false.

If these fail, the parent is not ready to recurse.
