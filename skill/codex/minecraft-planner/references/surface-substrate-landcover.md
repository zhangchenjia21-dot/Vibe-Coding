# surface-substrate-landcover.md

## Purpose

This reference makes **surface / substrate / land-cover character** a first-class planning input for `minecraft-planner`.

Terrain geometry alone is insufficient. Two locations can have the same elevation, slope and relief while supporting very different settlement, livelihood, drainage, access and construction logic.

Core invariant:

> **Flat is not the same as habitable, productive, buildable or equivalent. Geometry does not describe land character.**

---

## 1. Three evidence layers

Keep these concepts separate.

### A. Surface character

What is directly exposed at / near the ground surface?

Examples:

- grass / dirt family;
- exposed stone / rock;
- sand / gravel;
- mud / clay-like ground where actually observed;
- snow / ice;
- waterlogged / marsh-like surface where supported;
- cultivated / disturbed surface when directly observed.

### B. Near-surface substrate

What lies immediately beneath or forms the shallow ground structure?

Only use this if evidence actually exists. Do not infer deep geology from one visible surface block.

Possible planning distinctions:

- shallow soil over rock;
- deep soil-bearing ground;
- exposed or near-surface rock;
- loose sand / gravel;
- wet / soft ground;
- shallow artificial fill.

### C. Land cover / vegetation character

What occupies the ground surface spatially?

Examples:

- open grass / herbaceous cover;
- sparse vegetation;
- shrub / understory;
- forest / canopy;
- barren / rock exposure;
- wetland vegetation;
- cultivated cover when actually observed.

These three layers may overlap but are not synonyms.

---

## 2. Evidence discipline

Do not convert visible Minecraft blocks into claims they do not support.

Forbidden shortcuts:

> grass block = fertile farmland

> exposed stone = proven quarry / high-quality building stone / ore deposit

> forest = sustainable timber yield

> sand = desert society

> biome = soil fertility model

> mud / water = permanent wetland without hydrologic evidence

Allowed reasoning is narrower:

> extensive exposed stone suggests a bare-rock surface character and limited visible soil-bearing surface in the observed snapshot.

> grass/dirt-bearing ground provides more visible soil-covered surface than bare stone, but agricultural productivity remains unverified.

> dense tree cover changes clearing cost, visibility and movement even when timber yield is unknown.

Always preserve authority and uncertainty.

---

## 3. Why surface character changes planning

Surface / substrate / cover can affect:

### Settlement capacity

- usable everyday ground;
- food-support assumptions;
- household yard / garden plausibility;
- open-space function;
- expansion cost;
- dependence on external supply.

It does **not** imply a universal numeric penalty.

### Water / drainage

- infiltration / runoff questions;
- exposed-rock drainage pattern;
- wet-ground avoidance;
- water-retention uncertainty;
- need for storage / protected sources.

Planner should state the question, not invent real hydrology unsupported by Minecraft evidence.

### Movement / infrastructure

- road-bed preparation;
- clearing / excavation cost;
- erosion / washout risk;
- retaining / terracing pressure;
- whether paths can use exposed rock or must bridge soft / wet ground.

### Livelihood / productive landscape

- visible soil-bearing ground vs bare rock;
- pasture / garden / cultivation opportunity as a hypothesis only;
- mining / quarry / forestry interfaces as conditional opportunities;
- need for external food / fuel / soil inputs.

### Settlement morphology

- continuous spread vs fragmented pockets;
- rock-hugging / terrace-based clusters;
- courtyard / work-yard distribution;
- separation of productive soil pockets from built fabric;
- preservation of scarce soil-bearing or vegetated ground.

### Architecture Kit Requirements

Planner may communicate requirements such as:

- strong rock / terrain interface;
- retaining / stepped foundations;
- local heavy-masonry opportunity;
- minimal consumption of scarce soil-bearing pockets;
- forest-edge / wet-ground adaptation.

Planner still does **not** choose exact palette, block family, wall detail or structural geometry.

---

## 4. Scale discipline

### L0 POLITY_TERRITORY

Use coarse surface / cover classes only when they materially affect territorial roles, carrying capacity or route systems.

Do not classify every block nationwide if unnecessary.

### L1 REGIONAL_SYSTEM

Surface character becomes especially important when comparing settlement pockets with similar slope / elevation.

Regional candidates should, when evidence permits, report proportions or classes such as:

```text
exposed_rock_ratio
soil_bearing_surface_ratio
sand_gravel_ratio
vegetated_cover_ratio
wet_surface_ratio
surface_character_confidence
```

These names are conceptual; use the actual evidence schema available.

### L2 SETTLEMENT

Refine candidate envelopes using local surface mosaics, productive-ground protection, water / drainage evidence and inherited fabric.

### L3 / L4

Use detailed ground character to shape parcel / courtyard / service / lane logic, but leave exact material construction to Builder.

---

## 5. Surface Character Summary

For consequential candidates, a lightweight summary may contain:

```json
{
  "surface_character": {
    "observed_or_derived": "DERIVED",
    "dominant_character": "EXPOSED_ROCK",
    "exposed_rock_ratio": 0.68,
    "soil_bearing_surface_ratio": 0.19,
    "vegetation_cover": "SPARSE / UNVERIFIED",
    "near_surface_substrate": "UNRESOLVED",
    "planning_implications": [
      "local food-support capacity should not be inferred from flat area",
      "preserve verified soil-bearing pockets",
      "stone-building opportunity may be investigated"
    ],
    "uncertainty": [
      "fertility unknown",
      "water retention unknown",
      "rock quality / quarry suitability unknown"
    ]
  }
}
```

Do not invent a ratio if the evidence source cannot support it.

---

## 6. Surface-conditioned settlement capacity

Built-fabric capacity should consider land character in addition to geometry.

Conceptually:

```text
terrain geometry
+ surface / substrate / cover character
+ water / supply evidence
+ livelihood demand
+ accessibility
+ institutional / network role
→ capacity hypothesis
```

Important rule:

> **Low-slope area must not be used as a proxy for settlement carrying capacity without checking relevant surface character.**

Examples:

- a large bare-rock plateau may support substantial construction but weak local soil-based subsistence;
- a small soil-bearing terrace may be disproportionately valuable as productive / domestic ground and should not be casually built over;
- flat wet ground may have high geometric capacity but low practical settlement capacity without engineering;
- dense forest may be buildable after clearing but should not be treated as empty land.

Do not impose a universal formula. Record the direction of effect and uncertainty.

---

## 7. Surface / Substrate map

When surface character materially affects planning, provide a map or layer that makes the distinction visible to the Owner.

Useful layers:

- exposed rock / soil-bearing / sand-gravel / wet / snow-barren classes;
- vegetation / canopy / open-ground cover;
- scarce productive-ground pockets;
- surface uncertainty / unsurveyed areas;
- candidate settlement envelopes overlaid separately.

Do not style a surface map so strongly that Proposal boundaries look like Observed facts.

---

## 8. Recursive handoff

If current scale lacks adequate surface evidence, pass it explicitly through `DOWNSTREAM_TO_RESOLVE`.

Examples:

```text
verify exposed-rock vs soil-bearing surface in candidate envelope
identify scarce soil / vegetation pockets before fixing built extent
verify water-bearing / wet-ground conditions
verify whether visible stone is merely surface exposure or useful construction source
```

If lower-scale evidence materially changes capacity or role, use the normal `REVISION_TRIGGER / UPSTREAM_PLANNING_ISSUE` protocol.

---

## 9. Surface Character Necessity Test

Ask:

> If two candidate sites had identical elevation, slope and relief but different surface/substrate/land-cover character, would the plan change?

If the answer is “no” in a task where livelihood, carrying capacity, drainage or local material conditions matter, the terrain model is incomplete.

Also ask:

1. Did low slope get silently treated as good living / farming land?
2. Did bare rock become a quarry without evidence?
3. Did vegetation get ignored as if the land were empty?
4. Did a scarce soil-bearing pocket get consumed because only slope was mapped?
5. Did surface character change capacity, route, open-space or Architecture Kit requirements where it should?
6. Are unsupported ecological / geological claims clearly marked unknown?

---

## 10. Owner-facing principle

A useful planning map should let the Owner distinguish not only:

> where the land is high / low / steep / flat

but, when relevant:

> **what kind of ground the settlement is actually sitting on.**
