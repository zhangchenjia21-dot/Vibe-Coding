# G5 Selective World Evolution v0.1 Decision

Status: **FROZEN / CURRENT CANONICAL**  
Date: 2026-09-04  
Capability: **G5-04 Event / Priority-driven World Evolution**  
Owner: GPT  

## 1. Product purpose

G5-04 exists to prove that the world can create history outside direct Player causation **without** turning the game into a constant-event simulator.

Protected product rule:

> **World Independence + Player Spotlight.**
>
> **The world may advance without the Player causing every change; the Narrative spotlight still serves the Player's current experience.**

Historical play evidence established both sides of this requirement:

- strong GM behavior that advances world situations over days, months or years is valuable;
- `event → event → event` pacing is harmful when it leaves no room for free activity, daily life, relationships or quiet scenes.

Therefore:

```text
Persistent != Fully Simulated
World Loop != every turn must contain an event
Evaluation opportunity != causal obligation
```

`hold` is a first-class correct result.

## 2. First real consumer

A durable world-evolution record is not sufficient by itself. The first real consumer is the **next ordinary GM Narrative context**.

The vertical is:

```text
current Game-local world causality
→ optional background World Evolution evaluation
→ 0 or 1 durable world event
→ later GM Context sees the current event
→ GM freely decides when/how it becomes relevant to the Player scene
```

Do not force an evolution event directly into visible Narrative as an interrupt/system announcement.

A committed evolution event is omniscient GM world truth. It is **not** automatically:

- Player knowledge;
- any actor's Knowledge Provenance;
- a mandatory immediate scene;
- a Quest/Thread/Faction object.

## 3. What G5-04 v0.1 owns

World Evolution owns **concrete world changes that need not be attributable to one stable NPC's intentional choice**.

Examples include, when causally supported by the current Game:

- weather / environment / natural processes;
- aggregate conflict or military-front movement;
- institutional, economic or social processes;
- a deadline or time-sensitive consequence ripening;
- disaster or accident;
- chain reactions from earlier Player/NPC/world consequences;
- other already-supported processes whose conditions have become ripe.

If a concrete stable NPC is intentionally deciding and acting, G5-03 Agency remains the preferred owner of that action.

Faction-related aggregate consequences may appear as world events without introducing stable Faction actor identity. This does not grant Faction-private Knowledge, agenda, action history or execution authority. A real later Faction consumer must pull those capabilities separately.

## 4. Runtime ordering

Do not reopen the closed G5-03 ordering.

For an ordinary accepted Player turn:

```text
visible free-form GM Narrative accepted
↓
existing semantic lane
↓
existing standalone Agency Selector / optional Agency Cycle
↓
that Agency opportunity reaches terminal
↓
World Evolution Evaluator gets one best-effort opportunity
↓
hold
OR
advance at most one world event
↓
optional single durable World mutation
↓
next GM continuation can consume the event
```

The Player turn is a **safe scheduling opportunity**, not the cause of the event.

Running World Evolution after Agency is deliberate:

- semantic consequences for the accepted Narrative are already current;
- same-opportunity NPC Agency actions may also become causal input;
- G5-03 selector/actor behavior does not need to be redesigned.

Opening-only GM generation does not create a G5-04 opportunity in v0.1.

No offline/wall-clock progression while the application is closed is introduced.

## 5. Priority semantics stay model-owned

The Program must not decide world importance through a universal score, taxonomy or keyword gate.

Do **not** build in v0.1:

- numeric priority / urgency scores;
- fixed `every N turns` event cadence;
- persistent pressure queue;
- generic Quest/Thread scheduler;
- random-event table as the primary model;
- universal entity/relationship/Faction ontology;
- minute-by-minute simulation.

The evaluator model receives bounded current world material and answers one question:

> **Is any already-supported world pressure/process causally ready and worth advancing now? If yes, choose the single most meaningful one; otherwise hold.**

Prompt guidance must explicitly say:

- this is not a random-event generator;
- do not invent an event merely because evaluation was requested;
- leave room for Life Loop / downtime;
- do not duplicate consequences already durably established;
- prefer one causally meaningful development over broad simulation.

The one-event ceiling is a first-version safety bound, not a semantic claim that only one thing can happen in the world.

## 6. Evaluator output contract

The evaluator uses one dedicated lightweight Provider request. It is separate from Narrative, semantic extraction and actor execution so those responsibilities remain isolated.

Conceptual response:

```json
{
  "decision": "hold | advance",
  "event": "concise world event summary",
  "effects": ["concrete durable world effect"]
}
```

Program validation:

- `decision` must be `hold` or `advance`;
- `event` is a raw bounded string, recommended maximum 512 characters;
- `effects` is an array of raw bounded strings;
- `advance` requires non-empty `event` and 1..4 valid effects;
- each effect recommended maximum 512 characters;
- `hold` creates no durable event/mutation;
- malformed/invalid/provider-failure results fail-soft to no event;
- unknown extra model fields are ignored;
- the model supplies no authoritative ID, priority score, event type or Source provenance.

No Narrative acceptance or Agency completion can depend on evaluator success.

## 7. Durable record / Program identity

Keep the current `living_world.v0.1` World document. No new SQLite table/schema/migration is required.

Add an additive collection conceptually:

```text
living_world.world_evolution_events_by_turn
```

A durable event record contains:

```text
world_evolution_id          Program-owned
opportunity_turn_index      scheduling/currentness identity
opportunity_gm_sha256       exact accepted Narrative version
evolution_base_head_id      world snapshot evaluated
materialized_at
event
effects[]
```

`opportunity_turn_index/hash` means **which accepted ordinary turn provided the safe evaluation opportunity**. It does not mean the Player caused the event.

Program derives stable identities from at least:

```text
game_id
+ opportunity_turn_index
+ opportunity_gm_sha256
+ evolution_base_head_id
```

and owns the `world_evolution_id`, mutation ID and node ID.

One matching event at most may be committed for one accepted opportunity.

Same current accepted version + matching committed event must be replay-idempotent and must not issue another evaluator request merely to rediscover it.

A `hold`/malformed/provider-failure result does not create fake durable markers. In-memory attempted-opportunity state may prevent repeat work inside the current runtime; reopening must not retroactively evaluate arbitrary historical opportunities.

## 8. Currentness / foreground authority

At evaluator start freeze:

```text
opportunity turn index
opportunity GM hash
accepted Conversation count
evolution_base_head_id = current active head
```

Before durable commit require all of:

- the same opportunity is still the latest accepted ordinary turn;
- its GM hash is unchanged;
- accepted Conversation count is unchanged;
- current active head still equals `evolution_base_head_id`;
- foreground Conversation is not generating.

Any new Player foreground attempt wins immediately: cancel/invalidate uncommitted World Evolution work.

Restore / progress switch / close also invalidates uncommitted work. Late callbacks are inert.

Already committed world events remain durable according to Timeline/Save/Restore truth.

Regenerate/correction does not require deleting stale physical history. Current consumers include an evolution event only while its opportunity turn/hash matches current accepted Conversation truth.

## 9. G5-03 terminal observability seam

The current Agency Scheduler owns one dirty opportunity but does not expose one canonical outward signal for **the whole opportunity has truly reached terminal**.

G5-04 may make one minimal observational extension to G5-03:

```text
signal opportunity_finished(result)
```

It must fire exactly once for a dirty opportunity that truly started and reaches terminal, including:

- selector returns no actors;
- selector malformed/provider failure/cancel/stale terminal;
- selected actor cycle reaches terminal;
- equivalent immediate terminal such as all selected actions already committed.

The terminal result must carry the frozen opportunity turn index + GM hash so a late older terminal cannot wake evolution for a newer turn.

This signal is observability only. Do not change:

- dirty consumption;
- semantic-terminal wake ownership;
- selector request semantics;
- foreground invalidation;
- selector 0..4 ceiling;
- actor concurrency;
- actor-private material/Knowledge/history;
- retry policy.

Application composition may use the terminal signal to call the World Evolution evaluator for the same frozen opportunity.

## 10. Evaluator input

The World Evolution model gets a bounded **GM-level world causality view**, not actor-private cognition.

### 10.1 Frozen Game-local T0 World baseline

Use only the opened Game's durable frozen World projection:

- selected World identity/material;
- exact selected Entry material if any;
- world instructions / GM instructions;
- World semantic sections relevant to the frozen T0 setup.

Do not read mutable `SourceLibrary.current` during ordinary runtime.

Do not include Player/Character private Source material merely because it exists in the Game setup.

This baseline is necessary because early gameplay may have few post-T0 semantic changes while major T0 pressures already exist.

### 10.2 Current post-T0 world material

Boundedly include:

- latest accepted Player action + GM Narrative as current scene/cycle context;
- recent current-hash semantic world changes;
- recent current-hash committed Agency actions/effects;
- recent current-hash World Evolution events.

Do not include Actor Knowledge Provenance as causal world input. Actor-private knowledge must not become world truth by leakage.

Do not build G7 retrieval/summarization infrastructure here. If the frozen world-only baseline exceeds a safe explicit bound, fail-soft to hold/no evaluation rather than re-query Source current or silently truncate into misleading authority.

## 11. Context consumer

Extend the existing World Turn context projection with a bounded section conceptually:

```text
## World Evolution Events
```

Only committed events matching current accepted opportunity turn/hash are eligible.

Project a small recent set, recommended latest 4 within the existing overall context character budget.

Context guidance must make clear:

- these are omniscient GM current-world facts;
- they are not automatically Player knowledge;
- they are not automatically actor knowledge;
- the GM may surface them when scene, information flow and pacing make them relevant.

Do not automatically create G5-02 Knowledge events from materialization.

Do not inject World Evolution directly into actor execution requests in v0.1. Actor execution remains actor-local and Knowledge-scoped.

## 12. Protected boundaries

G5-04 v0.1 MUST NOT:

- become a Narrative finalize barrier;
- require an event every turn;
- insert forced event announcements into player-visible Narrative;
- reactivate semantic `agency_candidates`;
- redesign Agency v0.3 scheduling;
- treat an intentional stable-NPC action as a generic world event when Agency owns it;
- create stable Faction actors / Faction shared Knowledge merely for symmetry;
- create universal Quest/Thread/Event/Entity/Faction schemas;
- add a new SQLite schema/table/migration solely for World Evolution;
- consult mutable Source current during gameplay;
- grant Player/actor Knowledge from omniscient event truth;
- implement offline real-time simulation;
- add UI;
- change Public d20/mechanics.

## 13. Engineering acceptance

Deterministic focused proof must cover at least:

1. `hold` → one evaluation, no mutation/event;
2. valid `advance` → exactly one Program-owned durable event mutation;
3. malformed/invalid/provider failure → fail-soft, accepted Narrative remains valid;
4. Opening does not create an evolution opportunity;
5. actual ordering is semantic → Agency terminal → World Evolution;
6. evaluator input contains frozen Game-local World baseline + current semantic consequences + same-opportunity Agency results + prior current evolution events;
7. evaluator input contains no Actor Knowledge Provenance and performs no mutable Source lookup;
8. Program identity binds exact opportunity turn/hash + base head;
9. matching committed event replay issues no second evaluation / duplicate event;
10. regenerate/correction excludes stale event from current GM context;
11. foreground start / Restore invalidates uncommitted evaluation and late callbacks cannot commit;
12. Save/reopen/Restore preserves or removes event according to normal Timeline snapshot truth;
13. next production continuation request includes current event/effects under GM-only disclosure guidance;
14. G5-03 dirty/foreground/0..4/concurrency semantics remain unchanged except terminal observability;
15. no persistent pressure queue / numeric priority platform appears;
16. `git diff --check` clean and real Provider calls = 0 for deterministic Engineering Acceptance.

## 14. Product acceptance / Owner UAT

G5-04 directly changes the core world/pacing experience. Engineering PASS is insufficient for capability closure.

After implementation + GPT Independent Review, Owner UAT is required.

At minimum test two counterposed situations:

### A — Quiet / Life Loop opportunity

No world process is causally ripe.

Expected product behavior:

- evaluator can genuinely `hold`;
- no artificial escalation is manufactured;
- the Player retains room for free activity / daily life / relationships.

### B — Real world pressure

A T0 or accumulated current pressure is genuinely ripe even without direct Player causation.

Expected product behavior:

- world can advance one credible consequence;
- the consequence survives durability/continuation;
- later GM Narrative surfaces or uses it naturally when appropriate;
- it does not read as a forced random encounter inserted because a scheduler fired.

Owner judges whether the world now feels like it **moves on its own without constantly pushing the Player around**.

## 15. Execution slice

The first implementation slice is one bounded vertical:

```text
MW-002 — Selective World Evolution Evaluator
```

Capability Anchor: `G5-04`.

Do not split off a pressure database, Faction platform, event ontology or UI task before this vertical proves a real need.
