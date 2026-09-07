---
title: my world｜当前状态
status: current-project-status
version: 17.5
created: 2026-08-26
updated: 2026-09-07
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: MW-015 R1 Sparse Important Experiences Milestone Semantics
current_owner: Codex
parent_task: G6 Core Closure Package 0
semantic_owner: GPT
owner_uat_required: true
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.3
owner_uat_record: my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
active_task_packet: my-world/docs/tasks/MW-015_R1_SPARSE_IMPORTANT_EXPERIENCES_TASK.md
active_task_branch: mw-015-r1-sparse-milestones
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED
G5-GATE                                     PRODUCT PASS

G6 RPG Core Closure + UAT Observability + Internal Dynamic UI ACTIVE
```

Current Package 0 correction train：

```text
MW-018 R1 Known / Off-screen People          ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT RE-UAT PENDING
MW-015 R1 Sparse Important Experiences       CURRENT / CODEX
MW-019 R1 Recommendation UX                  QUEUED NEXT
↓
focused Owner re-UAT
↓
Package 0 close
```

UAT Observability / Debug Mode v0.1 remains the first package after Package 0 closes.

---

## 2. Owner UAT U1 — COMPLETE

Formal evidence：

`my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md`

Confirmed findings：

- MW-018: meaningful player-known/off-screen person could be ineligible for People because the identity evidence seam was too narrow；
- MW-015: Important Experiences over-generated into a per-turn recap；
- MW-019: recommendation chips need concise directions, detailed click-to-composer drafts, five independent alternatives and more readable sizing/spacing。

---

## 3. MW-018 R1 — INTEGRATED / ENGINEERING PASS_WITH_NOTES

Reviewed implementation：

- implementation HEAD: `318a0a2a79bb5f45f582605ac6b98d891a368b4c`
- submitted candidate: `8f48f3cc96a01b1c132c2ccb583193df9c93511a`
- Independent Review: `docs/mw018/MW-018_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `97c1862f3e0c9809e8b8879130b0aade2a848681`
- integration verification: `docs/mw018/MW-018_R1_INTEGRATION_VERIFICATION.md`
- integrated main tip after verification: `2680b2616db69987a451c9d1bf53b24339a9c7cb`

Result：accepted Player/GM person references may both provide bounded exact identity evidence; already-existing stable off-screen actors can become People candidates through Player recall/reference; GM/world-semantic off-screen establishment can still use the existing actor materialization path. Player-only unresolved assertions do not mint World Truth. No display-name/fuzzy/first-match identity authority was added.

Independent Review note to preserve in re-UAT：

> If natural, clearly player-known people are still systematically suppressed by identity guardrails, treat that as architecture over-restriction and reopen the seam instead of stacking more rules.

No Product PASS yet; focused Owner re-UAT happens after MW-015 R1 + MW-019 R1 are integrated.

---

## 4. MW-015 R1 — CURRENT

Frozen correction authority：

`architecture/ui/G6_IMPORTANT_EXPERIENCES_SPARSE_MILESTONE_UAT_CORRECTION_V1_0_DECISION.md`

Task Packet：

`my-world/docs/tasks/MW-015_R1_SPARSE_IMPORTANT_EXPERIENCES_TASK.md`

Branch：

`mw-015-r1-sparse-milestones`

Formal Code Base：

`2680b2616db69987a451c9d1bf53b24339a9c7cb`

Product target：

```text
ordinary accepted turn
→ very often experiences=[]

true life-shaping milestone
→ model may add concise Important Experience
```

Protected boundaries：

- model remains semantic owner of importance；
- no keyword/regex/event whitelist/importance score/every-N-turn/minimum-time Program classifier；
- Character change does not mechanically require a milestone, and a milestone does not mechanically require Character mutation；
- do not regress integrated MW-018 R1 People semantics；
- no `简要回顾` implementation and no navigation/IA change in this revision；
- no historical cleanup/backfill。

Codex highest return state：`READY FOR INDEPENDENT REVIEW`.

---

## 5. MW-019 R1 — QUEUED NEXT

After MW-015 R1 Independent Review / integration, GPT will automatically shape and dispatch the final Package 0 correction：

```text
short recommendation direction labels
→ click fills a detailed editable action draft
→ never auto-send
→ exactly five independently selectable next-action alternatives
→ bounded font/control/spacing readability increase
```

Character-guided recommendation personalization remains later Package 2 work; it does not replace the five-independent-alternatives requirement.

---

## 6. Owner-approved route after Package 0

```text
Package 1  UAT Observability / Debug Mode v0.1
↓
Package 2  OOC + Character-guided Recommendations
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

Debug Mode is deliberately first because it lowers Owner UAT cost across every later Core package.

---

## 7. Protected project invariants

- free-form Player action remains primary；
- `World Truth != actor Knowledge != human-player disclosure`；
- Model Freedom First：Program only owns structural/integrity boundaries, not open semantic meaning；
- Save / Restore / Regenerate currentness remains authoritative；
- no generic framework pulled forward solely for a correction。

---

## 8. Agent routing

```text
GPT
→ correction architecture / Task Shaping / Independent Review / integration / UAT interpretation

Codex
→ current MW-015 R1 implementer

Owner
→ one focused Product re-UAT after the correction train is complete
```
