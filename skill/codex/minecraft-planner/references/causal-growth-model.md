# causal-growth-model.md

## Purpose

This reference expands the causal-growth part of `minecraft-planner`. It teaches how territorial systems, settlements, districts and parcels emerge through accumulated causes instead of being arranged from a final masterplan.

---

## 1. Growth is recursive across scales

Growth may occur at several nested scales:

```text
Territorial Morphogenesis
→ Regional Morphogenesis
→ Settlement Morphogenesis
→ District Morphogenesis
→ Parcel Morphogenesis
```

Do not assume growth only means “which house was built first”.

Examples:

- a national port system may emerge before the inland market network that later depends on it;
- a mining frontier may turn into a politically important region after roads, fortifications and administration follow resource extraction;
- a market settlement may later subdivide frontage into narrow parcels;
- a former farm lane may survive as a crooked urban street long after farming disappears.

---

## 2. Growth step grammar

Use the following causal grammar for important stages:

```text
Driver
→ Spatial Response
→ New Constraint / Opportunity
→ Next Pressure
```

Example:

```text
Reliable ford
→ waiting / crossing / toll activity
→ people spend time at the crossing
→ food and lodging demand
→ mixed commercial frontage
→ frontage value increases
→ lateral parcel subdivision
→ narrow deep plots
→ rear service access becomes necessary
```

A valid stage should change what the next stage can reasonably do.

---

## 3. Anchor roles through time

### ORIGIN_ANCHOR

The reason a durable settlement or regional concentration first appears.

Examples:

- ford;
- spring;
- monastery;
- mine;
- protected harbor;
- manor;
- sacred site;
- strategic pass.

### GROWTH_ANCHOR

Appears later and redirects growth.

Examples:

- bridge replacing a ford;
- new market charter;
- city gate;
- dock basin;
- castle;
- regional temple;
- major workshop complex.

### STABILIZING_ANCHOR

Helps a mature system remain organized.

Examples:

- common well;
- public granary;
- court;
- council hall;
- cemetery;
- fortified warehouse;
- permanent market square.

For each important Anchor record:

```text
role
scale
why
when
attracts
repels
morphological effects
counterfactual removal effect
```

---

## 4. Path dependence

Historic environments are rarely globally optimal.

Preserve inherited constraints when they still plausibly matter:

- old road alignment;
- former gate;
- bridgehead;
- old parcel boundary;
- cemetery;
- sacred precinct;
- obsolete wall;
- former channel;
- absorbed farmstead;
- manor boundary;
- pre-existing irrigation;
- fire-rebuild patch;
- abandoned industrial yard.

Ask:

> Would later inhabitants really erase this, or would they adapt around it?

Do not preserve every relic automatically. A path-dependent feature survives only if demolition cost, ownership, ritual value, continued use, terrain or institutional inertia makes survival plausible.

---

## 5. Existing Evolution

For `EXISTING_EVOLUTION`, distinguish:

- `INHERITED_ACTIVE`: still used and constraining;
- `INHERITED_RESIDUAL`: no longer primary, but morphology persists;
- `REDEVELOPABLE`: can change with moderate cost;
- `PROTECTED / SACRED / LOCKED`: should not be casually altered;
- `OBSOLETE / REMOVED`: can disappear, but its previous existence may leave scars.

Typical transformations:

```text
wide estate plot
→ inheritance split
→ narrow parcels
→ rear lane
```

```text
outer gate road
→ suburb frontage
→ wall removed
→ former gate becomes intersection / market node
```

```text
burned block
→ rebuilding with new firebreak
→ local street widening
→ visible break in older parcel rhythm
```

---

## 6. Maturity State

A plan should identify settlement / territorial maturity when relevant:

- `FOUNDING`
- `EARLY_GROWTH`
- `EXPANDING`
- `MATURE`
- `STAGNATING`
- `DECLINING`
- `REBUILT / TRANSFORMED`

Maturity affects:

- infrastructure permanence;
- parcel subdivision;
- infill intensity;
- specialization;
- institutional buildings;
- edge condition;
- maintenance and abandonment;
- road hierarchy.

The same premise should not produce the same morphology at every maturity stage.

---

## 7. Territorial growth

At L0 / L1, causal growth may include:

```text
resource discovery
→ extraction node
→ freight corridor
→ processing center
→ regional market
→ administrative interest
→ defensive infrastructure
→ permanent frontier settlement network
```

or:

```text
agricultural core
→ surplus exchange
→ market center
→ river / coastal export
→ port growth
→ hinterland road strengthening
→ secondary market towns
→ political centralization
```

National-scale planning must not jump directly from “resource region” to individual facilities. First explain settlement and transport systems.

---

## 8. Historical Validity Test

For each proposed growth stage:

> If every later stage never happened, would this stage still be viable and understandable?

FAIL examples:

- a founding village reserves a grand future boulevard with no current use;
- an early settlement positions houses around a future market that does not yet exist;
- a frontier road follows a future political border not yet established.

---

## 9. Counterfactual Test

Change one major causal variable:

- remove the ford;
- move the mine;
- close the pass;
- shift the harbor;
- remove the monastery;
- change land tenure;
- reverse a trade connection.

Ask what should change.

If the final spatial plan barely changes, the cited causal factor may only be decorative explanation.

---

## 10. Anchor Removal Test

For each major Anchor, temporarily remove it and identify:

- routes losing purpose;
- building demand disappearing;
- density changing;
- public space weakening;
- district identity changing;
- settlement rank changing.

If no meaningful consequence follows, reconsider whether the object is truly an Anchor.

---

## 11. Growth Map requirements

A growth diagram should distinguish at least:

- existing / inherited fabric;
- earliest durable Anchor(s);
- subsequent growth stage(s);
- major route reinforcement;
- expansion fronts;
- scars / residual constraints;
- current morphology.

Avoid arbitrary colored rings when actual growth followed corridors, terrain pockets, riverbanks, ridges or fragmented ownership.
