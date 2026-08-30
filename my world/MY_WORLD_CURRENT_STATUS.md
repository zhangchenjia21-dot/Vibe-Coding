---
title: my world｜当前状态
status: current-project-status
version: 6.9
created: 2026-08-26
updated: 2026-08-30
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-02R1M1-IR01 Optional Temporal Scope Evidence Correction
current_owner: Codex
parent_task: G4-02R1M1
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
G4-02 Engineering implementation      HISTORICAL PASS for v0.1
G4-02R1 Semantic/full-fidelity phase  PASS / FROZEN
G4-02R1M1 mechanism implementation    READY FOR IR
G4-02R1 GPT Independent Review        CORRECTION REQUIRED — EVIDENCE GAP ONLY
G4-02R1M1-IR01 evidence correction    ACTIVE — CODEX
G4-02R1 Overall                       IMPLEMENTATION / IR PENDING
G4-03 Managed Local Source Library    PASS / CLOSED
G4-04 Multi-Game / Game Library       PASS / CLOSED
G4-05 Asset-only New Game Wizard      REWORK / HOLD
G4-06+                                HOLD
G4-GATE                               NOT YET
```

Current correction packet:

`my-world/docs/tasks/G4-02R1M1-IR01_OPTIONAL_TEMPORAL_SCOPE_EVIDENCE_CORRECTION_TASK.md`

IR01 Formal Code Base:

`74d1fe8d93e527794db33d0280d76d16674ae1d7`

IR01 Governance Base:

`14fb4d686c458c2a09a348e54088377f96655d65`

IR01 packet commit:

`f020848f23464c6022358acf2aa9b82bc940fd83`

Prior mechanism implementation:

- implementation commit: `eb11655f8ff592ae096915fab50553708c0b79df`
- evidence commit: `3b2c9d5fa0a9756ee670eb42179b5ae8359e0b2b`
- evidence doc: `my-world/docs/source/G4-02R1_R2_MECHANISM_IMPLEMENTATION_EVIDENCE.md`

Current execution owner: **Codex**.  
Final Independent Review owner after IR01 return: **GPT**.  
Return ceiling for Codex: **READY FOR INDEPENDENT REVIEW**.

---

## 2. G4-02R1 mechanism result so far

Codex implemented Source v0.2-r2 through the production Source seam and returned engineering evidence.

GPT Independent Review inspected production code, real fixtures and assertions rather than trusting the test summary.

Current IR finding:

> **No blocking production-mechanism defect found. One explicit acceptance evidence gap remains.**

Confirmed working:

- real 2 World + 6 Character load through production Source interface;
- exact selected World Entry projection;
- exact Character T0 profile projection;
- no latest/nearest/later/full-life fallback;
- Han closed temporal coverage mechanism;
- Afterglow shared 1287 profile bindings;
- cross-world zero-coverage non-hard-blocking route;
- fingerprint coverage wider than Runtime visibility;
- missing/tampered rich Source fail-loud;
- optional portrait absence;
- G4-03 regression;
- preserved G4-05 Wizard/Composition regression;
- Godot 4.7.2 / Windows evidence.

The explicit missing acceptance assertion was:

```text
孙权 249 -> exact profile
孙权 263 -> temporal incompatible
孙权 280 -> temporal incompatible   # production logic implies this, but no dedicated assertion existed
```

IR01 must add direct production-path evidence before final PASS.

---

## 3. Optional Temporal Source Scope｜CURRENT CLARIFICATION

Owner clarified on 2026-08-30 that T0 / post-T0 quarantine is **not** a mandatory mode for all World/Character assets.

Canonical decision:

`architecture/source/G4_OPTIONAL_TEMPORAL_SOURCE_SCOPE_DECISION.md`

Implementation semantic clarification:

`my-world/docs/source/G4-02R1_OPTIONAL_TEMPORAL_SCOPE_CAPABILITY_CLARIFICATION.md`

Formal rule:

> **Temporal Quarantine is an optional Source capability selected by authored need, not a universal asset mode.**

The game must support both:

```text
Temporal-scoped Source
→ multiple authored T0 cuts
→ exact selected projection
→ future/canon quarantine

Non-temporal Source
→ rich ordinary starting semantics
→ no artificial T0 profile matrix
```

Typical historical assets such as Han-era World/Characters need the first form because multiple selectable historical T0s create a real future-information hazard.

Original/fantasy assets such as future Afterglow-like content may use the second form when no temporal variation exists.

Do not add a global `historical`, `temporal_mode`, family flag or equivalent ontology. The capability is expressed only when the authored Source actually uses Entry-scoped temporal sections or Character `t0_profiles`.

`character_card.v0.2` keeps `t0_profiles` optional. A non-temporal Character may keep complete reusable semantics top-level and omit profiles entirely.

Entries may still exist for opening/scenario/location choices without implying historical quarantine.

The current Afterglow fixtures remain valid with one 1287 profile bound to all three Entries, but that structure is not a requirement for future fantasy/original assets.

---

## 4. Frozen Source v0.2-r2 architecture

Core shape:

```text
thin identity / catalog metadata
+ ordered rich semantic_sections
+ disclosure = gm_reference | gm_private
+ package-local UTF-8 Markdown/TXT content files
+ Entry-scoped World sections only where authored need exists
+ optional Character T0 profiles only where authored need exists
+ exact immutable generation fingerprint over every declared byte
```

When temporal scoping is used:

```text
World selected projection
= top-level always-safe sections
+ exact selected Entry sections

Character selected projection
= top-level always-safe sections
+ exact matching T0 profile sections
```

Never fallback to latest / nearest / later / complete-life biography.

Formal invariant for temporally scoped Source:

> **Do not show the model a post-T0 answer and then ask it to forget that answer.**

```text
Immutable Source Package Total Content
!= Selected T0 Source Projection
!= Game-local Canonical Reality
!= Runtime Relevant Set
!= Model-visible Working Set
```

Fingerprint coverage remains broader than Runtime visibility.

Character starting individuality remains required where T0 profiles exist:

> **Age / developmental stage is a capability boundary, not a personality template.**

---

## 5. G4-02R1M1-IR01 scope

Owner: **Codex**.

This is expected to be a test/evidence correction, not a production redesign.

Required proof:

1. complete direct Han temporal-coverage assertions, including Sun Quan 280;
2. a task-owned synthetic non-temporal Character with rich top-level semantics and no `t0_profiles`;
3. a task-owned synthetic non-temporal World whose Entries do not require temporal section partitioning;
4. proof that no global historical/temporal switch is introduced;
5. existing Han future quarantine remains strict;
6. Afterglow current bindings remain valid;
7. G4-03/G4-05 regressions stay green.

If new tests reveal a real production defect, Codex must report it rather than broadening architecture silently.

---

## 6. Game-local evolvable semantics｜FROZEN

Canonical decision:

`architecture/source/G4_GAME_LOCAL_EVOLVABLE_SEMANTICS_DECISION.md`

Formal invariant:

> **Source schema is not the possibility ceiling of the Living World. Game-local semantic structure is evolvable.**

After Final Create:

- exact Source generation remains immutable;
- Game-local canonical objects keep stable Program-owned identity/provenance/lifecycle kernels;
- model/runtime may add/update/remove previously unanticipated local semantic facets;
- existing canonical Domains win over duplicate generic truth;
- the model does not mutate Source, global Source contract or physical SQLite schema;
- local semantic evolution must be durable and Timeline/Save/Restore reversible.

---

## 7. G4-03 / G4-04 / G4-05 disposition

### G4-03

**PASS / CLOSED.** Preserve immutable generation, staged verified publish, restart truth, exact lookup and tamper/missing fail-loud semantics.

### G4-04

**PASS / CLOSED.** Formal topology remains:

> **One Game = One SQLite.**

Application index metadata != gameplay truth.

### G4-05

**REWORK / HOLD.** Candidate `145c3e1192b443f6284da7f36aee74619adad5bf` remains provisional accepted Wizard/Composition engineering evidence.

Old packet:

`my-world/docs/tasks/G4-05R1_REAL_ASSET_FIDELITY_CORRECTION_TASK.md`

Status:

> **SUPERSEDED / DO NOT EXECUTE**

Do not resume G4-05 until GPT final Independent Review closes G4-02R1.

---

## 8. Current execution order

```text
DONE
G4-02R1 semantic audit + v0.2-r2 contract correction
↓
DONE
2 World + 6 Character full-fidelity migration + cross-family shape freeze
↓
DONE
G4-02R1M1 production mechanism implementation + engineering evidence
↓
DONE
GPT first Independent Review pass
→ no production defect found
→ explicit evidence correction required
↓
NOW
G4-02R1M1-IR01 Codex evidence correction
↓
GPT final Independent Review
↓
if PASS
close G4-02R1
↓
resume G4-05 closure/regression
↓
G4-06 Atomic Final Create
↓
G4-07 First Playable A — Owner UAT
```

G4-07 must still include Narrative richness, Character individuality, anti-convergence product pressure and bounded-but-not-starved Context. Engineering PASS cannot substitute for future Owner UAT.

---

## 9. Current blockers / waiting

```text
Blocking G4-02R1 close:
- direct Sun Quan 280 temporal-incompatibility assertion
- regression proof that non-temporal World/Character assets are not forced into T0 matrices

Current task: G4-02R1M1-IR01
Current execution owner: Codex
Final reviewer: GPT
IR01 Formal Code Base: 74d1fe8d93e527794db33d0280d76d16674ae1d7
IR01 Governance Base: 14fb4d686c458c2a09a348e54088377f96655d65
IR01 packet commit: f020848f23464c6022358acf2aa9b82bc940fd83
G4-05R1: SUPERSEDED / DO NOT EXECUTE
G4-05: REWORK / HOLD
G4-06+: HOLD
```

---

## 10. GPT context takeover checkpoint

Current navigation handoff:

`handoff/GPT_CONTEXT_HANDOFF_CURRENT.md`

A replacement GPT must refresh both GitHub `main` HEADs, read current governance + implementation instructions, and respect the active owner.

If IR01 has not returned, do not duplicate Codex work.

If IR01 has returned, GPT's next primary responsibility is final Independent Review and Decision Propagation. Only after PASS may G4-05 resume.
