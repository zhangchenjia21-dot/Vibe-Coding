---
title: my world｜当前状态
status: current-project-status
version: 17.2
created: 2026-08-26
updated: 2026-09-07
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: Combined Owner UAT — MW-018 People + MW-019 Recommendations
current_owner: Owner
parent_task: G6 Core Closure Package 0
semantic_owner: GPT
owner_uat_required: true
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.3
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

Current G6 implementation state：

```text
MW-011 RPG Host / Player Profile            PRODUCT PASS / CLOSED
MW-012 Zhang Chen Player Character Card     ENGINEERING PASS / INTEGRATED
MW-014 Model-driven Information Curation    ENGINEERING PASS / INTEGRATED
MW-015 Character + Important Experiences    PRODUCT PASS / CLOSED
MW-016 People Architecture Audit            PASS / CLOSED
MW-017 People Identity Bridge               ENGINEERING PASS / INTEGRATED
MW-018 People Curation + Card Surface        ENGINEERING PASS / INTEGRATED / OWNER UAT ACTIVE
MW-019 Five Recommended Actions             ENGINEERING PASS / INTEGRATED / OWNER UAT ACTIVE

UAT Observability / Debug Mode v0.1         ROUTE AUTHORIZED AS NEXT AFTER PACKAGE 0
Internal Dynamic UI / MW-013 capability     ROUTE AUTHORIZED AS CORE
old MW-013 Task Packet                      STALE / DO NOT EXECUTE AS-IS
```

---

## 2. Owner-approved G6 order

Owner priority remains：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能与体验优化后置。**

Owner additionally required：

> **动态 UI 必须在 V0 Core Closure 前完成。**

> **当前 MW-018/019 这一轮结束后，优先实现 Debug Mode + 每回合后台数据变化可视化，以显著降低之后每轮 UAT 成本。**

CURRENT G6 order：

```text
Package 0  MW-018 + MW-019 Combined Owner UAT      ← CURRENT
↓
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

Package 1 is an explicit exception to “diagnostics usually post-closure” because it is a **UAT leverage capability** consumed by Packages 2–7.

---

## 3. Package 1 UAT Observability semantics

No executable MW Work ID has been minted yet; Task Shaping occurs only after Package 0 closes.

Target v0.1：

```text
Debug Mode OFF
→ normal play unchanged

Debug Mode ON
→ accepted Turn 后提供 compact UAT trace
→ 每个真实 domain/lane 显示 changed / no-change / failed / stale / cancelled
→ 出错时自动显示可理解原因
```

Initial real consumers：

- Narrative terminal；
- World semantic terminal + changed/no-change；
- actor/NPC materialization / identity bridge terminal；
- Character changed/no-change；
- Important Experiences changed/no-change；
- People changed/no-change；
- Recommendations success/malformed/unavailable/stale；
- Save / Restore / currentness 关键结果。

Later Packages add Open Threads / mechanics / Inventory to the same read-only diagnostic projection.

Protected boundary：

- default Debug view shows change existence + terminal state + Turn / Provider / Model / necessary evidence；
- hidden GM-private / NPC-private semantic values are not shown by default, avoiding accidental UAT spoilers；
- player-visible projections may expose safe diff；
- Debug Mode is read-only and cannot alter gameplay truth, model input, mechanics result or currentness；
- no API keys / credentials / unrelated local privacy；
- no giant EventBus / universal telemetry framework。

Full polished player-facing “本回合变化” remains a later product-expansion outcome; Package 1 only pulls forward the UAT/debug slice required to reduce Owner verification cost.

---

## 4. Protected information-curation authority

Canonical decisions remain：

- `architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`
- `architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md`
- `architecture/ui/G6_INITIAL_CHARACTER_CURATION_BASELINE_V1_0_DECISION.md`
- `architecture/ui/G6_PEOPLE_SURFACE_V1_0_DECISION.md`
- `architecture/ui/G6_PEOPLE_IDENTITY_AND_CURATION_V1_0_DECISION.md`

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

Program does not add keyword/name/encounter-count/importance/relationship heuristics to reproduce open semantics.

---

## 5. MW-018 People — OWNER UAT ACTIVE

Integrated behavior：

```text
人物
→ model-maintained latest-known cards
→ default collapsed
→ expand for relationship / latest-known details
→ hidden NPC changes do not auto-update
→ accepted replacement / Restore / reopen follow current history
```

Retained UAT risk：real new-actor identity correlation can occasionally be omitted by the model; exact bridge must continue refusing display-name guessing.

Owner continues testing one known person and one newly introduced person.

---

## 6. MW-019 Recommendations — OWNER UAT ACTIVE

Protected rule：

> **Five recommended actions != five allowed actions.**

Current integrated behavior：

```text
accepted GM Narrative
→ player-safe Action Recommender
→ exactly five actions on successful valid output
→ click fills PlayerInput only
→ Player edits/ignores freely
→ normal send / d20 route unchanged
```

### Active UAT finding captured — not yet final verdict

Owner observed that current recommendation chips place long full-action prose directly inside the recommendation area, making them crowded, incomplete-looking and visually hard to scan. Owner proposed a two-level interaction：

```text
recommendation chip
→ short action direction / concise label only
→ click
→ detailed editable action draft appears in PlayerInput
→ never auto-send
```

Owner also found the current recommendation/input UI generally too small and cramped for comfortable long-form play. Correction target should increase readable font/spacing/control size without turning this revision into final visual-polish scope.

Owner is **continuing UAT now**. Do not interrupt the play session with a dispatch. Accumulate additional findings; after Owner says this round is finished, GPT will shape one bounded MW-019 Revision covering the confirmed findings.

Retained technical risk remains：one real Kimi response returned fenced JSON and was intentionally rejected by strict parser. Only fix Structured Output if current real UAT proves it materially harms recommendation availability.

---

## 7. CURRENT — Combined Owner UAT ACTIVE

Accepted UAT artifact remains product-code HEAD：

`782daf65348f484d636d46260ac2374559cf554d`

Later implementation `main` governance-only commit `c19ed2c...` changes `AGENTS.md` only and does not invalidate the exported product bytes.

Owner continues normal play. Separate defect lineage remains：

- People issue → MW-018 revision；
- Recommendation issue → MW-019 revision。

No Product PASS exists until Owner explicitly accepts each outcome.

---

## 8. Dynamic UI route state

Dynamic UI remains Core-required, now Package 6 after observability / interaction / threads / System / Inventory.

Expected consumer evidence before Dynamic UI implementation：

- Character；
- Important Experiences；
- People；
- Open Threads；
- System / Public d20；
- Inventory。

Old MW-013 packet cannot be dispatched as-is. Re-Task-Shape from current consumers first.

v0.1 remains internal presentation only：typed player-safe projection/contribution, no omniscient local filtering, no arbitrary callbacks/NodePath/OS command/direct authoritative mutation, generic Action Intent deferred, external Source/Expansion Declarative UI deferred to G8.

---

## 9. V0 Core Closure target

Package 7 Reality Run must prove one continuous real Game across roughly 20–30 turns with new NPC, OOC, d20, item mutation, Save/reopen, Restore, free-form deviation from recommendations, at least 3 Dynamic UI consumer types, and useful Debug/UAT traces for key turns.

**Exit only by Owner explicit `V0 Core Game Loop = PRODUCT PASS`.**

---

## 10. Post-closure route

After V0 Product PASS：

```text
G7
→ Context Orchestrator / Structured Output Reliability
→ Knowledge Provenance / Epistemic / Freshness / Conflicts / Correction

G8
→ richer information surfaces + full player-facing Consequence Diff
→ player utility / archive
→ model operations / richer observability
→ Source Library / Reference / Creator
→ external UI contract only from proven Internal Dynamic UI vocabulary

G9
→ Standalone Alpha / Release Validation
```

---

## 11. Agent routing

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review / UAT interpretation

Codex
→ sole default production implementer + local UAT-build preparation

Owner
→ Product UAT / explicit verdicts
```
