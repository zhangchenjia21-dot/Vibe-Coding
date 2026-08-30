---
title: my world｜当前状态
status: current-project-status
version: 7.0
created: 2026-08-26
updated: 2026-08-30
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-05R2 Full-Fidelity New Game Wizard Closure — packet preparation
current_owner: GPT
parent_task: G4-05
semantic_owner: GPT
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Current

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G2-GATE                               PASS
G3 Persistent Game / Save / Timeline PASS / CLOSED
G3-GATE                               PASS
G4-01 Application Shell / Lifecycle  PASS / CLOSED
G4-02 original v0.1 engineering       HISTORICAL PASS
G4-02R1 Source semantic re-audit      PASS / CLOSED
G4-03 Managed Local Source Library    PASS / CLOSED
G4-04 Multi-Game / Game Library       PASS / CLOSED
G4-05 Asset-only New Game Wizard      REWORK → RESUMING FOR CLOSURE
G4-05R2 full-fidelity Wizard closure  PREPARING PACKET — GPT
G4-06 Atomic Final Create             HOLD
G4-07 First Playable A                HOLD
G4-GATE                               NOT YET
```

Implementation `main` accepted for the G4-02R1 close:

`1d8278f9a4bc33a748eb6444873af85d27d5a755`

---

## 2. G4-02R1 final result｜PASS / CLOSED

The Source v0.2-r2 mechanism and its Independent Review correction are complete.

Primary implementation/evidence commits:

- mechanism: `eb11655f8ff592ae096915fab50553708c0b79df`
- mechanism evidence: `3b2c9d5fa0a9756ee670eb42179b5ae8359e0b2b`
- IR01 tests: `b6f83851758a79dff2a24f621befdebdcc21c940`
- IR01 evidence: `1d8278f9a4bc33a748eb6444873af85d27d5a755`

Final GPT Independent Review verdict:

> **PASS. No blocking production defect remains.**

Confirmed:

- real 2 World + 6 Character full-fidelity v0.2 packages load through production Source seams;
- exact selected World Entry projection;
- exact Character T0 profile projection where authored temporal scope exists;
- no latest / nearest / later / complete-life fallback;
- direct Han closed-coverage assertions including 孙权 280 temporal incompatibility;
- Afterglow current same-profile multi-Entry bindings remain valid;
- cross-world / zero-coverage is not hard-blocked by invented family rules;
- fingerprint covers unselected/private declared bytes while Runtime visibility remains narrower;
- missing/tampered rich files fail loud;
- no-portrait assets remain honestly portraitless;
- G4-03 and preserved G4-05 regressions remain green;
- IR01 changed tests/evidence only, not production Source code or frozen 2 World + 6 Character fixtures.

G4-02R1 is therefore closed. Do not reopen it without new P0 evidence or an explicit new product decision.

---

## 3. Optional Temporal Source Scope｜FROZEN

Canonical governance:

`architecture/source/G4_OPTIONAL_TEMPORAL_SOURCE_SCOPE_DECISION.md`

Implementation clarification:

`my-world/docs/source/G4-02R1_OPTIONAL_TEMPORAL_SCOPE_CAPABILITY_CLARIFICATION.md`

Rule:

> **Temporal Quarantine is an optional Source capability selected by authored need, not a universal asset mode.**

The game supports both:

```text
Temporal-scoped Source
→ multiple authored T0 cuts where future-reference leakage is a real hazard
→ exact selected projection
→ future/canon quarantine

Non-temporal Source
→ complete rich ordinary starting semantics
→ no artificial T0 profile matrix
```

Do not add global `historical`, `temporal_mode`, `requires_quarantine`, family mode or equivalent switches.

Entries can be scenario/opening/location choices without being historical time partitions.

---

## 4. Why G4-05 still needs closure work

The existing G4-05 Wizard/Composition engineering candidate remains useful and its regressions pass, including:

- explicit click != list focus;
- exact generation pinning;
- World change clears Entry;
- Player/NPC role constraints;
- deterministic exact re-resolve Review;
- no Game SQLite / Game Library mutation / Provider call;
- honest disabled Final Create placeholder;
- cancel returns Main Menu / no Session.

However the current primary G4-05 product/reality test path still installs the old task-owned **v0.1 historical conversion fixtures** under:

`tests/fixtures/g4_05/历史真实资产转换/...`

Those old packages are no longer the canonical full-fidelity Source pressure after G4-02R1.

G4-05 cannot close until its real Wizard path is re-based onto the frozen v0.2 full-fidelity set:

`tests/fixtures/g4_02r1/full_fidelity/`

with the corrected temporal / non-temporal semantics.

---

## 5. Next task｜G4-05R2 Full-Fidelity New Game Wizard Closure

Primary outcome:

> **Make the existing New Game Wizard consume and present the current v0.2 full-fidelity Sources directly, then close G4-05 without entering Final Create.**

Expected focus is frontend/product integration, not backend redesign:

- replace old v0.1 conversion fixtures as the primary Wizard reality path with frozen v0.2 full-fidelity packages;
- keep Source Library / Composition backend contracts unchanged unless a concrete integration defect is proven;
- show chooser-facing `catalog_summary` so rich Sources are understandable before selection;
- remove universal player-facing `T0` wording from generic Wizard copy: historical Worlds may expose historical Entry names, but non-temporal Worlds must not look like they require a time-mode;
- use accurate Guaranteed NPC wording that does not imply opening appearance / same scene;
- prove a compatible Han route reaches valid Review;
- prove a temporally incompatible Han combination fails clearly at Review without creating a Game;
- prove Afterglow / non-temporal-compatible behavior is not restricted by historical rules;
- preserve responsive/keyboard/layout evidence;
- keep G4-06 disabled / out of scope.

Formal packet is being prepared from current accepted implementation HEAD.

---

## 6. Execution-agent routing

Canonical routing:

`AGENT_EXECUTION_ROUTING_CURRENT.md`

Default allocation:

```text
GPT        → Meaning / architecture / task shaping / Decision Propagation / Independent Review
Codex      → backend / mechanism implementation
Kimi       → frontend / UI / interaction implementation
Grok Build → search / external research / evidence discovery
```

Kimi may take backend work as a fallback when explicitly granted; Grok Build may occasionally implement self-contained code work. Task fit comes before quota availability.

For G4-05R2, the natural primary owner is **Kimi** because the remaining work is primarily Wizard frontend/product integration. If Kimi finds a real Source/Composition backend defect, return the boundary finding to GPT rather than silently redesigning backend code.

---

## 7. Current execution order

```text
DONE  G4-02R1 semantic/full-fidelity correction
↓
DONE  G4-02R1 Source v0.2-r2 mechanism
↓
DONE  GPT Independent Review + IR01 correction
↓
PASS/CLOSED G4-02R1
↓
NOW   prepare/execute G4-05R2 full-fidelity Wizard closure
↓
GPT Independent Review
↓
if PASS close G4-05
↓
G4-06 Atomic Final Create
↓
G4-07 First Playable A — Owner UAT
```

G4-07 remains the first full World+Character product UAT and must still evaluate Narrative richness, Character individuality, anti-convergence behavior and bounded-but-not-starved Context.

---

## 8. Holds / prohibitions

- `docs/tasks/G4-05R1_REAL_ASSET_FIDELITY_CORRECTION_TASK.md` remains **SUPERSEDED / DO NOT EXECUTE**.
- The old `G4-05_ASSET_ONLY_NEW_GAME_WIZARD_TASK.md` is historical implementation authority only; do not execute its obsolete v0.1 real-asset conversion instructions as a new task.
- Do not start G4-06 until G4-05R2 is independently reviewed and closed.
- Do not use G4-05 to redesign Source semantics, implement Expansion runtime, create Game SQLite, materialize canonical Game state or call the Provider.

---

## 9. GPT takeover checkpoint

Current navigation handoff:

`handoff/GPT_CONTEXT_HANDOFF_CURRENT.md`

A replacement GPT must refresh both `main` HEADs before acting. G4-02R1 is closed; do not resend its tasks. The next formal execution task is G4-05R2 after the repository-native packet is committed.
