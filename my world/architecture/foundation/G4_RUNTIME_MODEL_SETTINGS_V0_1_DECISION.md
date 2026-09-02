# G4-09R1｜Runtime Model Settings v0.1 Decision

Status: **FROZEN / IMPLEMENTED / ACCEPTED**  
Date: 2026-09-01  
Accepted: 2026-09-02  
Semantic owner: **GPT**  
Parent: **G4-09 First Playable B**  
Reason: Owner explicitly requested model/runtime selection before G4-09UATB.

## 1. Product decision

`my world` has one application-level **模型设置** surface with three choices:

```text
模型
上下文上限
思考强度
```

This is runtime infrastructure configuration. It is **not** Source, Game canonical reality, Character/World state, Expansion state, or a replacement for G7 long-session context architecture.

Before this decision the runtime was DeepSeek-only. v0.1 adds Kimi as a real second runtime Provider. Kimi's role as a development Agent is unrelated to this runtime Provider choice.

## 2. Exact model catalog

The player-facing model list is closed in v0.1:

| profile_id | Display Name | Provider | Provider model id |
|---|---|---|---|
| `deepseek_v4_pro` | DeepSeek V4 Pro | DeepSeek | `deepseek-v4-pro` |
| `deepseek_v4_flash` | DeepSeek V4 Flash | DeepSeek | `deepseek-v4-flash` |
| `kimi_k3` | Kimi K3 | Kimi | `k3-256k` at 256K, `k3` at 1M |
| `kimi_k27` | Kimi K2.7 | Kimi | `kimi-for-coding` |

No arbitrary model-id text field, custom endpoint, auto-routing, fallback chain, provider marketplace, or plugin registry is introduced.

Provider endpoints remain fixed in Program code:

```text
DeepSeek OpenAI-compatible  https://api.deepseek.com/chat/completions
Kimi OpenAI-compatible      https://api.kimi.com/coding/v1/chat/completions
```

## 3. Context setting semantics

Player-facing options:

```text
256K
1M
```

The setting means the **application runtime context ceiling/capability selection**, not a claim that every Provider exposes a request-body context-window switch.

Rules:

- DeepSeek V4 Pro: 256K or 1M allowed; provider model remains `deepseek-v4-pro`.
- DeepSeek V4 Flash: 256K or 1M allowed; provider model remains `deepseek-v4-flash`.
- Kimi K3: 256K selects `k3-256k`; 1M selects `k3`.
- Kimi K2.7: 256K only; 1M is incompatible and disabled/fail-closed.

The selected ceiling is runtime request-budget metadata. G2-05 may use less than the selected ceiling. Summarization, retrieval, compression and other long-session context systems remain G7 work.

## 4. Reasoning setting semantics

Player-facing requested choices:

```text
Low
Medium
High
Max
```

For DeepSeek V4 Pro / Flash and Kimi K3:

```text
Low    -> low
Medium -> high
High   -> high
Max    -> max
```

The UI must disclose Medium as effective High rather than pretending it is a distinct Provider effort.

For Kimi K2.7:

- Thinking remains ON;
- graded Low/Medium/High/Max control is disabled;
- UI shows fixed-thinking behavior;
- Program must not route to a different model to emulate graded effort.

Provider reasoning-only material must not be surfaced or persisted as player-visible GM narrative.

## 5. Persistence and scope

Settings are application-local preferences outside every Game database:

```text
user://my-world/settings/provider-runtime.json
```

Contract:

- local durable atomic/fail-safe persistence;
- default when missing: `DeepSeek V4 Pro / 256K / High`;
- malformed/unknown persisted values fail closed to a visible recoverable state;
- changing settings does not rewrite Games or Source;
- current validated profile is snapshotted when a Provider generation starts;
- an in-flight generation is never mutated underneath.

v0.1 exposes settings from Main Menu only. To change settings for an existing Game, return to Main Menu, change settings, then Continue.

## 6. Credentials

Recognized environment variables:

```text
DEEPSEEK_API_KEY
KIMI_API_KEY
```

Secrets never enter the settings file, Game data, UI node values, logs or evidence. The UI may show only non-secret configured/unconfigured status.

Missing credential for the selected Provider fails loud and sends no cross-provider fallback request.

`run-game.cmd` / `run-game.ps1` remains the canonical Owner launch path.

## 7. Runtime routing invariant

Every real model call obtains the same current validated runtime profile through one Program-owned seam:

```text
First Opening
ordinary Narrative continuation
Public d20 adjudication phase
Public d20 resolution-narrative phase
retry/reopen continuation calls
```

No hidden hard-coded DeepSeek runtime path remains.

For one active Provider request, endpoint/model/credential/reasoning profile are immutable until terminal completion/cancel/failure.

No automatic cross-provider fallback is allowed.

## 8. Provider adapter boundary

DeepSeek and Kimi use a thin OpenAI-compatible streaming contract. v0.1 remains a closed two-provider/four-profile implementation, not a generic provider framework.

Normalized outward contract remains conceptually:

```text
start_stream(messages, validated_runtime_profile)
is_busy()
cancel()
text_delta
completed
cancelled
failed
```

Provider-specific reasoning chunks are not emitted as GM narrative.

## 9. UI contract

Main Menu contains `模型设置` with:

```text
模型
上下文上限
思考强度
DeepSeek / Kimi credential status
实际配置摘要
取消 / 保存
```

Compatibility/effective truth comes from Runtime Settings L3 projection, not duplicated UI policy.

Required accepted behavior:

- K2.7 + 1M cannot save; 1M is visibly unavailable;
- K2.7 disables graded reasoning and visibly shows fixed Thinking ON, including invalid 1M intermediate state;
- DeepSeek/K3 Medium visibly discloses actual High;
- invalid persisted state is recoverable through L3 validated default and does not silently save;
- Cancel and Escape do not persist edits;
- settings survive application restart;
- no API-key editing, arbitrary endpoint/model id, provider marketplace, billing/benchmark UI or per-Game model pinning.

## 10. Acceptance result

Accepted implementation evidence proves:

- durable application settings and safe defaults;
- exact closed profile catalog and compatibility validation;
- Program-derived endpoint/model identities;
- reasoning mappings and K2.7 fixed-thinking behavior;
- selected-provider credentials only, no fallback;
- shared routing across Opening/Narrative/Public d20;
- no Game/Source/SQLite schema mutation;
- deterministic valid/invalid tests;
- real successful DeepSeek V4 Pro / Flash and Kimi K3 / K2.7 Provider calls;
- Main Menu settings UI with responsive 1280x720 / 960x540 / maximized layouts;
- real UI-selected DeepSeek V4 Pro -> Save -> Opening;
- real UI-selected Kimi K3 -> Save -> Opening;
- canonical Windows export freshness rebuilt/validated after UI acceptance;
- production World/Character/Public d20 UAT prerequisites intact and Owner Games unchanged.

Formal final review:

`my-world/docs/g4_09r1/G4-09R1P1_INDEPENDENT_REVIEW.md`

## 11. Current disposition

```text
G4-09R1S0 Semantic Freeze             PASS / CLOSED
G4-09R1M1 Backend Mechanism           PASS / CLOSED
G4-09R1M1C01 Projection/Kimi Proof    PASS / CLOSED
G4-09R1B1 Settings UI                 PASS / CLOSED AFTER CORRECTION-01
G4-09R1B1C01A L3 UI Support           PASS / CLOSED
G4-09R1B1C01B UI State Consistency    PASS / CLOSED
G4-09R1P1 Final Integration/Freshness PASS / CLOSED
G4-09R1 Runtime Model Settings v0.1   PASS / CLOSED
G4-09UATB Owner Product UAT           ACTIVE — OWNER
```

This decision no longer blocks Owner UAT B. G4-09 and G4-08 remain open until the Owner product verdict on Public d20. G4-GATE remains NOT YET; do not start G5 before G4-GATE.