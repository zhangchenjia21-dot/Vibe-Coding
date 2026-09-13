# regression-rubric.md

## Purpose

This reference defines how to test `minecraft-planner` without leaking answers into the task prompt.

The goal is to distinguish:

- actual Skill behavior;
- model execution quality;
- tooling / evidence limits;
- task-specific judgment.

Do not write the rubric back into the benchmark prompt.

---

## 1. Regression prompt discipline

A benchmark prompt should normally provide only:

- what must be planned;
- planning scale;
- allowed factual / Canon sources;
- prohibited answer-leaking sources;
- hard safety boundaries;
- required evidence / artifact class;
- output location.

Do not restate:

- Demand model;
- Anchor rules;
- Growth algorithm;
- Anti-zoning checklist;
- Capacity tests;
- Surface / substrate checklist;
- Recursive handoff schema;
- Critic questions.

Those must come from the Skill itself.

---

## 2. Independent review principle

Model self-report is evidence, not verdict.

Independent review should inspect:

- actual source register;
- planning logic;
- machine-readable objects;
- maps;
- downstream packages;
- Critic;
- factual consistency with allowed evidence;
- Owner visual / worldbuilding experience.

A clean JSON package does not prove good planning.

---

## 3. Finding classes

Use the same general classes as Builder regression where useful:

- `SKILL_GAP`
- `MODEL_EXECUTION_FAILURE`
- `TOOLING_LIMITATION`
- `TASK_SPECIFIC_JUDGMENT`
- `UNKNOWN`

Do not modify Skill merely because one weaker model executes it poorly.

Repeated failures across capable models or foundational contract failures are stronger Skill evidence.

---

## 4. Core planning audit dimensions

### Authority / Evidence discipline

Check:

- Observed / Derived / Canon / Assumption / Proposal are distinguished;
- uncertainty is preserved;
- coarse terrain is not treated as exact site geometry;
- surface observations are not overclaimed as fertility / quarry / ore / hydrology;
- current-world unknowns are not silently invented;
- Planning Proposal is not promoted to Canon.

### Planning Context correctness

Check:

- historical maturity and current fabric observation are separate;
- `EXISTING_FABRIC_UNVERIFIED` is not mislabeled as empty Greenfield;
- Existing Evolution respects inherited fabric when observed;
- unknown current fabric generates downstream investigation rather than demolition assumptions.

### Premise quality

Check:

- why people / institutions persist there;
- economy / society / terrain actually change planning;
- premise is not just “the task asked for a town”.

### Demand quality

Check:

- needs come from real social / institutional / productive pressures;
- Demand ≠ one building each;
- magnitude / frequency / throughput matter;
- embedded / shared / later dedicated responses are considered.

### Flow / Externality quality

Check:

- major people / goods / authority / waste / ritual flows are identified where relevant;
- roads / nodes follow flows;
- externalities explain adjacency / separation;
- functional zoning is not used as a shortcut.

### Anchor quality

Check:

- Anchor has causal role;
- timing matters;
- surrounding morphology depends on it;
- Anchor Removal Test produces consequences.

### Growth / Path Dependence

Check:

- growth is not decorative chronology;
- each step has driver → response → new constraint → next pressure;
- early stages stand alone;
- later form inherits earlier decisions;
- contraction / decline is possible where relevant.

### Terrain necessity

Check:

- terrain actually changes route / node / density / capacity;
- a flat-map translation would require meaningful redesign;
- major routes are not falsely certified from coarse samples.

### Surface / Substrate / Land-Cover necessity

When land character is consequential, check:

- surface / substrate / vegetation evidence is actually read or explicitly marked unresolved;
- low slope is not silently equated with good living / farming / building land;
- exposed rock changes carrying-capacity / livelihood / morphology reasoning where appropriate;
- scarce soil-bearing or vegetated pockets can be protected from built-fabric expansion;
- forest / wet / sand / gravel / barren differences are not ignored when materially relevant;
- grass ≠ fertility, stone ≠ quarry / ore, forest ≠ timber yield;
- Owner can see the land-character distinction in map / summary when it matters.

### Settlement hierarchy / catchment

Check:

- node rank is not merely population ordering;
- political / economic / symbolic / network roles can diverge;
- catchments reflect transport / terrain / competition;
- nodes are not mechanically scattered to fill the map.

### Settlement capacity / built-fabric scale

Check:

- important nodes communicate approximate built scale, not just points;
- `search envelope ≠ built fabric ≠ catchment`;
- area ranges have causal drivers;
- political significance is separate from physical size;
- freight importance is separate from resident scale;
- confidence matches evidence;
- no fake exact area is claimed from weak evidence;
- map visually communicates relative settlement magnitude;
- capacity does not use low-slope area as a stand-in for carrying capacity when surface character matters.

### Morphology / parcels / density

At scales where applicable, check:

- district relations are not modern zoning;
- parcels have formation logic;
- frontage / rear access / shared courts follow pressure and history;
- density is morphological, not just more objects.

### Architecture Kit requirements

Check:

- Planner asks for needed vocabulary / typologies;
- terrain / surface adaptation requirements may be stated without choosing exact palette;
- it does not design finished component geometry;
- Kit does not become prefab cloning.

### Scale Discipline

Check:

- L0 does not solve street corners;
- L1 does not solve parcel widths;
- L2 does not solve façade details;
- L3/L4 may progressively become concrete;
- lower-scale unknowns remain intentionally unresolved.

### Recursive Planner→Planner handoff

For L0–L3, check:

- recipient is the next Planner scale, not Builder by default;
- `UPSTREAM_FIXED` contains only meaningful parent constraints;
- `DOWNSTREAM_TO_RESOLVE` contains real lower-scale questions;
- `DOWNSTREAM_ADAPTABLE` preserves child planning freedom;
- revision triggers exist;
- capacity hypothesis is handed down separately from search / catchment;
- consequential surface / substrate uncertainty is handed down when unresolved;
- new evidence has an upstream issue protocol;
- Builder-specific fields do not dominate Planner→Planner packages.

### Planner→Builder handoff

At Builder-ready scales, check:

- fixed planning relations are clear;
- Builder retains architectural authorship;
- package boundaries / shared interfaces are implementable;
- world-write is not implicitly authorized.

### Visual planning evidence

Check:

- maps use real coordinates / terrain context;
- Observed / Proposal / Assumption are legible;
- scale / north / legend exist;
- node sizes / envelopes communicate morphology and capacity;
- search uncertainty is not visually confused with built extent;
- catchment is not shown as urbanized land;
- consequential surface / land-cover differences are visually understandable;
- visual output helps Owner understand the plan without reading every JSON field.

---

## 5. Strong Critic tests

### Counterfactual Test

Change a major geographic / institutional condition.

Question:

> Would the plan materially change?

If not, causal reasoning may be decorative.

### Anchor Removal Test

Remove one Anchor conceptually.

Question:

> Which flows / density / capacity / routes lose their reason?

### Historical Validity Test

For each stage:

> If later stages never occur, is this stage still viable?

### Anti-Zoning Test

Hide land-use labels.

Question:

> Does spatial structure still make sense from adjacency / flow / externality / history?

### Terrain Necessity Test

Move the plan to generic flat terrain.

Question:

> How much must change?

### Surface Character Necessity Test

Hold elevation / slope / relief roughly constant but change the ground from soil-bearing / vegetated to exposed rock, wet ground, sand-gravel or other materially different surface.

Question:

> Would capacity, livelihood, open-space, route, morphology or Kit requirements change?

If not in a task where those conditions matter, the terrain model is incomplete.

### Capacity Plausibility Test

For each important settlement:

> Where is the search area? What is the built-fabric scale? What is the catchment? Why that size? How uncertain is it? Does surface character materially alter that estimate?

### Recursive Handoff Test

Question:

> Can the child Planner continue intelligently without either redoing the parent plan or blindly obeying a frozen masterplan?

### Kit Clone Test

Question:

> Is Kit a language or a building copy machine?

---

## 6. Suggested regression ladder

Use heterogeneous scales so the Skill does not overfit one project.

Suggested ladder:

### P01 POLITY_TERRITORY

Tests:

- whole polity structure;
- national / alliance settlement hierarchy;
- long-distance flows;
- capacity scale;
- L0→L1 handoff.

### P02 REGIONAL_SYSTEM

Tests:

- regional settlement network;
- corridor / resource chain;
- local evidence refinement;
- capacity refinement;
- **surface / substrate / land-cover differentiation**;
- L1→L2 handoff.

### P03 SETTLEMENT

Tests:

- complete town / village morphology;
- anchors / movement / districts;
- built-fabric envelope;
- ground-character response;
- L2→L3 handoff.

### P04 DISTRICT / EXISTING_EVOLUTION

Tests:

- inherited roads / parcels;
- infill / subdivision;
- mixed-use logic;
- L3→L4 handoff.

### P05 URBAN_ENSEMBLE

Tests:

- multiple-building relationships;
- frontage / service / courtyard;
- final Planner→Builder handoff.

Additional synthetic cases should cover river ford, mining mountains, monastic agriculture, port / floodplain, exposed-rock vs soil-bearing plateau, wetland / dry-ground contrast, and existing town evolution.

---

## 7. No pass-seeking

During regression:

- do not give mid-test corrective hints;
- do not reveal independent review criteria;
- do not tell the model what previous tests failed;
- do not make the prompt a copy of the Skill;
- do not patch obvious omissions during execution.

Omissions are evidence.

---

## 8. Owner review role

Independent technical review and Owner experience are complementary.

Owner should especially judge:

- whether the polity / settlement feels believable;
- whether scale is understandable from maps;
- whether important ground-character differences are visible and affect planning plausibly;
- whether major nodes feel too large / too small / too evenly distributed;
- whether the planning reads as historical growth rather than masterplanning;
- whether maps make spatial consequences intuitive;
- whether the proposed world feels worth building.

Do not reduce Owner review to coordinate correctness.

---

## 9. Skill update policy

Do not update Skill after every minor failure.

Use stronger evidence when:

- a foundational contract is missing;
- the same failure recurs across multiple tests;
- a capable model cannot infer a required behavior because the Skill is ambiguous;
- a missing distinction causes unsafe / misleading downstream work.

Task-specific planning disagreements should normally remain review findings, not universal rules.

---

## 10. v0.2 regression result

v0.2 added three foundational contracts:

1. **Existing-but-unobserved context**；
2. **Settlement Capacity / Built-Fabric Scale**；
3. **Recursive Planner→Planner Handoff**。

P01R and P02 provided evidence that these contracts can operate across L0→L1.

---

## 11. v0.3 regression focus

v0.3 adds a fourth foundational environmental contract:

> **Surface / Substrate / Land-Cover Character**

The key question is not whether the model can color a terrain map by block type. It is whether land character changes planning causally without overclaiming unsupported geology or ecology.

A strong v0.3 regression should verify that:

- geometry-identical but surface-different candidates can produce different planning consequences;
- exposed-rock plateaus do not inherit lowland food-support assumptions merely because they are flat;
- scarce soil-bearing / vegetated pockets can constrain built-fabric expansion;
- surface evidence changes capacity / morphology / downstream questions where appropriate;
- surface uncertainty is propagated through recursive handoff;
- exact Builder palette remains downstream authorship.
