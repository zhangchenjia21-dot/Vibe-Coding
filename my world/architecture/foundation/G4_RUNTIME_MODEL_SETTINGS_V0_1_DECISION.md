# G4-09R1｜Runtime Model Settings v0.1 Decision

Status: **FROZEN / OWNER-REQUESTED**  
Date: 2026-09-01  
Semantic owner: **GPT**  
Parent: **G4-09 First Playable B**  
Reason: Owner explicitly requested model/runtime selection before G4-09UATB.

## 1. Product decision

Before Owner UAT B resumes, `my world` gains one application-level **模型设置** surface with three choices:

```text
模型
上下文上限
思考强度
```

This is runtime infrastructure configuration. It is **not** Source, Game canonical reality, Character/World state, Expansion state, or a replacement for G7 long-session context architecture.

The current product runtime is DeepSeek-only. This decision adds Kimi as a real second runtime Provider; Kimi's role as a development Agent is unrelated to this runtime Provider choice.

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

The label means **application runtime context ceiling/capability selection**, not a claim that every Provider exposes a request-body `context_window` switch.

Rules:

- DeepSeek V4 Pro: 256K or 1M allowed; provider model remains `deepseek-v4-pro`.
- DeepSeek V4 Flash: 256K or 1M allowed; provider model remains `deepseek-v4-flash`.
- Kimi K3: 256K selects `k3-256k`; 1M selects `k3`.
- Kimi K2.7: 256K only; 1M is incompatible and must be disabled/fail validation.

The selected ceiling is exposed to Context Assembly as runtime request-budget metadata. G2-05's currently accepted conservative recent-turn strategy may use less than the selected ceiling. This task does **not** add summarization, retrieval, memory compression, tokenizer-dependent exact packing, or other G7 systems.

UI must not imply that selecting 1M automatically fills 1M tokens.

## 4. Reasoning setting semantics

Player-facing requested choices:

```text
Low
Medium
High
Max
```

### DeepSeek V4 Pro / Flash

Effective Provider mapping:

```text
Low    → low
Medium → high
High   → high
Max    → max
```

Thinking remains enabled. `reasoning_content` is Provider-internal request/response material and must not be shown to the player or persisted as GM narrative.

### Kimi K3

Effective Provider mapping:

```text
Low    → low
Medium → high
High   → high
Max    → max
```

### Kimi K2.7

K2.7 Code currently exposes Thinking ON but not the same graded effort control. Therefore:

- selecting Kimi K2.7 keeps thinking enabled;
- the four-level effort selector becomes unavailable/disabled for this model;
- UI clearly says the model uses fixed thinking and does not fake an effective Low/Medium/High/Max value;
- Program must not silently route to another Kimi model merely to honor an effort value.

## 5. Persistence and scope

Settings are **application-local preferences** stored outside every Game database, for example under:

```text
user://my-world/settings/provider-runtime.json
```

Exact implementation path may vary, but the contract is:

- local durable persistence;
- atomic/fail-safe write;
- default on missing settings: `DeepSeek V4 Pro / 256K / High`;
- malformed/unknown persisted values fail closed to a visible recoverable settings error or validated default; never pass arbitrary values to Provider;
- changing settings does not rewrite existing Games or Source;
- the current validated profile is snapshotted when a Provider generation begins; an in-flight generation is never mutated underneath.

v0.1 exposes settings from Main Menu only. To change settings during an existing Game, return to Main Menu, change settings, then Continue.

## 6. Credentials

Secrets remain local environment material and are never written to the settings file or Game data.

Recognized variables:

```text
DEEPSEEK_API_KEY
KIMI_API_KEY
```

The product may launch with either/both/none configured. Provider call behavior is fail-loud for the currently selected Provider when its credential is missing.

The Settings UI may show only non-secret status such as `已配置 / 未配置`; it must never display, log, persist, or echo key values.

The canonical `run-game.cmd` / `run-game.ps1` launch path remains the only Owner launch path. Launcher changes may permit both credential variables but must not introduce a competing launcher.

## 7. Runtime routing invariant

Every real model call must obtain the same current validated runtime profile through one Program-owned seam:

```text
First Opening
ordinary Narrative continuation
Public d20 adjudication phase
Public d20 resolution-narrative phase
retry/reopen continuation calls
```

No call site may retain a hidden hard-coded DeepSeek route after this task.

For one active Provider request, endpoint/model/credential/reasoning profile are immutable until terminal completion/cancel/failure.

No automatic cross-provider fallback is allowed. If the selected Provider fails, surface that failure; do not silently call the other Provider.

## 8. Provider adapter boundary

DeepSeek and Kimi are both consumed through a thin OpenAI-compatible streaming contract. Program may generalize the existing DeepSeek-only adapter, but v0.1 remains a closed two-provider/four-profile implementation rather than a generic provider framework.

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

Provider-specific reasoning chunks must not be emitted as `text_delta` GM narrative.

## 9. UI contract

Main Menu gains `模型设置`.

Minimum surface:

```text
模型            [DeepSeek V4 Pro ▼]
上下文上限      [256K ▼]
思考强度        [High ▼]

凭据            DeepSeek 已配置 / Kimi 未配置
实际配置摘要    deepseek-v4-pro · 256K · High

[取消] [保存]
```

Compatibility behavior:

- Kimi K2.7 + 1M cannot be saved and 1M should be disabled with a concise explanation.
- Kimi K2.7 disables graded effort controls and shows fixed-thinking explanation.
- DeepSeek/K3 Medium remains selectable but the effective summary must disclose that Provider maps it to High.
- invalid persisted/profile combinations fail visibly; UI never silently substitutes a different model.

No API-key text entry, arbitrary endpoint input, model discovery browser, billing dashboard, latency benchmark UI, or per-Game model pinning in v0.1.

## 10. Acceptance / evidence

Backend acceptance must prove:

1. durable application settings and safe defaults;
2. exact closed profile catalog and compatibility validation;
3. DeepSeek and Kimi endpoints/model ids are Program-derived, never free-form;
4. correct reasoning mappings and K2.7 fixed-thinking behavior;
5. selected Provider key only; missing selected key sends no network request;
6. Opening, ordinary play, and both Public d20 Provider phases route through the same selected profile seam;
7. no cross-provider fallback;
8. existing Game/Source/SQLite schema v4 remain unchanged;
9. deterministic stub tests for all valid/invalid combinations;
10. real small-call evidence for both Provider families when local credentials permit. Missing Kimi credential/access is a visible acceptance blocker for claiming real Kimi runtime support, not something to fake with stubs.

Frontend acceptance must prove:

1. Main Menu settings surface;
2. exact four display models;
3. 256K/1M compatibility behavior;
4. Low/Medium/High/Max behavior with effective mapping disclosure;
5. K2.7 fixed-thinking UX;
6. save/cancel/reopen persistence;
7. no secret exposure;
8. 1280×720 / 960×540 / maximized layouts;
9. no regression to New Game / Continue / Public d20 UI.

After backend + frontend Independent Review, rerun current Windows export freshness and real Provider integration before resuming G4-09UATB.

## 11. Routing / progression

```text
G4-09UATB Owner Product UAT            HOLD — owner-requested prerequisite
G4-09R1 Runtime Model Settings v0.1    ACTIVE
G4-09R1S0 Semantic Freeze              PASS / CLOSED — GPT
G4-09R1M1 Backend Mechanism            ACTIVE — CODEX
G4-09R1B1 Settings UI                  HOLD — KIMI after M1 IR PASS
→ integration / real Provider evidence
→ GPT Independent Review
→ refresh Owner production export
→ resume G4-09UATB
```

This insertion does not close G4-09 or G4-08 and does not start G5/G7/G8.