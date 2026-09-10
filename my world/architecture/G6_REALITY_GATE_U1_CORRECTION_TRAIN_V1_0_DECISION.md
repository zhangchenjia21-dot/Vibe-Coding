---
title: my world｜G6 Reality Gate U1 Correction Train v1.0
status: FROZEN / CURRENT CORRECTION
version: 1.0
created: 2026-09-10
updated: 2026-09-10
phase: G6 V0 Core Closure correction
owner: Owner + GPT
triggered_by:
  - docs/uat/G6_V0_CORE_CLOSURE_REALITY_GATE_U1.md
  - docs/uat/G6_V0_CORE_CLOSURE_REALITY_GATE_U1_FINDINGS.md
implementation_consumer: MW-032
supersedes_when_conflicting:
  - architecture/ui/G6_PEOPLE_SURFACE_V1_0_DECISION.md stable-actor-only People identity requirement
  - architecture/ui/G6_PEOPLE_KNOWN_PERSON_ELIGIBILITY_UAT_CORRECTION_V1_0_DECISION.md requirement that every People subject already have authoritative stable actor identity
  - architecture/ui/G6_OPEN_THREADS_SURFACE_V1_0_DECISION.md nullable pass-through/no-stable-thread-identity shape
  - architecture/ui/G6_INTERNAL_DYNAMIC_UI_HOST_V0_1_DECISION.md Package-6-only restriction on Open Threads hide eligibility
  - architecture/ui/G6_MODEL_CURATED_SURFACE_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md Package-6 implementation-placement restriction, only for Threads now proven by UAT
---

# G6 Reality Gate U1 Correction Train｜FROZEN

## 1. Owner outcome and route

Package-7 U1 real play exposed four bounded defects. Owner explicitly ended the test and instructed:

> “我不想继续测试了，你修吧，修完了直接继续主线等下一次测试”

Formal result:

- U1 ends as **PASS_WITH_NOTES / BOUNDED CORRECTION REQUIRED**;
- one correction train fixes U1-F01 through U1-F04;
- there is **no immediate Owner re-UAT** after this train;
- Engineering Review + integration advance directly to G7 Package 8;
- G6 Product confirmation is deferred, not waived and not falsely recorded as Product PASS;
- the next later concentrated Product test must include these corrected behaviors.

This decision authorizes only the four U1 corrections below.

## 2. Shared authority

All corrections preserve:

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

And:

> **World Truth != actor Knowledge != human-player disclosure.**

Program must not repair these findings with keyword routing, name matching, fame tables, importance scores, turn-age expiry, event-type rules or fixed semantic classifiers.

## 3. U1-F01 — accepted Opening must receive semantic/bootstrap processing

### 3.1 Failure

Current World semantic materialization skips a GM-only Opening because its accepted `player_text` is empty. Current Initial Character Curation is intentionally Character-only. Therefore an Opening can visibly establish possessions, unresolved matters or important known persons while `行囊` / `事务` / People supporting identity remain empty until an unrelated later player turn.

### 3.2 Corrected rule

A successfully accepted GM Opening is a legitimate **opening semantic/bootstrap opportunity**.

Required flow:

```text
accepted GM Opening
→ bounded World semantic materialization
→ factual Inventory / legitimate runtime actor identity & bindings / durable consequences when actually established
→ bounded Information Curation for opening-visible information
→ Character / Experiences / People / Open Threads current projections refresh
```

Opening remains different from a protagonist action:

- empty Player text is not Character-choice evidence;
- Opening does not invoke Public d20;
- Opening does not become Agency foreground action;
- no synthetic Player action is fabricated;
- no default/fake Inventory or Threads are created merely to fill UI.

The same existing background Provider lanes should be reused. No opening-only duplicate semantic database or second truth owner is authorized.

### 3.3 Currentness / replay

Opening processing must be bound to exact current Game + accepted Opening version/hash and remain:

- fail-soft/non-blocking to Narrative acceptance;
- idempotent;
- stale-safe under Regenerate/Restore;
- no displaced-future rehydration;
- reopen must reuse durable valid results and not automatically replay Provider work merely because the Game reopened.

Existing historical Games are not exhaustively backfilled. A newly accepted Opening on the corrected path is the primary case; narrowly reusing already-durable matching records is allowed.

## 4. U1-F02 — People subject identity must not require prior World actor identity

### 4.1 Product correction

Freeze the distinction:

```text
player-known person subject / referent
!=
authoritative current-world stable actor
```

A People card answers what person the protagonist/player currently knows about. It does **not** by itself assert that this Game has already verified a corresponding physical World actor.

Therefore:

> **A pre-authored Character Card or pre-existing stable actor identity must not be an implicit ticket for People eligibility.**

A concrete person legitimately present in accepted player-visible information may become a People subject even if:

- the protagonist has not physically met them;
- the current World has not yet verified their existence/identity;
- no Character Card exists;
- only sparse information is known.

This specifically supports reputation/history/memory cases such as historically or socially important public figures.

### 4.2 Importance remains model-owned

Historically/socially/politically prominent persons are a **strong model semantic default candidate** for persistent People memory when the protagonist has actually learned/remembered them. Sparse cards are acceptable.

Program MUST NOT implement:

- historical-name/fame allowlists;
- fame or importance scores;
- `named person always card`;
- encounter-count thresholds;
- current-scene priority;
- display-name/fuzzy/first-match identity authority.

The model may still decide an incidental or low-value mention is not worth a card.

### 4.3 Stable People-subject identity

People gains a small Program-owned **People subject identity** inside the player-information domain.

This is not a second World actor registry and not a universal entity graph.

A People subject may be:

```text
A. actor-linked
   subject identity + exact stable actor link

B. player-known referent only
   subject identity + no authoritative actor link yet
```

Program owns the durable subject identity. The model never mints the durable ID.

New referent identity must be grounded in exact accepted player-visible evidence, preferably an exact accepted Player/GM source span + current accepted version. Deterministic identity material must not rely on display name equality.

Legacy actor-keyed People cards remain readable. For compatibility, their existing exact actor ID may continue to serve as the People-domain subject identity so existing cards and presentation preferences are not destructively migrated.

### 4.4 Request-scoped refs / linking

Model-facing curation may receive request-scoped opaque refs:

- `person_ref` → an existing current People subject;
- `actor_ref` → an exact current stable actor when available;
- exact player-visible source span/quote for a newly learned referent.

The model may semantically decide that an existing People referent and an exact actor reference are the same person. Program may then persist an exact link after validating both request refs.

No display-name matching is allowed for this link.

Linking must preserve the existing People subject identity/presentation identity where possible; learning that a remembered person corresponds to a verified actor must not make the card look like a brand-new unrelated card.

If identity is ambiguous, preserve separate subjects rather than guessing a merge.

### 4.5 Epistemic protection

A People referent learned from memory/reputation does not become:

- World truth;
- a Stable Actor Registry member;
- Agency-eligible;
- an NPC knowledge target;
- proof that a historical claim is true in this current world.

The People snapshot should reflect uncertainty when that uncertainty is part of player-visible evidence.

## 5. U1-F03 — Recommendation availability bounded recovery

### 5.1 Failure

The current recommendation lane marks an accepted-history prefix as attempted before the request. A malformed exact-five response, transient Provider failure or timeout can therefore leave an entire turn at `unavailable` with no recovery until another accepted lifecycle event occurs.

### 5.2 Corrected rule

Preserve the strict five paired recommendations contract and add **bounded same-prefix recovery**:

```text
current accepted prefix
→ initial recommendation request
→ recoverable terminal failure
→ at most ONE automatic recovery request for the same still-current prefix
→ ready OR final unavailable
```

Maximum total recommendation attempts for one unchanged prefix: **2**.

Eligible recovery classes include transient Provider failure, timeout and malformed/invalid recommendation response where a retry can reasonably produce a valid exact-five result. If the existing Provider seam can reliably distinguish a non-recoverable configuration failure such as missing credentials, it should fail immediately rather than waste a retry.

No retry for:

- stale request displaced by newer accepted history;
- foreground interruption/new player attempt;
- deliberate cancellation caused by foreground lifecycle;
- invalid/missing input material that cannot improve by repeating the same request.

Recovery must re-check current prefix/epoch/foreground before starting. No hidden infinite loop, exponential retry framework or fallback hard-coded actions.

Free-form input remains primary and never blocks on recommendations.

## 6. U1-F04 — Open Threads active lifecycle review + stable identity + Player hide

### 6.1 Semantic maintenance failure

The existing prompt technically permits removal, but UAT shows that nullable `open_threads=null` / passive carry-forward creates too much inertia. Threads can accumulate even after completion/expiry.

Corrected model responsibility:

> **At every legitimate lived/opening curation opportunity, the model actively re-reviews the current Open Threads set.**

It should semantically remove threads that are completed, resolved, superseded, invalidated, stale, or no longer worth continued player attention; update threads whose current state changed; and keep threads that genuinely remain unresolved.

This is not Program automation. Program MUST NOT use keyword completion rules, age/turn thresholds, inactivity expiry, priority scores or Quest states.

### 6.2 Stable Thread identity

Open Threads now has a proven need for stable item identity.

Add Program-owned stable `thread_id` inside the information-c​​uration domain. It exists for:

- exact update continuity across curation turns;
- player presentation visibility preference;
- Restore/Regenerate currentness.

It is not a Quest ID, World entity or model-authored identifier.

Model-facing current Threads use request-scoped opaque `thread_ref` mapped privately to current stable `thread_id`.

Recommended bounded new lived shape is an explicitly reviewed full current snapshot:

```json
"open_threads": [
  {
    "thread_ref": "request-scoped existing ref or null for a new thread",
    "title": "...",
    "summary": "...",
    "details": ["..."]
  }
]
```

Semantics:

- existing item returned with valid `thread_ref` = keep/update same stable Thread;
- existing current Thread omitted from the reviewed output = semantically remove it;
- `thread_ref=null` = model proposes a new Thread; Program mints a stable identity;
- `[]` = no current unresolved matters.

Equivalent narrower operation syntax is acceptable only if it proves the same explicit per-opportunity review and stable identity semantics. A general Quest operation framework is not authorized.

### 6.3 Backward compatibility

Current `information_curation_lived.v0.3` records remain readable.

When a legacy no-ID current Thread set first transitions into the new schema, Program may expose temporary request refs backed by identity derived from **validated legacy curation record identity + item ordinal** for that transition. This is a structural migration bridge, not title/text identity. Once an item is kept into the new schema, its stable `thread_id` persists across later updates.

Do not destructively rewrite old Timeline records and do not identify legacy threads by title/content matching.

### 6.4 Thread hide/recover

Once stable Thread identity exists, the already-approved cross-surface visibility rule now applies to `事务`.

Threads join the Host-level hideable set:

```text
事务 card → 隐藏
已隐藏 (N) → current hidden Threads
恢复显示 → current content
```

Hide semantics are identical to People/Important Experiences:

- presentation only;
- does not complete/delete the Thread;
- does not feed model importance or completion signals;
- hidden Thread may continue updating;
- model update does not auto-unhide;
- survives reopen;
- Restore does not rewind the hide preference;
- recovery displays current semantic content;
- zero Provider calls / zero gameplay mutation from hide/recover.

The presentation preference sidecar must evolve backward-compatibly so existing `ui_visibility.v0.1` People/Experience choices remain readable and preserved while Threads are added. Do not create a second preference owner.

## 7. Cross-correction sequencing

Preferred implementation order inside one work item:

```text
1. evolve durable/normalized People + Thread information identity contracts compatibly
2. allow accepted Opening semantic materialization + opening curation opportunity
3. wire People referent creation/linking and Thread active review
4. extend Dynamic UI visibility preference to stable Threads
5. add bounded recommendation retry
6. regression/currentness/build validation
```

This is one correction train because all four findings are required to stabilize the same V0 Core loop before moving on. Codex may organize commits internally, but must return one reviewed candidate lineage.

## 8. Protected invariants

Must preserve:

- raw accepted Narrative bytes and Conversation authority;
- free-form action primary;
- typed OOC semantics and OOC exclusion from protagonist/world-lived mutation;
- Public d20 Program truth / no-reroll;
- factual Inventory event semantics and exact item refs;
- one existing World semantic Provider lane, not a new Inventory/People bootstrap service;
- one Information Curator lane for Character/Experiences/People/Threads;
- Character soft evidence rules and sparse Important Experiences;
- player-safe disclosure boundaries;
- Save/Restore/Regenerate currentness and stale-future isolation;
- Internal Dynamic UI remains presentation-only;
- existing People/Experience visibility preferences remain valid;
- Debug remains read-only;
- ordinary gameplay typography >=20px.

## 9. Explicit non-scope

MW-032 does NOT authorize:

- general Context Orchestrator / long-session retrieval (G7);
- general Structured Output Reliability framework beyond the bounded recommendation retry needed for F03;
- universal entity/knowledge graph;
- general epistemic belief system or Reality Correction Mode;
- Quest engine, quest state/progress/rewards/priority/deadline framework;
- manual Thread complete/delete/edit controls;
- People Shared History;
- Organizations/Factions;
- map/Visual Runtime;
- external Mod/Source UI schema;
- generic Action Intent;
- Inventory hide;
- System hide;
- unrelated G3 Context debt or Shell decomposition.

## 10. Engineering acceptance

Independent Engineering evidence must prove at minimum:

1. accepted GM-only Opening receives exactly the intended bounded semantic/bootstrap opportunity without fabricating Player action;
2. Opening-established factual possession can enter Inventory before the first player action; absence of possession stays empty;
3. Opening-established unresolved matter can enter Threads before the first player action; absence stays empty;
4. Opening-known person can become a People referent without Character Card/stable World actor prerequisite;
5. player-known referent alone does not enter Stable Actor Registry/Agency/World truth;
6. historically/socially important referent may receive a sparse card by model judgment, without Program fame/name rules;
7. exact actor linking, when it later occurs, uses refs/validated identity rather than name matching and preserves subject continuity;
8. legacy actor-backed People remains readable and existing presentation hide identity is preserved where applicable;
9. same-name/ambiguous persons are not automatically merged;
10. every new curation opportunity explicitly reviews current Threads; keep/update/remove/new semantics are stable-ID based;
11. legacy v0.3 Threads remain readable and transition without title/text identity;
12. stable Thread update preserves identity and presentation key;
13. Thread hide/recover is presentation-only, persists reopen, does not rewind on Restore and does not auto-unhide after semantic update;
14. old `ui_visibility.v0.1` sidecars retain People/Experience hides after preference schema evolution;
15. System/Inventory remain non-hideable;
16. Recommendation initial malformed/transient failure can recover once on the same current prefix and then succeed;
17. repeated failure stops after maximum 2 same-prefix attempts;
18. foreground/stale/cancel lifecycle prevents obsolete automatic retry;
19. no hard-coded fallback recommendation actions and strict five `{label,draft}` success contract remains;
20. no extra default Provider lane is introduced beyond the already-existing semantic/curation/recommendation lanes;
21. Restore/Regenerate/reopen currentness remains correct across Opening semantic records, People subjects, Threads and Inventory;
22. existing Package 0–6 focused regressions remain green except exact-proven retained baseline debt;
23. 960×540 / 1280×720 / 1920×1080 remain usable with >=20px ordinary text;
24. Godot 4.7.2 final import + fresh Windows export + ValidateExportOnly pass.

## 11. Post-implementation route

After Codex return:

```text
GPT Independent Review
→ if Engineering PASS: non-force integration into main
→ NO immediate Owner build / NO immediate re-UAT
→ record G6 Engineering Core Complete / Product Confirmation Deferred
→ activate G7 Package 8 Long-session Core
```

The next later concentrated Product test will validate MW-032 together with G7 work. Only Owner may eventually declare `V0 Core Game Loop = PRODUCT PASS`.