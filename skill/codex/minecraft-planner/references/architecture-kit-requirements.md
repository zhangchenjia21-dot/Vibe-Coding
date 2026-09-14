# architecture-kit-requirements.md

## Purpose

This reference defines what `minecraft-planner` may require from a downstream Architecture Kit without stealing architectural authorship from `minecraft-builder`.

v0.4 adds one important principle: the Kit may need to support **human adaptation to planning constraints**—wells, retaining, stepped access, shared storage, loading interfaces, drainage relationships—without Planner specifying exact engineering geometry.

---

## 1. Architecture Kit is constrained vocabulary

A Kit exists to create:

> **family resemblance, not clones.**

Planner may require regional / settlement vocabulary, interfaces and adaptation capabilities. It must not define finished reusable buildings as the only valid solution.

---

## 2. Planner may specify required shared DNA

Examples:

- broad construction family;
- climate / terrain / ground response;
- typical base / wall / upper / roof relationship;
- frontage behavior;
- service access behavior;
- opening hierarchy in general terms;
- ornament restraint / hierarchy;
- courtyard / compound logic;
- cultural continuity across building types;
- forbidden modern / foreign combinations when Canon supports them.

These are design constraints, not block recipes.

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
- riverside storehouse;
- shared water / storage court where planning logic requires it.

A typology skeleton means:

```text
program relationships
street / yard interfaces
service logic
terrain / ground adaptation needs
possible growth / extension behavior
```

It does **not** mean a fixed Blueprint.

---

## 4. Planner may specify required adaptation capabilities

Human adaptation is a planning input, but exact engineering remains Builder work.

Planner may require the Kit / Builder system to be capable of responding to:

- exposed rock / shallow soil;
- retaining / stepped terrain;
- well / cistern / shared water court relation;
- drainage separation;
- small bridge / ferry / landing interface;
- loading / unloading threshold;
- stabilized road / yard transition;
- protected scarce soil-bearing ground;
- shallow-void avoidance / spanning requirement at planning level;
- shared storage / buffer space;
- seasonal / fallback use.

Planner should phrase this as **required capability / relationship**, not exact structure.

Good:

> The ensemble must support a shared well or equivalent bounded water-supply interface if L3 confirms that this is the selected mitigation.

Bad:

> Build a 3×3 stone well 12 blocks deep at X/Z with this exact block palette.

---

## 5. Planner may specify required component capabilities

Examples:

- shopfront family;
- warehouse door family;
- canopy / awning family;
- courtyard gate family;
- service stair family;
- arcade / covered frontage family;
- loading threshold family;
- retaining / stepped entrance family;
- water / drainage interface family where planning requires it.

Actual geometry, block states, proportions and architectural expression remain Builder work.

---

## 6. Planner may specify variation dimensions

Useful dimensions:

- wealth;
- age;
- parcel width / depth;
- corner / mid-block;
- slope / ground character;
- frontage importance;
- access mode;
- building role;
- construction phase;
- repair history as visual / architectural variation when already part of world history;
- public vs private importance;
- Actor / institution type where Canon supports difference.

The Kit should make variation possible without breaking regional identity.

---

## 7. Planner may specify forbidden mismatches

Examples:

- no isolated lawn setbacks on continuous commercial frontage;
- no giant warehouse door on ceremonial frontage without cause;
- no identical roofline repeated across every narrow plot;
- no modern detached zoning pattern in historically mixed street;
- no regionally foreign structural language without migration / elite / imported-tech cause;
- no universal mega-platform that erases meaningful terrain / ground differences;
- no water / bridge / access solution grossly disproportionate to settlement scale unless Canon justifies it.

Forbidden combinations should come from Canon / terrain / Actor / access / institution logic, not taste alone.

---

## 8. Builder owns geometry

Planner must not lock:

- exact bay width;
- exact floor count unless planning role truly requires it;
- exact wall thickness;
- exact roof pitch;
- exact window / door geometry;
- exact block palette;
- exact facade composition;
- exact structural system;
- exact well / bridge / retaining / drainage geometry;
- exact foundation solution beyond a planning-relevant constraint.

If a Kit requirement determines nearly the entire building or engineering detail, it is over-specified.

---

## 9. Site adaptation outranks Kit uniformity

The Kit must bend to:

- slope / terrace;
- surface / substrate;
- parcel geometry;
- corner condition;
- street hierarchy;
- drainage;
- neighboring buildings;
- building function;
- access / loading mode;
- shared rights / commons interface;
- building age / inherited fabric.

> **Regional identity should survive adaptation; adaptation should not be sacrificed to template purity.**

---

## 10. Planning mitigation does not equal mandatory architecture

A Planner may identify several proportionate mitigation options.

Example:

```text
water constraint
possible planning responses:
- well
- cistern / rain capture
- short-distance carried supply
```

Unless the planning scale has evidence to select one, the Kit should preserve the capability space rather than freeze one exact solution.

---

## 11. Kit evidence maturity

Useful states may include:

- `HYPOTHESIS`
- `DESIGN_BASELINE`
- `DESIGNED_AND_REVIEWED`
- `BUILT`
- `BUILT_AND_OWNER_ACCEPTED`

Planner should not treat unbuilt Kit hypothesis as proven construction truth.

---

## 12. Kit Clone Test

Ask:

1. Would buildings differ meaningfully if function / site / Actor / age changed?
2. Is Kit a language or one copied house?
3. Does terrain / ground adaptation alter geometry?
4. Can corner, deep, slope and shared-court plots all remain recognizably regional without becoming identical?
5. Can the Kit support the ordinary mitigation relationships the Planner requires without forcing one prebuilt module?

If not, the Kit is too blueprint-like.
