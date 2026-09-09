---
title: my world｜G6 Package 2 UAT Cleanup v1.0 Decision
status: FROZEN / CURRENT CORRECTION
version: 1.0
created: 2026-09-09
updated: 2026-09-09
phase: G6 Package 2 closure cleanup
owner: Owner + GPT
basis:
  - docs/uat/G6_PACKAGE2_OWNER_UAT_U1.md@v1.4
  - architecture/interaction/G6_CORE_INTERACTION_CONTROL_V1_0_DECISION.md
implementation_authorization: one bounded cleanup before Package 3
---

# G6 Package 2 UAT Cleanup

## 1. Purpose

Owner ended Package-2 exploratory UAT, judged the remaining findings small, and asked that they be solved once so the project can return to the main core route quickly.

This correction has one outcome:

> **Close the three confirmed Package-2 UAT rough edges without reopening Package-2 product design or pulling in deferred features.**

Exactly three product corrections are authorized:

1. public d20 truth / consequence continuity into later GM context;
2. OOC request-only implementation marker leakage;
3. recommendation short-label layout must actually reclaim Narrative space.

Everything else is non-scope.

## 2. Player-safe Public Mechanics continuity

### 2.1 Product rule

Once a Public d20 / NO_CHECK result is durably accepted and disclosed to the Player, it becomes legitimate **player-safe Program-owned context** for later GM Narrative and OOC reasoning.

The GM must not later deny that the disclosed check occurred, re-roll it, or behave as though accepted outcome/stakes never existed.

Failure does not mean permanent lockout. Later actions may create recovery, escalation or new opportunity. But recovery must grow from the accepted failure/outcome rather than silently erasing it.

### 2.2 Projection boundary

Create/reuse a small player-safe mechanics-history projection owned by the mechanics domain and exposed through an L3/public seam.

It may include only bounded fields already legitimately public to the Player, for current accepted mechanics records, such as:

- accepted turn position;
- CHECK vs NO_CHECK;
- check intent;
- DC / modifier / stance / selected roll / total where applicable;
- success/failure outcome;
- public success intent / failure stakes;
- public NO_CHECK reason.

Do not expose:

- raw control Provider request/response;
- hidden control reasoning;
- action IDs / check IDs / hashes as model content;
- private World/NPC/Knowledge/Agency material;
- raw `world_state`;
- credentials or debug-only material.

The projection must be bounded and current-Timeline aware. Prefer recent accepted mechanics records only; do not build a general mechanics history/retrieval platform.

### 2.3 Consumer placement

For ordinary continuation and OOC, append the player-safe mechanics projection to the same derived Game/GM context already assembled for Narrative continuation.

Place it before the literary style anchor so style remains expression-only and the mechanics truth remains factual context.

Do not add a Provider call, new persistence owner, new table/schema, or new currentness authority.

The existing durable mechanics state + accepted Conversation/Restore semantics remain authoritative.

Do not implement the future full `系统 / System Surface` here.

## 3. OOC request representation

Typed `action/ooc` remains Program-owned and must not be inferred from prose.

The current internal-looking wrapper such as:

`[GM OOC response | input_mode=ooc]`

must no longer be introduced into normal Provider-visible history in a way that can be copied into player-visible GM prose.

Use a minimal presentation-safe request representation. Preferred direction:

- active OOC remains explicitly declared in system/request metadata text;
- historical OOC Player guidance may be marked with a human-readable, non-implementation-facing label;
- historical assistant response does not require an internal implementation marker if the preceding typed OOC guidance already establishes its role;
- raw durable Player/GM bytes remain unchanged.

Do not solve this with broad regex/post-generation rewriting of arbitrary GM prose.

Do not remove typed mode semantics or convert OOC back into slash commands/keywords.

## 4. Compact recommendation presentation

The short-label design exists to return space to Narrative.

Therefore:

> **Short recommendation labels must render as genuinely compact choices, not short text inside old full-width bars.**

Required behavior:

- ordinary desktop widths use content-width compact controls with wrapping/flow behavior or an equivalent layout;
- five ordinary short labels should normally occupy about one or two compact rows;
- recommendation region height follows the actual compact rows and uses bounded scrolling only when needed;
- controls remain readable at >=20px and practically clickable;
- detailed draft remains out of the button and appears only after click in the composer;
- click still switches/ensures role-action mode, exact-prefills, never auto-sends;
- free-form input remains primary;
- do not reduce label semantic quality or shorten model output further merely to fit the UI;
- do not shrink Narrative typography.

Composer height is not part of this correction unless a direct implementation blocker proves the compact recommendation layout cannot work without a minimal local adjustment. Do not redesign the whole interaction host.

## 5. Protected behavior

This cleanup must preserve:

- Package-2 typed action/OOC currentness and persistence;
- OOC structural exclusion from d20/World/Curator;
- Character-guided recommendation semantics and one-call lifecycle;
- strict five `{label,draft}` output contract;
- accepted-action Character evidence semantics;
- Public d20 no-reroll / durable result semantics;
- Save/Restore/Regenerate currentness;
- Debug read-only behavior;
- >=20px gameplay readability baseline;
- free-form natural language as primary play path.

## 6. Explicit non-scope

Do not implement:

- People/general information hide preferences (deferred Package 6);
- Open Threads (next Package 3);
- full System surface;
- Inventory;
- Dynamic UI;
- Context Orchestrator;
- broad d20 redesign / rebalance;
- new consequence engine;
- new relationship/faction system;
- Narrative Preference;
- Reality Correction;
- general UI redesign;
- unrelated G3 Context debt or layer-boundary cleanup.

## 7. Acceptance direction

Engineering must prove:

- a current accepted Public d20 result appears in later continuation/OOC request context as bounded player-safe Program truth;
- Restore removes displaced mechanics context and exposes restored-current mechanics only;
- a later GM/OOC request can no longer legitimately claim a known check never happened because the context contains the disclosed Program truth;
- no hidden mechanics/control material leaks;
- no extra Provider call;
- new OOC requests/history no longer carry the internal implementation marker that leaked during UAT;
- durable accepted prose is unchanged;
- five short recommendation labels render compactly and materially return vertical Narrative space at 960×540, 1280×720 and 1920×1080;
- recommendation click and strict output contract remain unchanged.

After review/integration, Owner receives only a bounded spot-confirmation build. No second exploratory Package-2 UAT is required.
