---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-08-31
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-07 First Playable A
current_execution_task: G4-07UAT01 Owner Launch Freshness Correction
semantic_owner: GPT
current_execution_owner: Codex
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。不是 Product / Architecture / Status 的替代权威。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Read first

Governance:

1. `AGENTS.md`
2. `my world/MY_WORLD_CURRENT_STATUS.md`
3. `my world/MY_WORLD_架构_CURRENT.md`
4. `my world/MY_WORLD_核心设计原则_CURRENT.md`

Implementation:

5. `AGENTS.md`
6. `docs/tasks/G4-07UAT01_OWNER_LAUNCH_FRESHNESS_CORRECTION_TASK.md`
7. `run-game.cmd`
8. `run-game.ps1`
9. `export_presets.cfg`
10. `docs/tasks/G4-07_FIRST_PLAYABLE_A_OWNER_UAT.md`
11. `docs/g4_07b/G4-07B_INDEPENDENT_REVIEW.md`

---

## 2. Current state

```text
G4-06 Atomic Final Create             PASS / CLOSED
G4-07A Opening Runtime                PASS / CLOSED
G4-07B Playable UI Integration        PASS / CLOSED
G4-07 First Playable A                OWNER UAT BLOCKED — LAUNCH FRESHNESS
G4-07UAT01 Owner Launch Freshness     ACTIVE — CODEX
G4-GATE                               NOT YET
```

G4-07A/B engineering PASS remains accepted. The current blocker was discovered only when Owner used the canonical product launch path.

---

## 3. UAT blocker root cause

Owner launched `run-game.cmd` and saw historical UI where New Game remained unavailable.

Verified current mechanism:

```text
run-game.cmd
→ run-game.ps1
→ build/windows/my-world.exe
```

`build/` is gitignored. `run-game.ps1` checks whether the EXE exists but not whether it is stale versus the current checkout. Therefore an old exported EXE can silently survive source updates and be launched during Owner UAT.

This does **not** invalidate G4-07B IR, whose real application tests instantiated current `main.tscn` through Godot and exercised current code.

---

## 4. Current correction

Packet:

`my-world/docs/tasks/G4-07UAT01_OWNER_LAUNCH_FRESHNESS_CORRECTION_TASK.md`

Formal Code Base:

`1bd0d414dfe8ed5a7781d9db6813317d2f20bf91`

Owner: **Codex**. Reviewer: **GPT**.

Target:

```text
run-game.cmd / run-game.ps1
→ detect missing/stale Windows Desktop export
→ when stale, export current checkout with installed Godot
→ verify success
→ launch refreshed build/windows/my-world.exe
```

At minimum freshness accounts for `src/**`, `addons/**`, `project.godot`, `export_presets.cfg`.

If export fails: fail loud, never launch stale EXE.

Keep `.env.local` secret whitelist/restore behavior. Keep `build/` ignored. Do not alter gameplay/UI/backend semantics.

---

## 5. After correction

After Codex pushes G4-07UAT01:

1. refresh both repos;
2. Independent Review actual launcher implementation/evidence;
3. verify missing/current/stale/export-failure cases;
4. verify refreshed launched UI exposes current G4-07B New Game path;
5. if PASS, return G4-07 to `READY FOR OWNER UAT`;
6. Owner resumes `docs/tasks/G4-07_FIRST_PLAYABLE_A_OWNER_UAT.md`.

Only Owner product play can decide G4-07 PASS/CLOSED vs Product Correction.

Do not start G4-08 before G4-07 Product PASS.
