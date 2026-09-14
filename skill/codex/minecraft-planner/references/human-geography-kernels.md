# human-geography-kernels.md

## Purpose

This reference defines four foundational human-geography kernels for `minecraft-planner v0.4`.

They exist because settlement form is not produced by environment alone. People with limited knowledge, unequal rights, finite capabilities and recurring material needs choose, negotiate and modify space. Those choices then change the conditions of later choices.

The four kernels are:

1. **Agency & Bounded Knowledge**
2. **Human Adaptation & Effective Accessibility**
3. **Metabolism & Resilience**
4. **Competition, Demography & Feedback**

Use them only where they materially change spatial reasoning. Do not turn the Planner into a full economy, politics or population simulator.

---

# 1. Agency & Bounded Knowledge

## 1.1 Space does not change by itself

Important spatial changes should have plausible actors.

Possible actors include:

- households / kin groups;
- local communities;
- merchants / caravan organizers;
- guilds / workshops;
- religious institutions;
- landholders / estate owners;
- councils / courts;
- military authorities;
- rulers / state offices;
- migrant groups;
- shared commons institutions.

For consequential decisions, ask:

```text
Who wants this?
Who has authority or rights to do it?
Who bears the burden?
Who benefits?
Who can veto / resist / redirect it?
Who must cooperate?
```

A route, market, commons or district should not appear merely because it is globally efficient.

## 1.2 Rights and power are spatial mechanisms

Physical possibility does not imply political possibility.

Rights that may matter:

- ownership / tenure;
- easement / passage rights;
- market rights;
- toll rights;
- water access;
- common grazing / forestry rights;
- institutional privilege;
- sacred restriction;
- jurisdiction;
- public / shared access.

When rights are unknown and consequential, keep them unresolved rather than inventing a clean ownership model.

## 1.3 Planner knowledge is not actor knowledge

Separate evidence available to the Planner from information plausibly known by historical actors.

Useful epistemic states:

- `HISTORICALLY_KNOWN`
- `LOCALLY_KNOWN`
- `PARTIALLY_KNOWN`
- `UNDISCOVERED`
- `UNKNOWN_TO_PLANNER`
- `PLANNER_ONLY_EVIDENCE`

Do not force these labels on every fact. Use them when knowledge changes historical sequence.

Example:

```text
Planner evidence: remote mineral deposit exists
Early local actors: deposit not yet known

correct sequence:
exploration → discovery → exploitation → knowledge spreads → route strengthens

wrong sequence:
founding settlement immediately aligns a road to the future deposit
```

## 1.4 Bounded knowledge creates path dependence

Historical actors can make locally reasonable decisions that later look suboptimal.

This is valid morphology.

Do not retroactively optimize an old road, settlement or parcel merely because the Planner can see a better solution today.

---

# 2. Human Adaptation & Effective Accessibility

## 2.1 Civilizations inhabit transformed landscapes

Environmental conditions are not static suitability scores.

People may respond with period-appropriate, scale-appropriate adaptations such as:

- wells;
- cisterns / rain storage;
- drainage ditches;
- small bridges;
- ferries;
- retaining walls;
- steps / switchbacks;
- short cuts / fills;
- terraces;
- paved or stabilized roadbeds;
- quays / landing stages;
- modest canals / channels;
- walls / gates;
- shared storage or transfer facilities.

Core rule:

> **A missing natural convenience is not automatically a settlement blocker when ordinary period-appropriate adaptation can solve it at bounded, proportionate cost.**

## 2.2 Constraint Transformation Record

For a consequential constraint, reason through:

```text
Raw condition / constraint
Affected activity
Available mitigation options
Capability required
Relative intervention burden
Residual constraint after mitigation
Spatial consequence
Evidence / confidence
```

Relative intervention burden may use qualitative classes such as:

- `LOW`
- `MODERATE`
- `HIGH`
- `SYSTEMIC`

Do not invent precise costs without evidence.

## 2.3 Proportionate mitigation

Do not assume every constraint should be engineered away.

Compare the importance of the activity with the scale of intervention.

Examples:

- a small settlement digging a well: often proportionate;
- a local road building a short bridge: often proportionate if traffic justifies it;
- a tiny hamlet constructing a massive long-distance aqueduct: usually disproportionate without extraordinary cause;
- a valuable gateway building retaining walls on a constrained slope: potentially proportionate.

A plan should be rejected for a constraint only after plausible bounded adaptations are considered.

## 2.4 Residual constraints still matter

Mitigation can transform rather than eliminate a constraint.

Example:

```text
weak surface water
→ well / cistern possible
→ basic residence becomes plausible
→ large agriculture / rapid urban expansion may still be constrained
```

Thus one environmental condition may affect different activities differently.

## 2.5 Effective Accessibility

Route quality is not only terrain resistance.

Conceptually:

```text
physical access
+ legal / tenure access
+ political permission
+ security
+ season / event timing
+ transport mode compatibility
→ EFFECTIVE ACCESSIBILITY
```

Questions include:

- can people physically cross it?
- are they allowed to cross it?
- must they pay / negotiate?
- is it safe enough for the relevant user?
- does it work all year or only seasonally?
- can pedestrians use it but not carts / pack animals / heavy freight?

Do not collapse all access into one geometric shortest path.

---

# 3. Metabolism & Resilience

## 3.1 Flow is not enough

A settlement system survives through flows **and** temporary stocks / buffers.

For consequential subsistence, trade or utility systems consider:

```text
Flow
Stock / buffer
Consumption / use
Replenishment cadence
Peak / seasonal rhythm
Failure consequence
```

Examples:

- grain arrives seasonally but is consumed daily;
- firewood may arrive in batches;
- caravan trade creates periodic peaks;
- water storage can buffer irregular supply;
- livestock markets create episodic crowding.

Do not require quantitative simulation when qualitative rhythm is enough.

## 3.2 Time pattern creates space

Useful rhythm classes include:

- continuous / daily;
- periodic / weekly-like;
- seasonal;
- annual / harvest-linked;
- event / pilgrimage / assembly;
- emergency / reserve.

A space may be mostly empty during ordinary days and essential during peak events.

Therefore average demand alone does not determine spatial size.

## 3.3 Buffers can create facilities and commons

Stocks / waiting / storage may generate:

- granaries / storehouses;
- water storage;
- caravan yards;
- loading courts;
- livestock holding;
- reserve depots;
- sheltered waiting / transfer space.

Demand still does not imply one dedicated building. Buffers may be household, distributed, shared or institutional.

## 3.4 Efficient network ≠ resilient network

The lowest-cost network may create dangerous single points of failure.

Where consequences justify it, resilience may support:

- a secondary route;
- alternate crossing;
- multiple wells / water points;
- distributed storage;
- multiple local service centers;
- partial local self-support;
- fallback seasonal operation.

This is **reasoned redundancy**, not random duplication.

Ask:

> What failure would make this settlement or network nonfunctional, and is a bounded fallback justified?

Do not add redundancy everywhere. It must answer a credible vulnerability.

---

# 4. Competition, Demography & Feedback

## 4.1 Relative site value

Land is not equally valuable to all activities.

Qualitative site value may be shaped by:

```text
accessibility
+ visibility
+ throughput
+ prestige / ritual importance
+ externalities
+ rights / tenure
+ available space
→ relative site value
```

Different actors may value the same location differently.

Do not convert this into fake monetary rent unless the world provides such data.

## 4.2 Spatial competition

Where demand exceeds scarce valuable frontage / ground, plausible outcomes include:

- parcel subdivision;
- narrower frontage;
- higher coverage / verticality;
- rear extension;
- displacement of low-value uses;
- shared access / courts;
- institutional resistance to subdivision;
- emergence of secondary frontage / centers.

This mechanism should explain density rather than merely label it.

## 4.3 Demographic reproduction changes parcels

Households are not static counts.

Relevant processes may include:

- household formation;
- inheritance division;
- household merger;
- migration / newcomers;
- seasonal labor becoming permanent;
- dependents / apprentices / lodgers;
- household relocation.

At L2–L4 these processes may generate:

- parcel splitting;
- rear infill;
- shared yards;
- new lanes;
- satellite clusters;
- lodging becoming permanent residence.

Do not run a population simulator. Use these mechanisms only when they explain morphology.

## 4.4 Feedback loops

Growth is not always a one-way chain.

Important positive feedback example:

```text
bridge
→ traffic concentration
→ market activity
→ settlement growth
→ more demand for crossing capacity
→ stronger bridge / bridgehead importance
```

Important balancing feedback example:

```text
frontage intensification
→ congestion / externality
→ bypass or secondary route
→ new frontage opportunity
→ old center growth slows / changes role
```

Record major feedback where it changes the next stage.

## 4.5 Feedback does not imply deterministic destiny

A feedback loop may weaken, reverse or be interrupted by:

- rights / politics;
- competing centers;
- environmental limits;
- new information;
- changing demand;
- conflict / security;
- alternative infrastructure.

Keep historical validity: no later outcome is guaranteed.

---

# 5. Cross-kernel causal trace

For important planning decisions, a strong trace may look like:

```text
WORLD CONDITION
→ what relevant ACTORS know
→ their interests / rights / capabilities
→ demand / flow / stock pressure
→ available adaptation / negotiation
→ effective accessibility / spatial choice
→ morphology / infrastructure / anchor
→ feedback into the next world state
```

Not every decision needs all fields. Use the smallest subset that actually explains the spatial result.

---

# 6. Anti-overmodeling rule

`minecraft-planner` is not required to simulate an entire civilization.

Do **not** add by default:

- full monetary economy / rent model;
- detailed political game simulation;
- population microsimulation;
- infrastructure lifecycle / replacement schedule;
- resource depletion / regeneration simulation;
- maintenance engineering model;
- exhaustive Actor tables for trivial decisions.

The rule is:

> **Model a mechanism only when it materially changes spatial choice, morphology, capacity, hierarchy or downstream design.**

---

# 7. Core tests

## Agency Test

For an important spatial change:

> Who causes or permits it, who bears the burden, and who can resist it?

If no plausible actor exists, the spatial change may be automatic masterplanning.

## Knowledge Test

> Does a historical actor use information that only the Planner knows?

If yes, revise the historical sequence.

## Mitigation Test

> Before a constraint blocks / relocates / radically shrinks the plan, were proportionate period-appropriate adaptations considered?

Also reject disproportionate engineering used merely to rescue a preferred plan.

## Metabolism & Resilience Test

> Do important flows have plausible timing / buffer logic, and does the plan create an unjustified single point of failure?

## Feedback Test

> Do major choices / infrastructure alter the conditions of the next stage, or is growth only a decorative one-way chronology?
