---
title: my world｜当前状态
status: current-project-status
version: 17.9
created: 2026-08-26
updated: 2026-09-08
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: Package 0 Fresh Owner Build Prep
current_owner: Codex
parent_task: G6 Core Closure Package 0
semantic_owner: GPT
owner_uat_required: true
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.3
owner_uat_record: my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
audit_triage: architecture/reviews/DEVELOPMENT_AUDIT_2026-09-08_ADOPTION_DECISION.md
reviewed_implementation_main: 5e5fd006fd17683ae811b17138df76a18b0b96aa
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED

G6 RPG Core Closure + UAT Observability + Internal Dynamic UI ACTIVE
```

Package 0 correction train is now Engineering-complete:

```text
MW-018 R1 Known / Off-screen People          ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT RE-UAT PENDING
MW-015 R1 Sparse Important Experiences       ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT RE-UAT PENDING
MW-019 R1 Recommendation UX                  ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT RE-UAT PENDING
MW-020 Core Context Budget Accounting        ENGINEERING PASS_WITH_NOTES / INTEGRATED / NO STANDALONE PRODUCT GATE
↓
Fresh Owner Build Prep                       CURRENT / CODEX
↓
Focused Owner re-UAT
↓
Package 0 closes only on explicit Owner product verdicts
```

Package 1 UAT Observability / Debug Mode v0.1 remains first after Package 0 closes.

---

## 2. Reviewed implementation baseline for Owner build

Current reviewed implementation `main` after MW-020 integration verification:

`5e5fd006fd17683ae811b17138df76a18b0b96aa`

The Product re-UAT build must contain all four reviewed corrections above. Do not build from the old Owner checkout HEAD or an individual task branch.

Build preparation is operational only; it is not authorization for new production changes.

---

## 3. MW-018 R1 — People known/off-screen eligibility

Reviewed result:

- implementation HEAD: `318a0a2a79bb5f45f582605ac6b98d891a368b4c`
- submitted candidate: `8f48f3cc96a01b1c132c2ccb583193df9c93511a`
- Independent Review: `docs/mw018/MW-018_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `97c1862f3e0c9809e8b8879130b0aade2a848681`
- integration verification: `docs/mw018/MW-018_R1_INTEGRATION_VERIFICATION.md`

Product re-UAT note:

> Natural, clearly player-known/off-screen people should be eligible for model curation without requiring physical scene presence. If identity guardrails still systematically suppress such people, reopen the seam rather than stacking more Program rules.

Incidental people may legitimately remain uncarded. Exact identity remains required; no display-name/fuzzy/first-match authority.

No Product PASS yet.

---

## 4. MW-015 R1 — Sparse Important Experiences

Reviewed result:

- implementation HEAD: `aca189b35f5539a190e17a32590c5ca867459df3`
- submitted candidate: `a9cb2ba7c253e8a9c2d07cbf058a6b8d55d16338`
- Independent Review: `docs/mw015/MW-015_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `865f45c59abb8945f6dceb0dbceadc8f22e83335`
- integration verification: `docs/mw015/MW-015_R1_INTEGRATION_VERIFICATION.md`

Product re-UAT must test both sparsity and semantic freedom:

```text
ordinary accepted play
→ very often experiences=[]

true life-shaping event
→ model may retain a concise milestone
```

No Program keyword/category/score/turn-threshold classifier was added. Character mutation and milestone mutation remain independent.

No Product PASS yet.

---

## 5. MW-019 R1 — Recommendation UX

Reviewed result:

- implementation HEAD: `23ba847169f901497a36fabe0514704969452d9e`
- submitted candidate: `5fbb946e4841fc24fcff463e36603443db1664a6`
- Independent Review: `docs/mw019/MW-019_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `0967aceb43ff685dd08114b9c9bda83870d3ef62`
- integration verification: `docs/mw019/MW-019_R1_INTEGRATION_VERIFICATION.md`

Integrated behavior:

```text
one Action Recommender call
→ exactly five {label,draft} pairs
→ UI shows short labels
→ click fills paired detailed editable draft
→ click never sends
→ click/edit makes zero extra recommendation calls
```

Five-independent-next-action semantics remain model-owned. Program adds no semantic diversity/ranking/category classifier.

Re-UAT notes:

- judge whether labels are concise and useful;
- judge whether five items are genuinely standalone alternatives rather than a split plan;
- watch model grounding embellishment;
- observe recommendation latency (reviewed real cases were roughly 10–25 seconds);
- judge small-window scrolling/readability comfort;
- free-form input must remain effortless and primary.

No Product PASS yet.

---

## 6. MW-020 — Core Context Budget Accounting

Reviewed result:

- formal base: `f5dea508be2db5904c2d5ebc726b6f130d8c57fe`
- implementation HEAD: `b423a5b3c7cdecd1d836920f1b053e3675a3bb0e`
- submitted candidate: `9677f6987312230facb351964f6d800748618cf2`
- Independent Review: `docs/mw020/MW-020_INDEPENDENT_REVIEW_IR1.md`
- review commit: `66c28807e5074e1c59373b9139adb64fc2989944`
- integration verification: `docs/mw020/MW-020_INTEGRATION_VERIFICATION.md`
- integrated main tip after verification: `5e5fd006fd17683ae811b17138df76a18b0b96aa`

Engineering verdict: **PASS_WITH_NOTES**.

Result:

- `MAX_PROJECTED_CHARS` remains 16000;
- actual assembled context String is the sole budget truth;
- headings/newlines/separators count exactly once;
- Knowledge → Agency → Evolution each budget against current assembled text;
- exact 16000 is retained; candidate 16001 is omitted under existing whole-section policy;
- audit-equivalent durable Agency action now enters later GM context when it physically fits;
- Restore/currentness filtering remains intact;
- no Context Orchestrator / semantic retrieval / Provider call / persistence change.

Focused evidence: 178 checks / 0 failures. Six directly affected G5/MW regressions exit 0. Known G5-03/G5-04 resource-exit warnings remain baseline/non-blocking.

MW-020 has no separate Owner Product gate.

---

## 7. CURRENT — Fresh Owner Build Prep

Codex now prepares the single focused re-UAT artifact from reviewed `main`.

Required flow:

```text
refresh remote main
↓
confirm reviewed main / no newer production change
↓
safely inspect D:/AI/Projects/my-world
↓
preserve unknown/local modified/untracked Owner files
↓
bring tracked checkout to reviewed main without reset/clean/force/data loss
↓
run established import / Windows export validation
↓
verify run-game.cmd / run-game.ps1 launch path
↓
return Owner Launch Ready
```

Do not:

- add product changes;
- delete or overwrite Owner's unknown files;
- reset/clean/force the checkout;
- modify real Game/Source/settings data;
- consume real Provider calls merely to prepare the build;
- grant Product PASS.

If safe synchronization is blocked by an overlapping local modification, stop and report the exact conflict instead of discarding it.

---

## 8. Focused Package-0 Owner re-UAT

After Launch Ready, Owner tests one fresh build containing all reviewed corrections.

Primary targets:

### People

- a naturally known/off-screen meaningful person can become/update a card;
- current physical presence is neither required nor sufficient;
- incidental NPCs may remain absent;
- watch for architecture over-restriction;
- observe whether no-change background refresh collapses expanded cards enough to be a real UX blocker.

### Important Experiences

- ordinary travel/questioning/routine scene progress usually does not append a milestone;
- genuinely life-shaping choices/events can still be recorded;
- quiet-but-important events must not be mechanically suppressed.

### Recommendations

- five concise labels are easy to scan;
- clicking fills the corresponding detailed draft but never sends;
- draft remains freely editable;
- five entries are independently selectable next actions, not one plan split into steps;
- free-form action remains primary;
- assess arrival latency, grounding, readability and small-window scrolling.

### Living World / continuity

- no known context-budget omission contaminates observations;
- Save/reopen/Restore/currentness remains credible during ordinary play.

Product PASS remains Owner-only.

---

## 9. Package 1 — UAT Observability / Debug Mode v0.1

Immediately after Package 0 closes, GPT Task-Shapes Package 1.

Target:

```text
Debug OFF → normal play unchanged

Debug ON → per accepted Turn compact UAT trace
Narrative / World semantic / actor identity / Character / Experiences / People / Recommendations / Save-Restore
→ changed / no-change / failed / stale / cancelled
→ human-readable sanitized failure reason
```

Audit finding about collapsed recommendation/curation failure reasons belongs here.

Do not build a giant EventBus, expose credentials or reveal hidden GM/NPC-private semantic values by default.

---

## 10. Owner-approved core route after Package 0

```text
Package 1  UAT Observability / Debug Mode v0.1
↓
Package 2  OOC + Character-guided Recommendations + accepted-action Character evidence
↓
Package 3  事务 / Open Threads
↓
Package 4  System / Public d20 real consumer
↓
Package 5  factual Inventory vertical
↓
Package 6  Internal Dynamic UI Host v0.1
↓
Package 7  V0 Core Closure Reality Gate
```

Dynamic UI remains Core-required before V0 closure. Old MW-013 packet remains stale and must not be executed as-is.

---

## 11. Audit triage retained

Canonical:

`architecture/reviews/DEVELOPMENT_AUDIT_2026-09-08_ADOPTION_DECISION.md`

- context-budget bug: repaired by MW-020;
- failure diagnostics: Package 1;
- People no-change collapse UX: observe in re-UAT, fix only if materially disruptive;
- eight layer-boundary findings: architecture debt, opportunistic/bounded hygiene only;
- Application Shell concentration: evolutionary extraction as real consumers arrive;
- documentation drift: Current Status/Roadmap are authoritative; reduce duplicate fast-moving tables over time.

---

## 12. Protected project invariants

- Model Freedom First;
- free-form Player natural-language action remains primary;
- `World Truth != actor Knowledge != human-player disclosure`;
- Program owns structure/identity/currentness/persistence, not open semantic meaning;
- Save / Restore / Regenerate currentness remains authoritative;
- semantic tests cover both legitimate update and legitimate no-op/hold;
- no generic framework pulled forward solely for a correction.

---

## 13. Agent routing

```text
GPT
→ product semantics / Task Shaping / Independent Review / integration / UAT interpretation

Codex
→ Fresh Owner Build Prep from reviewed main only

Owner
→ focused Package-0 Product re-UAT after Owner Launch Ready
```
