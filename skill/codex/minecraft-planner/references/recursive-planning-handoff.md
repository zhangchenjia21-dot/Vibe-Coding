# recursive-planning-handoff.md

## Purpose

This reference defines the contract for **Planner → Planner** recursion across planning scales.

It applies to:

```text
L0 POLITY_TERRITORY → L1 REGIONAL_SYSTEM
L1 REGIONAL_SYSTEM → L2 SETTLEMENT
L2 SETTLEMENT → L3 DISTRICT
L3 DISTRICT → L4 URBAN_ENSEMBLE
```

It is intentionally different from `planner-builder-handoff.md`.

The goal is to preserve accepted higher-scale causality while allowing lower-scale planning to discover new evidence and retain real planning freedom.

---

## 1. Why recursive handoff exists

A national plan should not design every street.

A regional plan should not design every parcel.

A settlement plan should not decide every building roof.

Therefore higher-scale planning should hand down:

> constraints + questions + unresolved evidence

rather than finished lower-scale geometry.

Core rule:

> **Higher scales constrain lower scales; they do not replace them.**

---

## 2. Planning Package fields

A recursive Planning Package should contain:

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
Expected visual / machine-readable outputs
World-write authorization = false
```

Do not assume Package = settlement.

A package may represent:

- a natural-social region;
- a city / town search system;
- a frontier corridor;
- a district;
- an urban ensemble candidate;
- a cross-region interface that needs its own refinement.

---

## 3. UPSTREAM_FIXED

Only place a relation here if breaking it would invalidate the accepted parent-scale logic.

Examples:

### L0 → L1

- region role in national exchange;
- major political / territorial boundary;
- strategic gateway relationship;
- broad settlement hierarchy relation;
- major cross-region food / ore / authority flow;
- approximate capacity relationship between nodes;
- protected national commons / frontier / sacred area.

### L1 → L2

- settlement role in regional system;
- required connection to regional route / harbor / pass;
- regional catchment relation;
- production / processing role;
- approximate settlement capacity range;
- cross-settlement dependency.

### L2 → L3

- district role / anchor relation;
- major settlement movement route;
- protected commons / market / sacred / infrastructure space;
- broad density / frontage logic;
- district capacity / expansion relationship.

### L3 → L4

- block / parcel relation;
- required frontage / service route;
- shared courtyard / loading interface;
- protected lane / negative space;
- program relationship between building groups.

Do not freeze details merely because the parent Planner happened to imagine them.

---

## 4. DOWNSTREAM_TO_RESOLVE

This is the most important field for healthy recursion.

It lists questions the child Planner **must investigate or decide**, not simply inherit.

Examples:

- current existing fabric;
- exact settlement site within a search envelope;
- water availability;
- harbor / ford / bridge viability;
- terrain continuity;
- current road / path inheritance;
- local land tenure;
- real settlement capacity;
- exact catchment competition;
- current resource access;
- district / parcel morphology;
- seasonal constraints;
- local religious / institutional expression.

An upstream plan that leaves no meaningful questions for the child scale is probably over-designed.

---

## 5. DOWNSTREAM_ADAPTABLE

The child Planner may modify these without parent revision, provided UPSTREAM_FIXED relations remain true.

Examples:

- precise node location inside approved search logic;
- exact route corridor among several equivalent options;
- internal settlement hierarchy;
- number of local villages / hamlets needed;
- district boundaries;
- local capacity distribution;
- parcel / frontage evolution;
- lower-scale Architecture Kit requirements.

Adaptive freedom should be explicit enough that the child Planner does not treat the parent plan as a frozen masterplan drawing.

---

## 6. Capacity handoff

When the parent scale proposes an important settlement / node, hand down separately:

```text
location_search_envelope
built_fabric_capacity_hypothesis
functional_hinterland_relation
```

Never hand down one polygon ambiguously representing all three.

The child Planner should:

1. test local terrain / fabric;
2. refine or reject the built-fabric capacity range;
3. preserve the strategic role if evidence permits;
4. trigger parent revision if role and feasible capacity become incompatible.

Example:

```text
Parent L0:
regional market node
working built fabric 12k–25k blocks²
MEDIUM confidence

Child L1 finds:
only 4k–7k safe land near required crossing

Possible outcomes:
A. fragmented / vertical morphology still supports role → refine capacity;
B. nearby alternative site supports role → move within adaptable search;
C. no feasible location supports role → UPSTREAM_PLANNING_ISSUE.
```

---

## 7. Revision triggers

A Planning Package should say what evidence is strong enough to challenge its parent.

Typical triggers:

- proposed crossing is impossible;
- water supply cannot support proposed settlement scale;
- existing settlement fabric conflicts with parent Greenfield assumption;
- terrain prevents required regional route;
- resource / throughput evidence is far smaller or larger than assumed;
- political Canon changes;
- two upstream fixed relationships cannot both hold;
- capacity hypothesis collapses by an order of magnitude;
- important Anchor disappears / moves.

---

## 8. UPSTREAM_PLANNING_ISSUE

Use this status when the child scale cannot satisfy accepted parent logic with bounded local adaptation.

Report:

```text
parent package / fixed relation
new evidence
what fails
why local adaptation is insufficient
smallest parent object(s) that need revision
possible branches
```

Do not silently ignore the parent relation.

Do not automatically rebuild the whole parent plan.

---

## 9. Parent revision protocol

When an upstream issue is valid:

1. preserve unaffected parent objects;
2. revise the smallest causal chain necessary;
3. record predecessor / successor IDs where material;
4. update affected flows / capacities / packages;
5. increment plan revision;
6. rerun relevant Critic / Morphology / Handoff Gates;
7. mark downstream packages stale if their assumptions changed.

This is planning lineage, not destructive overwrite.

---

## 10. Cross-package dependencies

A parent may create multiple child packages that share dependencies.

Examples:

- two regions depend on the same crossing;
- multiple settlements depend on one market hierarchy;
- two districts share a freight route;
- a political commons must remain independently accessible from several regions.

Record such dependencies explicitly so one child Planner does not optimize locally and break the whole system.

---

## 11. No Builder leakage

Planner → Planner package must not use Builder-specific freedom as its main schema.

Wrong at L0→L1:

```text
builder_adaptable:
- roof
- facade
- palette
```

Those questions are several scales too low.

Correct:

```text
DOWNSTREAM_TO_RESOLVE:
- exact settlement site
- local capacity
- water
- internal network

DOWNSTREAM_ADAPTABLE:
- node location within search logic
- secondary settlement pattern
- regional route alternatives
```

Builder-specific fields appear only when the planning recursion actually reaches a Builder-ready package.

---

## 12. Child-scale output expectation

The parent package should say what type of answer would close it.

Examples:

### L0 → L1

Need:

- refined regional settlement network;
- local evidence refresh;
- settlement capacity refinement;
- cross-region interface validation;
- L2 settlement packages.

### L1 → L2

Need:

- settlement site / morphology;
- anchors / major movement;
- current / inherited fabric;
- district formation;
- L3 packages.

### L2 → L3

Need:

- district morphology;
- block / route / frontage relation;
- capacity / density distribution;
- L4 packages.

### L3 → L4

Need:

- parcel group / ensemble relation;
- shared space / service logic;
- concrete Builder package boundaries.

---

## 13. Recursive Handoff Gate

Before declaring parent `HANDOFF_READY`, verify:

- recipient scale is correct;
- UPSTREAM_FIXED contains only meaningful causal constraints;
- DOWNSTREAM_TO_RESOLVE contains real unresolved lower-scale questions;
- DOWNSTREAM_ADAPTABLE preserves planning authorship;
- capacity hypothesis is separated from search / catchment;
- uncertainty is visible;
- revision triggers exist;
- cross-package dependencies are explicit;
- no lower-scale geometry is prematurely frozen;
- no direct Builder authorization is implied;
- world-write remains false.

If these fail, the parent plan is not yet ready to recurse.