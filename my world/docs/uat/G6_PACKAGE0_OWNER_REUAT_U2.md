---
title: my world｜G6 Package 0 Focused Owner Re-UAT U2
status: OWNER UAT ACTIVE
version: 1.0
created: 2026-09-08
updated: 2026-09-08
owner: Owner
semantic_owner: GPT
product_code_artifact: 5e5fd006fd17683ae811b17138df76a18b0b96aa
launch_command: D:\AI\Projects\my-world\run-game.cmd
supersedes_for_current_uat:
  - my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
---

# G6 Package 0｜Focused Owner Re-UAT U2

## 1. Gate state

Package 0 correction implementation is Engineering-complete and the fresh Windows Owner build is reported **OWNER LAUNCH READY**.

This UAT is the single focused product gate for the correction train:

```text
MW-018 R1  Known / Off-screen People
MW-015 R1  Sparse Important Experiences
MW-019 R1  Recommendation UX
MW-020     Core Context Budget Accounting
↓
Owner focused real play
↓
Package 0 closes only on explicit Owner product verdict
```

The tested product-code baseline is frozen at:

`5e5fd006fd17683ae811b17138df76a18b0b96aa`

Do not introduce new product code while U2 is active.

## 2. Owner Launch Ready handoff

Owner-reported build preparation result:

- local HEAD == `origin/main` == `5e5fd006fd17683ae811b17138df76a18b0b96aa`;
- existing `.gitignore` modification and 10 pre-existing untracked sidecars preserved unchanged;
- Godot 4.7.2 final import exit 0, zero errors/warnings;
- Windows export freshly rebuilt and validated, exit 0, zero errors/warnings;
- new PCK differs from the old artifact and freshness corresponds to the reviewed checkout;
- `my-world.exe`, `my-world.pck` and SQLite DLL verified;
- no game launch, Provider call, real Game/Source/settings access or production-code modification occurred during prep.

Owner launch command:

```powershell
& "D:\AI\Projects\my-world\run-game.cmd"
```

## 3. UAT method

Use ordinary natural play first. Do not turn the session into a synthetic test harness unless a specific behavior needs confirmation.

Owner may report findings incrementally. GPT should accumulate findings without interrupting the session with new Codex dispatches. Shape/dispatch revisions only after Owner says the UAT round is finished, unless a hard blocker makes continued play impossible.

## 4. Primary product targets

### A. People｜MW-018 R1

Observe whether the People surface now behaves like player-known persistent memory rather than current-scene attendance.

PASS direction:

- a meaningful person the Player already legitimately knows may become/update a card even while off-screen;
- Player recall/reference can participate when identity is safely resolvable;
- physical presence is neither necessary nor sufficient;
- incidental guards/soldiers/passers-by may legitimately remain uncarded;
- no private/omniscient information appears;
- later learned information updates the same stable person rather than duplicating by display name.

Important architecture watch:

> If naturally and clearly player-known people are still systematically suppressed by identity guardrails, treat this as over-restriction and reopen the identity seam instead of stacking more Program rules.

Also observe whether background no-change refresh repeatedly collapses expanded cards enough to be a real UX blocker.

### B. Important Experiences｜MW-015 R1

PASS direction:

```text
ordinary movement / questioning / routine scene progress
→ usually no new Important Experience

true life-shaping choice / turning point
→ model may retain a concise milestone
```

Important Experiences must answer **“我是怎样走到现在的？”**, not become a rolling turn recap.

Sparsity must come from model semantic judgment, not from obvious rigid suppression. Quiet but genuinely important developments must remain possible.

No U2 decision is implied yet about adding a separate `简要回顾` or moving `重要经历` under Character; those remain separate product choices.

### C. Recommended Actions｜MW-019 R1

PASS direction:

- five visible items are concise, scan-friendly direction labels;
- clicking one fills the corresponding detailed natural-language draft into the existing composer;
- click never sends;
- the draft is freely editable;
- five items are five independently selectable next actions, not one composite plan split into five steps/sentences;
- free-form action remains effortless and primary;
- recommendations remain grounded in player-visible accepted information and do not promise outcomes;
- recommendation/composer typography, button size, padding and spacing are comfortable for sustained play.

Observe rather than automatically fail on:

- recommendation arrival latency; reviewed real cases were roughly 10–25 seconds;
- small-window scrolling;
- occasional model embellishment of plausible but not explicitly established details.

These become defects only if they materially harm normal play or violate truth/disclosure boundaries.

### D. Living World continuity｜MW-020 confidence check

MW-020 itself has no standalone Product gate; its accounting correctness is already Engineering-reviewed.

During ordinary play, simply watch for obvious continuity failures where durable NPC/world activity appears to be forgotten by later GM reasoning. Do not attempt to reverse-engineer the 16,000-character boundary manually.

Save/reopen/Restore/currentness should remain credible if naturally exercised during this UAT.

## 5. Verdict structure

Product verdicts remain Owner-only.

At U2 end, record separately:

```text
MW-018 R1 People               = PASS / FAIL_WITH_FINDING
MW-015 R1 Important Experiences = PASS / FAIL_WITH_FINDING
MW-019 R1 Recommendations       = PASS / FAIL_WITH_FINDING
Package 0                       = CLOSE only if required product outcomes pass
```

MW-020 requires no separate Owner verdict unless real play reveals a new product-visible continuity defect.

Aesthetics that do not impair readability/playability may be deferred. Readability, truth/disclosure, semantic usefulness, freedom and currentness are product-gate concerns.

## 6. After Package 0

If Package 0 passes, the next authorized work is:

**Package 1 — UAT Observability / Debug Mode v0.1**

Its purpose is to make future UAT cheaper by showing per accepted Turn which domains changed, remained unchanged, failed, became stale or were cancelled, with bounded human-readable failure reasons and without exposing hidden NPC/GM-private truth or credentials.
