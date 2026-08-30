---
title: my world｜当前状态
status: current-project-status
version: 7.3
created: 2026-08-26
updated: 2026-08-30
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-07A First Playable Opening Runtime
current_owner: Codex
parent_task: G4-07 First Playable A
semantic_owner: GPT
owner_uat_required: true
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
G4-02R1 Source semantic re-audit      PASS / CLOSED
G4-03 Managed Local Source Library    PASS / CLOSED
G4-04 Multi-Game / Game Library       PASS / CLOSED
G4-05 Asset-only New Game Wizard      PASS / CLOSED
G4-06 Atomic Final Create             PASS / CLOSED
G4-07 First Playable A                ACTIVE — OWNER UAT GATE
G4-07A Opening Runtime                ACTIVE — CODEX
G4-07B Playable UI Integration        HOLD — KIMI AFTER G4-07A
G4-GATE                               NOT YET
```

Current formal packet:

`my-world/docs/tasks/G4-07A_FIRST_PLAYABLE_OPENING_RUNTIME_TASK.md`

Packet commit:

`8d3bc6e7557c9687141e02a5e554ee90959c2a68`

Formal Code Base:

`39d7300790b2b067b12630f4d1efd4fd51b6d126`

Primary execution owner: **Codex**.  
Task nature: **backend/runtime/context/provider first-Opening vertical**.  
Semantic + Independent Review owner: **GPT**.  
Parent G4-07 Product gate requires **Owner UAT**.  
Return ceiling: **READY FOR INDEPENDENT REVIEW**.

---

## 2. G4-06 final result｜PASS / CLOSED

Accepted implementation/evidence:

```text
implementation     1457ca18c4ef19fd5757844820630649ea85fe6b
implementation doc 383481631cd3de3c4b9fd2cc47eef911961d8373
IR01 process proof 39d7300790b2b067b12630f4d1efd4fd51b6d126
```

GPT Independent Review verdict:

> **PASS. G4-06 Atomic Final Create is CLOSED.**

Confirmed production semantics:

- valid frozen Composition is exact re-reviewed before durable create side effects;
- immutable creation intent fixes `creation_id`, composition fingerprint, `game_id`, root ID, local World/Player/NPC IDs, exact Source pins and selected starting projections;
- same creation identity + same payload converges to the same Game/local identities;
- same creation identity + different payload fails `creation_payload_conflict` without mutating valid truth;
- identical Composition under different creation identities creates distinct Games and distinct local Character IDs;
- One Game = One SQLite remains intact;
- DB internal identity/root snapshot are verified before Game Library registration;
- wrong-existing DB identity fails loud and the foreign DB is preserved;
- Source tamper/missing exact generation fails before intent/Game DB/Game Library side effects;
- selected Source X remains pinned even if current changes to newer Y;
- early Han materialization excludes future/unselected Entry/profile bytes;
- no-Entry persists explicit null and materializes top-level-only World/Character semantics;
- non-temporal scenario Entry works without global historical mode;
- Guaranteed NPC remains canonical cast only, without invented opening/location/player-known/relationship state;
- Provider calls = 0 and accepted Conversation count = 0 during Final Create;
- production SQLite schema remains v4.

### G4-06IR01 actual process restart proof

The first Independent Review found one evidence-quality gap: the original restart test created a fresh FinalCreate object inside one Godot process rather than crossing an OS process boundary.

IR01 corrected only tests/evidence and changed no production code.

For each required crash window:

```text
Process A → injected durable interruption → exits
Process B → fresh Godot process → same disk roots / same creation_id / same Composition → resume
Process C → another fresh Godot process → exact replay
```

Distinct OS PIDs were required for A/B/C.

Proved windows:

- after intent publish;
- after DB commit;
- after Game Library record publish;
- after current publish.

Every row converged to exactly one SQLite + one Game Library record + matching current selection with the same fixed Game/root/local identities. Existing valid DB bytes were SHA-256 stable across resume/replay, proving no destructive delete/recreate recovery.

No correction-02 was required because real process restart exposed no production defect.

Do not reopen G4-06 absent new P0 evidence or explicit Owner/product decision.

---

## 3. Source / creation truth carried into play

Source v0.2-r2 remains frozen:

```text
thin identity/catalog metadata
+ ordered rich semantic_sections
+ disclosure gm_reference | gm_private
+ package-local UTF-8 Markdown/TXT
+ optional Entry-scoped World semantics
+ optional Character T0 profiles
+ exact immutable generation fingerprint over every declared byte
```

Temporal quarantine remains optional authored capability.

G4-06-created Game-local setup now owns the selected starting truth:

```text
selected Entry
→ World top-level + exact Entry
→ Character top-level + exact matching profile when authored

no Entry
→ World top-level only
→ Character top-level only
```

No latest/nearest/later/full-life fallback. No hidden historical mode.

After create, Source provenance remains exact but **Game-local durable materialization is runtime truth**. Future Source current changes must not rewrite or re-materialize an existing Game.

---

## 4. Current parent gate｜G4-07 First Playable A

Purpose:

> Prove that a real World + real Player Character + optional Guaranteed NPCs, with Expansion = none, becomes a genuinely playable AI RPG vertical.

Target product route:

```text
real Source
→ New Game Composition
→ Compatibility Review
→ Atomic Final Create
→ real AI GM Opening
→ continuous free play
→ Save / exit / reopen / Continue
→ Owner UAT A
```

Owner UAT must judge:

- Narrative richness;
- Character individuality;
- anti-convergence;
- Context not starved;
- whether the resulting experience actually feels like an AI RPG rather than a technically correct transcript demo.

Engineering PASS cannot substitute for this UAT.

---

## 5. Current execution task｜G4-07A Opening Runtime

Current packet:

`my-world/docs/tasks/G4-07A_FIRST_PLAYABLE_OPENING_RUNTIME_TASK.md`

Primary outcome:

```text
G4-06 created Game
→ existing-only open
→ read durable Game-local setup/current truth
→ assemble bounded-but-rich first Opening Context
→ real DeepSeek GM Opening
→ accept/persist exactly one first GM Opening
→ close/reopen same Game
→ continue from durable Conversation + World truth
```

Key requirements:

- Opening truth comes from the durable created Game, not Wizard state;
- do not re-resolve mutable Source current to reconstruct runtime semantics;
- existing-only open: missing/wrong/corrupt Game must fail loud, never silently create another Game;
- first accepted Opening must be durable exactly once;
- failed/cancelled/unaccepted attempts must not become accepted Conversation;
- reopen after accepted Opening must not auto-generate a second first Opening;
- first Opening is a GM turn, not a fake persisted Player prompt;
- early Han Provider-visible context must preserve temporal quarantine;
- no-Entry must remain no-Entry;
- Guaranteed NPC existence must not systematically force opening presence/player familiarity;
- reuse existing G2 Provider and G3 Conversation/persistence ownership;
- Context must be bounded but cannot collapse rich Source-derived semantics back to one-line summaries;
- production schema remains v4 unless task returns `BLOCKED` before migration.

Required real Provider proof: at least Han + Afterglow through production G4-06-created Games.

---

## 6. G4-07 execution decomposition

To keep ownership and failure domains clear:

```text
G4-07A — Codex
backend/runtime/context/provider Opening vertical
↓ GPT Independent Review
G4-07B — Kimi
Wizard Final Create → playable Narrative UI integration
↓ GPT Independent Review
G4-07 — Owner UAT
real play judgment
```

G4-07B remains HOLD until G4-07A exposes a stable reviewed seam.

This decomposition does not change the roadmap stage order; it only reduces mixed backend/frontend task risk.

---

## 7. Execution-agent routing

Canonical routing:

`AGENT_EXECUTION_ROUTING_CURRENT.md`

```text
GPT        → Meaning / architecture / task shaping / Decision Propagation / Independent Review
Codex      → backend / mechanism implementation
Kimi       → frontend / UI / interaction implementation
Grok Build → search / external research / evidence discovery
```

Task fit comes before quota availability.

G4-07A is assigned to Codex because the dominant risks are existing-only Game open, Context assembly, Provider orchestration, Conversation durability and retry/idempotence.

G4-07B will route to Kimi because it is frontend/application interaction wiring once the runtime seam is stable.

---

## 8. Holds / warnings

- G4-06 is CLOSED; do not add Provider/UI behavior back into Final Create.
- G4-07A must not implement G4-07B UI.
- G4-07 must not introduce Expansion before First Playable A succeeds.
- Do not build G5 Living World broad architecture or G7 long-session Context platform inside G4-07A.
- Do not hardcode family-specific narrative scripts to make Han/Afterglow appear distinct.
- Real Provider evidence must not commit API keys, auth headers or private environment data.

---

## 9. Current execution order

```text
PASS/CLOSED G4-06
↓
NOW G4-07A Opening Runtime — Codex
↓
GPT Independent Review
↓
G4-07B Playable UI Integration — Kimi
↓
GPT Independent Review
↓
G4-07 First Playable A — Owner UAT
↓
if Product PASS → next G4 work / G4-GATE progression
```
