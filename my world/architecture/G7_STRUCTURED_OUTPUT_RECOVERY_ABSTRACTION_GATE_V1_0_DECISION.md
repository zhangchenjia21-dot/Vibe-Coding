---
title: my world｜G7 Structured Output Recovery Abstraction Gate v1.0 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-14
updated: 2026-09-14
phase: G7 Long-session Context & Knowledge Hardening
owner: Owner + GPT
triggered_by:
  - MW-032 Recommendations bounded recovery
  - Public d20 control recovery
  - MW-034 Information Curator bounded recovery
---

# G7 Structured Output Recovery Abstraction Gate v1.0｜FROZEN

## 1. Decision

Post-MW-034 evidence still does **not** justify a shared Structured Output / retry lifecycle framework.

Three real recovery consumers exist, but their apparently similar "attempt 1 → recovery" shape hides materially different ownership and terminal semantics.

> **Do not extract a generic retry base class, Structured Output middleware, or cross-lane recovery state machine at this gate.**

This is an explicit architecture decision, not an absence of refactoring ambition.

## 2. Evidence by consumer

### Recommendations

- background optional UI lane;
- logical opportunity identity is the current accepted Conversation prefix;
- foreground Player generation invalidates recommendation work;
- one bounded recovery may follow malformed / timeout / provider failure except known configuration failure;
- final failure means recommendations become `unavailable`;
- result is non-durable presentation assistance;
- recovery/currentness is tightly coupled to recommendation availability and foreground interruption.

### Public d20 control

- foreground action adjudication pipeline;
- control is only one stage before CHECK/NO_CHECK/degraded Narrative;
- recovery exists only for malformed/invalid control parse;
- Provider failure is terminal rather than a control retry condition;
- second parse failure deliberately degrades to ordinary Narrative without inventing mechanics;
- no background opportunity queue or Curator-style Restore epoch lifecycle;
- durable check / NO_CHECK identity and Narrative acceptance are part of the same action pipeline.

### Information Curator

- background long-session state-maintenance lane;
- logical opportunity identity is Game + mode + binding/prefix/parent + Restore epoch;
- request serial is distinct from logical opportunity identity;
- one bounded recovery follows malformed response, Curator timeout, or an allowlisted transient transport/HTTP failure;
- second failure is fail-soft and may later be explicitly repaired through `retry_pending()`;
- successful result writes durable Character / Experiences / People / Threads state;
- request-scoped People/Thread/actor refs must be rebuilt and re-authorized on recovery.

## 3. Why the common code is not policy-free

The following dimensions still differ across consumers:

| Dimension | Recommendations | Public d20 control | Information Curator |
| --- | --- | --- | --- |
| Foreground/background | background, interrupted by foreground | foreground pipeline | background, Narrative-independent |
| Logical opportunity | accepted prefix | action/stage identity | Game + binding/prefix/parent + epoch |
| Retry trigger | malformed/timeout/provider except config | control parse failure only | malformed/timeout/allowlisted transient |
| Transport timeout owner | local timer | not the same recovery mechanism | local timer |
| Second failure | unavailable | degraded Narrative | terminal fail-soft + later explicit repair |
| Durable semantic write | no | possibly d20/NO_CHECK truth | yes, curation record |
| Request-scoped authority rebuild | no equivalent | control material reassembly | required for People/Thread/actor refs |
| Restore/currentness model | prefix/foreground invalidation | action pipeline/durable replay | epoch + prefix/parent/binding |

A shared class would therefore need callbacks for identity, foreground gating, retry classification, currentness, transport cancellation, degraded policy, persistence, diagnostics, request rebuilding, and terminal publication.

That is not a tiny policy-free primitive; it is a generic framework with hidden per-lane state machines.

## 4. Allowed future extraction

Future extraction is allowed only when repeated code becomes both:

1. mechanically identical across at least two consumers; and
2. independent of lane terminal semantics/currentness/authority.

Possible examples, only if future evidence supports them:

- a tiny helper to disconnect request-bound Provider signals safely;
- a pure function mapping already-classified transport statuses;
- a pure byte-count utility.

Do not migrate working production lanes merely to make their code look alike.

## 5. Package-8 consequence

Structured Output Reliability is considered sufficiently evidenced for the current Package-8 route through lane-local recovery where it is actually valuable.

Package 8 should now prioritize remaining **long-session currentness / latency / reality** defects rather than spend another Work Item on retry abstraction.

The next audited high-value defect is Public d20 control context currentness: its control stage still uses the full Opening-era Game-local projector plus the historical fixed-window Context path, while Narrative continuation already uses the G7 working-set owner.

That defect is handled by a separate consumer-specific architecture decision and executable Work Item.

## 6. Explicit non-scope

This decision does not authorize:

- generic Structured Output middleware;
- universal retry base class;
- JSON repair;
- Provider/model fallback;
- changing World semantic / Agency / Evolution retry semantics;
- converting all AI lanes to one orchestration framework;
- Package-9 provenance/epistemic work.
