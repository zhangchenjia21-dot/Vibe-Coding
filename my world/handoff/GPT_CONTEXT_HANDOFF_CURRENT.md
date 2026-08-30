---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-08-30
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-05
current_execution_task: G4-05R2
semantic_owner: GPT
current_execution_owner: Kimi
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 本文件是接管导航 / 最小充分摘要，不是 Product / Architecture / Status 的替代权威。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Read first

Governance:

1. `AGENTS.md`
2. `my world/MY_WORLD_项目启动总纲_CURRENT.md`
3. `my world/MY_WORLD_核心设计原则_CURRENT.md`
4. `my world/MY_WORLD_架构_CURRENT.md`
5. `my world/MY_WORLD_总体规划路线图_CURRENT.md`
6. `my world/MY_WORLD_CURRENT_STATUS.md`
7. `my world/AGENT_EXECUTION_ROUTING_CURRENT.md`
8. relevant Source decisions under `my world/architecture/source/`

Implementation:

9. `AGENTS.md`
10. `docs/tasks/G4-05R2_FULL_FIDELITY_WIZARD_CLOSURE_TASK.md`
11. `src/ui/新游戏向导.gd`
12. `src/ui/新游戏向导.tscn`
13. `src/建局/L3_外交层/建局公开接口.gd`
14. `docs/source/G4-02R1_OPTIONAL_TEMPORAL_SCOPE_CAPABILITY_CLARIFICATION.md`
15. `docs/source/G4-02R1_R2_MECHANISM_IMPLEMENTATION_EVIDENCE.md`

Do not default to rereading the whole historical repository.

---

## 2. Product core

`my world` is a local-first, single-player-first long-running AI RPG.

Key principles:

> **Model freedom first. Reversibility over prevention.**
>
> **Narrative richness over artificial brevity.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**
>
> **玩法拉出 Schema，不让玩法迁就万能 Schema。**

Engineering PASS != Product PASS.

---

## 3. G4-02R1 is closed

Final accepted implementation/evidence:

```text
mechanism      eb11655f8ff592ae096915fab50553708c0b79df
evidence       3b2c9d5fa0a9756ee670eb42179b5ae8359e0b2b
IR01 tests     b6f83851758a79dff2a24f621befdebdcc21c940
IR01 evidence  1d8278f9a4bc33a748eb6444873af85d27d5a755
```

GPT final Independent Review: **PASS — no blocking production defect remains**.

Do not resend or reopen G4-02R1 without new P0 evidence / explicit product decision.

Confirmed Source behavior:

- real 2 World + 6 Character v0.2 full-fidelity Sources;
- exact selected Entry/profile when authored temporal scope exists;
- no latest/nearest/later/full-life fallback;
- Han closed temporal coverage including 孙权 280;
- optional temporal capability: Character may omit `t0_profiles` entirely;
- World Entries may be scenario/opening choices without historical time partition;
- no global historical/temporal mode switch;
- fingerprint coverage wider than Runtime visibility;
- missing/tampered rich content fails loud;
- optional portrait absence is honest.

---

## 4. Optional temporal scope

> **Temporal Quarantine is an optional Source capability selected by authored need, not a universal asset mode.**

Support both:

```text
Temporal-scoped Source
→ authored multiple temporal cuts
→ exact selected projection
→ future/canon quarantine

Non-temporal Source
→ complete rich top-level semantics
→ no artificial T0 matrix
```

Do not add `historical`, `temporal_mode`, `requires_quarantine`, family mode or equivalent global classifiers.

When temporal scope is used, future isolation remains strict and there is no fallback.

---

## 5. Execution-agent routing

Canonical file:

`my world/AGENT_EXECUTION_ROUTING_CURRENT.md`

Defaults:

```text
GPT        → Meaning / architecture / task shaping / Decision Propagation / Independent Review
Codex      → backend / mechanism implementation
Kimi       → frontend / UI / interaction implementation
Grok Build → search / external research / evidence discovery
```

Task fit comes before quota availability. Kimi backend fallback requires explicit scope. Search does not become architecture authority.

---

## 6. Current task｜G4-05R2

Task:

> **G4-05R2 — Full-Fidelity New Game Wizard Closure**

Primary owner: **Kimi**  
Nature: **frontend/product integration**

Packet:

`docs/tasks/G4-05R2_FULL_FIDELITY_WIZARD_CLOSURE_TASK.md`

Packet commit:

`c0d8df812511745119d1e9ac8c14283b4c2bd5de`

Formal Code Base:

`1d8278f9a4bc33a748eb6444873af85d27d5a755`

Governance Base:

`32e7357d2eb86b6788d1a429b0dd97f2ba4a2caa`

Implementation AGENTS activation commit:

`8a6fd9ccc27a2005482aa65205a0b2083176c4a2`

Return ceiling:

> **READY FOR INDEPENDENT REVIEW**

Do not duplicate this task while Kimi is executing it.

---

## 7. Why G4-05R2 exists

Existing Wizard/Composition mechanics work, but the primary G4-05 reality path still installs obsolete task-owned v0.1 conversion fixtures under:

`tests/fixtures/g4_05/历史真实资产转换/...`

The current accepted real pressure is the frozen v0.2 set under:

`tests/fixtures/g4_02r1/full_fidelity/`

G4-05R2 must make the Wizard directly consume/present those current Sources.

---

## 8. G4-05R2 key requirements

Kimi should primarily edit:

```text
src/ui/新游戏向导.gd
src/ui/新游戏向导.tscn
```

Narrow shell UI wiring is allowed if needed.

Protected backend paths for this task:

```text
src/source/**
src/建局/**
src/persistence/**
src/runtime/**
src/provider/**
```

If a real backend defect is exposed, Kimi returns **BLOCKED — BACKEND DEFECT** with reproduction instead of silently crossing the boundary.

Required product corrections/evidence:

- primary Wizard reality test installs current frozen v0.2 2 World + 6 Character directly;
- chooser displays v0.2 `catalog_summary`;
- generic opening UI no longer universally says `T0`;
- historical Entry names may still show authored years/history;
- Guaranteed NPC wording must not imply opening appearance / same scene / current relationship;
- temporal incompatibility is explained in plain player language without weakening backend semantics or fallback;
- compatible Han route reaches Review;
- incompatible Han route fails Review clearly and non-destructively;
- Afterglow / ordinary scenario path is not forced into historical restrictions;
- Windows 1280×720 / maximized / 960×540 layout evidence;
- no Game SQLite, Game Library mutation, Session or Provider call;
- Final Create remains disabled; G4-06 stays out of scope.

---

## 9. Historical task warnings

- `G4-05R1_REAL_ASSET_FIDELITY_CORRECTION_TASK.md` = **SUPERSEDED / DO NOT EXECUTE**.
- `G4-05_ASSET_ONLY_NEW_GAME_WIZARD_TASK.md` = historical implementation reference only. Do not re-execute obsolete v0.1 conversion work.

---

## 10. What the next GPT does

If Kimi has **not returned**:

- do not duplicate G4-05R2;
- maintain governance / answer Owner questions only.

If Kimi **has returned**:

perform Independent Review, specifically inspect:

1. whether primary tests really use frozen v0.2 fixtures;
2. whether UI text visibly uses `catalog_summary` rather than merely loading it;
3. whether generic T0 jargon is removed without weakening historical behavior;
4. whether incompatible Han route is real backend rejection, not a UI-only fake;
5. whether no profile fallback occurs;
6. whether backend production paths remained untouched unless explicitly re-routed;
7. whether Windows/layout evidence is meaningful;
8. whether G4-06 side effects remain absent.

If PASS, Decision Propagation closes G4-05 and next route is **G4-06 Atomic Final Create**, naturally backend-heavy and normally assigned to Codex.

---

## 11. Route

```text
PASS/CLOSED G4-02R1
↓
NOW G4-05R2 — Kimi
↓
GPT Independent Review
↓
if PASS close G4-05
↓
G4-06 Atomic Final Create
↓
G4-07 First Playable A — Owner UAT
```
