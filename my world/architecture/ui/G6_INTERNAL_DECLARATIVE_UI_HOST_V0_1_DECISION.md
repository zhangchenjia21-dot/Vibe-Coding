---
title: my world｜G6 Internal Declarative UI Host v0.1 Decision
status: FROZEN / CURRENT
version: 0.1
created: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT
trigger: MW-011 R3 Owner UI UAT PASS + G6 visual re-entry audit
---

# G6 Internal Declarative UI Host v0.1 Decision

## 1. Why now

G6 has now produced real UI consumers instead of speculative surfaces:

- Player Host consumes player-safe identity, authored `player_profile`, recent actions and turn count;
- World Surface consumes World/Entry identity, player-known facts and existing Save controls;
- Owner UAT has exposed real information-density and information-placement constraints.

The visual-runtime re-entry audit on 2026-09-06 intentionally kept portrait / scene / authored-map resolution deferred because no materially authored first-party visual demand exists yet.

The project therefore has enough **real non-visual component demand** to prove an internal declarative Host without inventing external Mod UI.

Supporting design:

`architecture/ui/声明式UIHost设计.md`

Core rule:

> **Definition declares what should be expressed; Host owns how it is rendered. Runtime owns live truth; UI remains a projection.**

## 2. Outcome

Create the smallest internal reusable renderer that can express existing safe Player/World information without changing Runtime ownership.

Required data flow:

```text
Authoritative Runtime / frozen Source
→ existing player-safe domain projections
→ existing presentation ViewModel
→ internal bounded UI definition material
→ Internal Declarative UI Host
→ Godot Control tree
```

The internal definition is disposable presentation material. It is not persisted world state, not a query language, not Source authority and not an external schema.

## 3. v0.1 consumers

v0.1 must prove reuse against **at least two existing real consumers**:

1. Player Host profile/information material;
2. World Overview information material.

Existing Save controls remain hand-written in this task because they carry application/domain actions not yet represented by bounded Action Intent.

Narrative streaming/composer remains untouched.

## 4. v0.1 component vocabulary

Only implement kinds directly justified by current product data:

```text
section
status_list
fact_list
```

Conceptual internal shapes:

```text
section:
  component_id
  kind = "section"
  title
  children[]

status_list:
  component_id
  kind = "status_list"
  title?        # optional
  items[]:
    label
    value

fact_list:
  component_id
  kind = "fact_list"
  title?        # optional
  items[]       # display strings
```

Bounds:

- stable internal `component_id` safe token within one definition;
- authored/generated order is display order;
- bounded arrays and strings; reject unknown kinds/unknown fields;
- no nested arbitrary dictionaries outside the exact component shapes;
- recursion depth must be bounded and small;
- no executable values.

Do **not** implement `meter`, `badge`, `card`, `action_list`, `secondary_view`, `map_overlay` or filters until a real consumer requires them.

## 5. Definition ownership

v0.1 definitions are **program-internal** and generated from existing safe ViewModel data.

They are not loaded from World/Character/Expansion packages and are not external JSON contracts.

No Source/Mod may define UI in G6 v0.1.

A suitable flow is:

```text
RPG Host ViewModel
→ small task-owned definition builder/adapter
→ Host renderer
```

The ViewModel remains the safety/data boundary. Do not let definitions fetch data themselves.

## 6. Forbidden capabilities

The internal definition must not contain or execute:

- GDScript callbacks or method names;
- arbitrary NodePath;
- arbitrary expressions or `${...}` binding;
- SQL/runtime queries;
- filesystem / OS commands;
- Provider prompts/calls;
- raw `world_state`;
- raw Character `semantic_sections`;
- Source Library lookup;
- direct mutation intents;
- arbitrary Godot scene/resource paths supplied as data.

Formal invariant:

```text
Definition describes safe presentation structure
!= capability to inspect or mutate Runtime
```

## 7. Rendering ownership

The Host renderer owns:

- creation/removal of Godot Controls;
- Theme/font/color application using existing product theme;
- wrapping/overflow;
- vertical layout;
- accessibility-safe labels;
- fail-soft rendering behavior.

Malformed/unsupported internal definition material should render nothing for that contribution or a bounded non-authoritative fallback; it must never crash/block Narrative gameplay.

## 8. Player Host migration

Migrate only the **repeatable structured material** through v0.1 definitions:

- authored Player profile groups;
- safe Player/world/session status rows where useful;
- recent Player actions as a display list if it fits `fact_list` without changing semantics.

Do not change MW-011 disclosure/Source ancestry behavior.

The current information placement is accepted by Owner UAT. This task does not move profile information between left and right Hosts.

## 9. World Overview migration

World Overview may use declarative components for:

- World / Entry status;
- Player-known facts;
- safe session/turn metadata already present in the ViewModel.

Keep `概览 / 存档` navigation and all Save controls/actions in their existing imperative ownership.

Do not invent empty tabs for 人物 / 关系 / 势力 / 任务 / 物品 / 地图 / Timeline.

## 10. Information-architecture boundary

MW-011 final Owner UAT recorded that some material currently displayed on the left may later move to right-side surfaces once the World Surface information architecture is grounded.

This v0.1 renderer must make later redistribution easier, but **must not decide that redistribution now**.

A future surface must have a real domain owner and player-safe projection before it is implemented.

## 11. Persistence and identity

No new SQLite tables or columns.

UI definitions are regenerated from current safe ViewModel material and are not canonical Timeline/Save truth.

Stable `component_id` exists only to support deterministic rendering/diff/reuse within the UI layer; it is not a world/entity ID.

## 12. Acceptance

The vertical passes Engineering review when:

1. Player Host and World Overview both exercise the same bounded renderer;
2. visible information remains materially equivalent to the pre-task accepted product outcome;
3. current MW-011 profile data, recent actions and World known facts continue to refresh/reopen/Restore correctly;
4. Save controls/navigation remain unchanged and functional;
5. definitions receive only safe ViewModel material and cannot query Runtime;
6. malformed/unknown component definitions fail closed/soft without breaking Narrative;
7. no arbitrary callback/expression/NodePath/filesystem/Provider capability exists;
8. no external Source/Mod UI schema is introduced;
9. no persistence schema change occurs;
10. wide/1280/narrow layout remains coherent;
11. existing MW-009/MW-011/Save regressions remain green;
12. Windows export passes.

## 13. Explicit non-scope

- external World Pack / Mod declarative UI;
- G8 schema/versioning/authoring UX;
- arbitrary UI DSL;
- Action Intent;
- meters/stats;
- Inventory/Relationship/Faction/Quest/Map domains;
- visual asset resolution;
- portrait/scene/map rendering;
- right-side IA redistribution;
- UI preference persistence;
- replacing Narrative/Composer/Save actions with definitions.

## 14. Agent routing

This is architecture-critical shared UI infrastructure whose mistakes could weaken the safe projection boundary and shape the later G8 external contract.

**Primary implementer: Codex.**

KimiCode may be used later for bounded visual polish after the mechanism passes review, but is not the primary owner of v0.1.
