# settlement-capacity-and-scale.md

## Purpose

This reference defines how `minecraft-planner` represents settlement magnitude without confusing location uncertainty, built fabric and service hinterland.

v0.4 adds a critical distinction:

> **Natural capacity is not effective capacity.**

Human adaptation, external supply, access rights, stock / buffer needs and resilience can materially change the scale a settlement can support.

---

## 1. Three different envelopes

For every important settlement / candidate distinguish:

### A. Location Search Envelope

Where should the next scale look?

Represents location uncertainty, not settlement boundary.

### B. Built-Fabric Capacity Envelope

How much actual settlement fabric is plausibly needed at the current / target maturity?

May include buildings, streets / lanes, small courts / work yards, embedded civic / market space and necessary internal open space.

Normally excludes the full agricultural / extraction / service hinterland.

### C. Functional Hinterland / Catchment

What population, production area, route system or institution does the node serve?

This is a relationship field, not urbanized land.

Core invariant:

> **search envelope ≠ built fabric ≠ catchment**

---

## 2. Capacity is a causal result

Do not assign area because “regional center sounds large”.

A stronger v0.4 reasoning chain is:

```text
resident / household pressure
+ daily service demand
+ visitor / event peak
+ freight / production throughput
+ institutional role
+ terrain geometry / resistance
+ surface / substrate / land-cover character
+ ordinary affordable adaptation
+ effective accessibility
+ external supply support
+ stock / buffer requirements
+ resilience requirements
+ density morphology
+ required commons / work yards
+ productive ground that should remain outside built fabric
+ maturity / growth stage
→ Effective Built-Fabric Capacity Envelope
```

Do not treat each term as a numeric coefficient. Use the strongest evidence and causal direction.

---

## 3. Natural Capacity vs Effective Capacity

### Natural Capacity

What the site supports before ordinary human adaptation is considered.

Examples:

- visible surface water access;
- low-slope ground;
- soil-bearing surface;
- natural crossing;
- protected landing.

### Affordable Adaptation

Period-appropriate, scale-proportionate measures may improve usable capacity:

- well / cistern;
- short bridge / ferry;
- retaining / steps / switchback;
- drainage;
- modest terrace / cut / fill;
- road stabilization;
- local storage / transfer infrastructure.

### External Supply

A settlement may depend on external food, fuel, materials or water support if the network / Actor relations make that dependence plausible.

A specialized mining / port / political settlement need not be locally self-sufficient.

### Residual Constraint

After adaptation and supply are considered, identify what still limits scale.

Example:

```text
bare-rock plateau + no obvious surface water
→ ordinary well / cistern may support small residence
→ external food supply handles weak local agriculture
→ basic settlement remains plausible
→ large agricultural expansion and major population growth remain constrained
```

---

## 4. Recommended capacity fields

At L0 / L1, important nodes should attempt to provide:

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
adaptation_assumptions (when consequential)
external_supply_dependency (when consequential)
stock_buffer_need (when consequential)
resilience_dependency (when consequential)
effective_access_summary (when consequential)
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

## 5. Area range, not fake precision

Prefer ranges such as `8,000–15,000 blocks²` when evidence supports order of magnitude.

Do not claim fake exact values unless lower-scale geometry supports them.

If evidence is weak:

```text
class: SMALL_TOWN
working_range: 6,000–18,000 blocks²
confidence: LOW
```

If even order of magnitude cannot be supported:

```text
built_fabric_capacity: UNRESOLVED
```

and state what evidence / decision is missing.

---

## 6. Capacity classes are descriptive, not universal law

Project-local classes such as HAMLET / VILLAGE / TOWN / CITY may be useful, but never assume universal Minecraft area thresholds.

A mountain settlement, port, dense trading town and agricultural village can occupy different areas at similar resident scale.

Area range + morphology matter more than class name.

---

## 7. Morphology changes land consumption

### Compact trade / gateway town

continuous frontage / narrower parcels / shared courts / higher verticality / smaller footprint per unit of trade.

### Agricultural village

detached compounds / barns / animal yards / wider gaps / lower verticality; productive fields remain outside built fabric.

### Mountain settlement

fragmented terrace clusters / vertical stacking / wide geographic spread with small total built area.

Do not estimate all settlements with one density constant.

---

## 8. Current capacity vs future capacity

Distinguish:

- current / target built fabric;
- later growth potential;
- reserved / constrained expansion.

If current fabric is unobserved, call the estimate `planning capacity hypothesis` or `target maturity envelope`, not existing area.

---

## 9. Capacity and productive land

Built-fabric estimate must not silently consume land needed for:

- food production;
- pasture;
- flood storage;
- quarry / mine safety;
- defensive clear ground;
- sacred / burial ground;
- commons;
- water protection;
- scarce soil-bearing / vegetated ground where supported.

Do not model resource depletion / regeneration by default. The question here is current spatial competition and support, not long-horizon resource simulation.

---

## 10. Capacity and metabolism

A settlement may need land not because more people live there, but because periodic flows require buffers.

Examples:

- seasonal grain storage;
- caravan waiting / animal holding;
- water storage;
- event / market peak space;
- transfer / reserve yards.

Therefore capacity should consider peak / storage logic separately from resident scale.

---

## 11. Capacity and resilience

A highly efficient settlement may still be fragile.

Where vulnerability matters, capacity may include bounded redundancy such as:

- secondary water point;
- distributed storage;
- fallback access;
- multiple local service points.

Do not automatically duplicate facilities. The resilience space must answer a credible failure mode.

---

## 12. Capacity and effective accessibility

Catchment and scale depend on actual usable access, not only distance.

A physically near hinterland may contribute little if:

- passage rights are absent;
- route is seasonally unreliable;
- freight mode cannot use it;
- political / security barriers are high.

Conversely, a farther settlement may have strong capacity because a reliable, permitted transport corridor connects it.

---

## 13. Capacity and uncertainty

Useful labels:

- `HIGH`
- `MEDIUM`
- `LOW`
- `UNRESOLVED`

For LOW / UNRESOLVED, identify sensitivity.

Bad sensitivity:

> no surface spring observed → settlement impossible

Better:

> no surface source observed → test well / cistern / nearby supply; if ordinary mitigation closes daily demand, residence remains plausible; if not, reduce scale / seasonality.

Other examples:

```text
if freight volume high → gateway area expands
if current fabric occupies the site → evolution path recalculates
if access rights fail → catchment / route shifts
if seasonal peak is larger → shared yard / storage increases
```

---

## 14. Map representation

At L0 / L1, do not draw all nodes as identical dots.

Useful grammar:

- central point = location hypothesis;
- dashed envelope = search uncertainty;
- semi-transparent / scale-coded built-fabric magnitude;
- separate catchment / arrows;
- confidence styling;
- adaptation / conditional access annotation only when it materially changes interpretation.

Legend must state built-fabric envelope is approximate planning scale, not exact construction boundary.

---

## 15. Capacity Plausibility Test

Ask:

1. Does each important node have more than a point?
2. Is expected built scale visible to Owner?
3. Does area follow from demand / throughput / terrain / maturity?
4. Are political significance and physical size separate?
5. Are freight importance and resident scale separate?
6. Is search uncertainty separated from settlement size?
7. Is catchment separated from built land?
8. Is the range too precise?
9. Did low slope become a false carrying-capacity proxy?
10. Were proportionate ordinary adaptations considered before shrinking / rejecting capacity?
11. Is external supply allowed where network logic supports it, without assuming infinite imports?
12. Do stock / peak / resilience needs change land demand where relevant?
13. Does effective accessibility alter catchment / scale where relevant?
14. Does the next Planner know which assumptions must be refined?

A plan that locates towns but cannot communicate approximate effective settlement scale is incomplete at L0/L1.

---

## 16. Surface-conditioned Capacity

Surface / substrate / cover remain first-class inputs.

Important rule:

> **Low-slope area is not a proxy for settlement carrying capacity.**

But equally:

> **Poor natural surface conditions are not an automatic rejection if ordinary adaptation and external supply plausibly transform the constraint.**

Do not apply universal bare-rock / wetland / forest penalties. Explain direction, adaptation and residual uncertainty.

Detailed surface rules: `surface-substrate-landcover.md`.
Detailed human adaptation rules: `human-geography-kernels.md`.
