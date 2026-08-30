---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-08-30
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-06
current_execution_task: G4-06
semantic_owner: GPT
current_execution_owner: Codex
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。不是 Product / Architecture / Status 的替代权威。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Read first

Governance:

1. `AGENTS.md`
2. `my world/MY_WORLD_项目启动总纲_CURRENT.md`
3. `my world/MY_WORLD_核心设计原则_CURRENT.md`
4. `my world/MY_WORLD_架构_CURRENT.md`
5. `my world/MY_WORLD_总体规划路线图_CURRENT.md`
6. `my world/MY_WORLD_CURRENT_STATUS.md`
7. `my world/AGENT_EXECUTION_ROUTING_CURRENT.md`
8. `my world/architecture/creation/G4-06_OPTIONAL_ENTRY_MATERIALIZATION_DECISION.md`
9. relevant Source + G4-04 persistence decisions

Implementation:

10. `AGENTS.md`
11. `docs/tasks/G4-06_ATOMIC_FINAL_CREATE_TASK.md`
12. `src/建局/L3_外交层/建局公开接口.gd`
13. `src/source/L3_外交层/Source合同公开接口.gd`
14. `src/source/L3_外交层/Source库公开接口.gd`
15. `src/游戏库/L3_外交层/游戏库公开接口.gd`
16. `src/persistence/L3_外交层/世界持久化公开接口.gd`
17. `src/persistence/L3_外交层/数据库安全公开接口.gd`
18. `src/runtime/当前游戏会话运行时.gd`
19. G4-05R2 full-fidelity reality tests/evidence

Do not default to rereading the whole historical repository.

---

## 2. Project core

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

## 3. Execution-agent routing

Canonical file:

`my world/AGENT_EXECUTION_ROUTING_CURRENT.md`

Defaults:

```text
GPT        → Meaning / architecture / task shaping / Decision Propagation / Independent Review
Codex      → backend / mechanism implementation
Kimi       → frontend / UI / interaction implementation
Grok Build → search / external research / evidence discovery
```

Task fit comes before quota availability. Kimi backend fallback requires explicit assignment.

Current G4-06 is backend-heavy and assigned to Codex.

---

## 4. G4-02R1 and G4-05 are closed

G4-02R1 final accepted Source mechanism/evidence includes:

```text
mechanism      eb11655f8ff592ae096915fab50553708c0b79df
IR01 evidence  1d8278f9a4bc33a748eb6444873af85d27d5a755
```

G4-05R2 accepted frontend implementation/evidence:

```text
implementation  ebd5645804334fc2f79cd18543a0571e1587fb14
evidence / HEAD d9f58963db26055f6ca1a54c26689baf63263ede
```

GPT Independent Review of G4-05R2: **PASS**.

G4-05 primary reality path now consumes frozen v0.2 full-fidelity 2 World + 6 Character directly. Real Han incompatibility is backend-authoritative, non-temporal scenario behavior remains valid, and no Final Create side effects occur.

Do not reopen G4-02R1 or G4-05 without new P0 evidence / explicit Owner decision.

---

## 5. Source / temporal rules that G4-06 must preserve

Temporal quarantine is optional authored capability.

When an exact Entry is selected:

```text
World = top-level safe + exact Entry sections
Character = top-level safe + exact T0 profile when exact binding exists
```

No latest/nearest/later/full-life fallback.

Zero World profile coverage stays top-level-only/non-hard-blocking.

Fingerprint covers all declared bytes, but only the selected starting projection is materialized into the Game.

Source remains immutable after create; Game-local reality owns future development.

---

## 6. Optional Entry decision for G4-06

Canonical:

`architecture/creation/G4-06_OPTIONAL_ENTRY_MATERIALIZATION_DECISION.md`

Entry remains `0..1`.

If no Entry is selected:

```text
World materialization     = top-level semantic_sections only
Character materialization = top-level semantic_sections only
```

No Entry section or T0 profile is inferred.

Do not add global historical/temporal mode, family restriction or universal required Entry.

Product richness of the no-Entry path is deferred to G4-07 UAT; G4-06 must not invent hidden defaults to make it look richer.

---

## 7. Current task｜G4-06 Atomic Final Create

Packet:

`docs/tasks/G4-06_ATOMIC_FINAL_CREATE_TASK.md`

Packet commit:

`2ad814a6f001c72c57d0616016dc7201dc1258cd`

Formal Code Base:

`d9f58963db26055f6ca1a54c26689baf63263ede`

Governance decision base:

`62d7efcf0cad061543eb3c97779b311bf2563240`

Implementation AGENTS activation commit:

`7eec660fa67fd4715b090290ca174efe59f39b58`

Owner: **Codex**  
Reviewer: **GPT**  
Return ceiling: **READY FOR INDEPENDENT REVIEW**

Do not duplicate this task while Codex is executing it.

---

## 8. G4-06 required meaning

Target:

```text
Frozen exact Composition
→ immutable creating intent
→ fixed creation_id + composition fingerprint + game_id/local identities
→ exactly one managed Game SQLite
→ selected Source projection + exact provenance pins
→ DB identity verification
→ verified Game Library record
→ current selection
→ created
```

Important:

- composition hash is not the creation identity; identical compositions must eventually be able to create two separate Games;
- same `creation_id` + same payload must replay/converge to the same Game;
- same `creation_id` + different payload must fail loud;
- creating intent must be durable before first Game DB side effect;
- crash windows after intent / DB / Game Library record / current publish must converge after restart/retry;
- valid Game DB truth is repaired forward, not destructively rolled back;
- One Game = One SQLite remains frozen;
- exact Source generation pins never drift to newer current;
- Source asset IDs are provenance, not Game-local Player/NPC IDs;
- Guaranteed NPC is canonical cast only, not opening/current location/player-known/relationship;
- no physical production SQLite schema migration without returning `BLOCKED` first;
- no Provider call and no AI Opening.

Persistence already provides explicit initial Game/root/current World creation; Game Library already provides verified existing managed Game registration. G4-06 should compose/narrowly extend these seams.

---

## 9. Independent Review after Codex return

Do not trust the report alone. Inspect actual code and durable evidence.

Specifically hunt:

1. creating intent really exists before DB side effect;
2. same creation identity does not mint new game/local IDs on retry;
3. altered payload under same identity conflicts;
4. DB-after-crash before Library registration converges without deleting/recreating valid truth;
5. registration only happens after actual DB identity verification;
6. exact Source X remains pinned if current changes to Y;
7. materialized Han early start excludes future/unselected markers;
8. no-Entry truly uses top-level only, no hidden Entry/profile fallback;
9. non-temporal scenario route is not falsely time-gated;
10. local Character IDs differ from Source asset IDs and differ across two Games;
11. Game Library record/current exactly match DB;
12. no Provider/AI Conversation Opening exists;
13. production schema remains v4 unless task returned BLOCKED before any migration;
14. G3/G4-02R1/G4-03/G4-04/G4-05 regressions remain real and non-vacuous.

If PASS: Decision Propagation closes G4-06 and next task is G4-07 First Playable A, which requires Owner UAT.

---

## 10. Route

```text
PASS/CLOSED G4-02R1
↓
PASS/CLOSED G4-05
↓
NOW G4-06 — Codex
↓
GPT Independent Review
↓
if PASS close G4-06
↓
G4-07 First Playable A — Owner UAT
```
