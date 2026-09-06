---
title: my world｜G6 Player Character Profile Projection v0.1 Decision
status: FROZEN / CURRENT
version: 0.1
created: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
trigger: MW-011 Owner UI UAT after integrated main@6338af5665c5137d9a9528776e77a13ffb924ea6
---

# G6 Player Character Profile Projection v0.1 Decision

## 1. UAT finding and lineage

The integrated MW-011 R1 implementation is Engineering PASS, but the first real Owner UI UAT still shows a fresh Player Host with large unused space and almost no character-definition information even when the selected Player Character has a rich authored Character Card.

The same UAT simultaneously proves that MW-012 Character content is reaching GM context: the opening Narrative uses Zhang Chen-specific material such as physical transport, modern possessions and remembered historical context. Therefore the defect is not "the Character Card failed to load". The missing seam is **human-player-safe character-profile presentation**.

This remains the same MW-011 product outcome: make the Player Host materially useful with real safe data. Under `TASK_IDENTITY_AND_LINEAGE_V1_0.md`, this is **MW-011 Revision 2**, not a new flat Work ID.

## 2. Current cause

Current selected Character projection freezes `semantic_sections`, but Character Card v0.2 sections are explicitly classified only as `gm_reference` or `gm_private`. MW-009 correctly excludes those Source sections from human-player-safe UI projection. MW-011 R1 then consumes only identity/profile/World/Entry/known facts plus recent actions and turn count.

Therefore the current system has no authority-bearing field that means:

> this authored Character material is safe and intended for the human player to view as their own character profile.

Do not fix this by passing raw `semantic_sections` into UI, parsing arbitrary Markdown, or treating every `gm_reference` section as player-visible.

## 3. New bounded Source presentation field

Character Card v0.2 gains one **optional, backward-compatible internal presentation field**:

```text
player_profile
```

This does not create a new gameplay ontology and does not replace `semantic_sections`. It is authored, bounded presentation material for a Character when that Character is selected as the human Player Character.

Conceptual shape:

```text
player_profile:
  headline: String
  summary: String
  groups:
    - group_id: safe token
      title: String
      items: Array[String]
```

Bounds for v0.1:

- `headline`: non-empty, <= 120 chars;
- `summary`: non-empty, <= 360 chars;
- `groups`: 1..8, authored order is presentation priority/order;
- each group has exact fields `group_id/title/items`;
- `group_id` uses the existing safe-token rule and is unique inside the profile;
- `title`: non-empty, <= 60 chars;
- `items`: 1..8 non-empty strings, each <= 160 chars;
- no nested arbitrary dictionaries, callbacks, bindings, expressions, IDs/hashes/fingerprints, instructions or live-state fields.

Existing Character Card v0.2 packages that omit `player_profile` remain valid and keep the existing compact fallback. No World/Expansion schema is broadened. External Mod schema remains G8 work.

## 4. Authority and duplication rule

`player_profile` is **presentation-only authored material**.

It may restate information already present in semantic Character sections so the human UI can render a safe structured profile without interpreting GM reference prose. It is not a second source of gameplay truth, does not enter GM context, does not create mechanics, does not mutate Runtime state and must not be used by semantic/Knowledge/Agency/Evolution systems.

For first-party content, the profile must faithfully summarize the same Owner-approved Character concept. If an author creates a contradiction between `player_profile` and semantic sections, that is an authoring defect; Runtime must not resolve the contradiction by treating the profile as world authority.

## 5. Frozen Game-local ancestry

The selected Character projection must freeze `player_profile` into the Game-local Player Character source projection at Final Create, alongside the already selected Character identity/profile metadata.

Important consequence:

- an existing Game created from an older Character generation that has no `player_profile` remains unchanged and must **not** look up the newest Source generation to backfill UI;
- a newly created Game from a generation containing `player_profile` may render it;
- reopen/Save/Restore preserve the exact frozen profile of that Game.

This keeps Source ancestry coherent and avoids presentation silently rewriting old Games.

## 6. New safe projection seam

Do **not** broaden MW-009's G5 player-safe projection contract to inspect raw Character sections.

Add a separate bounded domain projection:

```text
Game-local frozen player_character.source_projection.player_profile
→ Player Character Profile Projection
→ MW-011 RPG Host ViewModel
→ Player Host
```

The projector must fail closed. It may read only the frozen `player_profile` object plus safe display identity needed to validate/use it. It must never fall back to `semantic_sections`, `catalog_summary`, current Source Library bytes or omniscient Runtime state.

MW-009 continues to own current Player-known facts and its existing disclosure boundary unchanged.

## 7. Player Host rendering v0.1

The Player Host remains a single surface in this revision; do not add a generic Player navigation framework or a separate full Character screen yet.

When a valid `player_profile` exists, render a compact structured character block before the existing World/recent-action/session material:

```text
主角
<display name>
<selected T0 profile label>
<headline>
<summary>

<group title>
• item
• item
...

世界 / Entry
最近行动
玩家回合
```

Use authored group order. The Host may be made vertically scrollable so rich characters do not force the Narrative Host to shrink. UI truncation, if any, is disposable and must not modify frozen profile bytes.

A fresh Zhang Chen game should visibly communicate at least age/modern-origin/background, personality, capabilities, limitations, goals/principles and starting possessions without showing raw Source prose.

## 8. Zhang Chen first real consumer

Update the first-party Zhang Chen Character generation with a `player_profile` derived strictly from the already approved MW-012 semantics.

Required visible concepts include:

```text
headline: 24岁 · 现代穿越者
summary: 退役武警义务兵、985高校出身、历史与军事爱好者

groups (authored order):
背景
性格
能力
局限
初始目标
行为原则
随身物品
```

The values must preserve the Owner-approved content and must not add new powers, equipment, relationships or guaranteed future outcomes.

Because Source bytes change, publish a new immutable Zhang Chen generation. Existing Games remain on their old frozen generation. Prefer a normal Character package version increment rather than pretending the bytes are unchanged.

## 9. Non-scope

This revision does not include:

- a universal Character ontology or stat system;
- HP/location/relationship/faction/quest/inventory Runtime domains;
- Runtime inventory mechanics based on the authored starting-possession display list;
- portrait / scene / map asset resolution;
- full Character Surface / generic tab framework;
- Internal Declarative UI Host;
- external Mod/Creator schema;
- Provider summarization or semantic extraction of profile prose;
- changing GM Narrative gates or G5 living-world semantics.

## 10. Acceptance intent

MW-011 R2 succeeds when a newly created Zhang Chen Han-end game opens with a Player Host that immediately communicates who the protagonist actually is, while the UI remains strictly player-safe and frozen-generation coherent.

The decisive safety property is:

```text
explicit authored player_profile
!= raw semantic_sections
!= GM reference/private material
!= Runtime/world authority
```
