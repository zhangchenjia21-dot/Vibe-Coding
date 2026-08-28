---
name: user-profile
description: 用户的个人画像、AI 协作偏好、期望的 AI agent 模式与多仓库体系速查。涉及了解用户协作习惯、任务指令格式、Final Report 协议、隐私边界，或需要按用户的治理规则（Freshness、最小充分上下文、权威优先级、agent-task-packet、生命周期阶段等）工作时使用。读取后按需深入对应仓库或总结文件。
whenToUse: 需要按用户偏好的方式协作、生成符合其规范的正式任务/审核/交接、判断事实冲突时按用户权威顺序处理、或需要定位用户各项目事实源时。
---

# 用户画像与 AI 协作模式（速查版）

> 本 skill 是本地综合用户资料的速查浓缩版。**它不是权威事实源**：任何涉及具体事实、Skill 版本、项目裁定的工作，必须回到 GitHub `main` 的 current 版本核验（Freshness 原则）。本文件只提供“知道去哪里找、按什么规则做”的地图。

## 1. 用户协作画像

- 深度 AI 用户，长期使用 GPT / Codex / Grok / Claude Code 等进行 AI 驱动项目开发。
- 强调证据优先、认知主权、表达保真、记录而不冻结、隐私最小化。
- 倾向把项目经验沉淀为生命周期规范、Skill、架构与复盘材料，并要求 AI 承担维护劳动，Owner 主要承担方向裁定与真实 UAT。

## 2. 多仓库体系（在哪取什么事实）

| 仓库 | 职责 |
|---|---|
| `Vibe-Coding` | 统一 AI 开发治理仓库：跨项目方法论、Lifecycle/Harness、Agent 协作规则、项目治理、复盘经验，以及 `skill/gpt/` / `skill/dsh/` 可复用 Skill 正式源 |
| `sillytavern` | 酒馆游戏实现事实源（代码 / 测试 / 运行 / Git 事实） |
| `sillytavern-assets` | 酒馆语义资产内容事实源（世界包 / 人物卡 / 拓展包 / 资产族 / 通用资产库） |
| `the-world` | The World / DSH 参考实现、真实长局与实验经验 |
| `my-world` | 当前独立 AI RPG 实现事实源 |
| `career` | 职业知识库 |
| `Zhang-Chenjia` | 用户长期个人档案 |

可复用 Skill current 统一从：

```text
zhangchenjia21-dot/Vibe-Coding/main/skill/
```

读取；旧独立 `Skill` 仓库不再是 current authority。

## 3. AI 协作哲学（10 条稳定偏好）

1. **GitHub main 是动态事实源**：聊天 / 附件 / 本地副本只是快照；正式任务开始前核对 current。
2. **权威优先级**：用户明确指令 > 项目正式裁定 > 可验证实现事实 > 跨项目治理规范 > Skill > 历史文档 > 模型记忆。
3. **冲突不得静默解决**：不得选更方便的一份、不得拼接第三套；列出冲突、影响与推荐裁定，交用户决定。
4. **最小充分上下文**：默认 3–7 个初始入口；但不得饥饿，目标 / identity / 状态 / 不变量 / 验收证据必须齐全。
5. **Current-only 治理**：同文档族 active 只留一个 current，旧版进入归档或 Git history。
6. **Freshness + Decision Propagation**：新事实必须判断是否影响 Stage / Task / Schema / Owner / DAG；写回前重新核对 HEAD。
7. **表达保真**：可能 ≠ 确定；讨论 ≠ 正式裁定；允许访问 ≠ 允许无差别写入。
8. **隐私最小化**：不向 Git 写 Key、客户敏感数据或不必要的私人信息。
9. **中文命名优先**：普通治理目录与文件优先中文，只保留技术必需英文名。
10. **AI 承担维护劳动**：去重、断链、旧值搜索、传播、隐私初筛、一致性审核由 AI 完成；Owner 主要做不可逆裁定和真实产品 UAT。

## 4. 期望的 AI Agent 模式

### 4.1 三层任务模型

```text
Canonical Spec      = 完整产品 / 架构 / 需求事实（留在仓库）
Decision Digest     = 执行环境无法直接读取时随任务携带的关键裁定
Execution Envelope  = 本次运行的目标、范围、验收、命令、Git 与返回协议
```

禁止把全部历史、完整 Lifecycle、上游全文复制成超长 Prompt；不得为变短删除 authority / security / acceptance / recovery 边界。

### 4.2 正式任务指令最低结构

正式任务使用：

`Vibe-Coding/skill/gpt/agent-task-packet/SKILL.md`

至少包含 Task ID、Owner、repo / branch / base HEAD、Outcome / Why Now、Authority / Source Manifest、Read First、DEC / INV / AC / NON、Allowed / Prohibited Scope、Deliverables、Acceptance Gates、Validation、Git / Integration、Stop / Return Conditions 与 Final Report。

默认**一个任务一个 Outcome 一个 Owner**；Implementation 与 Independent Review 分离。

### 4.3 角色分工

- `lifecycle-dev-process`：为什么做、在哪个 Gate、依赖谁；
- `agent-task-packet`：给执行 Agent 的任务如何组织、读取什么、修改什么、如何验证与返回；
- Owner：产品方向、不可逆决策、架构裁决、真实 UAT 与最终裁定；
- 规划 Agent：探索、事实整理、架构、路线、任务拆分、审核与推进裁决；
- 实现 Agent：读任务与规则、写代码、测试、Git、自验证与证据；
- 独立审核 Agent：对照目标和合同审核真实 diff，不依据实现者自述直接 PASS。

### 4.4 生命周期

生命周期与版本号一律读取：

`Vibe-Coding/skill/gpt/lifecycle-dev-process/SKILL.md`

不得在本速查文件硬编码旧版本。既有项目先恢复事实与 current authority；项目级先冻结任务轴，再进入正式实现；任务级按 current lifecycle 的实现、自动验证、真实集成、Independent Review、Owner UAT 与 Closeout Gate 执行。

### 4.5 Final Report 协议

```markdown
## Result     PASS | PARTIAL | BLOCKED
## Changed    文件与行为
## Evidence   命令与结果
## Git        base HEAD / final HEAD / commit / push / status
## Remaining  风险、未完成项、下一 Gate
```

**禁止只回复“完成”。**

## 5. DSH 落地映射

| 用户规则 | DSH 落地 |
|---|---|
| Freshness | git pull / GitHub API 核对 current；聊天记忆不算权威 |
| 最小充分读取 | `glob` / `grep` 定位 AGENTS.md 链与 Task Spec，`read` 按需 |
| 正式 Agent 指令 | 使用 agent-task-packet；`subagent` / `subagent_fork` 派发 |
| 独立审核分离 | `subagent_fork` 隔离审核 |
| 用户裁定 / 冲突上报 | `ask_user_question`，带推荐 |
| 多阶段跟踪 | `todo_write` 维护步骤与 Gate |
| Final Report | Result / Changed / Evidence / Git / Remaining |

## 6. 隐私与边界

- 不向 Git 写 Key、客户数据或不必要的 PII；敏感内容最小化存储。
- 涉及用户身份、人生或长期个人结论的正式判断必须回到对应事实源并尊重用户最终解释权。
- 本 Skill 只提供协作与路由地图，不替代项目 current、实现事实或用户本人裁定。