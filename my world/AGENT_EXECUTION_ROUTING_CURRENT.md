---
title: my world｜Execution Agent Routing
status: current-project-governance
version: 3.0
created: 2026-08-30
updated: 2026-09-06
owner: Owner + GPT
---

# my world｜EXECUTION AGENT ROUTING CURRENT

本文件定义 implementation / research execution agent 的当前默认分工与派工原则。

它不改变 Product / Architecture authority，也不改变 GPT 对 semantics / Task Shaping / Independent Review 的职责。

## 1. Core rule

> **MW-015 完成后，所有新的 implementation task 默认只使用 Codex。**

当前已经授权并正在执行的 `MW-015 Character + Important Experiences Surfaces v0.1` 仍由 **KimiCode** 完成本轮，不因本次路由更新中途换人。

从 MW-015 之后开始：

```text
GPT   → Product / Architecture / Task Shaping / Dispatch / Independent Review
Codex → default and sole implementation agent
Owner → Product UAT / explicit product verdict
```

除非 Owner 之后再次明确指定其它实现 Agent，否则 GPT 不再按任务复杂度在 Codex / KimiCode 之间进行常规分流，也不再默认拆成 `Codex mechanism + KimiCode consumer`。

## 2. Current routing

### GPT

Primary responsibilities:

- Product semantics；
- Architecture / decision freeze；
- Stage / route control；
- Task Shaping / Task Packet；
- agent assignment；
- Acceptance / Validation 设计；
- Decision Propagation；
- Independent Review；
- Owner UAT interpretation / status write-back。

GPT 不作为默认 production implementation owner。

### Codex

Primary implementation role after MW-015:

> **所有新的 production implementation task 的默认且唯一执行 Agent。**

包括但不限于：

- Runtime / world semantics / authority boundaries；
- Persistence / SQLite / transaction / Save / Restore / recovery；
- Source contract / loader / validator / fingerprint / managed library；
- game creation / materialization / canonical state；
- Context / Provider / durable domain logic；
- cross-module refactor；
- difficult debugging / integration；
- Godot UI scenes / controls / layout / navigation；
- Character / People / Save / System 等普通 Surface；
- responsive / keyboard / overflow / visual polish；
- content tooling / bounded tests / small refactors；
- architecture-critical shared UI infrastructure。

任务仍应按 Scope、Risk、Acceptance 和 Layering 做严格边界控制；“只用 Codex”不等于允许一个 Task Packet 无限扩 Scope。

### KimiCode

Current disposition:

```text
MW-015
→ remains assigned to KimiCode until this round completes
```

After MW-015:

```text
NOT DEFAULT / NOT ROUTED FOR NEW TASKS
```

只有 Owner 以后再次明确指定时，KimiCode 才可成为新的 task-local implementer。

### Grok Build / other search-heavy tools

Primary role remains:

- external ecosystem / standards / comparative research；
- search-heavy technical evidence；
- reference project audit。

Search result 不自动成为 Architecture authority，也不成为 production implementation 默认执行者。

## 3. Assignment rule after MW-015

MW-015 之后，GPT 不再需要在实现 Agent 之间做能力/成本路由。正式派工流程简化为：

```text
Product / Architecture Gate
↓
GPT shapes bounded executable Task Packet
↓
Codex implementation
↓
GPT Independent Review
↓
Integration only after Engineering PASS
↓
Owner UAT for product-facing outcome
```

GPT 仍需判断：

```text
Complexity
× Importance
× Blast Radius
× Architecture / Authority Coupling
```

但这些维度用于决定：

- Task 是否要拆分；
- 是否需要 Spike；
- Acceptance / Regression 深度；
- Review 强度；
- 是否应 STOP 回到 Product / Architecture Gate；

而不再用于 Codex / KimiCode 二选一。

## 4. Boundary rule

如果 Codex 发现当前任务需要跨越未授权边界：

```text
STOP
→ return BLOCKED / boundary finding
→ GPT decides architecture / split / re-shaping
```

禁止：

- UI task 顺手造新 Runtime truth；
- backend task 顺手改产品语义；
- research task 顺手冻结 Architecture；
- implementer 自己宣布 Independent Review PASS；
- 因“反正都是 Codex”而把多个独立 Outcome 塞进一个巨大 Task。

## 5. Evidence / handoff rule

所有 code-changing task：

- 使用 task-specific branch / worktree；
- 最终 evidence 必须来自 exact clean candidate；
- Agent 最高返回 `READY FOR INDEPENDENT REVIEW`；
- GPT review actual diff / tests / runtime evidence；
- Owner 对 product-facing outcome 做最终 UAT。

## 6. Current project-specific disposition

Owner 于 2026-09-06 明确更新路由：

```text
MW-015 当前轮次
→ KimiCode 继续完成，不中途换 Agent

MW-015 之后的新 implementation task
→ Codex only by default
```

因此此前 v2.0 的长期分工：

```text
Codex    → complex / critical implementation
KimiCode → bounded UI / ordinary surfaces
```

在 MW-015 完成后失效，不再作为 current routing。

Zcode 仍仅在 Owner 明确再次指定时使用；此前 temporary override 不恢复。

Gemini review remains CANCELLED / DO NOT EXECUTE。

`MW-013 Internal Declarative UI Host v0.1` 当前仍：

```text
HOLD / NOT AUTHORIZED YET
```

未来若重新授权 MW-013，其默认 implementer 也应为 Codex，除非 Owner 当时另有明确指令。
