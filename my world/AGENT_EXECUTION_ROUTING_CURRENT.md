---
title: my world｜Execution Agent Routing
status: current-project-governance
version: 1.1
created: 2026-08-30
updated: 2026-09-05
owner: Owner + GPT
---

# my world｜EXECUTION AGENT ROUTING CURRENT

本文件只定义 **implementation / research execution agent 的默认分工与派工原则**。它不改变 Product / Architecture authority，也不改变 GPT 对 Meaning / task shaping / Independent Review 的职责。

## 1. Core rule

> **按任务性质选择最适合的 agent；额度与临时可用性是第二层约束，不应反过来长期扭曲职责。**

一个正式 Task Packet 默认只有一个 primary execution owner。若任务跨明显前后端边界，应优先拆成有清晰 seam 的两个任务，而不是让一个 agent 无边界扩张。

## 2. Default routing

### GPT

Primary responsibilities:

- Product semantics / architecture / governance;
- task shaping / Task Packet;
- Decision Propagation;
- acceptance design;
- Independent Review;
- Owner-facing plain-language reporting.

GPT 不作为默认 production implementation owner。

### Codex

Primary execution role:

> **Backend / mechanism implementation owner.**

优先分配：

- persistence / SQLite / transaction / crash recovery;
- Source loader / validator / fingerprint / managed-library mechanism;
- game creation / materialization / canonical state;
- runtime / provider / context / durable domain logic;
- backend integration and failure semantics;
- backend-heavy automated evidence.

### Kimi

Primary execution role:

> **Frontend / UI / interaction implementation owner.**

优先分配：

- Godot UI scenes / controls / layout;
- Wizard / menus / settings / review surfaces;
- responsive / keyboard / text overflow behavior;
- player-facing copy when semantics are already frozen;
- frontend integration tests and visual evidence.

When Codex is unavailable or quota-limited, Kimi **may** take a backend task if the task is clearly scoped and the packet explicitly grants that backend scope. This is fallback capacity, not the default routing.

### Grok Build

Primary execution role:

> **Search / external research / evidence discovery owner.**

优先分配：

- web / ecosystem / reference / standards / comparative search;
- finding external implementation references or current technical facts;
- search-heavy evidence collection.

Grok Build may occasionally implement frontend or backend work when the task is search-heavy, self-contained, or other agents are unavailable, but it is not the default code owner.

## 3. Assignment decision checklist

Before every formal agent handoff, GPT should decide in this order:

1. What is the primary outcome: frontend, backend, search/research, or mixed?
2. Can mixed work be split at a stable seam?
3. Which agent is the natural primary owner by role?
4. Is that agent currently available / quota-capable?
5. If not, is fallback safe without increasing semantic or integration risk?
6. If fallback would blur ownership or cross a dangerous boundary, wait or split rather than force reassignment.

## 4. Boundary rule

Frontend task packets should not silently authorize backend redesign.

Backend task packets should not silently redesign player-facing product semantics.

Search tasks should not silently become architecture authority.

If implementation exposes a cross-boundary requirement not granted by the packet:

```text
STOP
→ return BLOCKED / boundary finding to GPT
→ GPT decides split / reassignment / semantic correction
```

## 5. Quota / temporary availability

Temporary quota exhaustion is execution state, not durable project architecture.

Do not persist transient reset times as long-term governance. GPT may use current availability when choosing the next agent, but future assignments must re-evaluate task fit and availability at handoff time.

## 6. Owner weekend implementation override｜2026-09-05 → 2026-09-06

Owner explicitly requested that the limited weekend Zcode capacity be used for the project's next substantive implementation work.

Effective window:

```text
start: immediately after Owner instruction on 2026-09-05
end:   2026-09-06 23:59 (+08:00)
```

During this window:

```text
GPT
→ Product semantics / architecture / task shaping / governance / Independent Review

Zcode + GLM-5.3-flash
→ primary implementation owner for NEW code-changing tasks issued after this routing decision

Kimi
→ finishes MW-005 Revision 2 because that correction was already assigned before this override
```

This override applies to backend, mechanism, integration and other code-changing work when a real approved Outcome exists. It does **not** authorize inventing low-value work merely to consume quota, bypassing Stage Gates, expanding task scope, or allowing an implementer to self-certify Independent Review.

Task packets still control scope. If a task is materially frontend/visual and using Zcode would create unreasonable risk, GPT may shape a narrow seam or return to Owner rather than forcing the assignment.

At `2026-09-07 00:00 (+08:00)`, unless Owner explicitly extends or changes this routing, the long-term default routing automatically resumes:

```text
Codex → backend / mechanism
Kimi  → frontend / UI / interaction
GPT   → semantics / review
```

MW-004's prior GPT direct implementation and MW-006's prior Zcode implementation remain historical task-local exceptions; neither changes long-term role ownership.
