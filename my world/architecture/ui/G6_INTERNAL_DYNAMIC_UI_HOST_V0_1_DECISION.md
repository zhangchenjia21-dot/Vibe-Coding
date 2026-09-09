---
title: my world｜G6 Internal Dynamic UI Host v0.1 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-09
updated: 2026-09-09
phase: G6 Package 6 Internal Dynamic UI Host
owner: Owner + GPT
implementation_consumer: MW-030
supersedes: architecture/ui/G6_INTERNAL_DECLARATIVE_UI_HOST_V0_1_DECISION.md@0.1
---

# G6 Internal Dynamic UI Host v0.1｜CURRENT

## 1. Why now

Package 6 was intentionally delayed until the product had several real information consumers. That condition is now satisfied.

Current production surfaces are grounded by real domain owners:

```text
Character
Important Experiences
People
Open Threads / 事务
Inventory / 行囊
System / Public d20
```

They currently repeat the same Godot presentation work in bespoke renderers or Shell methods: section titles, wrapped text, lists, cards, collapsed details, safe empty states and scrolling.

The project can therefore extract a **small internal dynamic presentation Host from proven consumers**, rather than designing a speculative UI platform.

Core rule:

> **Domain/player-safe projections decide what information exists; internal definitions describe how that already-safe information is structured for presentation; the Host owns rendering.**

And:

> **Dynamic UI is presentation convergence, not a new gameplay truth, model authority, or external Mod protocol.**

## 2. Product outcome

After v0.1, the right-side information area should still answer the same product questions, but multiple real surfaces are rendered through one bounded internal Host rather than each owning a custom Godot renderer.

Required production flow:

```text
Authoritative domain truth
→ domain-owned player-safe L3 projection
→ first-party presentation adapter
→ bounded internal UI definition
→ Internal Dynamic UI Host
→ Godot Controls
```

The Host must be used by at least these real surfaces:

```text
角色
重要经历
人物
事务
行囊
系统
```

`概览` may remain imperative in v0.1 if migration adds no meaningful convergence value. `存档`, Narrative/composer, Debug drawer and other action-bearing UI remain imperative.

The existing World Information navigation remains:

```text
概览 | 角色 | 重要经历 | 人物 | 事务 | 行囊 | 系统 | 存档
```

No new domain/surface is created merely to exercise the Host.

## 3. Internal definition is disposable presentation material

The definition is not persisted gameplay state and does not own currentness.

It is regenerated from current safe projections whenever the relevant surface refreshes.

It is NOT:

- World truth;
- Character/People/Thread/Inventory/Mechanics truth;
- Timeline truth;
- a query language;
- a binding/expression language;
- a Provider prompt;
- an external JSON contract;
- a Source / Expansion capability declaration.

Formal boundary:

```text
safe domain DTO
→ disposable presentation definition
→ renderer

NOT

raw Runtime/world_state
→ generic renderer filters truth itself
```

Leaf renderer code must never receive omniscient `world_state` and decide locally what the player may see.

## 4. v0.1 bounded component vocabulary

Only vocabulary directly proven by current surfaces is authorized.

The exact implementation names may vary, but semantics are bounded to:

### 4.1 `section`

Container/group with an optional/required title and ordered children.

Used for top-level surface sections and Character groups.

### 4.2 `text`

A bounded wrapped text block with a small closed presentation emphasis vocabulary such as ordinary / heading / muted.

No arbitrary BBCode/HTML/expression execution is accepted from definition data.

### 4.3 `fact_list`

Ordered display strings, suitable for Character group items / Thread details and similar factual bullet lists.

### 4.4 `field_list`

Ordered bounded `{label, value}` display pairs, suitable for System mechanics math/status facts and similar structured rows.

### 4.5 `card`

A bounded titled presentation unit with ordered children.

Current justified consumers include:

- People cards;
- Important Experience entries;
- Open Thread entries;
- System CHECK records;
- Inventory item entries.

A card may support a fixed `collapsible` presentation property where the current product already proves that behavior (People). This is not an arbitrary action/callback capability.

### 4.6 Explicitly not authorized

Do not add merely for future flexibility:

```text
meter
progress/stat bar
badge taxonomy
generic action_list
map/map_overlay
secondary_view
arbitrary table/grid DSL
form inputs
script/expression binding
component plugin registry
external resource/scene component
```

A future real consumer must justify new vocabulary.

## 5. Definition safety / validator

Create a small deterministic validator/materializer for internal definitions.

At minimum:

- closed known component kinds;
- exact/known fields per kind;
- stable internal component ID token within one definition tree;
- duplicate component ID rejected;
- bounded recursion depth;
- bounded component count, list count and string lengths;
- primitive strings/booleans/closed enums only where defined;
- no arbitrary nested dictionaries outside exact shapes;
- malformed/unsupported contribution fails soft/closed and never blocks Narrative gameplay.

Definition data must never contain/execute:

- arbitrary GDScript callback/method name;
- arbitrary NodePath;
- `${...}` / expression language;
- SQL/runtime query;
- filesystem/OS command;
- Provider call/prompt;
- raw `world_state`;
- Source Library lookup;
- arbitrary Godot resource/scene path;
- authoritative mutation intent.

The Host owns Control creation, product theme, >=20px ordinary gameplay typography, wrapping, spacing, scroll-compatible vertical layout, collapse presentation and bounded visibility controls.

## 6. Surface adapters and semantic preservation

Package 6 is a presentation migration, not a semantic redesign.

Each adapter must consume the existing player-safe domain projection and preserve that surface's current meaning.

### Character

Preserve current headline / summary / model-curated groups and quiet empty state.

Character remains “现在的我是谁”, not a log, inventory or live mechanics panel.

### Important Experiences

Preserve sparse milestone entries and current order. Do not add Brief Recap or turn/date labels.

### People

Preserve current player-known snapshot semantics, default collapsed card behavior and current five visible content fields.

No affinity score, name-based identity or omniscient dossier is introduced.

### Open Threads

Preserve current read-only unresolved-matters snapshot and model semantic authority. No checkbox, priority, completion button or Quest engine.

### Inventory

Preserve exact factual current possession projection. No item mutation buttons, equipment/stats or generic item engine.

### System / Public d20

Preserve exact Program-owned accepted/current CHECK projection, newest-first/max12, existing player-visible mechanics facts and NO_CHECK exclusion from persistent System history.

The existing Narrative inline d20 card remains imperative and unchanged.

## 7. Model-curated presentation visibility preference becomes active

Owner has approved the general principle:

> **Model decides what is semantically worth retaining; Player decides which retained model-curated presentation units they want visible in their own interface.**

Package 6 activates the first safe implementation of this principle as a Host-level presentation capability.

Visibility preference is NOT semantic deletion and MUST NOT be supplied to the model as importance feedback.

### 7.1 v0.1 eligible surfaces

Implement hide/recover for at least:

- **People cards**;
- **Important Experience entries**.

These two domains can provide legitimate stable Program-owned presentation identity without using display text.

### 7.2 v0.1 deliberately not hideable

Do NOT generically hide:

- System/Public d20;
- factual Inventory;
- blocking errors;
- Debug diagnostics;
- Recommendations.

Do not add hide to Character in v0.1 because its groups/items currently have no stable presentation identity contract.

Do not add hide to Open Threads in v0.1 because current Thread snapshots have no stable Thread identity. **Do not invent identity using title equality, rendered-text hash or array position.** The Thread domain must gain a legitimate stable identity in a future semantic/domain decision before per-Thread hide can be safe.

## 8. Stable opaque presentation keys

The Host must not use display text as hide identity.

### People

The internal People fold already has exact stable Game-local actor identity. A presentation-specific safe projection may derive an opaque `presentation_key` from that legitimate identity and expose the opaque key with the safe card DTO.

Raw actor/local IDs do not need to reach the renderer or persistence file.

### Important Experiences

Current validated curation records already have stable Program-owned record IDs. A presentation-specific safe projection may derive an opaque key from the current validated record identity + event ordinal, then expose only that opaque presentation key.

The key is UI identity only; it does not become a world/entity ID and must not be fed to models.

### Other surfaces

No stable identity → no per-item hide in this package.

## 9. Presentation preference owner

Hide state is a **Game-local presentation preference outside Timeline truth**.

Required behavior:

- survives normal reopen/Continue;
- Restore does not rewind it;
- Regenerate does not itself rewrite it;
- hidden underlying semantic information may continue updating;
- a hidden item does not auto-unhide because its contents change or the model later considers it more important;
- if the same stable current item becomes visible-current again, its hide choice still applies;
- hiding/restore visibility causes zero Provider calls and zero authoritative gameplay mutations.

Use one small bounded presentation-preference owner, separate from Game SQLite Timeline state. A validated Game-local sidecar under the product's `user://` data root is appropriate. Tests must support a task-owned override/root so no Owner file is touched.

Only persist opaque presentation keys and bounded preference metadata; do not persist copied semantic card content.

Do not create a generic preferences platform beyond this proven capability.

## 10. Fixed Host visibility UX

This is a fixed first-party presentation action, not generic Action Intent.

For eligible visible cards:

```text
隐藏
```

For a surface with hidden current items, provide a clear bounded recovery path such as:

```text
已隐藏 (N)
→ show current hidden items
→ 恢复显示
```

The hidden drawer must display the **current** safe projection for the item, not a stale copy saved at hide time.

The Host may emit a closed fixed signal such as hide/restore request with surface + opaque key. Definition data must not supply arbitrary callback names or executable intent.

## 11. Dynamic UI remains internal-only in G6

No World Pack, Character Card, Expansion or Mod may supply UI definitions in Package 6.

The Host vocabulary is intentionally an implementation-internal contract learned from first-party consumers.

External declaration remains G8 work after Package 7 proves the product loop.

Do not add:

- external schema versioning;
- UI authoring DSL;
- Source validation for UI;
- Mod surface ownership registry;
- arbitrary code/plugin support.

## 12. Currentness / failure behavior

Definitions are rebuilt from each domain's current player-safe projection, so:

- Restore/Regenerate removes restored-away/superseded material according to domain currentness;
- hidden preference remains separate and does not rewind;
- renderer has no separate currentness cache/truth;
- malformed presentation definition does not delete underlying semantic/factual data;
- renderer failure must fail soft locally and never gate Narrative acceptance, World semantics or Save/Restore.

## 13. Explicit non-scope

Package 6 does not implement:

- new Character/People/Thread/Inventory/System semantic behavior;
- Thread stable identity redesign;
- Character group/item hide identity;
- System or Inventory hide;
- generic Action Intent;
- player-editable semantic data;
- portrait / scene / map Visual Runtime;
- Map surface;
- external declarative UI;
- new Provider calls;
- new gameplay persistence tables;
- G3 Context debt cleanup;
- shell-wide architectural rewrite;
- UI appearance/polish redesign unrelated to convergence.

## 14. Engineering acceptance

Engineering acceptance must prove at least:

1. one shared Host renderer is used by at least **three distinct real surface classes**, and production migration covers the targeted Character / Experiences / People / Threads / Inventory / System surfaces unless an independently evidenced blocker requires a narrower safe subset;
2. pre-migration player-visible content/semantics of every migrated surface remain materially equivalent;
3. definitions are built only from existing/new presentation-specific **player-safe L3** DTOs, never raw Runtime/world truth in the leaf renderer;
4. closed vocabulary validation rejects unknown kinds/fields/duplicate IDs/excess depth/oversize without blocking Narrative;
5. People remains default-collapsed and all 8 World Information tabs remain usable;
6. System continues to show exact current d20 facts; Inventory continues to show exact current factual possessions; Threads remain read-only model-curated unresolved matters;
7. People + Important Experiences can be hidden and recovered using opaque stable presentation keys;
8. hide removes only ordinary visible presentation; underlying semantic projection remains intact and may continue updating;
9. hide survives reopen and **does not rewind on Restore**;
10. a hidden item update does not auto-unhide; recovery shows its then-current content;
11. no display-name/text/array-position identity is used for hide;
12. System/Inventory cannot accidentally acquire generic hide controls;
13. hide/recover/tab/render cause zero Provider calls and zero gameplay durable mutations;
14. Save controls, Narrative/composer, Debug and inline d20 card remain imperative/functional;
15. 960×540 / 1280×720 / 1920×1080 preserve >=20px ordinary text, wrapping, vertical reachability and Narrative usability;
16. no new SQLite gameplay owner/table and no external UI schema is introduced;
17. fresh Godot import + Windows export / ValidateExportOnly pass;
18. directly affected Package 0–5 / G2-G5 regressions remain green except already-proven exact-baseline debt.

## 15. Product acceptance

Owner Product confirmation may remain part of the concentrated Package 7 Reality Gate.

At Product UAT, the main questions are:

- do the information surfaces still feel correct and readable after sharing one Host;
- does the product feel more consistent without making different domains semantically homogeneous;
- can the Player declutter model-curated People/Experience information without training or fighting the model;
- does hidden information remain safely recoverable and current;
- has Narrative primacy or information freedom regressed.

Package 6 Engineering PASS does not itself equal Product PASS.
