# architecture-kit-requirements.md

## Purpose

This reference defines what `minecraft-planner` may require from a downstream Architecture Kit without stealing architectural authorship from `minecraft-builder`.

---

## 1. Architecture Kit is constrained vocabulary

A Kit exists to create:

> **family resemblance, not clones.**

The Planner may require a regional or settlement Kit to support certain typologies, interfaces and terrain responses. It must not define finished reusable buildings as the only valid solution.

---

## 2. Planner may specify required shared DNA

Examples:

- broad construction family;
- climate / terrain response;
- typical relationship between base / wall / upper level / roof;
- frontage behavior;
- service access behavior;
- opening hierarchy in general terms;
- level of ornament restraint;
- recurring courtyard / compound logic;
- cultural continuity across building types;
- forbidden modern / foreign combinations when Canon supports them.

These are design constraints, not block-level recipes.

---

## 3. Planner may specify required typology skeletons

Examples:

- mixed shop-house;
- short warehouse + dwelling;
- workshop-house;
- inn / food-service compound;
- small civic building;
- courtyard compound;
- farmstead;
- mining household;
- merchant courtyard;
- riverside storehouse.

A typology skeleton means:

```text
program relationships
street / yard interfaces
service logic
terrain adaptation needs
possible growth / extension behavior
```

It does **not** mean a fixed Blueprint.

---

## 4. Planner may specify required component capabilities

For example, a dense commercial region may need a Kit capable of producing:

- shopfront family;
- warehouse door family;
- canopy / awning family;
- courtyard gate family;
- service stair family;
- arcade / covered frontage family;
- loading threshold family;
- retaining / stepped entrance family.

The actual geometry, block states, proportions and architectural expression remain Builder work.

---

## 5. Planner may specify variation dimensions

Useful variation dimensions include:

- wealth;
- age;
- parcel width;
- parcel depth;
- corner / mid-block;
- slope;
- frontage importance;
- building role;
- construction phase;
- repair history;
- public vs private importance.

The Kit should make these differences possible without breaking regional identity.

---

## 6. Planner may specify forbidden mismatches

Examples:

- no isolated lawn setbacks on continuous commercial frontage;
- no giant warehouse door on a high-status ceremonial facade unless justified;
- no identical roofline repeated across every narrow plot;
- no modern detached zoning pattern in a historically mixed market street;
- no regionally foreign structural language unless there is a migration / elite / imported-technology cause.

Forbidden combinations should come from Canon / terrain / institutional logic, not personal taste alone.

---

## 7. Builder owns geometry

Planner must not lock:

- exact bay width;
- exact floor count unless planning role truly requires it;
- exact wall thickness;
- exact roof pitch;
- exact window size;
- exact door design;
- exact block palette;
- exact facade composition;
- exact structural system beyond a genuinely planning-relevant constraint.

If a Kit requirement accidentally determines nearly the entire building, it is over-specified.

---

## 8. Site adaptation outranks Kit uniformity

The Kit must bend to:

- slope;
- parcel geometry;
- corner condition;
- street hierarchy;
- drainage;
- neighboring buildings;
- building function;
- building age.

> **Regional identity should survive adaptation; adaptation should not be sacrificed to template purity.**

---

## 9. Kit evidence maturity

If the project tracks Kit maturity, useful states may include:

- `HYPOTHESIS`
- `DESIGN_BASELINE`
- `DESIGNED_AND_REVIEWED`
- `BUILT`
- `BUILT_AND_OWNER_ACCEPTED`

Planner should not treat an unbuilt v0.x Kit hypothesis as proven construction truth.

---

## 10. Kit Clone Test

Ask:

1. Would several buildings differ meaningfully if their function / site / age changed?
2. Is the Kit a set of relationships and components, or one complete house copied repeatedly?
3. Does terrain adaptation alter geometry?
4. Can a corner plot, deep plot and slope plot all remain recognizably regional without becoming identical?

If not, the Kit is too blueprint-like.
