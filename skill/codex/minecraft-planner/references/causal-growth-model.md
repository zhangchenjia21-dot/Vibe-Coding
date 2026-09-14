# causal-growth-model.md

## Purpose

This reference expands the causal-growth part of `minecraft-planner`. It explains how territorial systems, settlements, districts and parcels emerge through accumulated choices and feedback instead of being arranged from a final masterplan.

v0.4 adds two foundational ideas:

- historical actors have **bounded knowledge**;
- spatial choices create **feedback** that changes later conditions.

---

## 1. Growth is recursive across scales

```text
Territorial Morphogenesis
→ Regional Morphogenesis
→ Settlement Morphogenesis
→ District Morphogenesis
→ Parcel Morphogenesis
```

Do not assume growth only means “which house was built first”.

---

## 2. Growth step grammar

For important stages, prefer the full grammar:

```text
World condition / pressure
→ What relevant actors know
→ Actor interest / rights / capability
→ Choice / negotiation / adaptation
→ Spatial response
→ Modified condition / new anchor / new access
→ Next pressure / feedback
```

For simpler cases, the short form remains valid:

```text
Driver
→ Spatial Response
→ New Constraint / Opportunity
→ Next Pressure
```

A valid stage must change what later stages can reasonably do.

Example:

```text
Reliable ford is locally known
→ merchants and local authority value the crossing
→ waiting / toll / exchange activity
→ repeated use stabilizes access rights
→ bridge becomes proportionate investment
→ bridge concentrates traffic
→ bridgehead frontage becomes more valuable
→ market activity intensifies
→ congestion / subdivision pressure appears
```

---

## 3. Planner knowledge ≠ historical actor knowledge

A world fact visible to the Planner may not yet be known to historical actors.

When consequential, use epistemic states such as:

- `HISTORICALLY_KNOWN`
- `LOCALLY_KNOWN`
- `PARTIALLY_KNOWN`
- `UNDISCOVERED`
- `UNKNOWN_TO_PLANNER`
- `PLANNER_ONLY_EVIDENCE`

Example:

```text
remote ore exists in Planner evidence
but is UNDISCOVERED in founding stage
→ no founding road may target it
→ later exploration discovers it
→ extraction begins
→ route and settlement hierarchy change
```

Do not use future / global information to optimize early history.

---

## 4. Anchor roles through time

### ORIGIN_ANCHOR

Explains why a durable concentration first appears.

Examples: ford, spring, monastery, known mine, protected harbor, manor, sacred site, pass.

### GROWTH_ANCHOR

Appears later and redirects growth.

Examples: bridge replacing a ford, new market right, city gate, dock basin, castle, new road junction.

### STABILIZING_ANCHOR

Helps a mature system remain organized.

Examples: common well, shared granary, court, cemetery, permanent market square.

### Human-created Anchors

v0.4 explicitly allows infrastructure created by earlier pressures to become later Anchors:

```text
water difficulty
→ communal well
→ daily convergence
→ shared court / lane
→ surrounding household growth
```

```text
repeated crossing
→ bridge
→ traffic concentration
→ market / bridgehead growth
```

For each important Anchor record:

```text
role
scale
why
when
relevant actors
knowledge state when useful
attracts
repels
morphological effects
counterfactual removal effect
```

---

## 5. Path dependence

Historic environments are rarely globally optimal.

Possible inherited constraints:

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
- irrigation;
- fire-rebuild patch;
- abandoned industrial yard.

Ask:

> Would later inhabitants really erase this, or would rights, use, terrain, social meaning or switching burden keep it?

Do not preserve every relic automatically.

Do not introduce a full maintenance / lifecycle simulation; the relevant question is whether inherited spatial inertia changes the current plan.

---

## 6. Existing Evolution

For `EXISTING_EVOLUTION`, distinguish:

- `INHERITED_ACTIVE`
- `INHERITED_RESIDUAL`
- `REDEVELOPABLE`
- `PROTECTED / SACRED / LOCKED`
- `OBSOLETE / REMOVED`

Typical transformations:

```text
wide household / estate plot
→ household or inheritance split
→ narrower parcels
→ rear access pressure
```

```text
outer gate road
→ suburb frontage
→ wall loses function
→ former gate remains a high-access node
```

---

## 7. Maturity State

Useful states:

- `FOUNDING`
- `EARLY_GROWTH`
- `EXPANDING`
- `MATURE`
- `STAGNATING`
- `DECLINING`
- `REBUILT / TRANSFORMED`

Maturity can affect infrastructure permanence, parcel subdivision, infill intensity, specialization, edge condition and road hierarchy.

Do not add a detailed infrastructure replacement model by default.

---

## 8. Feedback loops

Growth is not always a one-way sequence.

### Reinforcing feedback

```text
bridge
→ traffic concentration
→ market activity
→ settlement growth
→ more crossing demand
→ stronger bridgehead importance
```

```text
market frontage
→ visibility / exchange value
→ subdivision / density
→ more activity
→ still higher frontage pressure
```

### Balancing feedback

```text
frontage intensification
→ congestion / externality
→ bypass / secondary route
→ new frontage opportunity
→ old center growth slows or changes role
```

```text
large central service dependency
→ vulnerability to disruption
→ secondary local service point
→ pressure on central node stabilizes
```

Feedback may weaken or be interrupted by rights, politics, competing centers, environmental conditions, new knowledge or changing demand.

---

## 9. Territorial growth

At L0 / L1, causal growth may include:

```text
exploration
→ resource discovery
→ local extraction
→ knowledge spreads
→ freight path strengthens
→ transfer / processing node
→ regional market
→ administrative / political interest
→ permanent settlement network
```

or:

```text
agricultural surplus
→ periodic exchange
→ market center
→ export route strengthens
→ gateway / port grows
→ hinterland access improves
→ secondary market towns
```

National-scale planning must not jump from “resource region” to individual facilities.

---

## 10. Historical Validity Test

For each proposed stage:

> If every later stage never happened, would this stage still be viable and understandable?

FAIL examples:

- a founding village reserves a grand future boulevard with no current use;
- early residents target an undiscovered future resource;
- a frontier road follows a political border not yet established.

---

## 11. Knowledge Test

Ask:

> Which facts are available to the Planner but not yet plausibly available to the actors in this stage?

If those facts drive a decision, revise the sequence.

---

## 12. Feedback Test

Ask:

> Did major infrastructure, access, rights, density or institutions modify the conditions of the next stage?

If growth is always only “more demand → more buildings”, the causal model may be too linear.

---

## 13. Counterfactual Test

Change one major causal variable:

- remove the crossing;
- move the known resource;
- change access rights;
- remove the institution;
- alter actor knowledge;
- remove a mitigation option;
- reverse a trade connection.

If final morphology barely changes, the cited cause may be decorative.

---

## 14. Anchor Removal Test

Remove one major Anchor and identify routes, demand, capacity, density, public space or hierarchy losing purpose.

If no meaningful consequence follows, reconsider whether it is truly an Anchor.

---

## 15. Growth Map requirements

A growth diagram should distinguish as relevant:

- existing / inherited fabric;
- earliest durable Anchor(s);
- knowledge / discovery transitions when consequential;
- subsequent growth stage(s);
- infrastructure / access changes;
- feedback-driven expansion or redirection;
- residual constraints / scars;
- current morphology.

Avoid arbitrary concentric rings when growth followed corridors, terrain pockets, rights, bridges, markets or fragmented ownership.
