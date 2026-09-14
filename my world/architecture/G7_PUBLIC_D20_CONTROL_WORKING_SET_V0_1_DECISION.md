---
title: my world｜G7 Public d20 Control Working-Set v0.1 Decision
status: FROZEN / CURRENT
version: 0.1
created: 2026-09-14
updated: 2026-09-14
phase: G7 Long-session Context & Knowledge Hardening
owner: Owner + GPT
implementation_consumer: MW-035
triggered_by:
  - MW-033 Narrative Working-Set Orchestrator v0.1
  - MW-034 Information Curator Bounded Recovery
  - G7 Structured Output Recovery Abstraction Gate v1.0
  - post-MW-034 long-session reality audit
---

# G7 Public d20 Control Working-Set v0.1｜FROZEN

## 1. Product problem

Public d20 has two model-facing phases:

```text
control
→ CHECK_REQUIRED / NO_CHECK
→ then Narrative
```

The Narrative phase already uses the G7 Narrative Working-Set owner.

The **control** phase still uses the old path:

```text
full Game-local Opening-era context
+ Expansion rules
+ Inventory
+ fixed recent Conversation window
→ mechanics control model call
```

`include_style=false` correctly removes literary style, but the remaining context still includes broad T0 material such as:

- Opening supplement;
- selected Entry opening seed;
- full World semantic sections;
- full Player Character starting sections;
- every Guaranteed NPC starting section.

At the same time it does not make current long-session Character and current World/Knowledge/Agency/Evolution first-class working-set contributions.

This creates two G7 defects:

1. **currentness risk** — a capability/world change that remains durable after its original Conversation turn falls outside the old recent window may not be visible to mechanics control;
2. **latency/capacity risk** — broad T0 source material is repeatedly resent as one large factual block without the model-capacity-aware P0/P1/P2 selection already proven by MW-033.

The product symptom to prevent is:

> after many turns, d20 control should judge the player's current action from the current character/world/inventory/mechanics, not mostly from the character/world as they existed at New Game.

## 2. v0.1 outcome

For `control` and `control_recovery` only, compose one bounded mechanics working set from canonical current owners and the current validated model capacity.

```text
canonical owner projections
→ mechanics-specific P0/P1/P2 blocks
→ existing Context structural selector
→ exact final-message byte budget
→ control Provider messages
```

Do not create a second Context platform.

Reuse the existing Context working-set selection/accounting engine wherever it is semantically valid; add only the narrow mechanics-consumer composition seam required by this consumer.

## 3. Consumer scope

In scope:

- Public d20 `control`;
- Public d20 `control_recovery`.

Out of scope / unchanged:

- First Opening;
- ordinary Narrative continuation;
- d20 `resolution_narrative`;
- d20 `no_check_narrative`;
- d20 `degraded_narrative`;
- Recommendations;
- Information Curator;
- World semantic / Agency / Evolution.

All d20 Narrative stages continue to use the already-integrated MW-033 Narrative working-set path.

## 4. Ownership

The d20 process owns mechanics semantics and terminal policy.

Context owns only:

- request composition;
- capacity accounting;
- structural include/omit;
- request-local safe diagnostics.

Context does not become mechanics truth, World truth, Character truth, Inventory truth or Timeline truth.

Do not persist final control messages or a Context cache.

## 5. Mechanics working-set tiers

The tiers below are **request structural priorities**, not semantic truth rankings.

### P0 REQUIRED

The request must fail loud before Provider start if these cannot fit whole:

1. mechanics-control system/protocol framing;
2. exact current Player action / active attempt exactly once;
3. exact materialized Expansion mechanics rules required for this d20 capability;
4. minimum current Game/World identity and World/GM instructions;
5. exact selected Entry identity when present;
6. `control_recovery` correction cue only on the second control parse attempt.

Expansion rules are mechanics authority for this capability. They are not silently truncated or omitted to make a request fit.

### P1 CURRENT CONTINUITY

Prefer current durable/accepted state over broad starting background:

1. latest complete accepted Conversation turn;
2. current Character snapshot from the existing Information Curation safe/model-context owner;
3. current World / Knowledge / Agency / Evolution projection from the existing World-turn context owner;
4. factual current Inventory projection;
5. current Public mechanics history/context;
6. remaining recent complete accepted Conversation turns while budget remains.

The current Character block is a derived current continuity snapshot, not a second mechanics truth. It may inform qualitative capability/current identity but cannot override exact durable Public d20 truth, factual Inventory, or accepted current World facts.

### P2 DURABLE STARTING BACKGROUND

When budget permits, include exact Game-local frozen background as whole blocks:

- Opening supplement;
- selected Entry opening seed;
- T0 World semantic/source sections;
- Player Character source sections;
- Guaranteed NPC source sections.

This remains starting inertia/reference, not a replacement for current lived truth.

`literary_style_reference` is excluded entirely from control/control_recovery.

## 6. Intentionally excluded current curation families

v0.1 does not add these as dedicated mechanics-control blocks:

- Important Experiences;
- People;
- Open Threads.

Reason:

- current Character already carries the protagonist's long-term qualitative identity/capability continuity;
- current World + Conversation carry current causal circumstances;
- Inventory and Public mechanics have their own factual owners;
- People is explicitly player-known epistemic material, not authoritative NPC/world truth;
- Open Threads are unresolved-attention state, not mechanics authority.

They may still be indirectly represented when relevant through current accepted Conversation or current World material.

Do not add semantic ranking/retrieval merely to select these families in MW-035.

## 7. Budget

Use current validated runtime model capacity exactly as MW-033 does.

Frozen v0.1 rule:

```text
safe control input bytes = floor(context_token_ceiling × 0.80)
```

Current known profiles therefore yield the same input ceilings:

- 256k → `209715` bytes;
- 1m → `838860` bytes.

Rules:

- count final serialized Provider `messages` UTF-8 bytes;
- no mid-block truncation;
- no partial accepted Turn;
- no partial source section;
- no partial Expansion rule block;
- optional block that does not fit is omitted whole and diagnosed;
- P0 overflow fails loud before Provider start;
- no fallback to a hardcoded capacity when runtime settings are invalid.

This budget is input safety only. It does not add a Narrative output `max_tokens` cap.

## 8. Conversation currentness

Current Player action appears exactly once as the active attempt.

Accepted Conversation contributions:

- only durable accepted entries;
- no failed/cancelled attempt;
- no displaced future after Restore/Regenerate;
- complete Player+GM Turn pair remains atomic;
- GM-only Opening keeps its no-fake-user-message behavior;
- newest accepted Turn may be selected first for fit, then rendered chronologically with all other retained Turns.

Do not reintroduce the old fixed recent-12 contract as the control authority.

## 9. Current Character boundary

Use the existing current curation projection/contract rather than raw `information_curation` storage.

Requirements:

- UI hide/recover preferences do not affect mechanics context;
- no internal durable IDs/request refs enter control merely for debugging;
- no raw Character curation owner JSON dump;
- if current Character is legitimately unavailable, the lane may continue from accepted/current source + World/Inventory/Conversation according to the structural policy; do not fabricate Character.

## 10. Current World boundary

Reuse the existing current/hash-bound World-turn context owner.

It may include current materialized:

- World consequences;
- Actor Knowledge provenance;
- Agency actions;
- World Evolution events.

Only current accepted-hash-matching durable records are eligible.

No raw whole `world_state`, stale branch record or displaced future material may enter the control request.

## 11. Source background boundary

Use the Game-local frozen continuation source projection, not mutable Source current.

The source layer provides inertia/reference only.

Do not restore the old all-or-nothing full Opening projector for control.

The current continuation source blocks already distinguish:

- minimum required Game/World identity/instructions;
- Opening supplement / Entry seed as optional starting background;
- source sections as atomic P2 blocks;
- style as a separate family.

Mechanics control must exclude style and may reuse the remaining continuation blocks under this consumer's tier policy.

## 12. Control recovery

Existing d20 recovery semantics remain frozen:

```text
control attempt 1 parse failure
→ one control_recovery request
→ parse success continues normal CHECK/NO_CHECK flow
→ second parse failure degrades to ordinary Narrative
```

MW-035 does not broaden recovery to Provider failure or add timeout retry.

The recovery request must be rebuilt from current canonical owners and current runtime capacity; do not replay a persisted/stale messages blob.

Existing d20 parser, CHECK/NO_CHECK schema and degradation policy remain unchanged.

## 13. Public d20 authority unchanged

MW-035 does not change:

- RNG timing;
- DC/modifier/stance schema;
- CHECK_REQUIRED / NO_CHECK semantics;
- durable check identity;
- durable NO_CHECK resolution identity;
- narrative-accepted marker/recovery;
- same-action replay/dedup;
- System/Public mechanics projection;
- World semantic grounding from durable mechanical resolution.

No second mechanics truth may be created in Context.

## 14. Diagnostics

Record safe request-local control context diagnostics equivalent to:

- runtime profile/context limit/token ceiling;
- derived safe input bytes;
- final serialized message bytes;
- considered/included/omitted block families;
- included/omitted bytes per family;
- selected accepted Turn count/range;
- whether current Character/World/Inventory/mechanics were included;
- source/NPC-source omission by budget;
- assembly latency;
- control vs control_recovery stage.

No raw Player/GM prose, source body, private NPC material, request refs, IDs, API secrets or full `world_state` in diagnostics.

Existing d20 timing diagnostics remain valid.

## 15. Required evidence

MW-035 focused vertical must prove at minimum:

1. **Long-session Character currentness** — after more than the historical transcript window, a current Character capability not present in retained recent prose still enters control.
2. **Long-session World currentness** — current accepted-hash World/Knowledge/Agency/Evolution material remains available after originating prose is old.
3. **Current Inventory** enters control from the factual owner.
4. **Public mechanics** context remains available from its owner.
5. **Large T0 does not crowd P1** — at 256k, oversized P2 source/NPC background can be omitted while current Character/World/Inventory and latest Turn remain.
6. **1m admits more P2** than 256k for the same fixture where capacity permits.
7. **Final serialized messages <= budget** for every successful control/control_recovery request.
8. **P0 overflow fail loud** — oversized required Expansion rules or required minimum instructions produce zero Provider start and zero RNG use.
9. **Whole-block omission** — no partial source section, NPC card, Opening supplement/seed or accepted Turn.
10. **Style absent** — literary-style canary never enters control/control_recovery.
11. **Active Player action exactly once**.
12. **Restore/Regenerate currentness** — displaced accepted/world material never appears in a later control request.
13. **control recovery** uses the same bounded current assembly and keeps exactly one parse-recovery attempt.
14. **second malformed control** still degrades to ordinary Narrative without a fake d20 result.
15. **CHECK_REQUIRED path** still rolls only after valid control parse, persists once, and Narrative respects durable result.
16. **NO_CHECK path** remains durable/replay-safe.
17. **Narrative phases unchanged** and continue through MW-033 working-set assembly.
18. **No paid Provider required** for deterministic Engineering acceptance.

## 16. Scope boundary

Do not add:

- shared Structured Output retry framework;
- generic all-agent Context platform;
- embeddings/vector DB/semantic retrieval;
- person/NPC semantic ranking;
- new stats/attribute system;
- rule DSL;
- Provider/model routing or fallback;
- output token limits;
- UI redesign;
- new persistence schema;
- World/Knowledge ownership rewrite;
- Package-9 provenance/correction.

## 17. Product promise

> In a long-running Game, Public d20 should judge the current action from the current character, current world, current inventory and current mechanics, while treating large New-Game source material as optional background rather than repeatedly making it the dominant control context.

No Owner UAT is required for MW-035 alone. The planned concentrated Product test remains the first Owner confirmation gate for the accumulated G6 + G7 long-session behavior.
