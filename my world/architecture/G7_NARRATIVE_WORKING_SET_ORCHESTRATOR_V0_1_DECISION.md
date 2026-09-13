---
title: my world｜G7 Narrative Working-Set Orchestrator v0.1 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-13
updated: 2026-09-13
phase: G7 Long-session Context & Knowledge Hardening
owner: Owner + GPT
implementation_consumer: MW-033
triggered_by:
  - MY_WORLD_总体规划路线图_CURRENT.md@v5.1 Package 8
  - MY_WORLD_CURRENT_STATUS.md@v18.0
  - my-world/tests/g3_03/上下文恢复与界面测试.gd retained assertion
  - my-world/tests/g3_05/恢复时间线持久化测试.gd retained assertion
  - G5/G6 production context consumers
---

# G7 Narrative Working-Set Orchestrator v0.1｜FROZEN

## 1. Product problem

`my world` has reached the point where a long-running Game contains several different kinds of durable/current information, while Narrative continuation is still assembled with a pre-G7 strategy.

Current production continuation effectively does:

```text
full frozen Game/T0 World + Character + Guaranteed NPC setup
+ recent current World materialization / Knowledge / Agency / Evolution
+ factual Inventory
+ Public mechanics history
+ full literary style anchor
+ most recent 12 accepted Conversation turns
+ current Player attempt
→ one Narrative request
```

Each producer has some local safety/currentness checks, but there is no single request-level owner answering:

```text
What is mandatory for this Narrative request?
What is still current?
What is authorized for this consumer?
What should be retained when the total working set exceeds the useful request budget?
What was included / omitted, and why?
```

At the same time, G6 already maintains model-curated current Character / Important Experiences / People / Open Threads, yet Narrative continuation does not use those current summaries as a first-class long-session working set.

The result is a two-sided risk:

- **context starvation**: after recent Conversation rolls out of the fixed window, current player-known identity/people/threads may disappear from Narrative continuity;
- **context flooding / stale inertia**: large frozen T0 material and multiple domain projections can be concatenated regardless of their current request value, reducing salience and creating long-session latency/limit pressure.

G7 Package 8 exists to fix this without turning Context into a second World/Knowledge owner or a speculative universal memory platform.

## 2. Evidence correction — the two retained G3 assertions

The two retained G3 regression failures are useful evidence, but their literal assertions predate later reviewed Game Context work.

Historical intent was valid:

> do not restore / inject opaque persisted Provider blobs, raw World JSON, or displaced-future branch truth as model Context.

The current literal checks are now too broad because they reject the presence of any system section named `Current Game Context`. G5/G6 legitimately introduced bounded, derived, current, owner-projected Game Context.

Therefore G7 freezes:

```text
raw / opaque / stale truth in Context = prohibited
legitimate bounded owner-projected current Context = required / allowed
```

MW-033 must update these regressions to the real boundary rather than “fixing” them by deleting Game Context.

## 3. v0.1 Outcome

Create one **Narrative continuation working-set owner** by evolving the existing `src/context` request-assembly responsibility.

For ordinary post-Opening Narrative requests:

```text
canonical owners
→ bounded/current/consumer-authorized contributions
→ Narrative Working-Set Orchestrator
→ deterministic whole-block selection under current model budget
→ final Provider messages + safe diagnostics
```

The Orchestrator owns only:

- request composition;
- cross-domain input budget;
- deterministic structural selection/order;
- whole-block include/omit decisions;
- request-local diagnostics.

It does **not** own semantic truth, player knowledge, actor knowledge, Timeline truth, UI truth or durable memory.

## 4. Existing Context owner evolves; no parallel platform

`src/context` is already the Provider request-material owner. G7 evolves that owner into the Narrative Working-Set Orchestrator rather than creating a second competing Context system.

The existing simple `上下文组装器` may be refactored/split internally, but the semantic ownership remains one Context request-assembly family.

Do not create:

- a second Context DB;
- a universal Memory service;
- a generic Agent framework;
- a vector database;
- embedding retrieval;
- a cross-project prompt framework;
- a second World/Knowledge projection authority.

## 5. Consumer scope — Narrative continuation first

v0.1 applies to **ordinary Narrative continuation after a Game has an accepted Opening/history**.

The first Opening remains on its current full frozen Game-local setup path in MW-033. First Opening has a different job: establish the initial playable scene from exact selected source material before long-session current state exists.

Other model lanes remain unchanged in this work item unless a narrow compatibility hook is required:

- World semantic materialization;
- Information Curator;
- Action Recommendations;
- Public d20 adjudication;
- Agency / World Evolution.

This first consumer is deliberate. Package 8 may later reuse proven Context vocabulary, but MW-033 does not build a universal all-agent Orchestrator.

## 6. Canonical contribution rule

The Orchestrator must not ingest raw `world_state` and then decide what the facts mean.

Every non-Conversation domain contribution must come through the domain's current L3/public projection seam or a new narrow L3 request-context projection owned by that domain.

Required contribution families for Narrative v0.1:

### 6.1 Core Game / Source inertia

From durable exact Game-local setup, expose a continuation-specific bounded source projection.

It must preserve at least:

- Game-local identity / selected Entry identity as needed for exact continuity;
- World instructions;
- GM instructions;
- stable T0 world/source inertia that remains legitimate background;
- literary style reference as explicitly labeled non-factual narrative style material.

It must no longer force every continuation request to resend the entire frozen Player Character + every Guaranteed NPC source section as one inseparable monolith.

Source remains starting-reference inertia, not current lived truth.

### 6.2 Current information curation

Information Curation must expose a model-context projection of the current validated:

- Character;
- Important Experiences;
- People;
- Open Threads.

This projection is derived from the same durable/current curation authority already used by product Surfaces, but is not the UI tree and is not affected by UI hide/recover preferences.

It must:

- exclude durable IDs / presentation keys / request refs unless structurally required and explicitly authorized;
- preserve epistemic wording already present in People/Threads snapshots;
- label People as player-known information, not World actor truth;
- remain accepted-history/Restore/Regenerate currentness-safe.

### 6.3 Current World / Knowledge / Agency / Evolution

Reuse the current World Turn Context projection family, which already filters by accepted-history hashes and domain validity. G7 may adapt its output into contribution blocks but must not weaken those currentness checks.

### 6.4 Inventory / mechanics

Reuse bounded factual Inventory and Public mechanics projections. Narrative Context may consume them as current factual/mechanical material; it does not become their owner.

### 6.5 Conversation

Conversation remains the canonical source for accepted transcript and current attempt.

Recent transcript is a dedicated continuity contribution, not the long-term truth database.

The Orchestrator may choose fewer/more recent complete accepted turns under the global request budget, but:

- current Player attempt is mandatory;
- accepted Player/GM bytes must not be summarized or rewritten by Program;
- complete Turn pairs remain atomic selection units;
- displaced future / cancelled / failed drafts never enter the working set;
- current OOC semantics remain preserved.

## 7. Structural priority, not Program semantic scoring

v0.1 is allowed to use **structural priority classes**, because these express consumer/authority roles rather than Program judgments about story meaning.

Frozen classes:

```text
P0 REQUIRED
→ GM system instructions / protocol
→ current Player attempt / current input mode
→ minimum current Game/World identity + instructions needed to interpret the Game

P1 CURRENT CONTINUITY
→ latest complete accepted Conversation turns
→ current Character
→ current Open Threads
→ current World/Knowledge/Agency/Evolution contribution
→ factual Inventory / current mechanics

P2 DURABLE BACKGROUND
→ Important Experiences
→ current People
→ selected T0 semantic/source background
→ Guaranteed NPC authored source background
→ literary style reference
```

Within one owner, existing domain order/currentness should be preserved unless that owner exposes a narrower bounded contribution API.

The Program must not introduce:

- person importance scores;
- fame/name rules;
- keyword scene matching;
- quest priority scores;
- embedding similarity;
- arbitrary story “importance” classifiers.

When budget pressure requires omission, choose by frozen structural tier + stable deterministic order / recency, not content semantics.

If evidence during implementation shows that one listed P1/P2 family cannot safely fit this structural model without semantic guessing, STOP with evidence rather than inventing a hidden ranking algorithm.

## 8. Working-set budget

The existing runtime settings already expose a validated current model `context_token_ceiling` (`256k` or supported `1m`). MW-033 must use that authoritative metadata as the model-capacity source.

However v0.1 does not pretend to own an exact tokenizer for every Provider/model.

Freeze the conservative rule:

```text
context token ceiling
→ derive safe Narrative input byte budget
→ select whole UTF-8 contribution blocks under that budget
```

UTF-8 byte count is an intentionally conservative upper-bound accounting unit for token count: one model token cannot encode zero input bytes, so `input_bytes <= reserved_token_capacity` cannot exceed that many input tokens. Exact tokenizer-specific optimization is not required in v0.1.

Default Narrative input reservation:

```text
max request input bytes = floor(context_token_ceiling * 0.80)
```

The remaining >=20% of the declared context window is reserved for generated Narrative plus Provider/request overhead. This is not a `max_tokens` output cap and must not artificially shorten GM prose.

If the Provider contract later exposes a stronger exact tokenizer/limit API, this accounting can evolve without changing Context ownership.

Rules:

- count the final encoded request material, not only domain bodies;
- no silent mid-block truncation;
- no partial accepted Turn;
- no half semantic section;
- if a non-P0 block does not fit, omit it and record a safe reason;
- if P0 itself cannot fit, fail the request assembly loudly and send **zero** Provider request rather than silently truncating authority/instructions/current Player input.

## 9. Selection direction

v0.1 must prioritize current continuity before replaying broad starting-source material.

Required direction:

```text
P0 required
↓
P1 current continuity
↓
P2 durable background while budget remains
```

For accepted Conversation, newest complete Turns are selected first and rendered back in chronological order.

For current curation sets, do not use UI hide preferences or Program importance ranking. The curation model has already made the semantic decision that these subjects/threads/experiences are current enough to retain.

For broad T0 source/background, use stable structural/source order and whole blocks. v0.1 may omit lower-tier authored background under pressure; it must expose diagnostics showing that omission rather than pretending the material was included.

This is a bounded first slice. Semantic retrieval of a specific old NPC/place/event from a large source/history is a potential later G7 outcome only after real v0.1 evidence. Do not add embeddings or model-driven retrieval to MW-033.

## 10. Request-local diagnostics

Context selection needs evidence before later G7 tuning.

MW-033 must expose safe read-only diagnostics sufficient to prove, for a request:

- model context ceiling / derived input byte budget;
- final request byte count;
- contribution families considered;
- included block counts/bytes by family;
- omitted block counts/bytes + structural reason (`budget`, `empty`, `not_current`, etc.);
- selected accepted Conversation turn range/count;
- whether current Character/People/Threads/Experiences were present;
- assembly latency measured locally if practical.

Diagnostics must not dump:

- API keys;
- raw private Source/NPC material beyond what the request already legitimately contains;
- raw omniscient world_state;
- hidden durable IDs purely for debug convenience.

Existing Debug/UAT infrastructure may display a compact safe summary later, but MW-033 does not require a new polished player-facing analytics page.

## 11. Restore / Regenerate / reopen

The final working set is always derived from **current** owners at request time.

Requirements:

- Restore cannot retain displaced future Conversation, curation, World/Knowledge/Agency/Evolution or Inventory contribution;
- Regenerate replacement must bind subsequent requests to the replacement accepted prefix/current domain projections;
- reopen must reconstruct the same semantic working set from durable current Game state without a stored Provider-message blob;
- Context working set itself is not durable truth and is not restored as a database object;
- UI visibility preferences never change model Context eligibility.

## 12. G3 regression correction

Update the two stale G3 checks so they prove the original safety property rather than prohibit all Game Context.

They must continue to prove at minimum:

- no persisted Provider-message/context blob is reused;
- no raw whole `world_state` JSON or persistence-internal blob is injected;
- no `materialization_json` / `accepted_turns_json` style storage payload enters Narrative Context;
- Restore/Recovery does not leak the displaced branch marker;
- only current accepted Conversation contributes transcript;
- owner-projected Game/World Context is allowed and demonstrably derived/current.

Do not simply delete the assertions or mark the suites expected-fail.

## 13. Structured Output Reliability boundary

MW-033 does **not** implement the Package-8 Structured Output Reliability layer.

Evidence already differs by lane:

- Recommendations now have a proven bounded one-recovery need;
- Information Curation / World semantic / adjudication have different parser/currentness/failure semantics;
- Narrative is streaming free-form content, not a structured-output lane.

A later executable work item must audit actual machine-schema failures across these lanes and extract only the smallest common reliability primitive justified by production evidence.

Do not use MW-033 to build JSON repair/retry middleware for every model call.

## 14. Explicit non-scope

MW-033 does not authorize:

- vector DB / embeddings / semantic similarity search;
- model-generated memory summaries as new truth;
- universal entity/knowledge graph;
- cross-agent generic Context protocol;
- new durable Context table/cache;
- automatic knowledge correction / Reality Correction Mode;
- Package 9 Provenance/Epistemic system;
- Provider/model routing redesign;
- Narrative output length cap;
- external Creator/Mod Context schema;
- UI redesign;
- full semantic retrieval of arbitrary old history/source;
- broad World/Knowledge/Information Curation ownership changes.

## 15. Engineering acceptance direction

MW-033 Engineering evidence must prove at minimum:

1. ordinary Narrative continuation routes through one Context working-set owner;
2. first Opening behavior remains unchanged and still has the exact frozen Game-local source material it requires;
3. budget comes from validated current model settings and changes correctly between supported 256k/1m profiles;
4. final input accounting includes message/system overhead and never exceeds the derived safe byte budget;
5. P0 overflow fails assembly before Provider start;
6. non-P0 overflow omits whole blocks, never partial Turns/sections;
7. current Character / Experiences / People / Threads can survive beyond the recent transcript window and enter Narrative Context from current curation authority;
8. People context remains explicitly player-known and does not become World actor truth by injection;
9. UI hidden People/Experiences/Threads remain eligible for model Context according to semantic currentness;
10. current World/Knowledge/Agency/Evolution contribution retains accepted-prefix/hash currentness protection;
11. Inventory/mechanics remain their own truth owners;
12. Restore/Regenerate/reopen produce current working sets and no displaced-future leak;
13. recent Conversation selection is newest-first under budget but rendered chronological, with current attempt exactly once;
14. old G3-03/G3-05 failures are replaced by stronger passing raw/stale-leak assertions rather than removed;
15. no raw `world_state`, persisted request blob or Source current is used as a shortcut;
16. request-local Context diagnostics are safe and sufficient to audit inclusion/omission/budget;
17. existing G4–G6 Narrative, OOC, Public d20, recommendation, curation and Restore paths remain regression-safe;
18. Godot 4.7.2 import/export validation remains green.

## 16. Product value acceptance

MW-033 is primarily long-session infrastructure; no immediate Owner UAT is required by itself.

Its product promise is:

> **As a Game gets longer, the GM should continue from the current character/world/matters/people rather than depending mainly on whatever happened to fit in the last 12 turns, while avoiding an ever-growing dump of starting and historical data.**

Automated Engineering PASS cannot prove that a live model feels coherent over a long session. The next concentrated Owner Product test, after sufficient G7 slices, must validate long-play continuity together with the deferred MW-032 corrections.

## 17. Post-MW-033 route

After reviewed MW-033 integration:

```text
collect production-shaped context diagnostics / long-session evidence
→ decide whether next Context slice needs semantic retrieval / source recall
AND separately
→ audit proven machine-schema failure patterns for Structured Output Reliability
```

Do not pre-commit the next executable Work Item until MW-033 evidence is reviewed.
