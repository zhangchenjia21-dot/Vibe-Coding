# regression-rubric.md

## Purpose

This reference defines how to test `minecraft-planner` without leaking answers into the benchmark prompt.

The goal is to distinguish:

- actual Skill behavior;
- model execution quality;
- tooling / evidence limits;
- task-specific judgment；
- Planner↔Builder interface-contract completeness.

Do not write the rubric back into the benchmark prompt.

---

## 1. Regression prompt discipline

A benchmark prompt should normally provide only:

- what must be planned;
- planning scale;
- allowed factual / Canon sources;
- prohibited answer-leaking sources;
- hard safety boundaries;
- required artifact class;
- output location.

Do not restate:

- Demand model;
- Actor / knowledge rules;
- mitigation algorithm;
- growth / feedback rules;
- Anti-zoning checklist;
- capacity tests;
- recursive handoff schema;
- Critic questions;
- Planner–Builder shared-contract checklist.

Those must come from the Skills themselves.

---

## 2. Independent review principle

Model self-report is evidence, not verdict.

Independent review should inspect:

- source register;
- planning logic;
- machine-readable objects;
- maps;
- downstream packages;
- Critic;
- factual consistency with allowed evidence;
- Owner spatial / worldbuilding experience.

For Planner→Builder integration, also inspect:

- which BDP fields Builder actually consumed;
- whether it reread forbidden upstream context;
- whether planning-fixed interfaces were preserved;
- whether missing handoff data was reported instead of guessed.

A clean JSON package does not prove good planning or good integration.

---

## 3. Finding classes

Use:

- `SKILL_GAP`
- `HANDOFF_CONTRACT_GAP`
- `MODEL_EXECUTION_FAILURE`
- `TOOLING_LIMITATION`
- `TASK_SPECIFIC_JUDGMENT`
- `UNKNOWN`

Do not modify Skill merely because one weaker model executes it poorly.

Repeated failure across capable models or a missing foundational contract is stronger evidence.

---

# 4. Core planning audit dimensions

## Authority / Evidence discipline

Check:

- Observed / Derived / Canon / Assumption / Proposal are distinguished;
- uncertainty and freshness are preserved;
- coarse terrain / surface is not treated as exact geometry;
- current-world unknowns are not invented;
- Planning Proposal is not promoted to Canon.

## Planning Context correctness

Check:

- historical maturity and current fabric observation are separate;
- unknown existing fabric is not Greenfield;
- Existing Evolution respects inherited fabric when observed;
- unknown current fabric generates investigation, not demolition assumptions.

## Agency / Rights

Check:

- important spatial changes have plausible actors;
- actor interests / authority / rights matter where they should;
- physical possibility is not automatically social permission;
- public / common / private access is not silently flattened;
- conflict / veto / cooperation is considered when materially consequential.

Do not require Actor tables for trivial decisions.

## Bounded Knowledge

Check:

- Planner knowledge is not automatically historical actor knowledge;
- discovery / information spread is modeled when it changes growth;
- early actors do not target future resources / borders / markets they could not know.

## Premise quality

Check:

- why people / institutions persist there;
- economy / society / terrain / rights actually change planning;
- actors / capabilities exist to make important changes;
- premise is not just “task asked for a town”.

## Human Adaptation / Constraint Transformation

Check:

- environmental conditions are not treated as static suitability scores;
- proportionate period-appropriate mitigation is considered before major rejection / relocation;
- small ordinary works are not treated as extraordinary blockers;
- disproportionate megaprojects are not invented to rescue preferred sites;
- residual constraints remain visible after mitigation.

## Demand quality

Check:

- needs come from social / institutional / productive pressures;
- Demand ≠ one building each;
- magnitude / frequency / throughput matter;
- embedded / shared / later-dedicated responses are considered.

## Metabolism quality

Check:

- important flows include stock / buffer / replenishment rhythm when spatially consequential;
- seasonal / periodic peaks affect yard / storage / access where relevant;
- average demand is not used to erase event / harvest / caravan peaks;
- no unnecessary numerical inventory simulation is invented.

## Flow / Externality quality

Check:

- people / goods / authority / waste / ritual flows identified where relevant;
- routes follow flows and effective accessibility;
- externalities explain adjacency / separation;
- modern zoning is not used as shortcut.

## Effective Accessibility

Check:

- terrain resistance is not the only access factor;
- tenure / legal rights, permission, security, seasonality and transport mode are considered when consequential;
- a geometric line is not falsely certified as usable route;
- catchments do not rely only on Euclidean distance.

## Anchor quality

Check:

- Anchor has causal role;
- timing matters;
- morphology depends on it;
- human-created infrastructure may become later Anchor;
- Anchor Removal Test produces consequences.

## Growth / Path Dependence

Check:

- growth is not decorative chronology;
- important stages have actor / knowledge / choice when needed;
- each stage stands alone without future teleology;
- later form inherits earlier decisions;
- inherited suboptimal form is allowed when historically plausible.

## Feedback

Check:

- important infrastructure / access / density changes later conditions;
- reinforcing / balancing loops are recognized where they materially change growth;
- feedback is not treated as deterministic destiny.

## Terrain necessity

Check terrain actually changes route / node / density / capacity; flat-map translation should require redesign when terrain-driven.

## Surface Character necessity

Check:

- low slope is not high carrying capacity by default;
- exposed rock does not become quarry / ore automatically;
- soil / vegetation differences change planning where they should;
- surface is a raw condition, not automatic blocker;
- ordinary adaptation is considered before rejecting difficult ground.

## Settlement hierarchy / catchment

Check:

- node rank is not merely population ordering;
- political / economic / symbolic / network roles can diverge;
- catchments reflect effective transport / rights / competition / seasonality;
- nodes are not scattered merely to fill map.

## Settlement capacity / built-fabric scale

Check:

- important nodes communicate approximate scale;
- `search envelope ≠ built fabric ≠ catchment`;
- area ranges have drivers;
- political significance separated from physical size;
- freight importance separated from resident scale;
- confidence matches evidence;
- no fake exact area;
- natural limitation is not automatically effective-capacity collapse;
- affordable adaptation and external supply are considered where justified;
- stock / peak / resilience needs change land demand where relevant.

## Resilience

Check:

- network does not optimize itself into unjustified single points of failure;
- fallback route / water / storage / local service exists when failure consequence justifies it;
- redundancy has a real vulnerability reason;
- every facility is not duplicated mechanically.

## Morphology / Site Value / Parcels / Density

At applicable scales check:

- district relations are not modern zoning;
- relative site value explains competition where relevant;
- access / prestige / throughput / externalities / rights affect frontage;
- household formation / inheritance / migration can explain subdivision / infill where relevant;
- parcels have formation logic;
- density is morphological, not just more objects.

## Architecture Kit requirements

Check:

- Planner asks for vocabulary / typologies / adaptation capabilities;
- exact engineering / building geometry remains Builder work;
- Kit does not become prefab cloning.

## Scale Discipline

Check:

- L0 does not solve streets;
- L1 does not solve parcels;
- L2 does not solve facade / exact engineering;
- L3/L4 progressively become concrete;
- lower-scale unknowns remain intentionally unresolved.

## Recursive Planner→Planner handoff

For L0–L3 check:

- recipient is next Planner scale;
- `UPSTREAM_FIXED` contains only meaningful parent constraints;
- `DOWNSTREAM_TO_RESOLVE` contains real lower-scale questions;
- `DOWNSTREAM_ADAPTABLE` preserves child freedom;
- revision triggers exist;
- capacity / search / catchment remain separate;
- Actor rights / epistemic / mitigation / access / metabolism dependencies are handed down when consequential;
- parent rollback happens only when bounded child adaptation is insufficient;
- Builder-specific fields do not dominate.

## Planner→Builder handoff｜v0.5

At Builder-ready scales check:

- package has clear WHY / role / program;
- `PLANNER_FIXED` is small but meaningful;
- `BUILDER_ADAPTABLE` preserves Architecture Design authorship;
- Builder handoff readiness (`CONCEPT_DESIGN_READY / DESIGN_FREEZE_READY / INCOMPLETE_HANDOFF`) matches actual package completeness;
- every planning-fixed cross-package physical interface required for the next Builder stage has a local Interface Baseline or direct immutable ref;
- interface geometry semantic is explicit;
- topographically sensitive interfaces include enough local elevation / section control;
- nominal width is not confused with minimum clear requirement;
- adjustable edge / threshold range is explicit where relevant;
- public / common / private rights survive into the package;
- continuous / diagonal planning boundaries declare Minecraft discretization semantics where voxel legality depends on them;
- external water / drainage / waste / loading / service relationships declare responsibility rather than being left for Builder to invent;
- uncertainty carries `resolve_before` when stage timing matters;
- package / interface revision lineage is visible;
- world-write is not implicitly authorized.

A BDP can be semantically excellent yet still fail design-freeze readiness if a fixed public interface is only named rather than geometrically resolvable.

## Visual planning evidence

Check:

- maps use real coordinates / terrain context;
- Observed / Proposal / Assumption legible;
- scale / north / legend exist;
- node sizes / envelopes communicate capacity;
- search uncertainty not confused with built extent;
- catchment not urbanized land;
- conditional / unresolved access is not drawn like certified road;
- surface / rights / adaptation layers appear when they materially change Owner understanding;
- Builder-facing interface slices are readable when design freeze depends on them.

---

# 5. Strong Critic tests

## Counterfactual Test

Change major geographic / institutional / Actor / access condition. Would plan materially change?

## Anchor Removal Test

Remove one Anchor. Which flows / density / capacity / routes lose reason?

## Historical Validity Test

If later stages never occur, is each stage still viable?

## Anti-Zoning Test

Hide land-use labels. Does structure still make sense from flow / rights / adjacency / externality / history?

## Terrain Necessity Test

Move plan to generic flat terrain. How much must change?

## Surface Character Necessity Test

Hold geometry constant, change rock / soil / vegetation / wetness. Does relevant planning change?

## Agency Test

Who causes / permits the important change, who bears burden, who can resist?

## Knowledge Test

Is a historical actor using Planner-only information?

## Mitigation Test

Before a raw constraint blocks / relocates / radically shrinks the plan, were proportionate period-appropriate adaptations considered? Was disproportionate engineering avoided?

## Metabolism & Resilience Test

Do important flows have timing / buffers? Is there an unjustified single point of failure?

## Feedback Test

Do major choices modify next-stage conditions?

## Capacity Plausibility Test

Where is search area? Built scale? Catchment? Why that size? What adaptation / supply assumptions matter? How uncertain?

## Parcel Causality Test

Do rights, competition, households, inheritance, migration or frontage pressure actually explain parcel form?

## Recursive Handoff Test

Can child Planner continue intelligently without redoing parent or blindly obeying frozen masterplan?

## Builder Interface Completeness Test

Can Builder preserve every planning-fixed cross-scope relation using only the target BDP plus its direct immutable interface refs, without reading the complete upstream plan or guessing geometry / responsibility?

If not, classify whether the issue is:

- `HANDOFF_CONTRACT_GAP`;
- task-specific missing evidence;
- or model execution failure.

## Kit Clone Test

Is Kit a language or copy machine?

---

# 6. Suggested regression ladder

### P01 POLITY_TERRITORY

Tests whole polity structure, hierarchy, long-distance flows, capacity scale, L0→L1.

### P02 REGIONAL_SYSTEM

Tests regional network, local evidence, surface, effective access, capacity refinement, L1→L2.

### P03 SETTLEMENT

Tests complete settlement morphology, anchors, movement, adaptation, built-fabric envelope, L2→L3.

### P03M MITIGATION / HUMAN-GEOGRAPHY MICRO-REGRESSION

Useful after foundational kernel changes.

Use a real or synthetic site with several ordinary constraints such as:

- weak surface-water evidence;
- shallow void;
- slope / terrace;
- bare rock / scarce soil;
- external food dependence;
- alternate access.

Do **not** tell model how to solve them. Test whether it distinguishes:

- ordinary mitigable constraint;
- residual operating / capacity constraint;
- effective-access issue;
- genuine upstream blocker.

Also inspect Actor / knowledge / stock / resilience reasoning where relevant.

### P04 DISTRICT / EXISTING_EVOLUTION

Tests inherited routes / parcels, rights / easements, site-value competition, household / inheritance logic, infill, L3→L4.

### P05 URBAN_ENSEMBLE

Tests multiple-building relationships, frontage / service / courtyard, final Planner→Builder handoff.

### I01 PLANNER→BUILDER INTEGRATION

Use one P05 Builder Design Package with `minecraft-builder`.

Prompt should tell Builder which BDP to consume and forbid full upstream reread. Do **not** remind it of the expected WHY / rights / access / fixed fields.

Audit:

- did Builder actually consume BDP causal context?
- did architecture differ because of planning role / site / flow?
- were PLANNER_FIXED relations preserved?
- did Builder retain architectural authorship?
- were missing interface data reported rather than guessed?
- did Builder avoid reading forbidden upstream plans?

### I01R CONTRACT REGRESSION

After handoff-contract changes, rerun the same architectural scope with a regenerated BDP.

Primary question:

> **Can the new package reach `DESIGN_FREEZE_READY` and pass Builder Planning Fidelity Gate without full upstream reread?**

If yes, the integration fix is validated.

Additional cases can cover ford / bridge, mountain service center, monastic agriculture, port / floodplain and existing-town evolution.

---

# 7. No pass-seeking

During regression:

- no mid-test corrective hints;
- no independent-review criteria leakage;
- no explanation of previous failures;
- no prompt copy of Skill;
- no patching omissions during execution.

Omissions are evidence.

---

# 8. Owner review role

Owner should especially judge:

- whether polity / settlement feels believable;
- whether scale is understandable;
- whether human choices feel plausible rather than environmentally deterministic;
- whether modest constraints are treated with common-sense adaptation;
- whether history feels like bounded people acting, not omniscient masterplanning;
- whether routes / commons / parcels feel socially possible;
- whether Builder architecture still feels like the same place / social system Planner described;
- whether world feels worth building.

---

# 9. Skill update policy

Do not update Skill after every minor failure.

Use stronger evidence when:

- foundational contract missing;
- same failure recurs;
- capable model cannot infer required behavior because Skill ambiguous;
- missing distinction causes misleading downstream work.

Task-specific planning disagreements normally remain review findings.

---

# 10. v0.5 regression focus

v0.5 retains v0.4's four foundational human-geography kernels:

1. **Agency & Bounded Knowledge**
2. **Human Adaptation & Effective Accessibility**
3. **Metabolism & Resilience**
4. **Competition, Demography & Feedback**

and adds a fifth regression concern:

5. **Planner→Builder Interface Completeness**

The primary v0.5 question is:

> **Can Planner hand Builder enough local causal, spatial, rights and interface information to preserve planning intent and freeze Architecture Design without either over-specifying architecture or forcing a full upstream reread?**

Explicitly out of scope by default:

- infrastructure lifecycle / replacement simulation;
- resource depletion / regeneration simulation;
- full monetary economy;
- political or demographic microsimulation;
- heavyweight BIM / GIS exchange formats.
