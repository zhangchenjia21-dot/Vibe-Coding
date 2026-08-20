---
title: Agent Task Packet GitHub 原生交付增量裁定
status: current
version: 1.0
created: 2026-08-20
updated: 2026-08-20
scope: sillytavern-project
supplements:
  - Skill/main/skill/gpt/agent-task-packet/SKILL.md
  - sillytavern/代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md
---

# Agent Task Packet｜GitHub 原生交付增量裁定 v1.0

## 1. 裁定目的

当 GPT、目标代码仓库与执行 Agent 已经通过 GitHub 打通时，正式 Agent 指令不再默认以聊天中的超长可复制 Prompt 作为主要交付介质。

正式冻结：

```text
Canonical Spec
= GitHub current sources

Execution Task Packet
= target repository 中已提交的 Markdown 文件

Chat handoff
= repo + ref/branch + task file path + 最少启动说明
```

这条规则补充 `agent-task-packet` Skill 的内容结构规则；不改变其 Authority / Read First / INV / AC / Validation / Git / Final Report 要求。

## 2. 默认交付规则

若同时满足：

1. GPT 对目标仓库具有合法写权限；
2. 执行 Agent 能读取同一 GitHub 仓库；
3. 当前任务是正式 implementation / review / migration / UAT-support / planning task；

则：

> **正式 Task Packet 默认必须生成 `.md` 文件并提交到目标仓库。**

聊天中不再重复粘贴完整 Task Packet，只发送：

```text
Repository
Branch / Ref
Task Packet path
Task ID
必要的 Base / stop note
```

只有以下情况才回退到聊天全文：

- GitHub 当前不可写；
- 目标 Agent 无法读取该仓库；
- 用户明确要求直接聊天 Prompt；
- 临时故障导致 repository-native handoff 不可用。

## 3. SillyTavern 路径约定

SillyTavern 的 Grok Build 正式任务包继续沿用现有目录：

```text
grok build/
```

命名建议：

```text
<TaskID>_GrokBuild_<Outcome>执行任务包_vX.Y_YYYY-MM-DD.md
```

历史先例：

```text
grok build/G9-02C_GrokBuild_Breadth_Worktree执行任务包_v1.0_2026-08-19.md
```

## 4. Branch / Main 规则

若项目采用 `main = Protected Integration Line` 与 `agent/<task-id>` 临时分支：

```text
Task Packet
→ 优先提交到对应 agent/<task-id> 任务分支
→ Agent 从该分支读取并建立/使用 worktree
→ main 在 Independent Review PASS 前不接收实现提交
```

这样：

```text
派任务
!= 修改 main runtime truth
```

如果历史流程已经采用 docs-only coordination commit 写入 main，可以继续识别，但新任务默认优先使用 task branch packet。

## 5. Task Packet Commit Identity

正式派发时必须同时记录：

```text
Formal Code Base SHA
Task Branch
Task Packet Path
Task Packet Commit SHA
```

其中：

- `Formal Code Base SHA` = 本次实现开始前的 canonical implementation base；
- `Task Packet Commit SHA` 可以是该 Base 的纯文档后代；
- Independent Review 必须区分 packet-only commit 与 implementation commits；
- 不允许把 Task Packet commit 冒充实现完成证据。

## 6. Agent 启动口径

聊天给执行 Agent 的默认指令缩减为：

```text
读取 <repo>@<branch>:<task-packet-path>，严格按该 Task Packet 执行。
先遵守仓库 root / nested AGENTS.md 和 Task Packet 的 Start / Worktree Gate。
完成后只返回 Final Report + exact Task Final SHA，不要自行合并 main。
```

Agent 不需要依赖聊天中的重复项目背景；Task Packet 自身通过 Source Manifest 指向 Canonical Specs。

## 7. 更新与修订

如果任务在执行前发生 current decision / main drift：

- GPT 更新原 Task Packet 或生成明确 superseding Task Packet；
- 不在聊天中偷偷追加与仓库文件不一致的新约束；
- 如果只是 correction，优先更新同一任务分支中的 packet / correction 文件并保持可追溯性；
- Agent 只执行当前明确标记的 active packet。

## 8. 与 Chat / GitHub 的职责边界

```text
Chat
= 协调、解释、通知、回包转发

GitHub Task Packet
= 正式执行指令

GitHub Canonical Sources
= 产品 / 架构 / Stage / Contract 动态事实

Git branch / commits
= 实现与证据事实
```

因此：

> **当 GitHub 已打通时，正式任务不应再要求项目所有者手工复制长 Prompt；GPT 应直接落库任务文件，用户只需让 Agent 读取对应文件。**

## 9. Decision Propagation

本裁定只改变 Agent 指令的**交付介质与交接流程**，不改变：

- 当前 Stage Gate；
- Runtime / Asset / Creator 架构；
- Task Outcome；
- Independent Review；
- exact-SHA integration；
- worktree isolation；
- main fast-forward gate。

影响级别：`P2 / execution-governance improvement`。

从本文件生效后，SillyTavern 后续正式 Grok Build / Coding Agent Task Packet 默认按 GitHub 原生 `.md` 方式派发。
