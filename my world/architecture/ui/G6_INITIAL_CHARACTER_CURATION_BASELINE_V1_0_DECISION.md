---
title: my world｜G6 Initial Character Curation Baseline v1.0 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-06
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
owner: Owner + GPT
triggered_by: MW-015 R2 architecture gap cbe0f12c411f046cc17318fd1be856dcd2c13e43
parent:
  - architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md
  - architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md
---

# G6 Initial Character Curation Baseline｜FROZEN

## 1. Product decision

MW-015 R2 must make the right-side `角色 / Character` Surface materially useful **before the player has completed a new lived Turn**.

The initial Character experience must not depend on the GM opening succeeding first.

```text
Game can be activated
+ frozen Game-local player-safe starting protagonist material exists
↓
model-driven Initial Character Curation
↓
materially useful Character Sheet
```

If opening is pending, cancelled or temporarily failed, Character initialization remains independently eligible.

Atomic Final Create remains Provider-free.

## 2. Semantic authority

The existing frozen rule remains unchanged:

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

The model decides which starting facts belong in current Character and how to summarize them. Program must not classify authored profile prose using keywords, group-title matching, regexes, scores, named-character special cases or per-domain semantic rule trees.

Initial curation uses only frozen Game-local / player-safe protagonist material and concise Character Surface semantics. It does not consult Source Library current and does not require accepted opening Narrative as semantic input.

## 3. Initial baseline is Game/T0-scoped, not Turn-scoped

Initial Character curation is a distinct information-c​​uration baseline:

```text
Initial Character Baseline
→ scope: current Game / frozen T0 starting protagonist material
→ purpose: "开局时 / 尚未发生 lived curation 时，我是谁"

Turn Curation Records
→ scope: accepted lived Conversation history
→ purpose: "经历这些事情以后，现在的我是谁"
```

Do not fabricate an accepted Turn, negative turn index or synthetic Conversation entry merely to reuse the turn-shaped contract.

Do not make the initial baseline depend on a real opening Turn.

## 4. Persistence / owner decision

Reuse the existing `information_curation` owner and existing World/Timeline durable mutation machinery. **No new SQLite table is authorized.**

A narrow schema evolution is authorized so the owner can represent an optional Program-owned initial record, for example:

```text
information_curation
→ schema
→ optional initial
→ turns
```

Exact field naming may differ if Codex proves an equivalent narrower representation, but the semantics below are mandatory.

Backward compatibility is required:

- existing `{schema, turns}` owners remain readable;
- old Games with no initial baseline remain valid and become eligible for bounded initialization;
- adding the initial baseline must preserve existing turn records;
- no Source-current backfill.

## 5. Initial identity / validity

Initial baseline identity is Program-owned and bound to the **normalized frozen Game-local player-safe starting protagonist material** used as model input.

The model must not mint authoritative IDs/hashes.

The initial record must be rejectable when its binding no longer matches the Game-local frozen starting material. A stable hash/fingerprint of the normalized frozen input is an acceptable machine-level binding.

Initial baseline is not bound to accepted Player/GM prefix hashes.

## 6. Turn chain must remain independent

The new initial baseline must **not** become the parent of existing MW-014 turn records if that would invalidate historical turn identities.

Preferred projection semantics:

```text
thin frozen safe fallback
↓
valid model-curated initial Character baseline, when present
↓
current valid lived turn curation records in existing causal order
```

Later turn Character snapshots remain authoritative over the initial baseline when they provide a current Character result.

A turn result with `character = null` means the previous current Character remains, which may ultimately derive from the initial baseline.

Existing turn record IDs / parent linkage should remain valid unless Codex proves this is impossible and STOPs again with evidence.

## 7. Important Experiences boundary

Initial baseline does **not** create Important Experiences from static biography.

There is no accepted lived event to classify at T0 initialization, so initial curation persists Character only. Any structured initial response must therefore contain no milestone additions.

This is a lifecycle/causality boundary, not a Program semantic importance classifier.

## 8. Trigger / failure behavior

Initial curation runs through a bounded post-create / Game-activation information-maintenance lane.

Requirements:

- independent of opening success;
- may wait for Provider availability, but not for opening semantics;
- non-blocking to core Game activation/Narrative path;
- fail-soft and retryable;
- idempotent;
- one valid baseline does not trigger Provider calls on every render/reopen;
- UI refreshes when the baseline commits successfully.

A failed initial curation may temporarily leave the existing thin safe fallback visible, but failure must not corrupt the Game or block play.

## 9. Restore / Regenerate currentness

Initial baseline validity is **Game/T0-bound**, not accepted-history-bound.

Therefore:

- Regenerate of later Narrative does not semantically invalidate a valid initial baseline;
- Restore must remove/revert lived turn curation according to restored history;
- Restore must not import future lived Character/milestones;
- if a restored snapshot lacks the initial baseline but the same frozen starting-material binding still applies, the Runtime may safely preserve/re-attach/re-materialize the same valid Game/T0 baseline because it contains no future lived information;
- avoid repeated Provider calls when a valid baseline result for the same frozen binding is already durably available;
- if no valid baseline result is available, bounded initial curation may run again.

Implementation must prove that this special Game/T0 baseline behavior cannot leak displaced future turn curation.

## 10. Existing Game compatibility

Existing Games that already contain frozen Game-local player-safe starting protagonist material must be eligible without recreation.

Expected product path:

```text
open existing Game
→ no valid initial baseline
→ bounded model initial curation from that Game's own frozen material
→ durable baseline
→ Character Surface refreshes
```

If an old Game lacks sufficient frozen player-safe material, STOP with evidence. Never consult latest Source to retrofit it.

## 11. Product acceptance

After Engineering PASS + integration + Owner-build preparation, Owner should observe:

```text
open Game
→ right 角色 becomes materially useful without first taking a new action
→ information is selected/summarized by the model
→ no obvious possessions/private/debug leakage
→ later meaningful play can evolve the same Character Surface
→ left Player Status Host remains hidden while it has no real HUD contribution
```

This decision does not authorize People / Inventory / 事务 / System / Map implementation or MW-013 Declarative Host.
