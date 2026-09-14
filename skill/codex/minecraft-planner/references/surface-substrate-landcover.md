# surface-substrate-landcover.md

## Purpose

This reference makes **surface / substrate / land-cover character** a first-class planning input for `minecraft-planner`.

Terrain geometry alone is insufficient. Two locations can have the same elevation, slope and relief while supporting different settlement, livelihood, drainage, access and construction logic.

v0.4 adds a second principle:

> **Land character is a raw condition, not a final suitability verdict. Ordinary human adaptation may transform some constraints into manageable costs or different spatial forms.**

Core invariants:

> **Flat is not the same as habitable, productive, buildable or equivalent.**

> **Poor natural convenience is not automatically a settlement blocker if proportionate adaptation can close the gap.**

---

## 1. Three evidence layers

Keep these concepts separate.

### A. Surface character

What is directly exposed at / near ground surface?

Examples: grass / dirt family, exposed rock, sand / gravel, mud / clay-like ground when observed, snow / ice, wet surface signals, cultivated / disturbed surface when directly observed.

### B. Near-surface substrate

What lies immediately beneath or forms shallow ground structure?

Only use if evidence exists. Do not infer deep geology from one visible surface block.

Possible distinctions:

- shallow soil over rock;
- deeper soil-bearing ground;
- exposed / near-surface rock;
- loose sand / gravel;
- wet / soft ground;
- shallow artificial fill;
- observed shallow void / cave relation.

### C. Land cover / vegetation character

Examples: open grass / herbaceous, sparse vegetation, shrub, forest / canopy, barren rock, wetland vegetation, cultivated cover when actually observed.

These layers overlap but are not synonyms.

---

## 2. Evidence discipline

Forbidden shortcuts:

> grass block = fertile farmland

> exposed stone = proven quarry / ore deposit

> forest = sustainable timber yield

> sand = desert society

> biome = soil fertility model

> mud / water = permanent wetland without hydrologic evidence

> shallow cave = globally unstable construction zone

Allowed reasoning is narrower:

> extensive exposed stone indicates bare-rock surface and limited visible soil-bearing ground in the observed snapshot.

> grass/dirt-bearing ground provides more visible soil-covered surface, but productivity remains unverified.

> observed shallow void should influence local foundation / placement investigation, but does not automatically prohibit all nearby construction.

Always preserve authority / uncertainty.

---

## 3. Surface character changes planning

### Settlement capacity

May affect everyday usable ground, local food-support assumptions, household yard / garden plausibility, expansion pressure and dependence on external supply.

It does **not** imply a universal numeric penalty.

### Water / drainage

May affect runoff / infiltration questions, wet-ground avoidance, storage need or source protection.

Do not invent hydrology.

### Movement / infrastructure

May affect road-bed preparation, clearing / excavation, retaining / terracing and whether paths use exposed rock or cross softer ground.

### Livelihood / productive landscape

May affect visible soil-bearing opportunity, pasture / cultivation hypothesis, conditional mining / quarry / forestry interfaces and external food / material dependence.

### Settlement morphology

May affect continuous vs fragmented spread, rock-hugging clusters, courtyard / work-yard distribution and preservation of scarce soil-bearing ground.

### Architecture Kit Requirements

Planner may require strong rock / terrain interface, retaining / stepped capability, minimal consumption of scarce soil pockets, forest-edge / wet-ground adaptation, shallow-void avoidance / spanning capability at a high level.

Planner still does not choose exact palette / structure / foundation geometry.

---

## 4. Surface character + Human Adaptation

v0.4 requires the Planner to distinguish **raw ground condition** from **residual constraint after bounded adaptation**.

Examples:

### Bare-rock plateau

Raw implications:

- weak visible soil support;
- drainage / water storage questions;
- easy stone-surface use may be possible;
- local farming cannot be assumed.

Possible adaptation:

- well / cistern;
- compact settlement supplied from elsewhere;
- rock-surface paths / work yards;
- preserve rare soil pockets.

Residual constraint may be:

- limited local agriculture;
- stronger external food dependence;
- expansion constrained by water / supply rather than basic residence.

### Wet / soft ground

Raw implication: high geometric flatness but difficult direct construction / movement.

Possible adaptation: raised path, drainage, limited bridging / piling concept, selecting drier edges.

Residual constraint depends on scale and intervention burden.

### Shallow void

Raw implication: local ground uncertainty.

Possible responses:

- avoid;
- shift footprint / route;
- span / bridge locally if Builder later proves it reasonable;
- preserve void as part of spatial character.

Do not force “fill every void” or “ban every void”.

---

## 5. Proportionate mitigation rule

Before surface / substrate evidence causes major relocation or settlement rejection, ask:

```text
What activity is affected?
What period-appropriate adaptation exists?
Is intervention proportionate to settlement value / scale?
What residual constraint remains?
```

A small well, drainage ditch or short retaining wall may be ordinary.

A huge terrain-flattening megaproject may be disproportionate.

Detailed rules: `human-geography-kernels.md`.

---

## 6. Scale discipline

### L0 POLITY_TERRITORY

Use coarse classes only when materially affecting territorial roles, carrying capacity or route systems.

### L1 REGIONAL_SYSTEM

Especially important when comparing candidates with similar slope / elevation.

Possible evidence summaries:

```text
exposed_rock_ratio
soil_bearing_surface_ratio
sand_gravel_ratio
vegetated_cover_ratio
wet_surface_ratio
surface_character_confidence
```

### L2 SETTLEMENT

Refine local surface mosaics, productive-ground protection, water / drainage evidence, current fabric and plausible ordinary mitigation.

### L3 / L4

Use detailed ground character to shape parcel / court / service / lane logic. Leave exact construction to Builder.

---

## 7. Surface Character Summary

For consequential candidates, lightweight summary may contain:

```json
{
  "surface_character": {
    "authority": "DERIVED",
    "dominant_character": "EXPOSED_ROCK",
    "exposed_rock_ratio": 0.68,
    "soil_bearing_surface_ratio": 0.19,
    "vegetation_cover": "SPARSE / UNVERIFIED",
    "near_surface_substrate": "UNRESOLVED",
    "raw_constraints": [
      "limited visible soil-bearing ground",
      "water storage / source unresolved"
    ],
    "mitigation_options": [
      "well / cistern investigation",
      "external food supply",
      "preserve soil pockets"
    ],
    "residual_constraints": [
      "local agriculture remains uncertain"
    ],
    "uncertainty": [
      "fertility unknown",
      "water table unknown",
      "rock quality unknown"
    ]
  }
}
```

Do not invent ratios / mitigation feasibility if evidence cannot support them.

---

## 8. Surface-conditioned capacity

Conceptually:

```text
terrain geometry
+ surface / substrate / cover
+ ordinary adaptation
+ external supply
+ water / access evidence
+ livelihood / service demand
→ effective capacity hypothesis
```

Important rules:

> **Low-slope area must not be used as a carrying-capacity proxy without relevant land character.**

> **Bare rock may weaken local soil-based subsistence without making residence impossible.**

> **A scarce soil-bearing terrace may deserve preservation even when it is geometrically ideal for building.**

Do not impose universal formulas.

---

## 9. Surface / Substrate map

When consequential, provide map / layer showing supported classes, vegetation / cover, scarce productive-ground pockets, shallow-ground evidence and uncertainty.

Keep Proposal boundaries visually separate from natural evidence.

---

## 10. Recursive handoff

If evidence is inadequate, pass targeted requirements through `DOWNSTREAM_TO_RESOLVE`.

Examples:

```text
verify exposed-rock vs soil-bearing surface
inspect shallow substrate / void near proposed frontage
investigate proportionate well / storage option
verify whether wet ground needs avoidance or modest drainage
identify scarce soil / vegetation pockets before fixing built extent
```

If lower-scale evidence materially changes capacity or role **after bounded adaptation is considered**, use normal revision protocol.

---

## 11. Surface Character Necessity Test

Ask:

> If two candidates had identical elevation / slope / relief but different ground character, would plan change?

Also ask:

1. Did low slope become good farming / living land automatically?
2. Did bare rock become quarry / ore without evidence?
3. Did vegetation vanish as if land were empty?
4. Did scarce soil get consumed merely because it is flat?
5. Did surface character change capacity / route / open-space / Kit requirements where relevant?
6. Were ordinary mitigation options considered before major rejection / relocation?
7. Did Planner overcorrect by assuming every ground problem can be engineered away?
8. Are unsupported ecology / geology claims marked unknown?

---

## 12. Owner-facing principle

A useful plan should let the Owner see not only:

> where land is high / low / steep / flat

but:

> **what kind of ground it is, what people can reasonably do with it, and what limitations remain after that adaptation.**
