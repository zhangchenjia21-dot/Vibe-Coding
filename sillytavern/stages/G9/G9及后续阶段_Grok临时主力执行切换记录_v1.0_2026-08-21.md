---
title: G9 及后续阶段 Grok 临时主力执行切换记录
status: current-execution-override
version: 1.0
date: 2026-08-21
scope: agent-routing
---

# G9 及后续阶段｜Grok 临时主力执行切换记录 v1.0

## 1. 当前事实

Project Owner 已明确说明：当前 Codex 使用额度耗尽，接下来一段时间实现主力预计切换为 Grok。

该决定符合现有：

`G9及后续阶段_Agent资源分配与Codex默认代码协作裁定_v1.1_2026-08-20.md`

其中已允许：

```text
当前 Codex 不可用
+
Project Owner 选择 Grok 作为替代
→ Grok Build 可成为合法执行 Agent
```

因此本记录不是推翻 v1.1 的永久默认规则，而是当前资源条件下的 Owner explicit opt-in。

## 2. 当前执行路由

从 G9-05C correction-01 起，在 Project Owner 没有重新恢复 Codex 主力前：

```text
GPT
= Product / Architecture Lead
= Task Packet Owner
= exact-SHA Independent Reviewer
= Integration Gate Owner

Grok Build
= 当前优先 Implementation Executor
```

任务仍应按实际职责裁剪：

- UI / 交互 / 产品化纵向：Grok 可直接主导；
- 已冻结 Contract 下的窄逻辑修改：可交给 Grok，但 Task Packet 必须把语义与禁止项写死；
- 涉及 Canonical Owner、Runtime authority、协议重定义、数据库 authority、恢复状态机等架构变化：不得让 Grok自行裁定，先返回 GPT；
- 若某任务过于偏后端，优先由 GPT 把它拆成“已冻结设计 + bounded mechanical implementation”，而不是把开放式架构问题交给 Grok。

## 3. 不变的治理

执行 Agent 变化不改变：

- Canonical Spec / Current Source 权威；
- `main` protected integration line；
- `agent/<task-id>` 分支隔离；
- 不直接 push main；
- 不 rebase / amend / force push / reset --hard / clean / stash；
- focused staging；
- GPT exact Task Final SHA Independent Review；
- P0/P1 清零后才 fast-forward main；
- Project Owner 只做产品方向与最终 UAT，不承担人工调试路由。

## 4. 当前任务

G9-05C correction-01 的 Grok active execution packet：

```text
zhangchenjia21-dot/sillytavern
branch: agent/g9-05c-world-creator
agent tasks/G9-05C_Grok_correction-01_WorldCreator产品收口执行包_v1.0_2026-08-21.md
```

旧 Codex correction packet 保留为历史/详细问题说明，不再是当前执行入口。

## 5. 退出条件

若 Project Owner 后续明确说明：

- Codex 额度恢复并重新作为主力；或
- 选择其它执行 Agent；

则重新进行 Agent routing，不把本临时 override 当作永久默认。
