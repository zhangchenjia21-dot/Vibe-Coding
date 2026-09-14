# flows-externalities-and-demand.md

## Purpose

This reference explains how social demand, throughput, flows, stocks, rhythms and externalities generate spatial relationships. It prevents checklist planning such as “every medieval town needs a tavern, blacksmith and church”.

v0.4 extends the model from **Flow** to **Metabolism** and adds resilience reasoning where system failure would materially change planning.

---

## 1. Demand is a social requirement, not a building name

Represent important demand as:

```text
Demand
WHY / Driver
Users / Actors
Magnitude / Throughput
Frequency / Rhythm
Maturity Stage
Spatial Dependencies
Externalities
Possible Spatial Responses
```

Examples:

```text
Need: local dispute resolution
WHY: autonomous local community
Early response: periodic meeting in an existing large room
Later response: shared court / council space
```

```text
Need: grain buffering
WHY: seasonal surplus + daily consumption + interruption risk
Possible responses: household storage / barns / shared granary / merchant stores
```

One demand may create several spatial responses; several demands may share one space or building.

---

## 2. Demand levels

Use as planning priority, not quota:

- `ANCHOR / CORE`
- `ESSENTIAL_SUPPORT`
- `ORDINARY_DAILY`
- `SPECIALIZED`
- `LATER_GROWTH`
- `OPTIONAL`

At L0 / L1, prefer territorial / settlement functions before individual buildings.

---

## 3. Magnitude and throughput

Useful qualitative or ranged measures include:

- households / resident order of magnitude;
- daily / periodic / seasonal visitors;
- freight pressure;
- market catchment;
- institutional importance;
- production volume;
- livestock / caravan / ship frequency;
- defensive staffing pressure.

Magnitude must affect space.

---

## 4. Flow families

### People

residents / workers / visitors / pilgrims / officials / soldiers / traders / seasonal labor.

### Goods

food / livestock / timber / stone / ore / fuel / manufactured goods / luxury goods / waste / by-products.

### Environmental / utility

water / drainage / sewage / waste / flood movement where relevant.

### Institutional / symbolic

ritual / taxation / information / judicial access / military response / political assembly.

For each major flow identify when useful:

```text
origin
sink / destination
frequency
magnitude
time pattern
terrain / surface sensitivity
transport mode
rights / permission
sharing compatibility
security / ceremonial constraints
```

---

## 5. Metabolism = Flow + Stock + Rhythm

A settlement or regional system does not survive only through lines on a map.

For consequential systems consider:

```text
Flow
+ Stock / Buffer
+ Consumption / Use
+ Replenishment Cadence
+ Peak / Seasonal Rhythm
+ Failure Consequence
```

Examples:

```text
harvest arrives seasonally
→ grain stored for daily use
→ storage space persists near exchange / households
```

```text
caravan arrives periodically
→ short intense animal / loading / lodging peak
→ yard and frontage sized for peak, not average day
```

```text
water source is intermittent
→ storage / multiple collection points may buffer use
→ spatial demand differs from continuous stream access
```

Do not simulate inventories numerically unless evidence and task require it.

---

## 6. Time / rhythm classes

Useful classes:

- continuous / daily;
- periodic / market-day-like;
- seasonal;
- annual / harvest-linked;
- event / pilgrimage / assembly;
- emergency / reserve.

A space can be mostly empty most days and still be spatially essential.

Average occupancy is not the only sizing logic.

---

## 7. Flow creates route hierarchy through effective accessibility

Do not draw roads first.

Derive route choice from:

```text
origin → destination → magnitude
→ physical resistance
→ legal / tenure access
→ political permission
→ security
→ seasonality
→ transport mode
→ effective route
```

A path may become primary because many modest flows overlap, not because it is labeled “main road”.

A route can decline when access, anchor, trade or knowledge changes.

---

## 8. Buffers and storage are spatial objects

Stocks / waiting / transfer may create:

- household storage;
- distributed barns;
- granaries / storehouses;
- cistern / reservoir / water court;
- caravan / loading yard;
- livestock holding space;
- reserve depot;
- sheltered waiting / transfer area.

Demand still does not imply one dedicated building. Choose distributed / embedded / shared / institutional response according to scale and actors.

---

## 9. Externalities

Common externalities:

- smoke;
- heat;
- noise;
- fire risk;
- smell;
- contamination;
- crowding;
- flood / erosion;
- security risk;
- animal traffic;
- dust;
- sacred restriction;
- ceremonial visibility;
- defensive clear field.

Externalities create trade-offs, not automatic modern zoning.

Mitigation can change residual externality, but do not assume every externality disappears through engineering.

---

## 10. Adjacency reasoning

Useful labels:

- `MUST_ADJOIN`
- `PREFER_NEAR`
- `SHARE_ACCESS`
- `SHARE_COURTYARD`
- `NEEDS_FRONTAGE`
- `NEEDS_REAR_SERVICE`
- `BUFFER_FROM`
- `SEPARATE_FLOW`
- `VISUAL_RELATION`
- `CAN_EMBED_IN`

Do not make every relationship hard. Historical settlements tolerate imperfect adjacency.

---

## 11. Mixed use is often the default historical condition

Residence, commerce and production need not be separated.

Possible embedded relationships:

- shop + dwelling;
- workshop + dwelling;
- inn + stable + family rooms;
- merchant house + short warehouse;
- monastery + agriculture + lodging + production;
- manor + administration + storage + household.

Ask whether standalone specialization is justified by scale, wealth, regulation, maturity, externality or ownership.

---

## 12. Land tenure and Actor rights change demand response

The same pressure may produce different morphology under different rights.

### Family hereditary parcels

- inheritance subdivision;
- frontage competition;
- rear extension;
- shared alleys.

### Guild / institutional holdings

- larger persistent compounds;
- controlled frontage;
- shared courtyards;
- resistance to subdivision.

### Manor / estate control

- clustered tenant settlement;
- dominant service yard;
- common fields;
- weaker private parcel expression.

If rights / tenure are unknown and consequential, preserve branches rather than inventing a frictionless land market.

---

## 13. Resilience｜reasoned redundancy

The lowest-cost network may create dangerous single points of failure.

Where consequence justifies it, resilience may support:

- secondary route / crossing;
- multiple wells / water points;
- distributed grain / fuel storage;
- multiple local service centers;
- partial local supply;
- seasonal fallback mode.

Ask:

```text
What failure would make this system nonfunctional?
How severe is the consequence?
Is a bounded fallback spatially justified?
```

Do not duplicate every facility “for redundancy”.

---

## 14. Demand / Metabolism anti-checklist test

Before accepting the model, ask:

1. Could any demand be embedded or shared rather than create a new building?
2. Does each high-priority demand have real users / actors?
3. Does magnitude justify its spatial scale?
4. Does rhythm / peak alter the required space?
5. Do stocks / buffers matter for survival or exchange?
6. Are flows changing placement and route hierarchy?
7. Are rights / access conditions ignored?
8. Are externalities changing adjacency?
9. Is a single point of failure being mistaken for elegant efficiency?
10. Would the same model appear unchanged in a completely different terrain / economy / rights system?

If yes to the last question, the model is probably too generic.
