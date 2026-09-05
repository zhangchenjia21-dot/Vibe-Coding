# G5 World Product Test Matrix v0.1 Decision

Status: **FROZEN / CURRENT**  
Phase: **G5 World Semantics & GM Runtime**  
Capability: **G5-07 World Product Tests**  
First executable Work Item: **MW-010 G5 Living-World Integrated Reality Matrix**

## 1. Product purpose

G5 now has individually proven verticals for:

- durable Narrative consequences;
- actor Knowledge Provenance;
- independent NPC Agency;
- selective World Evolution;
- mechanics-grounded consequences;
- player-safe Runtime → UI projection.

G5-07 does **not** add another world system. Its job is to prove that these capabilities remain coherent when composed into one real Game timeline.

Protected principle:

> **Integrated reality proof before more feature growth.**

## 2. Engineering proof vs Owner product judgment

Automated/integration tests may prove:

- ordering;
- persistence/currentness;
- privacy/disclosure boundaries;
- no duplicate/replay corruption;
- Save/reopen/Restore consistency;
- cross-capability composition.

They cannot prove that the resulting RPG feels alive, well paced, readable or narratively convincing.

Therefore:

```text
MW-010 engineering matrix
→ GPT Independent Review
→ one combined Owner product UAT
→ G5-GATE decision
```

Deterministic Provider stubs are appropriate for the engineering matrix. A real Provider call is not required merely to repeat prose-quality testing already reserved for Owner UAT.

## 3. Production-first test topology

The matrix should prefer:

```text
real FinalCreate Game
+ real CurrentGameRuntime
+ real SQLite persistence
+ real Shell / existing production composition where relevant
+ deterministic task-owned Provider stubs for semantic/Agency/Evolution/adjudication decisions
```

Do not replace the whole application with an in-memory simulator merely to make the test convenient.

Production code diff should preferably be **zero**. If the integrated matrix exposes a real defect in an already-closed capability, do not silently repair it under MW-010. Stop, identify the owning Work Item/capability and return the blocker so GPT can route the correct Revision lineage.

## 4. Canonical integrated scenarios

### A. Player Absence / World Independence

Prove that the world can produce a current durable non-player development without treating the Player action as its causal author.

A valid deterministic scenario may use an ordinary low-impact Player turn as the scheduling opportunity, then let existing Agency and/or World Evolution produce a separate current development.

Required distinction:

```text
Player turn = safe scheduling opportunity
!= causal source of every world change
```

Also retain one quiet/hold case so the integrated test does not normalize mandatory escalation every turn.

### B. Counterfactual Propagation through current timeline

Use existing Save/Restore rather than inventing a branch simulator.

Example:

```text
Save S before causal turn
→ Path A accepted Narrative
→ A-derived semantic consequence / knowledge / Agency-or-Evolution truth
→ verify A is current
→ Restore S
→ author materially different Path B
→ verify restored-away A material is absent from current Context/player-safe projection
→ verify B may establish its own current material
```

Physical historical rows may still exist where existing architecture retains history. The requirement is **currentness isolation**, not destructive history deletion.

Do not reuse a restored-away caller-owned Public-d20 `action_id` merely to probe the known MW-007 advisory; that edge is not the purpose of G5-07.

### C. Independent Actor

Prove that a stable NPC can author a durable action through the existing G5-03 lane without the Player choosing that NPC's action.

The resulting action may enter omniscient GM continuation Context as current world reference, but it must not automatically appear in the human-player-safe side panel.

### D. Knowledge Boundary

Prove an asymmetry:

```text
NPC-only post-T0 fact
→ NPC has durable Knowledge Provenance
→ Player Character does not
→ player-safe UI does not show it
```

Then explicitly establish Player Character knowledge of the same/substantively related fact through the normal knowledge seam and prove that it may now appear.

Do not infer Player knowledge merely from World truth or GM omniscience.

### E. Mechanics → living-world consequence

Include one bounded smoke path using the existing Public d20 capability:

```text
meaningful risky Player action
→ existing Program-owned d20 resolution
→ accepted free-form GM Narrative
→ MW-006 grounding in normal semantic opportunity
→ durable consequence consistent with the mechanical truth
```

Prove no reroll/duplicate materialization across reopen. Do not redesign Public d20 or map `success/failure` directly to hardcoded world effects.

### F. Save / reopen / Restore consistency

Across the composed scenario, verify that current:

- Conversation;
- durable world consequences;
- Player/NPC Knowledge Provenance;
- independent actor action(s);
- World Evolution current event(s), when present;
- mechanics-grounded consequence;
- player-safe projection

reconstruct coherently after close/reopen and rewind coherently when restoring to an earlier Save.

## 5. G5-06 disclosure invariant remains protected

The integrated test must keep proving:

```text
Runtime / GM omniscient truth
!= human-player-safe projection
```

The side panels may show safe identity and current Player Character known facts only. Hidden Agency/Evolution/NPC knowledge/raw consequences do not become player-visible merely because the integrated test created them.

## 6. No new platform

G5-07 must not create:

- a generic scenario engine;
- a universal simulation harness that production depends on;
- new SQLite schema/tables;
- a World/Entity/Quest/Faction graph;
- a reactive event bus/ViewModel platform;
- new mechanics protocols;
- new Narrative output gates/classifiers/retries;
- a second Timeline/branch system;
- G6 dynamic/declarative UI infrastructure.

Task-local fixture helpers are allowed when they only reduce test duplication and are not production dependencies.

## 7. Owner combined UAT after engineering PASS

The Owner may perform one combined real-play checkpoint instead of separate UAT loops for every remaining item.

That checkpoint should observe at least:

1. world can remain quiet when appropriate and can advance independently when pressure is mature;
2. stable NPCs feel capable of independent action;
3. NPC/world secrets do not leak into the player-safe side panel before Player knowledge exists;
4. meaningful risky action can invoke Public d20, produce a natural consequence, and remain coherent later;
5. Save/reopen/Restore feels consistent;
6. MW-004 protagonist-choice boundary still feels natural;
7. MW-009 side panels are useful rather than omniscient/debug-like;
8. Three Kingdoms prose after the Owner-requested bounded style-weight polish is perceptibly closer to the intended voice without becoming forced pseudo-classical prose;
9. MW-008 Markdown-lite rendering is unobtrusive and preserves authored text.

This combined UAT may be used to resolve the remaining product statuses for G5-05, MW-005 and other still-open UAT items before the final G5-GATE verdict.

## 8. G5-GATE interpretation

G5-GATE is not a requirement to simulate every entity at every moment.

PASS means the product has demonstrated, in composition:

- free-form Narrative can create durable consequences;
- timeline persistence/currentness remains coherent;
- knowledge/disclosure boundaries are real;
- non-player actors can act independently;
- the world can selectively evolve outside direct Player causation;
- mechanics can matter to living-world semantics;
- player-facing projection does not leak omniscient truth;
- the architecture has not devolved into a universe simulator or model-format gate forest.
