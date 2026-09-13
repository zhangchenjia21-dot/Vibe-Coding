# settlement-capacity-and-scale.md

## Purpose

This reference defines how `minecraft-planner` represents settlement magnitude without confusing location uncertainty, built fabric and service hinterland.

The goal is to let the Owner understand **how much land a settlement is expected to occupy**, while preserving scale discipline and uncertainty.

---

## 1. Three different envelopes

For every important settlement / settlement candidate, distinguish:

### A. Location Search Envelope

Question:

> Where should the next planning scale look for the settlement or anchor?

It may be:

- a point + search radius / halfwidth;
- a broad terrain pocket;
- a shoreline segment;
- a valley / terrace / pass search area.

It represents **location uncertainty**.

It is not the settlement boundary.

### B. Built-Fabric Capacity Envelope

Question:

> At the current / target maturity, how much actual built settlement fabric is plausibly needed?

It may include:

- buildings;
- streets / lanes;
- small courtyards / work yards;
- embedded civic / market space;
- necessary internal open space.

It should normally exclude the full agricultural / extraction / service hinterland.

This is the Owner-facing scale estimate that answers “how big is the town / node expected to be?”

### C. Functional Hinterland / Catchment

Question:

> What population, production area, route system or institution does this node serve?

This may be many times larger than built fabric.

It is a **relationship field**, not urbanized land.

Core invariant:

> **search envelope ≠ built fabric ≠ catchment**

---

## 2. Capacity is a causal result

Do not assign area because “regional center sounds large”.

Estimate capacity from the strongest available drivers:

```text
resident / household pressure
+ daily service demand
+ visitor / event peak
+ freight / production throughput
+ institutional role
+ terrain geometry / resistance
+ surface / substrate / land-cover character
+ transport accessibility
+ density morphology
+ required commons / work yards
+ productive land that must remain outside built fabric
+ maturity / growth stage
→ Built-Fabric Capacity Envelope
```

Different roles may produce very different scale profiles.

Example:

```text
political center
resident scale = low–medium
political significance = very high
event peak = high
freight throughput = low
built fabric = small–medium
```

versus:

```text
freight conversion center
resident scale = medium
political significance = medium
freight throughput = very high
built fabric = medium but compact
```

Do not collapse these into one `city_size` adjective.

---

## 3. Recommended capacity fields

At L0 / L1, important settlement nodes should attempt to provide:

```text
resident_scale
built_fabric_area_range_blocks2
core_area_range_blocks2 (when meaningful)
morphology_tendency
service_intensity
freight_throughput
political_significance
symbolic_significance
maturity_state
surface_character_summary (when consequential)
confidence
main_drivers
sensitivity / unresolved evidence
```

Optional:

```text
seasonal_peak_scale
future_growth_range
contraction_floor
estimated_verticality
```

Use only relevant fields.

---

## 4. Area range, not fake precision

Prefer ranges such as:

```text
8,000–15,000 blocks²
20,000–35,000 blocks²
```

when evidence reasonably supports that order of magnitude.

Do not claim:

```text
17,428 blocks²
```

unless geometry or a lower-scale plan actually supports that precision.

When evidence is weak, use broad class + range:

```text
class: SMALL_TOWN
working_range: 6,000–18,000 blocks²
confidence: LOW
```

If even order of magnitude cannot be supported:

```text
built_fabric_capacity: UNRESOLVED
```

and state exactly what evidence the next scale must obtain.

---

## 5. Capacity classes are descriptive, not universal law

The planner may use project-local classes such as:

```text
HAMLET
VILLAGE
LARGE_VILLAGE
SMALL_TOWN
TOWN
LARGE_TOWN
CITY
MAJOR_CITY
```

but must never assume fixed universal Minecraft area thresholds.

A mountain settlement, port, dense trading town and low-density agricultural town can occupy different areas at similar resident scale.

Therefore area range + morphology are more important than the class name.

---

## 6. Morphology changes land consumption

Built-fabric capacity depends on settlement form.

Examples:

### Compact trade / gateway town

- continuous frontage;
- narrower parcels;
- more shared walls / courts;
- higher verticality;
- smaller built area per resident or unit of trade.

### Agricultural village

- detached compounds;
- barns / animal yards;
- wider service gaps;
- lower verticality;
- larger settlement footprint per household, while productive fields remain outside built fabric.

### Mountain settlement

- fragmented terrace clusters;
- non-contiguous built fabric;
- vertical stacking;
- large geographic spread may coexist with small total built area.

Do not estimate capacity using one density constant for all cultures and terrains.

---

## 7. Current capacity vs future capacity

When planning a growth sequence, distinguish:

- `current / target built fabric`；
- `later growth potential`；
- `reserved / constrained expansion`。

A node may have:

```text
current built fabric: 8k–12k
mature plausible range: 15k–25k
```

without drawing the entire future extent as if already built.

If current fabric is unobserved, do not call a capacity estimate “current existing area”. Use:

```text
planning capacity hypothesis
```

or

```text
target maturity envelope
```

---

## 8. Capacity and productive land

A settlement-scale estimate must not silently consume land needed for:

- food production;
- pasture;
- woodland / fuel renewal;
- flood storage;
- quarry / mine safety;
- defensive clear ground;
- sacred / burial ground;
- commons;
- water supply protection;
- scarce soil-bearing / vegetated ground where such scarcity is supported.

Especially at L0/L1, built-fabric growth should be balanced against the same economic / ecological system that supports the settlement.

---

## 9. Capacity and uncertainty

Confidence should reflect evidence, not model confidence rhetoric.

Useful labels:

- `HIGH`: lower-scale geometry / current fabric / strong throughput evidence supports range;
- `MEDIUM`: multiple independent drivers support order of magnitude, but exact fabric unresolved;
- `LOW`: mostly Canon / role / coarse terrain reasoning;
- `UNRESOLVED`: evidence cannot support a useful area range.

For LOW / UNRESOLVED, identify sensitivity, e.g.:

```text
if water supply fails → node remains seasonal / much smaller
if freight volume high → gateway built fabric expands
if existing settlement already occupies site → evolution path must be recalculated
if surface is mostly exposed rock with little soil-bearing ground → local food-support assumption must be reduced / external supply increased
```

---

## 10. Map representation

At L0 / L1, do not draw all settlement nodes as identical dots.

Recommended visual grammar:

- central symbol / point = location hypothesis / anchor;
- dashed search envelope = location uncertainty;
- semi-transparent built-fabric envelope / scaled disk / irregular blob = estimated settlement magnitude;
- separate faint catchment / arrows = functional hinterland;
- confidence / provisional styling = uncertainty.

The legend must explicitly state that the built-fabric envelope is **approximate planning scale, not exact construction boundary**.

If map scale makes literal polygons misleading, use scale-coded symbols but show the area range in label / tooltip / side table.

---

## 11. Capacity Plausibility Test

Before Morphology / Handoff Gate, ask:

1. Does each important settlement node have more than a point?
2. Is its expected built scale visible to the Owner?
3. Does the area range follow from demand / throughput / terrain / maturity?
4. Are political significance and physical size kept separate?
5. Are freight importance and resident population kept separate?
6. Is search uncertainty separated from settlement size?
7. Is catchment separated from built land?
8. Is the range too precise for the evidence?
9. Would a major change in transport / water / population pressure change the capacity estimate?
10. Does the next planner know which capacity assumptions need refinement?
11. Did low slope or large flat area get silently treated as high carrying capacity without checking relevant surface / substrate / land-cover character?

A plan that locates important towns but cannot communicate approximate settlement scale is incomplete at L0/L1.

---

## 12. Surface-conditioned Capacity｜v0.3

Settlement capacity must distinguish **geometric opportunity** from **land-character support**.

A flat plateau can be:

- deep soil-bearing and vegetated;
- mostly exposed rock;
- loose sand / gravel;
- wet / soft ground;
- densely forested;
- snow / barren surface.

These conditions can have similar slope / relief but different settlement implications.

Important rule:

> **Low-slope area is not a proxy for settlement carrying capacity.**

When surface character is consequential, capacity reasoning should explicitly consider:

```text
surface / land-cover evidence
→ effect on local food-support assumptions
→ effect on water / drainage questions
→ effect on clearing / foundation / maintenance cost
→ effect on productive-ground preservation
→ effect on compact / dispersed / fragmented morphology
→ capacity sensitivity / confidence
```

Do not apply a universal numeric penalty. A bare-rock plateau may still support substantial construction, but it may require stronger external food/water support and should not be treated like fertile lowland merely because it is flat. Conversely, a scarce soil-bearing terrace may deserve protection from building even if it is geometrically ideal.

If surface evidence is absent, capacity should carry an explicit unresolved requirement rather than silently assume neutral land character.

Detailed rules: `surface-substrate-landcover.md`.
