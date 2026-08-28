# Vibe-Coding｜AI 协作与动态事实源规则

状态：current  
适用范围：本仓库，以及以本仓库作为开发治理 / AI 协作来源的项目与聊天

## 1. 仓库定位

`Vibe-Coding` 是统一的 AI Development Control Plane：

```text
AGENTS.md
→ 仓库级 authority / freshness / governance

skill/
→ 跨项目可复用执行方法与模板

项目经验/
→ 跨项目复盘、经验与方法论证据

<project>/
→ 各项目 Product / Architecture / Roadmap / Current Status

99_归档/
→ superseded / closed process evidence
```

项目实现代码、测试、构建和运行事实继续留在各自 implementation repository；本仓库不取代实现仓库。

若本文件与用户当前明确指令冲突，以用户当前明确指令为最高优先级。

---

## 2. Authority 顺序

默认：

1. 用户当前明确指令；
2. 当前项目最新、已批准的 Product / Architecture / Contract / Roadmap 决策；
3. 当前实现仓库可验证的代码、测试、运行与 Git 事实；
4. 本仓库当前适用的跨项目治理 / Lifecycle；
5. `skill/` 中当前适用的通用 Skill；
6. `项目经验/`、历史项目、归档与 Legacy Reference；
7. Agent 一般经验与模型记忆。

**同仓库不等于同 Authority。** `skill/` 提供跨项目默认方法，不能覆盖项目已经冻结的正式裁定。

两个 current source 实质冲突时不得静默选择或拼接第三套规则；先依据 status、version、supersedes 与 authority 解决，仍不能安全解决时提交 Owner 裁定。

---

## 3. Freshness / Decision Propagation

正式任务开始、阶段切换、Independent Review、生成 Agent 指令、更新核心文件或用户要求“按最新规则”时，先检查 GitHub `main` current 和对应 implementation HEAD。

```text
Freshness
!=
Decision Propagation
```

读到新决定后必须继续判断它是否改变：Stage Gate、Current Task、Prerequisite、Task DAG、Owner、identity/state model、Schema/Protocol timing、Safety/Persistence boundary、已发任务与下一步。

正式写回 authoritative source 前再次检查：

```text
Task Base HEAD
!= Current HEAD ?
```

- 未变：正常写回；
- 有无关变化：吸收后回归；
- 改变 Product / Architecture / Roadmap / Skill / Stage / Owner /目标文件：重新 audit + Decision Propagation。

禁止旧 Base 静默覆盖新 current。

---

## 4. Skill 使用与维护

Skill 正式源已经并入本仓库：

```text
skill/<runtime>/<skill-name>/SKILL.md
```

例如：

```text
skill/gpt/lifecycle-dev-process/SKILL.md
skill/gpt/agent-task-packet/SKILL.md
```

用户点名 Skill、流程引用 Skill、或 AI 判断需要某 Skill 时，必须读取本仓库 `main` 下对应 current `SKILL.md`；聊天副本、本地缓存和旧独立 Skill 仓库不再构成 current authority。

Skill 子树的版本、active-only、质量与 promotion 规则见 `skill/AGENTS.md`。

项目中形成新的稳定方法时：

```text
项目事实
→ 留在项目 current

跨项目经验
→ 项目经验/

已验证、可重复执行的方法 / 模板
→ skill/
```

不要把项目动态状态、临时 Task ID、账号、路径或当前阶段硬编码进通用 Skill。

---

## 5. 开发前路线 Gate

新项目、重大重构或会改变主用户路径 / 大量下游依赖的工作，优先读取：

`skill/gpt/lifecycle-dev-process/SKILL.md`

当前通用顺序要求先做：

```text
Primary Purpose / Core Value
→ Draft Task Axis
→ Reference Audit Pass 1
→ Reference / Path Audit Pass 2
→ Risk / Omission / Ordering Review
→ Owner Discussion + Improvement Proposals
→ Revised Task Axis
→ Owner Approval
→ Route Freeze
→ Implementation
```

除低成本 spike / prototype 外，开发前 Route Gate 未关闭时，不应产生大规模工程沉没成本。

---

## 6. 正式 Agent 指令

为 Codex、Grok、Claude Code、KimiCode 或其它执行 Agent 生成正式 implementation / review / migration / UAT-support 指令前，必须读取：

`skill/gpt/agent-task-packet/SKILL.md`

若同时涉及阶段、架构或迁移，再结合：

`skill/gpt/lifecycle-dev-process/SKILL.md`

职责：

```text
lifecycle-dev-process
→ 为什么做 / 何时做 / 依赖与 Gate

agent-task-packet
→ 任务怎样组织 / 读什么 / 改什么 / 如何验收
```

正式任务使用：

```text
Canonical Spec
+
必要 Decision Digest
+
Execution Envelope
```

默认一个主要 Outcome、一个主要 Owner；Implementation 与 Independent Review 分离。初始读取集通常 3–7 个入口，证据不足时再扩展。不得默认“阅读整个仓库”。

Product-facing Task 必须区分 Engineering Acceptance 与 Product Value Acceptance；需要真人体验时，Agent 最高状态通常是 `READY FOR OWNER UAT`，不能代替 Owner 宣布 Product PASS。

---

## 7. 项目与实现仓库边界

本仓库项目目录负责：

- Product definition；
- Core principles；
- Architecture / decisions；
- Roadmap / Stage Gate；
- Current Status；
- supporting experience / governance evidence。

独立 implementation repository 负责：

- 代码；
- 测试；
- 构建与导出；
- runtime / filesystem / database 事实；
- repository-native Task Packet；
- implementation-level `AGENTS.md`。

不得以治理文档替代代码事实，也不得让实现仓库复制整套跨项目治理正文。

---

## 8. 文档治理

> **Root is map; subfolders are depth.**

项目 active 顶层默认只保留少数长期入口，例如：

```text
README.md
项目/产品总纲_CURRENT.md
核心设计原则_CURRENT.md
架构_CURRENT.md
总体规划路线图_CURRENT.md
CURRENT_STATUS.md
```

新事实优先写入已有 canonical owner。只有当内容会明显污染核心文件、能独立演化、被多个 Task 重复引用或需要详细 contract/trade-off/migration 时，才新增 supporting doc，并放入 `architecture/`、`decisions/`、`experience/` 等子目录，由核心文档导航。

已关闭 Task、Preflight、Exit checklist、旧 handoff 与被 supersede 的过程资料进入 `99_归档/` 或依赖 Git history。

同一事实族 active 只保留一个 current；不要长期并列 `final/final2/latest/new`。

---

## 9. 人工文档版本

使用一位小版本：

```text
v1.0 → ... → v1.9 → v2.0 → v2.1
```

`vM.N` 中 `N` 只允许 `0–9`；`vM.9` 后进入 `v(M+1).0`。重大变化用 status / supersedes / ADR / migration note 表达，不从 major 位猜 breaking semantics。

---

## 10. 默认执行摘要

```text
任务开始
→ 读取本 AGENTS.md
→ 读取项目 current + 相关 skill current + implementation HEAD
→ Freshness + Decision Propagation
→ 最小充分工作集
→ 实施 / 验证 / Independent Review / Owner UAT（按任务需要）
→ pre-push HEAD revalidation
→ current docs / task / skill 传播
→ closeout
```

> **项目仓库保存项目真相；`项目经验/` 保存可复用经验；`skill/` 保存可重复执行的方法；实现仓库保存运行事实。**
