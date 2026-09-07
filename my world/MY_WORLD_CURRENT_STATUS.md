---
title: my world｜当前状态
status: current-project-status
version: 17.4
created: 2026-08-26
updated: 2026-09-07
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: MW-018 R1 Known / Off-screen People Eligibility Correction
current_owner: Codex
parent_task: G6 Core Closure Package 0
semantic_owner: GPT
owner_uat_required: true
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.3
owner_uat_record: my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
active_task_packet: my-world/docs/tasks/MW-018_R1_KNOWN_PERSON_ELIGIBILITY_TASK.md
active_task_branch: mw-018-r1-known-person-eligibility
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

Current Package 0 state：

```text
MW-015 Character + Important Experiences    HISTORICAL PRODUCT PASS / POST-PASS FINDING REOPENED
MW-018 People Curation + Card Surface        PRODUCT FAIL / R1 AUTHORIZED
MW-019 Five Recommended Actions             PRODUCT FAIL / REVISION QUEUED AFTER MW-015 R1

MW-018 R1                                   CURRENT / CODEX
UAT Observability / Debug Mode v0.1         NEXT AFTER PACKAGE 0 CLOSE
```

---

## 2. Owner UAT U1 — COMPLETE

Formal evidence：

`my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md`

Confirmed findings：

- MW-018: meaningful player-known/off-screen person can be ineligible for People because only current receipt-bound actors are legal Curator candidates；
- MW-019: recommendation chips need concise directions + detailed click-to-composer drafts + independent alternatives + more readable UI sizing；
- MW-015: Important Experiences over-generates into per-turn recap and must return to sparse milestone semantics。

---

## 3. MW-018 R1 — CURRENT

Frozen correction architecture：

`architecture/ui/G6_PEOPLE_KNOWN_PERSON_ELIGIBILITY_UAT_CORRECTION_V1_0_DECISION.md`

Implementation Task Packet：

`my-world/docs/tasks/MW-018_R1_KNOWN_PERSON_ELIGIBILITY_TASK.md`

Branch：

`mw-018-r1-known-person-eligibility`

Formal Code Base：

`fc308e8ee4347ddb8a67e40360f8ce84222d437b`

Core correction：

```text
current accepted Player+GM pair
→ bounded exact person references from Player and/or GM
→ exact existing stable actor resolution or legitimate GM/world-semantic materialization
→ People Curator candidate
→ model decides persistent memory value
```

Protected boundaries：

- current scene presence is neither necessary nor sufficient for a People card；
- Player assertion alone does not mint World Truth；
- no display-name/fuzzy/first-match identity authority；
- unresolved identity = no People update, never guessed actor；
- model remains semantic owner of card worth；
- incidental scene NPCs are not automatically important；
- no universal resolver / new People-specific model call / new SQLite table。

Codex highest return state：`READY FOR INDEPENDENT REVIEW`.

---

## 4. Remaining Package 0 correction queue

After MW-018 R1 Engineering PASS / integration：

```text
MW-015 R1
→ tighten Important Experiences to sparse milestone semantics
→ ordinary turns often no-op
→ no Product IA change yet
↓
MW-019 R1
→ short recommendation labels
→ click produces detailed editable PlayerInput draft
→ five standalone alternative next actions
→ bounded readability sizing correction
↓
focused Owner re-UAT
↓
Package 0 close
```

Do not add `简要回顾` or move `重要经历` under Character without separate explicit Owner approval.

---

## 5. Owner-approved G6 order after Package 0

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

Debug Mode is deliberately pulled forward because it lowers Owner UAT cost across Packages 2–7.

---

## 6. Package 1 UAT Observability target

```text
Debug Mode OFF
→ normal play unchanged

Debug Mode ON
→ per accepted Turn compact UAT trace
→ Narrative / World semantic / actor identity / Character / Experiences / People / Recommendations / Save-Restore
→ changed / no-change / failed / stale / cancelled
→ human-readable error reason
```

Default view does not expose hidden GM/NPC-private semantic values. Player-visible projections may show safe diffs. Debug Mode is read-only and cannot alter game truth, model inputs, mechanics, validation or currentness.

---

## 7. Protected project invariants

- free-form Player action remains primary；
- `World Truth != actor Knowledge != human-player disclosure`；
- exact identity over name guessing；
- Model owns open semantic interpretation/curation；Program owns structure/currentness/persistence；
- Save / Restore / Regenerate currentness remains authoritative；
- no generic framework pulled forward solely for a correction。

---

## 8. Agent routing

```text
GPT
→ correction architecture / Task Shaping / Independent Review / integration / UAT interpretation

Codex
→ current MW-018 R1 implementer

Owner
→ focused Product re-UAT after correction train
```
