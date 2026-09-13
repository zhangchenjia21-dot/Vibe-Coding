---
title: my world｜G7 Information Curator Bounded Recovery v1.0 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-13
updated: 2026-09-13
phase: G7 Long-session Context & Knowledge Hardening
owner: Owner + GPT
implementation_consumer: MW-034
triggered_by:
  - MY_WORLD_总体规划路线图_CURRENT.md@v5.2 Package 8
  - MY_WORLD_CURRENT_STATUS.md@v18.3
  - MW-032 recommendation bounded recovery evidence
  - Public d20 control recovery evidence
  - post-MW-033 machine-schema lane audit
---

# G7 Information Curator Bounded Recovery v1.0｜FROZEN

## 1. Audit conclusion

Package 8 calls for Structured Output Reliability, but the current production lanes do **not** justify a universal structured-output framework yet.

The post-MW-033 audit finds materially different terminal semantics:

| Lane | Current response policy | Recovery semantics |
| --- | --- | --- |
| Recommendations | strict five-action machine schema | initial + at most one bounded recovery for recoverable malformed/timeout/transient failure; final unavailable |
| Public d20 control | strict CHECK/NO_CHECK machine schema | one malformed-control recovery; second parse failure degrades to ordinary Narrative without inventing a check |
| Information Curator | strict Character/Experiences/People/Threads schema | **no automatic recovery**; malformed/timeout/provider failure ends that curation opportunity until explicit retry seam |
| World semantic materialization | strict core changes schema plus independently fail-soft optional fields | no automatic recovery; different factual/receipt semantics and existing structural compatibility behavior |
| World Evolution | strict hold/advance schema | intentionally best-effort; comments explicitly freeze no automatic retry, failure == no event/hold |
| Agency execution | per-actor hold/act schema | intentionally best-effort per actor; failed actor simply does not commit this cycle |

Therefore:

> **Do not extract a generic Structured Output Reliability layer now.**

There are not yet three sufficiently equivalent consumers with the same request lifecycle and terminal semantics. A premature abstraction would either flatten legitimate lane differences or grow callbacks/policy hooks into a generic framework.

Consumer-before-abstraction applies: harden the next real consumer locally, collect evidence, then reassess.

## 2. Why Information Curator is the next consumer

Information Curator is the best next reliability slice because one response maintains four long-session player-information surfaces:

- Character;
- Important Experiences;
- People;
- Open Threads.

Its current process already has:

- strict parser and bounded response size;
- one background Provider lane;
- 120-second timeout;
- Restore epoch isolation;
- accepted-prefix + parent currentness checks;
- durable success records that prevent reopen replay;
- an explicit `retry_pending()` seam for manual repair.

But a recoverable malformed response, timeout or transient Provider failure currently ends the attempt immediately. `_attempted` then prevents normal same-runtime automatic replay. For lived curation this can leave Character/People/Threads stale for that accepted opportunity; for initial Character curation it can leave the useful pre-action Character baseline unavailable until explicit retry.

This is a concrete reliability gap at an already-proven consumer, not a platform-design exercise.

## 3. v1.0 outcome

For each still-current Information Curator opportunity, allow at most:

```text
initial Provider start
+
one bounded recovery start
=
maximum 2 Provider starts
```

Apply to both Curator request modes owned by the same process:

1. initial Character baseline curation;
2. lived Opening/action curation.

Recovery is transport/request lifecycle only. It does not alter Curator semantic ownership, parser schema, durable curation format or downstream UI.

## 4. Recoverable terminal classes

A recovery is allowed only when all currentness guards still hold and the first attempt ends as one of:

- `malformed_response` from the strict lane parser;
- Curator-owned timeout;
- transient Provider failure.

The Provider failure signal must preserve/classify the provider status instead of collapsing every failure to one generic code.

Known permanent configuration/input failures are not recoverable, including equivalent statuses for:

- missing credential/key;
- invalid persisted settings/profile;
- unknown/incompatible context limit or reasoning/profile configuration;
- deterministic input oversize;
- deterministic response oversize;
- invalid frozen/profile/storage prerequisites.

Do not retry persistence commit failure, stale history/parent, Restore invalidation, shutdown/cancel, or semantic-dependency/currentness failure.

If a provider returns an unknown failure status, treat it as transient only if the existing Provider contract classifies it as transport/transient; otherwise fail closed rather than building a permanent-code blacklist that silently assumes every unknown error is recoverable.

A small lane-local classifier is allowed. Do not create a global Provider-error ontology in MW-034.

## 5. No parser repair

Keep the Information Curator parser strict.

MW-034 must not add:

- Markdown-fence stripping;
- prefix/suffix JSON extraction;
- regex repair;
- missing-field fabrication;
- semantic coercion;
- schema relaxation;
- fallback Character/People/Thread content.

Recovery asks the model again; Program does not rewrite a malformed answer into truth.

The existing Curator response schema remains authoritative.

## 6. Recovery request

The recovery attempt uses the **same current semantic opportunity** and the same bounded source material, but should include one small system-owned correction cue equivalent to:

> the previous machine response was unusable; return only the exact required JSON schema, with no Markdown or explanation.

Do not include the malformed raw response in the recovery prompt.

Do not expose parser internals, persistent IDs, private bindings, durable hashes, or raw world state merely to improve retry success.

For lived curation, request-scoped People/Thread refs may be regenerated for the recovery attempt. They remain request-local and must never be matched by display name. The recovery must validate against the refs generated for that specific recovery request.

## 7. Attempt/currentness identity

One logical Curator opportunity owns one recovery budget.

### Initial opportunity identity

Bound to:

- current Game;
- current initial profile binding;
- current Runtime/Restore epoch.

### Lived opportunity identity

Bound to:

- current Game;
- accepted source index;
- exact accepted prefix at that index;
- current curation parent identity;
- current Runtime/Restore epoch.

The second attempt must not create a new logical curation opportunity or bypass the `_attempted`/successful-record rules.

## 8. Callback isolation is mandatory

The current Curator connects Provider callbacks for the worker lifetime. Introducing retry without request isolation would allow a late callback from attempt 1—especially after timeout/cancel—to terminate or publish attempt 2.

MW-034 must establish request-attempt isolation equivalent in strength to the proven recommendation/semantic lanes:

- monotonic request serial or equivalent attempt token;
- callbacks bound to the serial/attempt;
- late delta/completed/failed/cancelled from an old attempt are ignored;
- before starting recovery, the old transport is terminal/disconnected or otherwise provably unable to mutate the new attempt;
- Restore/shutdown increments/invalidate the epoch and makes every prior callback stale.

Do not rely only on `_active` being non-empty.

## 9. Recovery scheduling / transport discipline

### Malformed completed response

The transport is terminal; if current and first attempt, schedule one recovery.

### Timeout

The Curator owns timeout. Recovery must not start while the timed-out transport can still emit an unscoped terminal into the new attempt. Cancel/disconnect old request safely, then start recovery only through the isolated request lifecycle.

### Provider failure

If classified transient and current, one recovery may start after the failed transport is terminal.

### Explicit cancellation / shutdown / Restore

No recovery.

## 10. Initial vs lived terminal policy

### Initial Character baseline

If the second attempt fails, retain current fail-soft behavior:

- no fabricated Character;
- no Important Experience;
- no durable fake success record;
- gameplay/Narrative remains available;
- later explicit `retry_pending()` may still be used if the product exposes/uses that repair seam.

### Lived curation

If the second attempt fails:

- accepted Narrative remains authoritative and uninterrupted;
- no partial Character/Experience/People/Thread mutation is committed;
- no fake Thread/People/Character content;
- the semantic barrier is not re-run;
- later opportunities remain processable;
- explicit retry may clear failed attempted markers as it does today.

Do not make Curator success a Narrative finalize gate.

## 11. Durable atomicity

Exactly as today, only a successfully parsed and current result may reach the single durable curation commit.

Recovery must not create:

- duplicate records for the same index/prefix/parent;
- duplicate Important Experiences;
- a second People subject identity for the same request proposal;
- multiple Thread identities for one proposal;
- partial write from attempt 1 followed by attempt 2.

Persistence failure occurs after a semantically valid candidate reaches storage and is **not** an automatic retry condition in MW-034.

## 12. Diagnostics

Extend the existing Curator diagnostics only enough to audit reliability safely.

Per logical opportunity expose/request-safe fields equivalent to:

- logical opportunity identity/version, without private raw text;
- attempt number (1 or 2);
- initial vs lived mode;
- terminal class (`committed`, `malformed_response`, `timeout`, `configuration_failure`, `provider_failure`, `stale`, `cancelled`, etc.);
- whether recovery was scheduled/started;
- elapsed time.

Do not log raw response, API credentials, private actor bindings, internal durable IDs, Player/GM prose or full curation payload.

## 13. Scope boundary

MW-034 is **Information Curator only**.

Do not modify recovery semantics for:

- Recommendations;
- Public d20 control;
- World semantic materialization;
- Agency;
- World Evolution;
- Narrative;
- Context Orchestrator.

Do not extract a shared recovery class/helper merely because three files now look similar. After MW-034 is reviewed, Package 8 will compare the third real consumer's evidence with Recommendations and d20 control before deciding whether a tiny shared primitive is justified.

## 14. Explicit non-scope

MW-034 does not authorize:

- universal Structured Output middleware;
- JSON repair/sanitization framework;
- response-schema DSL;
- Provider routing/fallback to another model;
- multiple retries/backoff loops;
- Context/source retrieval changes;
- Information Curation semantic redesign;
- People/Threads identity redesign;
- new UI;
- new SQLite tables;
- Package 9 provenance/epistemic work.

## 15. Engineering acceptance direction

MW-034 must prove at minimum:

1. initial malformed → one recovery → valid baseline commit; exactly 2 starts;
2. lived malformed → one recovery → valid curation commit; exactly 2 starts;
3. timeout → one recovery, old cancel/terminal cannot kill the recovery;
4. transient Provider failure → one recovery;
5. second malformed/timeout/transient failure → final fail-soft, no third start;
6. permanent configuration failure → no retry;
7. input/response oversize → no retry;
8. explicit cancel/shutdown → no retry;
9. Restore during attempt 1 or before recovery invalidates old work and prevents displaced-history recovery;
10. late callbacks from attempt 1 after attempt 2 begins cannot publish/commit/terminate attempt 2;
11. lived prefix/parent change blocks stale recovery/commit;
12. successful result commits once with unchanged current durable schema/identity semantics;
13. malformed attempt commits zero partial state;
14. recovery prompt contains no raw malformed response/private refs from attempt 1;
15. later lived opportunities continue after final failed recovery;
16. existing `retry_pending()` explicit repair seam still works after terminal failure;
17. Narrative/foreground gameplay is not blocked by Curator recovery;
18. current Information Curator / People / Threads / initial Character regressions remain green;
19. Recommendations/d20/World semantic/Agency/Evolution behavior is unchanged;
20. Godot 4.7.2 import + fresh Windows export + `ValidateExportOnly` pass.

## 16. Product evidence boundary

No immediate Owner UAT is required for MW-034 alone.

Engineering proves that a transient machine-response failure is less likely to leave long-session player information stale. It does not prove live-model curation quality.

The later concentrated G7 Product test must still judge whether Character/People/Threads remain coherent and useful over real long play.

## 17. Post-MW-034 decision gate

After reviewed MW-034 integration:

```text
compare three proven recovery consumers:
Recommendations
+ Public d20 control
+ Information Curator
↓
ask whether a genuinely tiny common lifecycle primitive exists
```

If the common part is only “two attempts” while failure classification/currentness/terminal behavior remains lane-specific, keep implementations local.

Only extract a shared primitive if it reduces code/bugs **without** requiring a policy-hook framework.
