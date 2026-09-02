---
title: my world｜Visual Asset Runtime Deferral to G6 Decision
status: current-canonical-roadmap-architecture-decision
created: 2026-09-02
updated: 2026-09-02
phase: G4 Primary Source Assets & Local Game Creation
owner: OWNER + GPT
supersedes_when_conflicting:
  - my world/architecture/source/G4_RUNTIME_ASSET_RESOLUTION_V0_1_DECISION.md G4 execution timing
  - MY_WORLD_总体规划路线图_CURRENT.md v3.2 G4-10 / G4-GATE visual requirements
---

# Visual Asset Runtime Deferral to G6｜Canonical Decision

## 1. Owner decision

On 2026-09-02 the Owner explicitly decided that portrait / scene / authored-map production assets are not yet materially authored and are not part of the current core experience. Runtime visual-asset integration should therefore **not consume the G4 critical path**.

Decision:

```text
G4-10 Runtime Asset Resolution
DEFERRED / MOVED TO G6

G4-10M1
SUPERSEDED / DO NOT EXECUTE
```

This is a roadmap change, not an engineering failure.

## 2. Why this is the correct ordering

Current proven core value is:

```text
rich World + Character Source
→ exact Game creation
→ real AI GM play
→ durable progression
→ optional Expansion mechanics
→ Save / reopen / continue
```

Portrait / scene / authored-map image loading is presentation support. There is currently no mature visual consumer whose absence blocks G4 or G5 core runtime semantics.

The project therefore follows:

> **Vertical before platform. Consumer before infrastructure.**

Do not build an exact-generation media resolver merely because Source contracts already permit visual declarations.

## 3. G4 route after this decision

```text
G4-09 First Playable B                  PASS / CLOSED
↓
G4-11 Two Primary Asset Families Reality Test
↓
G4-GATE
↓
G5 World Semantics & GM Runtime
```

G4-11 no longer requires exact visual asset generation/resolution.

Its product question is now strictly:

> Do the two real Primary Source families produce meaningfully different, coherent, durable RPG worlds through the actual product path?

Required reality pressure remains:

- historical / low-magic family and high-magic / fantasy family;
- independent Games;
- real Provider Opening / continuation;
- Character individuality and world-specific causal/narrative texture;
- durable progression;
- Save / reopen / Continue;
- switching between Games without identity/context leakage;
- exact Source generation provenance and Source-update isolation for semantic Source truth.

Visual differentiation is **not** a G4 acceptance condition.

## 4. G4-GATE amendment

Remove these former G4 requirements:

```text
Runtime asset resolution
exact visual asset generation
```

G4-GATE still requires the two-family real Provider proof and Owner UAT that the two worlds feel materially different as RPG worlds.

## 5. G5 independence

Deferring visual asset runtime does not block G5.

G5 authoritative domains are semantic/runtime concerns such as:

- World Turn / semantic materialization;
- Knowledge provenance;
- NPC / Faction agency;
- Event-driven evolution;
- mechanics integration;
- runtime projection.

None of these may depend on an authored map image, portrait, Godot Texture, or scene illustration as canonical truth.

Formal invariant:

```text
authored visual presentation
!=
World / Character / Location / Event / Knowledge authority
```

An authored map image is not topology, current location, travel graph, pathfinding, distance, fog-of-war, GIS or spatial simulation truth.

## 6. G6 destination

Runtime Asset Resolution moves into G6, where real presentation consumers already exist.

Recommended G6 order:

```text
real Runtime projection / ViewModel consumers
→ exact-generation visual resolution where actually needed
→ portrait / scene / authored-map presentation
→ RPG surfaces / navigation / polish
```

The previously frozen semantic notes remain useful future guidance unless a later G6 consumer disproves them:

- authored visual bytes remain part of immutable Source generations when declared;
- old Games must not silently mutate semantic Source ancestry;
- visual fallback must not impersonate authored truth;
- visual presentation failure should normally fail-soft rather than block Narrative gameplay;
- map image != map gameplay semantics.

However, **no G6 implementation contract is frozen now**. G6 must re-audit the actual visual consumer before implementation.

## 7. Old-game visual update question is intentionally deferred

A future case may be:

```text
old Game pins Character generation A with no portrait
later Source generation B adds a portrait only
```

Whether an old Game may opt into a non-canonical presentation override while preserving semantic Source ancestry is a real G6 product decision.

Do not design that mechanism in G4 or G5.

## 8. Decision propagation

After this decision:

- supersede `G4-10M1_RUNTIME_ASSET_RESOLUTION_MECHANISM_TASK.md`;
- amend roadmap to v3.3;
- remove visual resolution from G4-GATE;
- activate a visual-independent G4-11 reality-prep task;
- keep G5 blocked until G4-11 + G4-GATE pass;
- move visual runtime work to G6 planning.
