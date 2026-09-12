# regression-rubric.md

## Purpose

This reference defines how to test and independently review `minecraft-planner` without overfitting prompts to the Skill.

The objective is to test whether the Skill changes planning reasoning, not whether an Agent can imitate a checklist.

---

## 1. Test discipline

Regression prompts should contain only:

- planning subject;
- planning scale / hard scope;
- source data / allowed evidence;
- safety boundaries;
- Skill path / version;
- required deliverables;
- evidence / archive requirements.

Do **not** restate the Skill’s internal planning method in the task prompt.

No mid-test corrective hints.

If a model omits an important causal layer, that omission is evidence.

---

## 2. Default world-write policy

Planner regression should normally use:

> `world writes = 0`

A valid test may read:

- Minecraft terrain data;
- Current Natural Atlas / surveys;
- existing buildings / roads;
- approved Canon;
- prior accepted planning hierarchy.

It should produce planning artifacts and maps only.

---

## 3. Core review dimensions

### A. Premise / Authority

Check:

- scale is appropriate;
- source authority is distinguished;
- assumptions are not disguised as Canon;
- settlement / territorial premise explains durable human presence;
- magnitude / maturity is plausible.

### B. Terrain Causality

Check:

- terrain actually changes routes / nodes / density / edges;
- slope / water / crossing / resource / hazard are not decorative background layers;
- coarse evidence is not overclaimed as precise local fact.

### C. Demand

Check:

- demands come from users / institutions / economy / security;
- demand is not a genre building list;
- demand magnitude influences specialization;
- embedded / shared facilities are considered.

### D. Flow / Externality

Check:

- important flows have origins and destinations;
- route hierarchy follows flows;
- externalities affect adjacency / separation;
- freight / public / service / ritual relations are considered where relevant.

### E. Anchors

Check:

- anchors have temporal role;
- anchors materially affect morphology;
- removal would change the plan;
- scale of anchor is appropriate.

### F. Growth / Path Dependence

Check:

- stages are causal, not merely chronological labels;
- early stages work without future knowledge;
- inherited roads / parcels / scars influence later form;
- Existing Evolution does not reset the site.

### G. Territorial / Settlement Hierarchy

At L0 / L1 check:

- node roles differ beyond population size;
- hinterland / catchment logic exists;
- long-distance flows support major centers;
- frontier / gateway / specialized nodes have reasons.

### H. Morphology

Check:

- roads are not arbitrary organic scribbles;
- districts are relation-driven, not modern zoning blocks;
- commons / negative spaces have ownership / use / environmental reason;
- settlement edge and expansion directions are explained.

### I. Parcel / Frontage

At L3 / L4 check:

- parcel width / depth / frontage have a causal mechanism;
- subdivision / amalgamation / rear access are plausible;
- mixed-use and service relationships survive;
- plot geometry is not just a regular grid unless planning institutions justify it.

### J. Density

Check:

- density is represented through morphology variables;
- “high density” does not mean uniform fill;
- local density and regional density are not confused;
- terrain / property / flow explain gradient.

### K. Architecture Kit Interface

Check:

- requirements specify vocabulary capability, not complete clones;
- regional continuity is preserved;
- site adaptation remains possible;
- unbuilt kit hypotheses are not described as proven.

### L. Builder Handoff

Check:

- packages explain WHY;
- PLANNER_FIXED is minimal and meaningful;
- BUILDER_ADAPTABLE preserves architectural authorship;
- dependencies are explicit;
- Growth Sequence and Implementation Sequence are distinct;
- Planner does not prematurely design facade / roof / palette.

### M. Visual Planning Evidence

Check:

- maps are legible;
- source vs proposal is distinguishable;
- scale / north / coordinates / legend are present;
- maps make causal relationships understandable;
- 3D massing does not over-design architecture.

---

## 4. Mandatory Critic tests

### Counterfactual Test

Change one major terrain / anchor / flow / institution variable.

Expected: meaningful spatial consequence.

Failure signal: almost identical plan regardless of changed cause.

### Anchor Removal Test

Remove major Anchor.

Expected: routes / demand / density / hierarchy lose or change rationale.

Failure signal: anchor is only a label.

### Historical Validity Test

Inspect each growth stage without future stages.

Expected: stage independently viable.

Failure signal: future-aware teleological layout.

### Anti-Zoning Test

Hide land-use labels.

Expected: spatial relations remain legible from morphology.

Failure signal: four colored functional blocks with weak causal mixing.

### Terrain Necessity Test

Transfer plan to different terrain.

Expected: terrain-driven plan needs material revision.

Failure signal: plan is effectively portable.

### Parcel Causality Test

For important parcels ask why width / depth / access exist.

Failure signal: parcel grid has no formation logic.

### Kit Clone Test

Inspect whether Kit would generate clones.

Failure signal: same whole-house template repeated with superficial material changes.

---

## 5. Finding classes

Use when helpful:

- `SKILL_GAP`
- `MODEL_EXECUTION_FAILURE`
- `TOOLING_LIMITATION`
- `TASK_SPECIFIC_JUDGMENT`
- `UNKNOWN`

Do not modify the Skill after every isolated model mistake.

Repeated / foundational issues are stronger update candidates.

---

## 6. Suggested first regression ladder

Do not hard-code these subjects into the Skill.

### P01 — Whole Polity / Territory

Use an existing terrain-rich world and approved Canon.

Test:

- L0 territorial settlement system;
- major regions;
- settlement hierarchy;
- long-distance flows;
- historical growth;
- maps / planning tree.

No world-write.

### P02 — Regional System

Choose one large region with terrain / resource contrast.

Test:

- settlement network;
- catchments;
- specialized centers;
- regional flow / processing chain;
- regional Architecture Kit Requirements.

### P03 — Complete Settlement

Plan one town / port / mining settlement from premise to districts.

Test:

- anchors;
- growth;
- movement;
- district formation;
- density / edge.

### P04 — Existing District Evolution

Provide existing roads / parcels / buildings.

Test:

- path dependence;
- infill;
- subdivision;
- scars;
- no reset-to-masterplan behavior.

### P05 — Urban Ensemble

Use a dense local area.

Test:

- parcel / frontage;
- service lane;
- shared yards;
- Builder Packages;
- local terrain / circulation integration.

---

## 7. Independent review order

Prefer reviewing in this order:

```text
source / authority
→ premise
→ causal model
→ growth
→ morphology
→ planning objects
→ maps
→ handoff packages
→ self-critic
```

Do not let a polished map override weak causal logic.

Do not let a sophisticated JSON schema override bad morphology.

Do not let a long historical essay compensate for missing downstream interfaces.

---

## 8. Owner review

Owner feedback is especially valuable for:

- whether the territorial / settlement structure feels believable;
- whether maps communicate the plan intuitively;
- whether density / hierarchy match intended world character;
- whether the plan creates useful future building opportunities;
- whether downstream packages feel appropriately bounded.

Independent technical review should preferably happen before Owner reveals detailed aesthetic or strategic feedback when regression independence matters.

---

## 9. Pass philosophy

A regression should not be judged PASS because:

- all required files exist;
- every section heading is filled;
- a map looks attractive;
- JSON validates;
- the Agent claims the Gates passed.

The core question is:

> **Did the Skill cause the model to derive a believable multi-scale spatial system from real constraints, and did it leave downstream design both constrained and genuinely free?**
