---
title: my world｜G6 RPG Host ViewModel v0.1 Decision
status: FROZEN / CURRENT
version: 0.1
created: 2026-09-05
phase: G6 RPG Experience & Internal Declarative UI Host
---

# G6 RPG Host ViewModel v0.1 Decision

## 1. Why G6 starts here

G5-GATE passed. The final Owner checkpoint also exposed the first real G6 product demand: the existing MW-009 side panels are safe and dynamic, but too thin to justify their screen space as the final RPG experience.

Therefore the first G6 vertical follows the canonical roadmap rule:

> **Runtime projection → ViewModel → real UI consumer. Consumer before platform.**

Do not start G6 with a universal declarative renderer, external Mod schema, generic event bus, or speculative Character/Inventory ontology.

## 2. Product outcome

Turn the existing three-Host skeleton into a first meaningful RPG information experience while preserving Narrative as the primary surface.

The first vertical must establish:

```text
Authoritative Runtime / Conversation / Save metadata
→ existing player-safe domain projection(s)
→ presentation-only RPG Host ViewModel
→ Player Host + World Surface real consumers
```

The ViewModel is disposable presentation material. It is not canonical Game state, not a second Knowledge store, and not persisted as world truth.

## 3. Player Host v0.1

Player Host answers:

> **我是谁，我正在做什么。**

It may consume only player-owned / player-safe material. The first real content set is deliberately bounded to data already established by current product owners:

- Player display identity;
- selected Player profile / T0 identity label;
- current World / Entry context as safe identity context;
- bounded recent accepted Player actions from current Conversation timeline;
- optional small presentation metadata such as current accepted-turn count when deterministically derivable.

Do **not** invent HP, location, inventory, relationship scores, faction rank, quest state, wounds, resources or other RPG state merely to fill the panel. Those require real domain owners later in G6.

Do not expose GM-reference Character Source sections merely because the Player owns that Character Card. `gm_reference` remains non-player disclosure unless a later authority decision changes it.

## 4. World Surface v0.1

World Surface answers:

> **这个世界有哪些我现在值得查看的信息。**

The first surface split is:

```text
Overview
Save
```

### Overview

Consumes only player-safe material:

- World display identity;
- selected Entry identity;
- bounded current Player Character known facts from the existing G5 disclosure boundary;
- safe presentation metadata such as current accepted-turn count when useful.

Hidden NPC Knowledge, raw semantic consequences, Agency actions, World Evolution events, GM/source instructions, Literary Style Reference, internal IDs/hashes/fingerprints and Public-d20 control material remain absent.

### Save

Reuse the existing Save controls and existing G3 semantics. Move/reframe them as a real World Surface rather than allowing Save controls to dominate the default World Overview.

This task does not redesign Save/Restore semantics.

## 5. ViewModel boundary

The first ViewModel may be handwritten and internal.

Required properties:

- deterministic;
- side-effect free;
- no Provider call;
- no persistence table/schema;
- no raw omniscient `world_state` passed into leaf widgets;
- explicit bounded fields;
- current timeline / Restore / reopen aware;
- safe text truncation/formatting may occur only as disposable presentation behavior and must not alter source bytes.

A suitable conceptual shape is:

```text
player_host:
  identity
  profile
  world_context
  recent_actions[]
  turn_count

world_host:
  overview:
    world_identity
    entry_identity
    known_facts[]
    turn_count
  save_surface:
    existing Save UI state/actions owned by Application/G3
```

Exact internal names are implementation details; the ownership and disclosure boundary are not.

## 6. Navigation / layout

The existing three-Host desktop skeleton remains:

```text
Player Host | Narrative Host | World Surface Host
```

Narrative remains primary.

For this vertical:

- World Surface gains bounded internal navigation between Overview and Save;
- Player Host does not need its own tab system yet;
- wide-screen and narrow responsive behavior must remain coherent;
- no generic navigation framework is required;
- no new permanent design token system is required.

The Owner's final G5 screenshot is accepted evidence that the previous always-visible Save block and sparse Player panel do not yet form a satisfactory information hierarchy.

## 7. Declarative UI timing

This first vertical is **not** the Internal Declarative UI Host v0.1 itself.

Its purpose is to produce a real ViewModel and real surfaces from which the first safe component vocabulary can be inferred.

After one or more real G6 surfaces establish actual needs, G6 may implement internal definitions using the existing supporting design vocabulary such as:

```text
section / card / badge / meter / status_list / fact_list / action_list
```

Do not freeze an external schema in G6. External World Pack / Mod declaration belongs to G8.

## 8. Runtime Asset Resolution

Portrait / scene / authored-map assets remain the next class of real visual consumer work, but are not pulled into this first information-architecture vertical.

When a real portrait/scene/map consumer is started, re-audit G4-10 Runtime Asset Resolution against immutable Source generation and Game-local presentation semantics before loading assets.

## 9. Acceptance intent

The first G6 vertical succeeds when a normal Three Kingdoms session no longer presents the Owner with:

- a Player Host containing essentially only two identity lines and large dead space;
- Save controls consuming the default World information hierarchy;
- UI widgets directly inspecting omniscient Runtime truth.

It must improve information usefulness using **real existing safe data**, not fabricated RPG state.
