---
title: my world｜G6 People Known-person Eligibility UAT Correction v1.0 Decision
status: FROZEN / CURRENT CORRECTION
version: 1.0
created: 2026-09-07
updated: 2026-09-07
phase: G6 RPG Core Closure
owner: Owner + GPT
triggered_by:
  - my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
supersedes_in_part:
  - architecture/ui/G6_PEOPLE_IDENTITY_AND_CURATION_V1_0_DECISION.md sections 3-7 only where current receipt eligibility is narrower than this correction
---

# G6 People Known-person Eligibility UAT Correction｜FROZEN

## 1. UAT failure

Real Owner UAT proved this product failure:

```text
accepted play already established 钟繇 as known to the protagonist
→ later Player turn explicitly recalled 钟繇
→ no 钟繇 People card

same scene
→ two incidental soldiers received People cards
```

The current MW-018 implementation packet says:

> **Only current receipt-bound actors are eligible for People update in that turn.**

Current identity evidence is centered on the current World-semantic/GM-span actor receipt. Therefore a player-known but off-screen person can be semantically important yet never become a legal Curator candidate.

That violates frozen People product semantics, which allow People cards for persons the player has actually learned about through accepted play without requiring a current physical encounter.

## 2. Correction principle

Freeze:

> **People eligibility follows accepted player-known person evidence, while authoritative identity still follows exact Program-owned stable actor identity.**

And:

> **Current scene presence is neither necessary nor sufficient for a People card.**

This correction broadens the exact identity-evidence seam; it does **not** authorize Program name matching or automatic card creation.

## 3. Accepted person-reference evidence

A current player-authored accepted Turn may expose bounded exact person-reference evidence from the full accepted pair:

```text
accepted Player input
+
accepted GM Narrative
```

Each valid request-scoped candidate may carry:

```text
actor_ref
source_role = player | gm
overbatim accepted span / quote
span coordinates within that exact accepted message
```

Program privately owns:

```text
actor_ref → exact stable local_character_id
```

The model never receives or mints the durable ID.

### Existing stable actor referenced by Player

If the current Player input refers to an already-existing stable actor and the semantic identity lane can exactly resolve that reference, an exact Player-span binding is allowed.

This directly supports cases such as:

> “我回忆钟繇……”

when `钟繇` already has a legitimate stable Game-local actor identity.

### GM reference to off-screen real person

A person does not have to be physically present in the current scene. If accepted GM Narrative establishes/refers to a distinct real person in the current Game, the semantic lane may produce an exact GM-span binding even when that person is off-screen.

### Newly established off-screen person

If a current player-authored Turn's accepted GM Narrative legitimately establishes a new distinct person as part of current Game reality, the existing semantic actor materialization lane may mint the stable actor and produce the same-turn exact binding even when that person is not an active scene participant.

This is an extension of existing stable-actor materialization, not a new People-only identity system.

## 4. Critical epistemic boundary

Player assertion alone must **not** create World Truth.

For a Player-span person reference:

```text
existing exact stable actor can be resolved
→ eligible exact binding

no legitimate stable identity / existence remains unresolved
→ no authoritative binding
→ no People update for that reference
```

Do not materialize a new real actor solely because the Player says or remembers a name.

If later GM/world semantics legitimately establish that person, the normal exact stable-identity path may then bind them.

This preserves:

```text
Player belief / memory
!= automatic World Truth
```

## 5. Receipt evolution

The existing Game-local `living_world` identity receipt may evolve through a backward-compatible schema/result variant so a binding can record accepted source role + exact span provenance.

Requirements:

- old receipts remain valid/readable;
- no new SQLite table solely for this correction;
- receipt stays bound to current Game + accepted turn + full Player+GM prefix/version + activation/Restore epoch;
- actor materialization and same-turn binding remain atomically consistent where both occur;
- stale/failed/cancelled/timeout semantic work cannot publish current bindings;
- max current-turn candidate ceilings remain bounded; this correction does not create an unbounded roster dump.

Exact field names are implementation-owned if equivalent semantics are proved.

## 6. People Curator candidate boundary

For the People component, the current Information Curator may receive all **current exact receipt-bound person references** from the accepted Player+GM pair, not only current GM-span/scene candidates.

Conceptual safe input:

```json
{
  "actor_ref": "request-scoped-ref",
  "source_role": "player|gm",
  "quote": "verbatim player-visible accepted span",
  "current_snapshot": null
}
```

If that exact actor already has a current player-known People snapshot, the bounded current snapshot may be included.

Still forbidden:

- complete stable actor roster;
- raw actor profile/game-local material;
- NPC-private Knowledge;
- Agency plans;
- hidden World Evolution;
- Source-current hidden content;
- local_character_id in model-visible prose;
- display-name authoritative dedupe/matching.

## 7. Importance / card-worth semantics

Identity eligibility and card worth remain separate.

The Curator prompt must explicitly preserve:

```text
being present in the current scene
!= automatically worth a persistent People card

being off-screen
!= automatically unimportant
```

The model decides whether a person is worth persistent memory based on accepted player-visible meaning.

Incidental guards/soldiers/passers-by may legitimately remain uncarded. A historically/socially important off-screen person the protagonist has explicitly learned about may legitimately receive a card.

Program must not implement:

- encounter-count thresholds;
- named-person allowlists;
- fame tables;
- importance scores;
- “named NPC always card” rules;
- current-scene priority rules.

## 8. Existing cards and currentness

Existing People currentness rules remain unchanged:

```text
Restore before knowledge/binding
→ card absent

Restore after earlier valid snapshot
→ earlier snapshot returns

Regenerate/correction replaces source history
→ stale binding/update non-current

reopen
→ current cards reconstruct with zero Provider call
```

No displaced-future rehydration and no Source-current backfill.

## 9. No historical backfill in this correction

Do not replay the whole old transcript.

For an existing UAT Game, the corrected path is proved by a **new accepted player-authored Turn** that references/learns an eligible known person.

If older accepted history already contains a valid stable actor but no People card, a new current reference can now make that actor eligible. No automatic retroactive Provider pass is required.

## 10. Acceptance direction

Engineering must prove at minimum:

1. current Player-span reference to an exact existing stable actor can become a People candidate;
2. current GM-span reference to a real off-screen person can become a People candidate;
3. same-turn newly established off-screen person can be materialized + bound when GM/world semantics legitimately establish existence;
4. unresolved Player-only name/reference never mints or guesses a stable actor;
5. same-name actors remain exact and distinct;
6. no display-name/fuzzy/first-match authority is introduced;
7. incidental current-scene actor can remain uncarded when model judges no persistent value;
8. off-screen known actor can receive/update a card when model judges persistent value;
9. no raw hidden actor material enters Curator/UI;
10. Save/Restore/Regenerate/reopen currentness remains correct;
11. existing MW-017/MW-018 historical receipt/curation data remains readable;
12. no extra default People-specific Provider call or new SQLite table.

## 11. Product re-UAT target

Owner should be able to perform a normal new Turn such as explicitly recalling a legitimately established off-screen person and see that person become/update the correct People card when the model judges them worth remembering, without requiring that person to be physically present.

At the same time, incidental scene NPCs should not be promoted merely because they happened to be current exact candidates.

## 12. Non-scope

This correction does not authorize:

- universal entity graph/resolver;
- player-side false-person / imaginary-person card identity system;
- numeric relationship domain;
- historical People backfill;
- People search/sort/filter;
- Provenance/Epistemic Status product expansion;
- generic Dynamic UI Host work;
- Character-guided recommendations.
