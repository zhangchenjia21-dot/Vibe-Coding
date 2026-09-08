---
title: my world｜当前状态
status: current-project-status
version: 17.10
created: 2026-08-26
updated: 2026-09-08
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: Package 0 Focused Owner Re-UAT U2
current_owner: Owner
parent_task: G6 Core Closure Package 0
semantic_owner: GPT
owner_uat_required: true
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.3
owner_uat_record: my world/docs/uat/G6_PACKAGE0_OWNER_REUAT_U2.md
audit_triage: architecture/reviews/DEVELOPMENT_AUDIT_2026-09-08_ADOPTION_DECISION.md
reviewed_implementation_main: 5e5fd006fd17683ae811b17138df76a18b0b96aa
owner_launch_ready: true
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

Package 0 correction train is Engineering-complete and Owner Launch Ready:

```text
MW-018 R1 Known / Off-screen People          ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT RE-UAT ACTIVE
MW-015 R1 Sparse Important Experiences       ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT RE-UAT ACTIVE
MW-019 R1 Recommendation UX                  ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT RE-UAT ACTIVE
MW-020 Core Context Budget Accounting        ENGINEERING PASS_WITH_NOTES / INTEGRATED / NO STANDALONE PRODUCT GATE
↓
Focused Owner Re-UAT U2                      CURRENT / OWNER
↓
Package 0 closes only on explicit Owner product verdicts
```

Package 1 UAT Observability / Debug Mode v0.1 remains first after Package 0 closes.

---

## 2. Frozen U2 product artifact

Implementation `main` is confirmed at:

`5e5fd006fd17683ae811b17138df76a18b0b96aa`

This is the frozen product-code artifact for U2. Do not add production code during active Owner UAT.

Formal U2 handoff:

`my world/docs/uat/G6_PACKAGE0_OWNER_REUAT_U2.md`

Owner Launch Ready result reports:

- Owner checkout HEAD == origin/main == reviewed artifact;
- existing `.gitignore` modification + 10 pre-existing untracked sidecars preserved unchanged;
- Godot 4.7.2 final import exit 0 with zero errors/warnings;
- Windows export freshly rebuilt, exit 0 with zero errors/warnings;
- new PCK freshness corresponds to current reviewed checkout;
- EXE / PCK / SQLite DLL verified;
- build prep did not launch game, call Provider, access real Game/Source/settings data or modify product code.

Owner launch command:

```powershell
& "D:\AI\Projects\my-world\run-game.cmd"
```

---

## 3. MW-018 R1 — People known/off-screen eligibility

Reviewed result:

- implementation HEAD: `318a0a2a79bb5f45f582605ac6b98d891a368b4c`
- submitted candidate: `8f48f3cc96a01b1c132c2ccb583193df9c93511a`
- Independent Review: `docs/mw018/MW-018_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `97c1862f3e0c9809e8b8879130b0aade2a848681`
- integration verification: `docs/mw018/MW-018_R1_INTEGRATION_VERIFICATION.md`

U2 product watch:

- meaningful player-known/off-screen people may become/update cards;
- physical scene presence is neither necessary nor sufficient;
- incidental actors may legitimately remain uncarded;
- Player reference/recall may participate when exact identity is safely resolvable;
- no private/omniscient information leak;
- if clearly known people are still systematically suppressed, reopen the identity seam rather than stack more Program rules;
- observe whether no-change refresh collapsing expanded cards is materially disruptive.

No Product PASS yet.

---

## 4. MW-015 R1 — Sparse Important Experiences

Reviewed result:

- implementation HEAD: `aca189b35f5539a190e17a32590c5ca867459df3`
- submitted candidate: `a9cb2ba7c253e8a9c2d07cbf058a6b8d55d16338`
- Independent Review: `docs/mw015/MW-015_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `865f45c59abb8945f6dceb0dbceadc8f22e83335`
- integration verification: `docs/mw015/MW-015_R1_INTEGRATION_VERIFICATION.md`

U2 product watch:

```text
ordinary accepted play
→ very often experiences=[]

true life-shaping event
→ model may retain a concise milestone
```

Important Experiences must remain selected protagonist life history, not rolling recap. Sparsity must remain model-semantic rather than a rigid Program classifier; quiet but genuinely life-shaping events remain possible.

No U2 decision is implied yet on adding `简要回顾` or moving `重要经历` under Character.

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

U2 product watch:

- labels concise and scan-friendly;
- detailed draft corresponds to label and remains editable;
- five items are standalone alternatives, not one plan split into steps;
- free-form input remains effortless and primary;
- judge grounding embellishment, recommendation latency, readability and small-window scrolling by material impact on real play.

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
- audit-equivalent durable Agency action enters later GM context when it physically fits;
- Restore/currentness filtering remains intact;
- no Context Orchestrator / semantic retrieval / Provider call / persistence change.

MW-020 has no standalone Owner Product verdict. During U2, only note obvious continuity failure if naturally observed.

---

## 7. CURRENT — Focused Owner Re-UAT U2

Owner should play naturally first. U2 is not intended to become a synthetic engineering test script.

Findings may be reported incrementally. GPT accumulates them and does not dispatch Codex during active UAT unless a hard blocker makes continued play impossible. When Owner explicitly says the UAT round is finished, GPT classifies findings by lineage and decides Package 0 verdict/next work.

Primary U2 questions:

### People

- Do meaningful known/off-screen people appear/update when they should?
- Can incidental people legitimately remain absent?
- Does the system still feel over-restricted by identity guardrails?
- Does background no-change refresh collapse expanded cards enough to hurt reading?

### Important Experiences

- Does ordinary play now stay sparse?
- Do genuine life turning points still get retained?
- Does the surface read like life history rather than per-turn recap?

### Recommendations

- Are five short labels easy to scan?
- Does click fill the corresponding detailed editable draft without sending?
- Are there five genuinely independent next actions?
- Is free-form action still primary?
- Are latency, grounding and small-window scrolling acceptable in real play?

### Continuity

- Does normal world/NPC continuity feel credible?
- If Save/reopen/Restore is naturally exercised, does currentness remain credible?

Owner may stop when enough real-play evidence exists; there is no fixed turn quota for this focused correction UAT.

---

## 8. U2 verdict structure

At U2 end, record separately:

```text
MW-018 R1 People                 = PASS / FAIL_WITH_FINDING
MW-015 R1 Important Experiences = PASS / FAIL_WITH_FINDING
MW-019 R1 Recommendations       = PASS / FAIL_WITH_FINDING
Package 0                       = CLOSE only if required product outcomes pass
```

MW-020 requires no separate verdict unless a new product-visible continuity defect appears.

Aesthetic polish that does not impair readability/playability may be deferred. Truth/disclosure, semantic usefulness, model freedom, currentness and comfortable readability remain product-gate concerns.

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
- People no-change collapse UX: observe in U2, fix only if materially disruptive;
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
→ UAT interpretation / finding accumulation / product verdict / next Task Shaping after U2 closes

Codex
→ HOLD during active U2 unless GPT dispatches a hard-blocker correction

Owner
→ current focused Package-0 Product re-UAT
```
