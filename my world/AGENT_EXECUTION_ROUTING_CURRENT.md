---
title: my world｜Execution Agent Routing
status: current-project-governance
version: 2.0
created: 2026-08-30
updated: 2026-09-06
owner: Owner + GPT
---

# my world｜EXECUTION AGENT ROUTING CURRENT

本文件定义 implementation / research execution agent 的当前默认分工与派工原则。

它不改变 Product / Architecture authority，也不改变 GPT 对 semantics / Task Shaping / Independent Review 的职责。

## 1. Core rule

> **按任务复杂度、重要性、影响半径与 architecture authority 选择 agent；不要只按“前端/后端”机械分配。**

正式 Task Packet 默认有一个 primary implementer。跨稳定 seam 的 mixed work 可拆成两个明确任务；如果无法安全拆分且触及 Runtime / Persistence / authority boundary，优先 Codex。

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

Primary implementation role:

> **高复杂度 / 高重要性 / 高影响半径 / architecture-critical implementation owner.**

优先分配：

- Runtime / world semantics / authority boundaries；
- Persistence / SQLite / transaction / Save / Restore / recovery；
- Source contract / loader / validator / fingerprint / managed library；
- game creation / materialization / canonical state；
- Context / Provider / durable domain logic；
- cross-module refactor；
- difficult debugging / integration；
- 即使是 UI，只要它会建立共享 Host / external-contract foundation 或紧耦合核心 state，也可由 Codex 主责。

### KimiCode

Primary implementation role:

> **边界清晰、风险较低、建立在已有 seam 上的 frontend / interaction / ordinary consumer owner.**

优先分配：

- Godot UI scenes / controls / layout / navigation；
- 已有 safe projection 上的 Character / People / Save 等普通 Surface；
- responsive / keyboard / overflow / visual polish；
- content tooling / batch content work once contracts are stable；
- bounded test additions / small refactors；
- player-facing copy when semantics are already frozen。

KimiCode 不应在 frontend task 中静默重设 backend authority / persistence / world semantics。

### Grok Build / other search-heavy tools

Primary role:

- external ecosystem / standards / comparative research；
- search-heavy technical evidence；
- reference project audit。

Search result 不自动成为 Architecture authority。

## 3. Assignment dimensions

GPT 在每次正式派工前至少判断：

```text
Complexity
× Importance
× Blast Radius
× Architecture / Authority Coupling
```

辅助问题：

1. Outcome 是 mechanism、ordinary UI consumer、research，还是 mixed？
2. mixed work 是否存在稳定 seam 可拆？
3. 做错后会影响一页 UI，还是污染 Runtime / persistence / future contracts？
4. 是否需要修改新的 Domain owner？
5. 是否需要大量 cross-module reasoning / debugging？
6. 当前 agent availability / quota 是否足以完成，但不会反向扭曲职责？

## 4. Practical routing examples

```text
Character Sheet on existing player_profile projection
→ KimiCode

People Surface requiring only existing player-safe actor projection
→ KimiCode

People Surface first requires new actor-disclosure / relationship authority
→ Codex mechanism task first
→ KimiCode surface second

Internal Declarative UI Host shared renderer
→ Codex

Save / Restore semantics or SQLite migration
→ Codex

Pure visual polish on accepted Host
→ KimiCode
```

## 5. Boundary rule

如果 Agent 发现当前任务需要跨越未授权边界：

```text
STOP
→ return BLOCKED / boundary finding
→ GPT decides architecture / split / reassignment
```

禁止：

- frontend task 顺手造新 Runtime truth；
- backend task 顺手改产品语义；
- research task 顺手冻结 Architecture；
- implementer 自己宣布 Independent Review PASS。

## 6. Evidence / handoff rule

所有 code-changing task：

- 使用 task-specific branch / worktree；
- 最终 evidence 必须来自 exact clean candidate；
- Agent 最高返回 `READY FOR INDEPENDENT REVIEW`；
- GPT review actual diff / tests / runtime evidence；
- Owner 对 product-facing outcome 做最终 UAT。

## 7. Current project-specific disposition

2026-09-06 Owner 开通 GPT Pro 后，明确取消“新任务默认交 Zcode”的临时模式。

当前长期规则：

```text
GPT      → semantics / architecture / shaping / assignment / IR
Codex    → complex / critical implementation
KimiCode → bounded UI / interaction / ordinary surfaces / tooling
Owner    → Product UAT
```

Zcode 仅在 Owner 明确再次指定时作为新的 task-local implementer；此前 weekend override 已结束，不再作为 current governance。

当前 `MW-013 Internal Declarative UI Host v0.1` 已因 G6 路线纠正而：

```text
HOLD / NOT AUTHORIZED YET
```

如果 Codex 已建立 isolated branch/worktree，应保留但停止继续代码修改，等待后续明确 re-authorization。

当前 active work 是 GPT + Owner 的 G6 Surface / Information Architecture 讨论，不是 implementation task。
