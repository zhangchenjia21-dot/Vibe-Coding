---
title: AI 驱动项目全生命周期开发流程规范
aliases:
  - AI项目开发流程规范
  - AI工程协作总规范
  - Project Development Harness Standard
tags:
  - 开发
  - AI协作
  - 项目管理
  - 产品设计
  - 软件架构
  - 任务规划
  - 测试
  - 审核
  - UAT
status: active
version: 1.6
created: 2026-08-15
last_updated: 2026-08-18
scope: cross-project
supersedes: AI驱动项目全生命周期开发流程规范_v1.5_2026-08-18.md
---

# AI 驱动项目全生命周期开发流程规范 v1.6

> [!abstract] 文档定位
> v1.6 **SUPERSEDES v1.5**。
>
> v1.6 完整保留 v1.5 已冻结的 Stage Exit A/B/C/D、Host before Protocol、Fixture vs Production、Declarative Boundary、Source Identity、Durable Preference、Supersession、Minimal Semantic Slice、Owner-aligned Multi-Agent Execution 等原则。
>
> 本版本重点补充：**长上下文治理、网页端 GPT 生成正式 Agent 指令、Canonical Spec 与 Execution Prompt 分层、最小充分工作集、Prompt 回归评测。**

---

## 1. Prompt 长度不是独立质量指标

不能使用：

```text
Prompt 越短越好
```

作为项目规则。

正确目标是：

> **在权威与安全边界内，为当前职责提供最小充分上下文。**

需要同时防止两个极端：

```text
Overloaded Context
= 重复规则 + 无关历史 + 多任务混装 + 全文复制

Starved Context
= 缺 identity + 缺目标 + 缺状态 + 缺 refs + 缺不变量 + 缺验收
```

长但相关、结构清晰、无冲突的 Canonical Spec 可以保留；真正应压缩的是执行 Prompt 中的重复与无关内容。

---

## 2. 仓库总量不等于 Agent 工作集

```text
Repository Total Files
→ Search / Navigation
→ Initial Working Set
→ Evidence-driven Expansion
→ Effective Context
```

仓库文件很多不会自动让 Agent 失败。高风险来自：

- current 与 archive 混在同一入口；
- README 指向旧路线；
- 没有 `AGENTS.md`；
- Agent 被要求无差别阅读整个仓库；
- 同一事实在多个文件中竞争权威；
- 工具输出、日志和历史逐步挤占当前任务上下文。

因此每个实现仓库应至少提供：

- 根级 `AGENTS.md`；
- current source manifest 或等价入口；
- 清晰的 archive / legacy 标记；
- 模块职责地图；
- focused 与 full validation 命令。

复杂模块可增加局部 `AGENTS.md`，只写该目录特有差异，不复制根规则。

---

## 3. 正式任务采用三层结构

```text
Canonical Spec
= 完整产品、架构、需求、决策与验收事实

Decision Digest
= 当前任务必须携带、但执行 Agent 无法直接可靠读取的关键裁定

Execution Envelope
= 本次运行目标、范围、交付、验证、Git 和返回协议
```

### 3.1 Canonical Spec

允许详细，但必须：

- 唯一；
- 可版本化；
- 可 supersede；
- 与 archive 分离；
- 使用稳定编号；
- 不把聊天副本当 current。

### 3.2 Decision Digest

只保留真正改变实现方式的 5–12 条关键裁定，并附：

- `DEC / INV / AC / NON` 编号；
- source path；
- status / version；
- commit / SHA；
- 无法直接读取源时所需的最小摘要。

### 3.3 Execution Envelope

只描述本次增量，不重新讲完整项目历史。

---

## 4. GPT 网页端生成 Agent 指令的前置流程

当用户要求 GPT 为 Codex、Grok、Claude Code 或其它 Agent 生成正式任务时：

```text
1. Cross-chat / Cross-agent Freshness Preflight
2. Decision Propagation Gate
3. Implementation repo HEAD / status check
4. 读取项目 current source
5. 读取 Skill current
6. 使用 agent-task-packet 生成任务
7. 输出前做重复度与可执行性审核
```

正式任务前必须满足：

```text
Freshness PASS
+
Decision Propagation PASS
+
Roadmap / Stage Consistent
+
No Superseding Decision Ignored
+
Task Base Identified
```

如果并行 Agent 已经 push，新任务必须基于最新 HEAD；不得继续沿用旧 Base。

如果已有任务正在执行，新决策只影响后续，则不重复发同一任务；若会建立错误基础，则必须主动建议修订或撤回。

---

## 5. Authority / Source Manifest

每份正式 Agent 指令必须区分：

```text
产品应该是什么
→ project current decision / spec

代码实际上是什么
→ implementation repo / tests / HEAD

任务应如何执行
→ current Skill

曾经是什么
→ archive / legacy / old handoff
```

建议结构：

```markdown
## Authority / Source Manifest
1. 用户当前明确指令
2. `<canonical source>` — status / version / ref / SHA
3. `<supporting decision>` — status / version / ref / SHA
4. 当前代码与测试 — repo / branch / base HEAD
5. current Skill — exact path

Not authoritative unless explicitly referenced:
- archive / legacy-reference
- superseded roadmap
- old handoff
- historical chat summary
- model memory
```

如果执行 Agent 无法访问跨仓库 source，仅提供路径是不充分的；必须附带必要 Decision Digest 或可读取附件。

---

## 6. 最小充分初始读取集

默认初始工作集为 3–7 个入口：

1. 适用的 `AGENTS.md` 链；
2. 当前 Task Spec；
3. 主要实现入口；
4. 直接相关测试；
5. 必要公开契约 / DTO。

任务应明确：

> 只有在现有证据不足时才扩大读取范围；扩大后说明原因。

不得把 Dependency Graph 等同于 Context Inclusion Graph。

```text
System Total State
!= Enabled Capability Set
!= Current Relevant Set
!= Agent Working Set
```

---

## 7. Task Atomicity

一份正式任务默认只拥有：

- 一个主要 Outcome；
- 一个主要 Owner；
- 一个清晰 Gate。

优先拆分：

- implementation 与 independent review；
- 架构裁定与大规模实现；
- 不同 Owner 的互不相关工作；
- 当前阶段与未来阶段；
- 不可逆 migration 与尚未冻结的产品方向。

允许跨模块纵向任务，但必须由一个清晰 Outcome 连接，并明确 cross-owner integration。

---

## 8. Agent Task Packet 最低结构

正式指令至少包含：

1. Task ID、Type、Owner、Base；
2. Outcome；
3. Why Now；
4. Authority / Source Manifest；
5. Read First；
6. Decision Digest / Invariants；
7. Allowed / Prohibited Scope；
8. Deliverables；
9. Acceptance Gates；
10. Validation Commands；
11. Git / Integration；
12. Stop / Return Conditions；
13. Final Report。

验收必须可证伪，不能用“代码整洁”“测试很多”“看起来正确”代替真实行为、authority、persistence、security、recovery 与 UAT 证据。

---

## 9. Validation 与真实 Provider 分层

```text
Focused Offline Tests
→ Full Offline Gate
→ Build / Lint / Typecheck / Disclosure
→ Real Integration / Provider Gate
→ Independent Review
→ Project Owner UAT
```

真实 Provider、联网或高成本实验必须在离线 Gate 通过后执行。

不得把 Fixture PASS 自动写成 Production PASS，也不得把 Engineering PASS 自动写成 Product / UAT PASS。

---

## 10. Git 与并行协作

正式 Agent 指令至少要求：

- 开始时记录 repo / branch / base HEAD / status；
- 不 reset、clean、stash 或覆盖未知 dirty worktree；
- task-owned diff 默认由 Executor 完成 focused Gate 与精确 integration；
- authoritative write 前重新检查 HEAD；
- HEAD 改变时审计 Base → Current 增量；
- final report 提供 final HEAD、commit / push / PR 和 final status。

不得因为 Prompt 很长而省略 Git 边界。

---

## 11. Stop / Return Conditions

Agent 只在以下情况停止并返回：

- current source 实质冲突；
- 需要用户产品裁定；
- 必需 secret / provider / artifact 缺失；
- 发现 P0 或不可逆迁移风险；
- 任务必须越过明确禁止范围；
- worktree 无法安全识别。

普通局部不确定性应先调查并作最小安全实现，不要把所有可解决问题抛回用户。

---

## 12. Prompt 长度审计

以下为内部治理启发式，不是模型硬限制：

- 单模块 Execution Envelope：约 800–1800 中文字符；
- 跨模块任务：约 1500–3500 中文字符；
- Decision Digest：通常 5–12 条；
- 初始读取集：通常 3–7 个文件；
- 直接任务 Prompt 超过约 4000 中文字符：触发重复度与 Canonicalization 审计。

压缩顺序：

```text
删除 superseded 历史
→ 合并重复规则
→ 用 path + stable ID 替代全文
→ 背景回归 Canonical Spec
→ 跨阶段任务拆分
→ 稳定仓库规则进入 AGENTS.md
→ 保留当前差异、Gate 与证据
```

禁止先删除 Acceptance、Prohibited Scope、Security 或 Recovery。

---

## 13. A/B Prompt Regression

对高频任务建立回归集，至少覆盖：

- 单文件 Bug；
- 跨模块实现；
- Migration；
- Independent Review；
- UI / Runtime 接线；
- 文档与代码同步。

比较旧长 Prompt 与新 Task Packet：

- Acceptance PASS；
- 错误路径修改；
- scope creep；
- Review 新发现数；
- 读取文件与工具调用；
- token / 时间 / 成本；
- 关键不变量遗漏。

只有正确率不下降时，才正式采用更短版本。

---

## 14. Current Skill 对应

项目执行时先读取 GitHub `main` 的实际 current：

- `lifecycle-dev-process`：多阶段项目、架构、迁移与 Stage Gate；
- `lifecycle-templates`：结构化治理模板；
- `agent-task-packet`：Codex / Grok / Agent 正式指令生成与审核。

项目专属裁定优先于通用 Skill；通用 Skill 不覆盖项目业务事实。

---

## 15. v1.6 Change Log

相对 v1.5 新增 / 强化：

- Prompt 长度与信息密度的正确区分；
- Repository Total Files ≠ Agent Working Set；
- Canonical Spec / Decision Digest / Execution Envelope；
- GPT 网页端正式 Agent 指令前置 Gate；
- Authority / Source Manifest；
- 3–7 个初始读取入口与 evidence-driven expansion；
- Task Atomicity；
- Agent Task Packet 最低结构；
- Offline / Real Provider / Review / UAT 分层；
- Prompt 长度审计与 A/B regression；
- `agent-task-packet` Skill 路由。
