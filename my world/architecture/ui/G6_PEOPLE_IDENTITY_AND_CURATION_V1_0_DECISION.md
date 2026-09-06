---
title: my world｜G6 People Identity Binding + Curation Architecture v1.0 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-06
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
owner: Owner + GPT
triggered_by:
  - my-world/docs/mw016/MW-016_PEOPLE_ARCHITECTURE_AUDIT.md @ 4aeff59108bc5f3084b23237f00a34e9252d76dc
parent:
  - architecture/ui/G6_PEOPLE_SURFACE_V1_0_DECISION.md
  - architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md
  - architecture/world/G5_STABLE_ACTOR_REGISTRY_AND_MATERIALIZATION_V0_2_DECISION.md
  - architecture/world/G5_KNOWLEDGE_PROVENANCE_V0_1_DECISION.md
---

# G6 People Identity Binding + Curation Architecture｜FROZEN

## 1. Decision summary

MW-016 architecture audit is accepted.

People v0.1 uses this ordered vertical:

```text
accepted player-authored Turn
↓
existing World semantic lane
→ stable actor materialization when needed
→ exact accepted-person → stable-NPC identity binding receipt
↓
current-version terminal barrier
↓
existing Information Curator
→ Character / Important Experiences / People in one bounded curation call
↓
information_curation durable currentness
↓
player-safe People projection
↓
card UI
```

Protected rules:

- no display-name authoritative matching;
- no full stable-roster/raw NPC material passed to People curation;
- no People-specific default third model call;
- no new SQLite table;
- no People UI reading raw `world_state` and filtering locally;
- model still decides whether a card is warranted and what player-known information belongs in it.

## 2. Why identity binding is separate from People meaning

There are two different semantic questions:

```text
Identity resolution
→ “accepted text中的这个人，对应本局哪个 stable NPC identity？”

People curation
→ “玩家现在知道这个人什么？是否值得建卡？该怎样更新卡片？”
```

They must not be collapsed into Program name matching.

The World semantic lane already owns stable actor materialization and Program-owned actor identity. It therefore owns the narrow identity-resolution receipt.

Information Curator owns player-facing information meaning and curation. It therefore owns People latest-known snapshots.

An identity receipt does **not** itself create a card, relationship, Knowledge event or disclosure.

## 3. Exact identity rule

Every People card is keyed by one Program-owned stable NPC `local_character_id`.

Model never mints the authoritative ID.

For an existing stable NPC, the semantic request may expose a request-scoped opaque `actor_ref` mapped by Program to the current stable local ID.

For a same-response runtime-created NPC, the semantic result may use a bounded transient `candidate_ref` only for correlation inside that response. Program must:

```text
parse/normalize valid candidate
→ mint/reuse Program-owned stable local ID
→ resolve valid candidate_ref binding to that ID
→ persist only canonical local ID in the receipt
```

`candidate_ref` is never durable identity.

Forbidden:

- display-name equality;
- fuzzy name match;
- first same-name match;
- name-based dedupe;
- UI-local actor lookup;
- silently creating a duplicate stable actor because identity was unresolved.

If two same-name or otherwise ambiguous actors cannot be reliably resolved from the current semantic opportunity, **unresolved means no binding and no People update for that actor**. Accuracy is preferred over a guessed card.

v0.1 does not expand the semantic resolver to a full dump of hidden actor profiles merely to solve rare ambiguity. If real product evidence later shows same-name ambiguity is common, a separate bounded resolver-input decision is required.

## 4. Identity receipt owner

The receipt belongs to the existing Game-local World / `living_world` owner, adjacent to stable actor materialization.

Authorized additive shape:

```text
living_world.people_identity_turns_by_index
  <accepted turn index>
    schema
    game_id
    full accepted Player+GM prefix binding
    receipt id
    status = resolved | empty
    bindings[]
      local_character_id
      accepted GM span provenance
```

Exact names may vary if implementation proves an equivalent narrower shape.

Machine-level requirements:

- old `living_world.v0.1` data without this collection remains valid;
- receipt identity is Program-derived from schema/Game/turn/full prefix/normalized bindings;
- `resolved` may contain zero or more valid bindings; `empty` explicitly records a successfully processed opportunity with none;
- failures/timeouts/cancellation must not masquerade as a successful empty receipt;
- accepted span is validated against the exact current GM text;
- maximum 8 bindings per turn and maximum 600 characters per bound span are approved structural ceilings, not semantic importance thresholds;
- runtime-created actor applicability must still match current accepted origin version;
- receipt grants no Knowledge or player disclosure by itself.

Actor materialization and its same-turn valid receipt must commit through the same existing semantic durable mutation when both exist.

No new SQLite table/schema migration is authorized solely for this feature.

## 5. Same-turn ordering — Scheme A approved

MW-016 Scheme A is approved.

People must not intentionally wait until the next player Turn when the current Turn already established a usable person.

Required ordering:

```text
accepted Turn durable
→ World semantic opportunity reaches terminal state
→ if successful/no-op: durable current receipt is readable
→ Information Curator opportunity may run
```

The current implementation's worker-construction order is not a sufficient barrier.

A narrow coordination seam is authorized.

### Failure policy

World semantic failure/timeout/cancellation must remain fail-soft:

```text
accepted Narrative remains valid
Character / Important Experiences curation may still proceed
People component for that opportunity = no-op / retain previous snapshots
```

An absent/failed identity receipt must never mean “clear all People”.

No new default People-specific repair model call is authorized in v0.1. Render/reopen must never trigger a model call. Existing/future explicit semantic repair policy may be used only if separately authorized by its owner.

## 6. Minimal semantic terminal / epoch extension

To make the barrier real, MW-017 may add the narrowest needed terminal/currentness semantics to the existing World semantic worker:

- exact current Game;
- accepted turn index;
- full Player+GM prefix/version binding;
- current activation/Restore epoch;
- success/no-op/failure/cancel/timeout terminal outcome;
- durable receipt ID when success/no-op was committed.

Restore must invalidate in-flight old-epoch callbacks before they can publish a receipt or unblock a stale People update.

A bounded timeout/cancellation seam is authorized if needed to prevent People coordination from waiting forever.

This does not authorize a generic job scheduler/event bus or a redesign of G5 semantics.

## 7. People Curator input boundary

People curation extends the existing Post-turn Information Curator rather than adding a default separate People request.

Allowed People evidence in the curator request:

```text
current accepted Player input
+ current accepted GM Narrative
+ current safe Character / Important Experiences
+ current People player-known snapshots
+ bounded current identity candidates:
    request-scoped actor_ref
    + exact accepted GM quote/span
```

Program privately owns `actor_ref → exact local_character_id` and receipt dependency.

Do NOT pass to the People curator merely for identity enrichment:

- complete stable actor roster;
- raw `source_projection`;
- `game_local_material` profile text;
- GM-private Character sections;
- NPC-private Knowledge;
- Agency plans;
- hidden World Evolution;
- Source-current content;
- hidden backend changes learned after the player's last information.

The People curator may use prior People snapshots because those are already player-known.

The output may reference request-scoped `actor_ref`; Program resolves it to canonical local ID before persistence. The model still never mints durable identity.

## 8. People durable owner

People latest-known content remains under `information_curation`, not stable actor truth and not a new Relationship Domain.

A backward-compatible new per-turn curation record/result variant is authorized.

Conceptually:

```text
new curation record
→ existing Character snapshot/no-change
→ existing Important Experiences additions
→ people_updates[]
    actor_ref during model response
    ↓ Program resolution
    exact local_character_id
    full latest-known snapshot | tombstone
```

People snapshot product shape may include:

```text
display_name
headline
summary
relationship        # natural-language player-known relationship summary
details[]
```

Model owns the prose and whether to create/update/remove a card.

Program owns only structural limits, identity resolution, currentness and persistence.

Approved structural ceilings for v0.1:

- maximum 8 distinct People updates per curation Turn;
- display_name ≤ 64 chars;
- headline ≤ 160 chars;
- summary ≤ 400 chars;
- relationship ≤ 600 chars;
- details ≤ 8 items, each ≤ 600 chars;
- existing whole-request/response byte ceilings continue to apply.

These are payload safety limits, not importance scores.

Duplicate updates to the same actor in one response are invalid rather than interpreted as ordered semantic operations.

A full snapshot replaces that actor's prior player-known snapshot. `null`/explicit tombstone removes the card. No field-by-field Program merge semantics are authorized.

## 9. Backward compatibility

Existing MW-014/MW-015 curation records and their IDs/parent chain must remain valid.

Do not normalize old records by injecting `people_updates: []` before validating their historical ID.

A new record variant may include its own schema/version and identity-receipt dependency; old and new validated record IDs may form one mixed parent chain.

Projection semantics:

```text
valid current records in accepted order
→ fold Character/Experiences under existing rules
→ fold People by exact stable local ID
→ latest valid snapshot replaces previous
→ tombstone removes
```

People UI receives only player-safe snapshot DTOs and never the internal local ID/receipt/prefix/hash.

## 10. Currentness / Save / Restore / Regenerate

People currentness belongs to current accepted history.

Required behavior:

```text
learn person A → A card appears
learn newer info → A snapshot replaces old one
Restore before A → A disappears
Restore between A-v1 and A-v2 → A-v1 returns
Regenerate/correction replacing source history → stale People update disappears immediately
runtime actor origin replaced → old actor binding/card cannot remain current
reopen → equivalent snapshots rebuild without Provider call
```

People must not reuse the special MW-015 T0 Character baseline recovery rule. Never recover People from displaced future Timeline nodes merely because the same NPC still exists.

Unrelated Agency/World Evolution changes must not silently mutate People cards.

## 11. Opening and legacy policy — v0.1

### Accepted GM-only opening

People v0.1 does **not** extend World semantic + Information Curator processing to GM-only opening.

Reason: supporting opening now would add new startup model work and broaden the already-closed G5 opening/semantic behavior. It is not required to prove the first real People vertical.

Therefore first People cards/updates begin from **player-authored accepted Turns**.

If an opening introduced someone, the card may be established later when a player-authored accepted Turn supplies a normal People opportunity.

This is an explicit v0.1 product limitation, not a claim that opening information is semantically unimportant.

### Existing Games / historical backfill

Do not silently replay/re-analyse all pre-feature history.

Existing Games keep their prior history unchanged and begin accumulating People cards from new accepted player-authored opportunities after the feature is installed.

No Source-current, stable-registry dump or historical model backfill is authorized in v0.1.

A bounded history backfill may be considered later only from real product demand and explicit cost/currentness design.

## 12. Disclosure guarantee and residual model risk

The v0.1 guarantee is structural:

> Given the same accepted player-visible text, current safe People snapshots and identity receipts, changing hidden NPC private material must not directly change the People curator input or UI projection.

The identity resolver may still make a semantic coreference mistake. Exact IDs prevent Program-side name confusion; they do not mathematically prove the model understood prose correctly.

We accept this residual model-semantic risk under:

> **Model freedom first. Reversibility over prevention.**

Do not add a Program semantic judge to eliminate every possible model mistake.

## 13. Implementation sequence

Freeze two executable outcomes:

```text
MW-017
People Identity Bridge + Same-turn Barrier
→ backend only
→ no People UI
→ GPT Independent Review / Engineering PASS required

then

MW-018
People Curation + Card Surface
→ extend existing Information Curator
→ safe People L3 projection
→ card UI, default collapsed / expandable
→ Owner UAT required
```

MW-018 is not authorized to start until MW-017 Engineering PASS proves the identity/currentness seam.

MW-013 Internal Declarative UI Host remains HOLD.

## 14. MW-017 acceptance direction

MW-017 must prove at minimum:

- same-name stable actors never use display-name authoritative matching;
- existing actor binding uses exact Program identity;
- same-turn runtime-created actor can be minted then bound in the same semantic durable commit;
- invalid/transient candidate refs cannot bind the wrong normalized actor;
- unresolved ambiguity produces no binding, not a guess;
- success/no-op durable receipt replays without another model call;
- semantic failure/timeout does not block accepted Narrative or existing Character/Experience curation;
- Restore cancels stale in-flight receipt publication;
- Regenerate/correction invalidates stale receipt currentness;
- no raw actor material/hidden knowledge becomes People-curator evidence;
- no new SQLite table;
- existing G5 actor/knowledge/agency and MW-014/015 semantics regressions remain green.

Only after this Engineering Gate passes should MW-018 be shaped against the proven seam.
