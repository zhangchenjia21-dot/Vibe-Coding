---
title: my world｜G6 Important Experiences Sparse Milestone UAT Correction v1.0
status: FROZEN / CURRENT CORRECTION
version: 1.0
created: 2026-09-07
updated: 2026-09-07
phase: G6 RPG Core Closure
owner: Owner + GPT
triggered_by: my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
supersedes_when_conflicting:
  - architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md
---

# Important Experiences Sparse Milestone UAT Correction｜FROZEN

## 1. UAT finding

Extended Owner play showed `重要经历 / Important Experiences` behaving like a per-turn recap: ordinary movement, questioning, routine scene progression and other small events were repeatedly appended as if they were life milestones.

This violates the frozen product question:

> **“我是怎样走到现在的？”**

Important Experiences is selected protagonist life history, not a rolling summary of recent turns.

## 2. Corrected semantic target

The model remains the semantic owner, but the prompt must make the intended sparsity unmistakable:

```text
Most ordinary accepted turns
→ experiences = []

A real life-shaping milestone
→ 0..N concise milestone additions
```

A candidate experience should normally deserve long-term retention because omitting it would materially weaken a future explanation of how the protagonist became the person they are now, what major life path they entered/left, or what irreversible/significant personal turning point occurred.

This is a semantic orientation for the model, **not** a Program scoring rule.

## 3. Model freedom first

Do not implement:

- event-type whitelist/blacklist;
- keyword/regex milestone classifier;
- importance scores;
- every-N-turn thresholds;
- minimum elapsed time before a change can matter;
- fixed categories such as “only war/marriage/death counts”.

A quiet or apparently ordinary event may still be a milestone in context. Conversely, combat, meeting a named NPC, passing a check, receiving information, travelling or finishing a small task is not automatically important.

The model decides importance from accepted context.

## 4. Character remains independent

A turn may legitimately change current Character without adding an Important Experience.

```text
Character changed
!=
Milestone required
```

Likewise, an Important Experience may be added even if the current Character wording does not need to change in the same turn.

Do not force synchronized mutation.

## 5. No IA change in this correction

This R1 does **not** authorize:

- adding `简要回顾`;
- moving `重要经历` under Character;
- removing the existing top-level tab;
- changing navigation order;
- adding timeline/history UI.

Those are separate product decisions. This correction only restores the existing Important Experiences concept to its intended semantic meaning.

## 6. Persistence / currentness

Existing curation storage, IDs, Timeline, Save/Restore/Regenerate/reopen behavior remain unchanged unless a narrowly necessary backward-compatible correction is proven by code evidence.

No historical rewrite/backfill is required. Existing over-generated historical entries may remain in old saves; this correction governs new lived curation going forward. A future explicit cleanup/edit capability is separate.

## 7. Acceptance direction

Engineering must prove no structural regression to Character / People / curation persistence/currentness.

Real Provider validation must include at least:

1. a clearly ordinary turn where `experiences=[]` is a reasonable result;
2. a clearly life-shaping turn where the model can add a concise milestone;
3. no Program heuristic deciding which of those two cases is important.

Final product acceptance remains Owner focused re-UAT after the Package 0 correction train.
