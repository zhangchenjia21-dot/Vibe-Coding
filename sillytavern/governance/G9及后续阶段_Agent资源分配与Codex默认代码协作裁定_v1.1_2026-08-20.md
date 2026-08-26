# G9 及后续阶段｜Agent 资源分配与 Codex 默认代码协作裁定 v1.1

状态：`CURRENT EXECUTION GOVERNANCE`
日期：2026-08-20
supersedes：`G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
scope：G9+ implementation / review / migration / agent-allocation

## 1. 当前资源裁定

从本裁定起：

```text
Default Coding Agent = Codex
Grok Build = explicit opt-in only
```

除非 Project Owner 当前明确要求使用 Grok Build，GPT 不得再把 Grok 作为默认主力实现 Agent，也不得在新的正式 Task Packet 中默认写 `Executor: Grok Build`。

若用户未指定执行 Agent：

```text
implementation / correction / deterministic migration / adapter / compiler / tests
→ Codex
```

若用户明确点名 Grok：

```text
→ Grok Build
```

历史任务中已写死 Grok、但尚未开始执行的 active Task Packet，必须在执行前 supersede / update 为 Codex 版本，避免仓库指令与聊天指令冲突。

## 2. GPT 职责不变

GPT 继续负责：

- Product / Architecture Lead；
- Freshness Preflight；
- Decision Propagation；
- Canonical Spec；
- Agent Task Packet；
- exact-SHA Independent Review；
- Stage Gate / UAT routing。

默认：

```text
Executor != Independent Reviewer
```

Codex 完成代码后仍由 GPT 基于 exact SHA 独立审核；Project Owner 只承担真正产品体验 / UAT，不承担工程诊断。

## 3. Codex 默认执行范围

在 Canonical Spec / contract 已冻结后，Codex 默认执行：

- typed registry / adapter / parser / compiler；
- deterministic service / migration；
- Runtime / Product 接线；
- React / UI / settings / Creator implementation；
- domain module implementation against frozen Host；
- fixtures / negative corpus / regression；
- lint / typecheck / build / smoke closure；
- bounded correction tasks。

Codex 不得因为成为默认执行 Agent 而获得架构裁定权。

遇到以下情况必须停止实现并返回 architecture question / BLOCKED：

- 需要改变 Canonical Owner；
- 需要改变 Source / Game-local / Runtime 三层模型；
- 需要改变 Formal Turn / Atomic Commit / Save / Restore / Recovery；
- 需要新增 arbitrary plugin/eval/script execution seam；
- 需要改变 Router / Context / Player Agency authority；
- 需要修改尚未冻结的 external protocol；
- 真实资产揭示当前 Canonical Spec 无法表达的 P0/P1 protocol gap。

## 4. Grok Build 的新定位

Grok Build 仍是合法执行 Agent，但不再默认分配。

只有以下情况之一成立时使用：

1. Project Owner 当前明确指定 Grok；
2. Project Owner 明确批准 GPT 将某个 bounded task 转交 Grok；
3. 当前 Codex 不可用且 Project Owner 选择 Grok 作为替代。

不得以“过去 G9 默认 Grok”为理由自动继续使用 Grok。

## 5. Formal Task Packet

GitHub 已打通时继续执行 repository-native delivery：

```text
Canonical Spec = Vibe-Coding current source
Execution Packet = target task branch 中 .md
Chat = 最小启动指令 / 回包
```

新的 Task Packet 默认：

```text
Executor: Codex
```

只有用户明确指定时改成 Grok Build。

Agent Task Packet 的 authority / Read First / Allowed / Prohibited / Acceptance / Validation / Git / Return 结构不因 Agent 变化而降低。

## 6. Writer Serialization

同一 repo 的重叠代码写入继续串行：

```text
Codex writing
→ no overlapping Grok write task

Grok writing
→ no overlapping Codex write task
```

并行时必须独立 branch、无重叠 ownership、明确 integration owner。

## 7. Git / Review Gate

继续保持：

- `main` = protected integration line；
- task 使用 `agent/<task-id>`；
- Agent 不直接 push main；
- 不使用 reset --hard / clean / stash / rebase / amend / force push；
- focused staging；
- Agent push task branch；
- GPT exact `Task Base → Task Final` review；
- PASS 后 GPT 才 fast-forward main 到 exact reviewed SHA。

## 8. 当前 G9 资源路线

```text
G9-03                         PASS / CLOSED
G9-04 Adapter / Compiler      Codex DEFAULT
G9-05 Creator                 Codex DEFAULT
后续冻结合同后的 implementation Codex DEFAULT
Grok Build                    OWNER EXPLICIT OPT-IN
```

## 9. 面向 Project Owner 的语言

继续保持中文优先。技术英文仅在字段名、接口名、命令、commit、协议精确名称等必要场景保留。

## 10. 最终原则

> **Codex 现在是项目默认代码执行 Agent；Grok 从默认主力调整为用户明确选择的备选执行 Agent。执行 Agent 变化不改变 Canonical Authority、Stage Gate、Git 隔离或 Independent Review 标准。**
