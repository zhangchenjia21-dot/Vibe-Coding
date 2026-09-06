---
title: my world｜Execution Agent Routing
status: current-project-governance
version: 3.1
created: 2026-08-30
updated: 2026-09-06
owner: Owner + GPT
---

# my world｜EXECUTION AGENT ROUTING CURRENT

本文件定义 implementation / research execution agent 的当前默认分工与派工原则。

它不改变 Product / Architecture authority，也不改变 GPT 对 semantics / Task Shaping / Independent Review 的职责。

## 1. Core rule

> **所有新的 production implementation task 默认只使用 Codex。**

`MW-015 Character + Important Experiences Surfaces v0.1` 已完成原 KimiCode 授权轮次并进入 Owner UAT。后续默认分工：

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

### KimiCode / Zcode / other implementation agents

```text
NOT DEFAULT / NOT ROUTED FOR NEW TASKS
```

只有 Owner 以后再次明确指定时，才可成为新的 task-local implementer。

### Grok Build / other search-heavy tools

Primary role remains:

- external ecosystem / standards / comparative research；
- search-heavy technical evidence；
- reference project audit。

Search result 不自动成为 Architecture authority，也不成为 production implementation 默认执行者。

## 3. Assignment rule

正式派工流程：

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
Owner-build preparation for product-facing work
↓
Owner UAT
```

GPT 仍需判断：

```text
Complexity
× Importance
× Blast Radius
× Architecture / Authority Coupling
```

这些维度用于决定：

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

## 5. Evidence / implementation handoff rule

所有 code-changing task：

- 使用 task-specific branch / worktree；
- 最终 evidence 必须来自 exact clean candidate；
- Implementer 最高返回 `READY FOR INDEPENDENT REVIEW`；
- GPT review actual diff / tests / runtime evidence；
- Engineering PASS 后才允许 integration；
- Owner 对 product-facing outcome 做最终 UAT。

未经 GPT Independent Review 的 task branch 不得安装到 Owner canonical playable checkout 并冒充正式试玩版本。

## 6. Owner UAT build handoff — product-facing work mandatory

Owner canonical local playable checkout：

`D:/AI/Projects/my-world`

当前 `run-game.cmd` / `run-game.ps1` 会保证 Windows export 与**当前本地 checkout**一致，但不会证明本地 checkout 已经同步到 GitHub 上最新、已审核并集成的 `main`。

因此 product-facing task 在 integration 后必须增加明确的 UAT-build handoff：

```text
Engineering PASS
→ integrate reviewed candidate
→ safely synchronize D:/AI/Projects/my-world to exact integrated main
→ validate exact local HEAD
→ rebuild / validate Windows export
→ Owner Launch Ready
→ Owner UAT
```

默认由具备本机执行能力的 Codex 完成该 UAT-build preparation，除非 Owner 另有指定。

执行要求：

1. 先检查 `D:/AI/Projects/my-world` 的 branch / status / worktree state；
2. 不覆盖 unknown dirty work、本地未推 commit 或分叉；
3. 只有 clean 且可安全 fast-forward 时，才 fetch 并同步到 intended integrated `origin/main` / integration SHA；
4. 明确验证 local `HEAD` 等于本次 UAT 应测试的集成 commit；
5. 执行 `run-game.ps1 -ValidateExportOnly` 或等价命令，确认 `build/windows/my-world.exe`、`.pck` 和 freshness metadata 对应当前 checkout；
6. 返回 exact local HEAD + export validation result；
7. 只有这一步 PASS 后才通知 Owner 运行 `run-game.cmd`；
8. 如果 checkout dirty、diverged、unexpected branch 或无法安全同步，STOP 并报告，不使用 `reset --hard`、`clean -fd`、force 等破坏性手段隐藏问题。

不要把“每个任务都修改 `run-game.cmd`”当刷新机制。Launcher 保持通用；正确机制是：

> **reviewed main synchronization → fresh export validation → Owner UAT handoff**

不需要 Owner UAT 的纯后台 / 非产品面任务，不自动要求这一 build handoff。

## 7. Current project-specific disposition

```text
MW-015
→ ENGINEERING PASS / INTEGRATED / OWNER UAT PENDING
→ current immediate need is canonical local checkout + fresh export preparation

new implementation task
→ Codex only by default

MW-013 Internal Declarative UI Host v0.1
→ HOLD / NOT AUTHORIZED YET
```

此前 v2.0 的长期分工：

```text
Codex    → complex / critical implementation
KimiCode → bounded UI / ordinary surfaces
```

已失效，不再作为 current routing。

Zcode / KimiCode 仅在 Owner 明确再次指定时使用。

Gemini review remains CANCELLED / DO NOT EXECUTE。
