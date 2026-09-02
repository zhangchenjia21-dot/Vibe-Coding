# G4 Narrative Responsiveness v0.1 Decision

Status: **FROZEN / OWNER-REQUESTED — C02 FRAMING AMENDMENT**
Date: 2026-09-02
Semantic owner: **GPT**
Applies now to: **G4-09UATB correction**
Carries forward as a runtime ordering invariant for later G5/G6 work.

## 1. Trigger / Product finding

During real Owner UAT, the Owner accepted the gameplay value/semantics of `判定与检定：公开 d20`, but reported that visible GM narrative feels substantially too slow.

Implementation inspection established two original facts:

1. Ordinary Opening / ordinary Narrative already stream Provider `text_delta` into an in-memory provisional Conversation draft and UI; accepted Conversation persistence happens only after Provider completion. Per-token SQLite persistence is **not** the cause of slow visible text.
2. The Public d20 Host buffered Provider output and only published narrative after the whole structured response / whole resolution-narrative call completed. C01 removed that whole-response buffering.

A later focused Owner retest exposed an adjacent robustness defect: C01's physical-first-line JSON framing is too strict for harmless real-model formatting variance, and the Public d20 UI currently hides the concrete safe failure reason behind a generic unfinished-action state.

This remains a core-loop responsiveness/reliability defect, not merely visual polish.

## 2. Primary runtime principle

```text
Visible Narrative First
+
Canonical Commit Behind a Turn Finalize Barrier
```

### INV-NR-01 — Visible narrative is the foreground critical path

Once a Provider has produced player-visible narrative text that is semantically safe to expose, the runtime should project it to the Narrative UI as soon as practical. Persistence, indexing, future character/event/world derivation, or other bookkeeping must not intentionally hold the whole narrative until terminal Provider completion.

### INV-NR-02 — Visible draft may be provisional

Streaming text is allowed to be visible before it becomes authoritative durable truth. Until finalize succeeds, it remains a provisional draft. Cancel/failure/persistence failure must preserve the existing rule that partial draft is not accepted into future Context.

### INV-NR-03 — No per-token canonical writes

Provider deltas must not trigger SQLite/world-state/file writes per token or per small text chunk. Streaming accumulates in memory; canonical persistence is coalesced at semantic boundaries.

### INV-NR-04 — Turn Finalize Barrier

The next player action must not enter Provider Context until the previous turn's authoritative effects required by that turn have completed their durable commit.

Conceptually:

```text
visible narrative stream
→ Provider terminal completion
→ validate/coalesce authoritative turn effects
→ durable commit(s) / acceptance markers
→ Turn Finalize Barrier PASS
→ next player action becomes eligible
```

This prevents responsiveness optimization from creating eventual-consistency gameplay where the next turn sees stale world/character/event truth.

### INV-NR-05 — Future semantic background lane

When G5/G6 later introduce character changes, event changes, relationship/location/world updates, derived indexes, or similar semantic work, those computations should be eligible to run while the player is reading already-visible narrative **when safe**, but they must converge before the Turn Finalize Barrier if the next turn depends on them.

This decision does **not** implement those future systems in G4 and does not authorize a generic job framework now.

## 3. Public d20 ordering remains authoritative

The accepted no-reroll / stable-action semantics remain unchanged.

For `CHECK_REQUIRED`:

```text
short adjudication control response
→ validate proposal
→ Program RNG
→ durable exact check result
→ start result-narrative Provider call
→ stream result narrative visibly as deltas arrive
→ durable Conversation acceptance
→ durable narrative_accepted marker
→ Turn Finalize Barrier PASS
```

The exact d20 result must remain durable **before** result narrative begins. That short write is intentionally foreground because it prevents reroll/reinterpretation after crash/retry.

For `NO_CHECK`, preserve **one Provider call** and no-dice semantics. The control protocol is a semantically framed JSON object followed by raw narrative body:

```text
optional leading JSON framing whitespace
→ first complete JSON object:
  {"decision":"NO_CHECK","reason":"..."}
→ optional framing whitespace after the object
→ raw player-visible narrative body, streamed incrementally
```

`CHECK_REQUIRED` uses the same first-complete-object framing but has no narrative body:

```text
{"decision":"CHECK_REQUIRED","proposal":{...}}
```

### Framing requirements after C02 amendment

- physical line boundaries are **not** semantic authority;
- the parser must find the first complete JSON object incrementally across arbitrary Provider/SSE chunk boundaries and across pretty-printed physical lines;
- leading whitespace before the JSON object is allowed;
- JSON object completeness must be determined structurally, including nested objects, quoted strings and escapes; braces inside strings must not terminate the object;
- the first non-whitespace framing character must still begin the JSON object; Markdown fences, natural-language preamble, or arbitrary prose before the object remain fail-loud and are not guessed through;
- the complete control object must validate exact branch fields/semantics before any NO_CHECK narrative is exposed;
- after a validated NO_CHECK control object, framing whitespace may be consumed before the first narrative content; narrative then streams progressively and is accumulated in memory for exact final persistence;
- after a validated CHECK_REQUIRED object, only trailing whitespace is allowed; any non-whitespace body remains a protocol error;
- on Provider completion, NO_CHECK narrative must be non-empty;
- exact NO_CHECK resolution (including the finalized narrative) is persisted once after Provider completion and before Conversation durable acceptance;
- if Provider fails before completion, no completed NO_CHECK resolution exists and the provisional Conversation attempt fails/cancels;
- if exact NO_CHECK resolution was durably written but later Conversation/marker acknowledgement fails, retry/reopen reuses that exact durable narrative and does not ask Provider to invent a replacement.

The parser may be tolerant of harmless formatting, but it must never silently reinterpret ambiguous or malformed control content. If the same real-model framing seam still fails after correction-02, stop adding format special cases and redesign the control protocol.

## 4. Provisional Conversation ownership for d20 streaming

Public d20 Host may begin/retry the current provisional Conversation turn before the narrative Provider call finishes so that deltas can flow through the existing Conversation/UI projection.

Requirements:

- one player action still maps to one stable action identity;
- failed/cancelled provisional attempts never become durable accepted Conversation;
- retry in the same process must reuse the existing unaccepted Conversation turn rather than append duplicate player turns;
- reopen after process loss reconstructs only durable truth and resumes from durable check / durable NO_CHECK state as already specified;
- accepted ordering remains `Player → visible mechanic card when applicable → GM narrative`.

## 5. Latency and failure observability

The runtime must retain non-secret, non-persistent monotonic timing evidence sufficient to distinguish Provider latency from application-added buffering.

At minimum measure or expose for tests/evidence:

```text
request_started
first_provider_content_delta
first_visible_narrative_delta
provider_completed
finalize_completed / turn_ready
```

For CHECK_REQUIRED also distinguish adjudication control completion, durable check completion, and resolution-narrative request start.

Do not log API keys, Authorization headers, prompts, raw hidden reasoning, or full player/GM content as performance telemetry.

No fixed production SLA is frozen in v0.1 because Provider/model/network latency varies. The required structural proof is that, when a narrative response arrives in delayed chunks, `first_visible_narrative_delta` occurs before `provider_completed` rather than only after it.

Public d20 terminal failures must also produce a safe player-visible reason category. The UI may map stable public failure codes to concise messages, but must not display raw Provider payloads, prompts, secrets, Authorization, hidden reasoning, or unbounded transport bodies. A generic unfinished-action recovery state may remain, but it must not be the only diagnostic visible to the player.

## 6. Protected accepted semantics

This correction must not change:

- model profile/catalog/settings semantics;
- selected-provider-only credential routing / no fallback;
- Source / Final Create ownership;
- SQLite production schema v4;
- Public d20 Program-owned RNG/outcome;
- no nat-1/nat-20 special rule;
- stable action/check identity and no-reroll behavior;
- no-Expansion G4-07 natural-language path;
- current Context Assembly / G7 deferrals;
- Game Library / Save / Restore semantics.

## 7. Explicit non-scope

Do not use this correction to implement:

- G5 character/world/event semantic mutation systems;
- generic background worker/job queue architecture;
- long-session summarization/retrieval/tokenizer work;
- speculative event sourcing redesign;
- UI redesign unrelated to safe failure visibility;
- Provider timeout/watchdog policy unless a separate reproduced blocker requires it.

## 8. Gate effect

The Owner's positive finding that Public d20 gameplay itself is worthwhile remains preserved. The focused Owner retest is paused for correction-02 after the real product hit an unfinished action under the C01 framing protocol.

```text
G4-09UATBC01 Narrative Responsiveness      PASS / CLOSED
G4-09UATB Owner Product UAT               HOLD — CORRECTION-02
G4-09UATBC02A Framing Robustness           ACTIVE — CODEX
G4-09UATBC02B Failure Visibility           HOLD — KIMI
```

After both engineering corrections pass Independent Review and Windows/product freshness is current, Owner UAT resumes with a focused check of progressive narrative, reliable action completion/retry, and safe visible failure feedback.
