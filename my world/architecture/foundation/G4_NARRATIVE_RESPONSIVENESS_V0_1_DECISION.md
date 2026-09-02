# G4 Narrative Responsiveness v0.1 Decision

Status: **FROZEN / IMPLEMENTED / OWNER-ACCEPTED**
Date: 2026-09-02
Semantic owner: **GPT**
Applies now to: **accepted G4 runtime behavior**
Carries forward as a runtime ordering invariant for later G5/G6 work.

## 1. Primary product finding

The Owner accepted the gameplay value/semantics of `判定与检定：公开 d20`, then identified two experience problems during real play:

1. visible GM narrative must appear as soon as practical instead of waiting behind bookkeeping or whole-response buffering;
2. the game must not become fragile because a model emitted harmless formatting variance or failed to satisfy an unnecessarily strict machine-readable text protocol.

Ordinary Opening/Narrative already stream into provisional in-memory Conversation and persist only after completion. Per-token SQLite/file writes are not the source of slow text. C01 removed Public d20 whole-response buffering, but its mixed control+narrative framing made model formatting a new blocking gate. That coupling is rejected.

## 2. Core principles

```text
Model Freedom First
+
Visible Narrative First
+
Canonical Commit Behind a Turn Finalize Barrier
```

### INV-NR-01 — Narrative channel stays free-form

Player-visible GM narrative must be ordinary natural-language text. Do not require the narrative stream itself to contain JSON headers, sentinels, Markdown contracts, exact physical-line framing, or other machine protocol that can make harmless model formatting errors block play.

### INV-NR-02 — Minimize blocking gates

A model-format/parser gate is not allowed merely for implementation convenience or call-count optimization. Hard blocking gates are reserved for facts that must be authoritative before play can safely continue, such as an exact Program-owned d20 result, persistence integrity, or an unsupported capability that would corrupt semantics.

Recoverable model-format/control failures must be absorbed by the orchestration layer whenever possible rather than surfaced as a dead-end turn.

### INV-NR-03 — Visible narrative is the foreground critical path

Once narrative is semantically safe to expose, stream it to the existing provisional Conversation/UI path immediately. Persistence, indexing, future character/event/world derivation, or other bookkeeping must not intentionally hold the full narrative until terminal completion.

### INV-NR-04 — Visible draft may be provisional

Streaming text may be visible before authoritative durable acceptance. Cancel/failure/persistence failure keeps partial draft out of future Context.

### INV-NR-05 — No per-token canonical writes

Provider deltas stay in memory. Do not write SQLite/world/files per token or small chunk. Canonical persistence occurs at semantic boundaries.

### INV-NR-06 — Turn Finalize Barrier

The next player action may not enter Provider Context until the previous turn's authoritative effects required by that turn have durably finalized.

### INV-NR-07 — Future semantic background lane

Future G5/G6 character/event/relationship/location/world derivation should be eligible to run while the player reads already-visible narrative when safe, but must converge before the next turn depends on it. This decision does not implement those systems or a generic job queue in G4.

## 3. Public d20 control and narrative must be decoupled

The mixed C01 protocol (`control JSON + raw narrative in one Provider response`) is superseded.

### Control lane

The d20 Host may use a short, isolated adjudication request to determine only:

```text
NO_CHECK
or
CHECK_REQUIRED + proposal
```

This lane is not player-visible narrative. Its output may be structured because it represents mechanics, not prose. However, control-format handling must be resilient and bounded:

- prefer provider-native structured response support when already available through the current Provider seam, but do not redesign the Provider architecture solely for this task;
- otherwise parse the smallest isolated structured response rather than coupling structure to GM prose;
- harmless whitespace/pretty-print formatting must not create product failure;
- one bounded automatic repair/retry is allowed for malformed control output;
- do not loop indefinitely or create a generic retry framework;
- do not expose raw control payloads, prompts, hidden reasoning, keys or Authorization to the player.

### Narrative lane

GM narrative is a separate free-form request/stream:

- no JSON framing requirement;
- no sentinel requirement;
- no exact first-line contract;
- stream Provider content deltas directly into provisional Conversation as ordinary narrative;
- narrative persistence remains behind Provider completion/finalize, never per token.

### NO_CHECK

The prior optimization requiring exactly one Provider call is **superseded**. Reliability and model freedom are more important than minimizing call count.

Normal path:

```text
short adjudication control
→ NO_CHECK
→ free-form narrative request
→ visible narrative streaming
→ durable Conversation/finalize
```

If the control lane still cannot produce a valid decision after its bounded recovery attempt, the runtime must **fail-soft rather than dead-end the turn**:

```text
control unavailable/unparseable
→ transparently degrade this action to the ordinary no-Expansion natural-language narrative path
→ no d20 roll is created
→ show at most a concise non-blocking notice that the optional check mechanic was skipped for this action
→ continue play
```

This degradation must never be silent in durable mechanic semantics: no fake check, no fake NO_CHECK durable marker pretending adjudication succeeded, and no provider fallback to another model.

### CHECK_REQUIRED

When the control lane produces a valid CHECK_REQUIRED proposal, preserve the accepted authoritative ordering:

```text
validated proposal
→ Program RNG/outcome
→ durable exact check
→ free-form result-narrative request
→ visible result narrative streaming
→ durable Conversation acceptance
→ narrative_accepted marker
→ Turn Finalize Barrier PASS
```

The exact d20 result remains durable before result narrative begins. Retry/reopen must reuse the exact same durable check and never reroll.

## 4. Failure behavior

The player must not be trapped merely because model control formatting was imperfect.

- recoverable control-format failures use bounded internal recovery, then fail-soft ordinary narrative degradation;
- Provider/network/credential failures may still stop the current Provider request because no narrative can be produced, but the UI must show a concise safe reason and keep the stable retry path available;
- persistence failures remain hard failures because accepting a turn without durable truth would corrupt history;
- unsupported authored capability remains fail-loud rather than silently changing rules.

A generic `行动未完成` message may accompany recovery controls, but it must not be the only player-visible diagnostic for a terminal failure.

## 5. Latency observability

Keep non-secret, non-persistent monotonic timing sufficient to distinguish:

```text
control_request_started
control_completed / control_recovery
narrative_request_started
first_provider_content_delta
first_visible_narrative_delta
provider_completed
finalize_completed / turn_ready
```

Do not log prompts, narrative content, hidden reasoning, credentials or Authorization.

No fixed Provider SLA is frozen. Application acceptance is structural: once free-form narrative content begins arriving, it must become visible before Provider completion.

## 6. Protected accepted semantics

Do not change:

- model profile/catalog/settings semantics;
- selected-provider-only credential routing / no fallback;
- Source / Final Create ownership;
- SQLite schema v4;
- Program-owned RNG/outcome and no nat-1/nat-20 special rule;
- stable action/check identity and no-reroll;
- Game Library / Save / Restore semantics;
- no-Expansion G4-07 natural-language behavior except reuse as the explicit fail-soft degradation path;
- current G7 deferrals.

## 7. Correction-budget rule

C01's progressive-streaming goal remains accepted, but its mixed control+narrative protocol is superseded because real Owner UAT exposed it as a fragile gate.

Correction-02 is therefore a **protocol decoupling redesign**, not another accumulation of parser special cases. If the decoupled control lane itself later repeatedly fails in real Provider use, stop adding format rules and revisit whether Public d20 adjudication should use a different program/provider capability.

## 8. Accepted gate result

The complete correction chain passed Independent Review and final Owner retest:

```text
G4-09UATBC01 Narrative Responsiveness       PASS / CLOSED
G4-09UATBC02A d20 Protocol Decoupling       PASS / CLOSED
G4-09UATBC02B Failure Visibility            PASS / CLOSED AFTER C01
G4-09UATBC02BC01 Persistence Visibility     PASS / CLOSED
G4-09UATBC02P1 Final Windows Freshness      PASS / CLOSED
G4-09UATB Owner Product UAT                 PASS / CLOSED
G4-09 First Playable B                      PASS / CLOSED
G4-08 Expansion Pack v0.1                   PASS / CLOSED
```

Owner returned final verdict `PASS` on 2026-09-02.

These runtime ordering/model-freedom invariants carry forward into later G5/G6 work. Current execution proceeds to G4-10 Runtime Asset Resolution; this decision does not authorize reopening the accepted Public d20 protocol absent a concrete regression.
