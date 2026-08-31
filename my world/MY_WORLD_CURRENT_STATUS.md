---
title: my world｜当前状态
status: current-project-status
version: 7.6
created: 2026-08-26
updated: 2026-08-31
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-07UAT01 Owner Launch Freshness Correction
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
G4-07 First Playable A                OWNER UAT BLOCKED — LAUNCH FRESHNESS
G4-07A Opening Runtime                PASS / CLOSED
G4-07B Playable UI Integration        PASS / CLOSED
G4-07UAT01 Owner Launch Freshness     ACTIVE — CODEX
G4-GATE                               NOT YET
```

Current formal correction packet:

`my-world/docs/tasks/G4-07UAT01_OWNER_LAUNCH_FRESHNESS_CORRECTION_TASK.md`

Formal Code Base:

`1bd0d414dfe8ed5a7781d9db6813317d2f20bf91`

Primary execution owner: **Codex**.  
Reviewer / semantic owner: **GPT**.  
Return ceiling: **READY FOR INDEPENDENT REVIEW**.

Owner UAT resumes after this correction passes Independent Review.

---

## 2. Why Owner UAT is temporarily blocked

Owner launched through the canonical repository-native path:

```text
run-game.cmd
→ run-game.ps1
→ build/windows/my-world.exe
```

Observed result: historical UI where New Game still appeared unavailable.

Current repository inspection established the mechanism:

- `run-game.ps1` launches the pre-existing exported `build/windows/my-world.exe`;
- `build/` is intentionally gitignored, so Kimi/Codex source commits do not update that local binary;
- the launcher checks whether the executable exists, but not whether it is older than the current checkout;
- therefore an old export can silently be launched after current source changes.

This is a **launch-path freshness defect**. It does not invalidate the G4-07A or G4-07B engineering PASS results, which exercised current project code directly through Godot and real Provider tests.

---

## 3. G4-07UAT01 target

Required normal Owner path:

```text
run-game.cmd
→ determine whether Windows Desktop export is missing/stale
→ if missing/stale, export current checkout with Godot preset `Windows Desktop`
→ verify refreshed executable exists
→ launch refreshed build/windows/my-world.exe
```

At minimum freshness covers product-build inputs:

- `src/**`
- `addons/**`
- `project.godot`
- `export_presets.cfg`

Rules:

- do not make docs/tests alone force rebuild unless they are export inputs;
- if export fails, fail loud and do not fall back to stale binary;
- preserve `.env.local` secret whitelist and restoration behavior;
- keep `build/` gitignored and do not commit binaries;
- do not change G4-07 gameplay/UI/backend semantics for this correction.

---

## 4. Accepted G4-07 engineering state remains frozen

### G4-07A

Accepted implementation/evidence:

```text
implementation  dac0e8e4bf655a234ca5b8d0952f6a199373b4af
continuation    221710941950198c4fced9c30991bd295fea39ef
evidence / HEAD fdb6a30ad138c332837f17af1d8c74b5643db44b
```

GPT Independent Review: **PASS / CLOSED**.

### G4-07B

Accepted implementation/evidence:

```text
implementation  e13099384c12090197822d1d504089decc1f893b
evidence / HEAD 2f45614baa0a3c38dac3439934122084817d4602
GPT IR record   my-world/docs/g4_07b/G4-07B_INDEPENDENT_REVIEW.md
```

GPT Independent Review: **PASS / CLOSED**.

Do not generically reopen either task because the Owner launcher is stale.

---

## 5. Owner UAT after correction

Existing UAT packet remains:

`my-world/docs/tasks/G4-07_FIRST_PLAYABLE_A_OWNER_UAT.md`

After G4-07UAT01 PASS, Owner resumes real play and judges:

- Narrative richness;
- Character individuality;
- Han vs Afterglow distinctness;
- anti-convergence;
- Context sufficiency;
- temporal/future leakage;
- no-Entry playability;
- whether the product feels like an AI RPG rather than an engineering demo.

Only then may GPT decide:

```text
G4-07 PASS / CLOSED
```

or

```text
G4-07 Product Correction ACTIVE
```

Do not start G4-08 before G4-07 Product PASS.
