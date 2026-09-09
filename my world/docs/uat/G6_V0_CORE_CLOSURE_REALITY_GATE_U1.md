---
title: my world｜G6 V0 Core Closure Reality Gate U1
status: OWNER UAT PREP / BUILD NOT YET INSTALLED
version: 1.0
created: 2026-09-09
updated: 2026-09-09
phase: G6 Package 7 V0 Core Closure Reality Gate
owner: Owner
reviewer: GPT
reviewed_implementation_main: 69ac2030b90f4165deb2ecb5302e3743422af585
owner_build_pck_sha256: pending
owner_verdict: pending
---

# G6 V0 Core Closure Reality Gate｜U1

## 1. Purpose

This is the first concentrated Owner reality gate for the complete G6 V0 core loop.

It is not a synthetic engineering checklist and not a request to retest every historical task in isolation.

Primary Product question:

> **Can the current build sustain a coherent, free, understandable AI-RPG session in which Narrative, world consequences, actors, information surfaces, mechanics, factual Inventory, Dynamic UI and Save/Restore behave like one game rather than disconnected features?**

G6 closes only if Owner explicitly judges:

> **V0 Core Game Loop = PRODUCT PASS**

Engineering PASS from earlier packages does not substitute for this verdict.

## 2. Frozen build lineage for prep

Reviewed/integrated implementation main before Owner build preparation:

`my-world/main@69ac2030b90f4165deb2ecb5302e3743422af585`

This main includes reviewed MW-030 lineage:

- Formal Base: `396bfcc0c91cdff6e6816795826b95fa0c0d358c`
- MW-030 Production Implementation: `2d27860ae2123092d83684523df6a4e0b680c635`
- MW-030 Submitted Candidate: `b1dd4ee9884aaabb0bcd5a702206bde93643f406`
- MW-030 Independent Review: `bdb838cb37719b89feea25c145f37f732cf25ec7`
- MW-030 Integration Verification / current main: `69ac2030b90f4165deb2ecb5302e3743422af585`

Owner build PCK hash remains pending until bounded build prep completes.

No Product verdict is allowed before the exact Owner build is recorded here.

## 3. What this UAT combines

This concentrated gate intentionally absorbs deferred Product evidence from:

- Package 2 — OOC / GM Guidance, Character-guided Recommendations, MW-026 cleanup;
- Package 3 — Open Threads / 事务 semantic usefulness;
- Package 4 — System/Public d20 history usefulness and continuity;
- Package 5 — factual Inventory extraction and natural GM use;
- Package 6 — shared Dynamic UI Host consistency and People/Important Experience visibility preference.

Already Product-PASS Package-0/1 work remains regression context rather than being reopened by default.

## 4. Natural-play coverage target

Owner should play naturally rather than force each feature immediately. Across the session, aim to encounter at least:

- roughly 20–30 ordinary role-play turns;
- at least one newly established continuing NPC;
- at least one OOC / GM Guidance exchange;
- at least one real Public d20 CHECK;
- at least one factual Inventory ADD and a later UPDATE or REMOVE where story circumstances naturally support it;
- at least one Save + exit/reopen/Continue;
- at least one Restore;
- at least one meaningful free-form action that differs from the five recommendations;
- multiple right-side information surfaces used during play;
- at least one People or Important Experience hide + recover action;
- Debug Mode used only when it helps explain whether a background lane actually changed/failed.

Do not distort the story merely to satisfy coverage. If a naturally required event does not occur after substantial play, that itself may be useful Product evidence.

## 5. Product observations that matter most

### Narrative / freedom

- Narrative remains the primary visual and experiential surface.
- Free-form action remains genuinely primary; recommendations feel optional.
- GM responses remain coherent with durable world/mechanics/Inventory truth.
- OOC feels like speaking to the GM rather than the protagonist acting in-world.

### Living world / actors

- accepted consequences continue mattering instead of being forgotten after one turn;
- continuing NPCs feel persistent enough to support play;
- People cards represent useful latest-known information without becoming omniscient dossiers.

### Character / information curation

- Character feels like the current protagonist rather than a mutation log;
- Important Experiences remain genuinely sparse milestones rather than turn summaries;
- Open Threads capture useful unresolved matters and can remain quiet/remove resolved matters;
- model curation is not visibly trapped by Program keyword/importance heuristics.

### Mechanics

- a real d20 CHECK has understandable stakes/result;
- later GM/OOC does not deny an already public Program result;
- failure/success has believable narrative continuity;
- System history remains useful without becoming an internal log.

### Inventory

- only actually established possessions enter structured Inventory;
- current item state feels factual and useful rather than arbitrary extraction noise;
- later GM/mechanics can naturally respect/use current Inventory;
- items do not persist after story-established transfer/loss merely because they were once mentioned.

### Dynamic UI / visibility

- Character / Experiences / People / Threads / Inventory / System still feel semantically distinct even though they share one Host;
- text remains comfortable/readable and Narrative space remains dominant;
- People and Important Experiences hide/recover reduces clutter without deleting semantic information;
- hidden People can update without automatically becoming visible;
- Restore does not rewind the Player's hide preference;
- System/Inventory do not accidentally expose generic hide controls.

### Durability / reversibility

- Save/reopen preserves the current game coherently;
- Restore rewinds Timeline-owned World/Conversation/information/mechanics/Inventory truth;
- non-Timeline presentation preference behaves independently as designed;
- displaced future information does not leak back into current play.

## 6. Debug usage

Debug Mode is a UAT aid, not a required permanent play style.

Use it when a product symptom needs explanation, for example:

- did World semantics change or no-change?;
- did Character/Experiences/People/Threads curation commit?;
- did mechanics record CHECK/NO_CHECK/failure?;
- did Inventory change/no-change/fail?;
- did Recommendations fail/stale?;
- did Restore clear the prior diagnostic epoch?

Debug evidence must not override the actual Product experience. A technically correct lane can still correspond to a poor player experience.

## 7. Finding handling during U1

Owner may send findings incrementally.

GPT should accumulate them and distinguish:

- Product blocker;
- bounded defect worth correcting before G6 exit;
- known retained debt appropriate for G7+;
- subjective preference / later polish;
- non-issue / intended behavior.

Do not interrupt the session with a new implementation task for every minor finding. Prefer one bounded correction train after Owner ends the session, unless a hard blocker prevents meaningful continuation.

## 8. Exit outcomes

### PRODUCT PASS

Owner explicitly accepts the complete V0 core loop. G6 may close and route advances to G7 Long-session Context & Knowledge Hardening.

### PASS_WITH_NOTES / bounded correction required

Core loop is accepted in direction but one bounded correction train is necessary before final G6 Product PASS.

### FAIL / core blocker

A defect materially breaks freedom, continuity, truth/currentness, usability or reversibility. G6 remains open and GPT shapes the smallest correction that addresses the failed outcome.

No automated test or GPT review may announce PRODUCT PASS on Owner's behalf.

## 9. Current state

**OWNER UAT PREP / BUILD NOT YET INSTALLED**

Next step:

1. bounded Owner build prep from exact reviewed `my-world/main`;
2. record exact Owner checkout SHA + PCK hash here;
3. change state to `OWNER UAT ACTIVE`;
4. Owner launches via the existing `D:\AI\Projects\my-world\run-game.cmd` path and plays normally.