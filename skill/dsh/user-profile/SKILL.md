---
name: user-profile
description: 用户（张宸嘉/琛迦）的个人画像、AI 协作偏好、期望的 AI agent 模式与多仓库体系速查。涉及了解用户身份背景、协作习惯、任务指令格式、Final Report 协议、隐私边界，或需要按用户的治理规则（Freshness、最小充分上下文、权威优先级、agent-task-packet、生命周期阶段等）工作时使用。读取后按需深入对应仓库或总结文件。
whenToUse: 需要了解用户是谁、按用户偏好的方式协作、生成符合其规范的正式任务/审核/交接、判断事实冲突时按用户权威顺序处理、涉及酒馆项目/职业/人生档案等用户领域时。
---

# 用户画像与 AI 协作模式（速查版）

> 本 skill 是 `D:\AI\deepseekharness\用户画像与AI协作模式.md` 的速查浓缩版。**它不是权威事实源**：任何涉及具体事实、Skill 版本、项目裁定的工作，必须回到用户各仓库 GitHub `main` 的 current 版本核验（Freshness 原则）。本文件只提供"知道去哪里找、按什么规则做"的地图。

## 1. 用户是谁

- 张宸嘉（GitHub 昵称 琛迦 / `zhangchenjia21-dot`），2003-05 生，浙江丽水人。
- 浙大管院会计学本科（2027-06 毕业，GPA 3.46/5）；2022.03–2024.03 武警义务兵；2026 夏物产中大国际财务部实习；**2027 届秋招中**（主路线：四大核心审计，杭州/深圳双主城；备选大型企业财务；止损强内资所）；CPA 2027 四科 + 2028 两科。
- 深度 AI 用户：用 GPT 网页端 / Codex / Grok / Claude Code 做 vibe coding，自建 SillyTavern 酒馆游戏项目（TypeScript，G1–G9 阶段 Gate）。
- 强方法论自省：把开发经验沉淀为生命周期规范与 skill 体系；把个人信息建造成「人生演化档案」系统（Zhang-Chenjia 仓库，v0.15）。
- 价值观：证据优先、认知主权（"最终解释权属于我本人"）、表达保真（可能≠确定）、记录而不冻结、生活优先于记录、隐私最小化。

## 2. 多仓库体系（在哪取什么事实）

| 仓库 | 职责 |
|---|---|
| `Skill` | 可复用 Skill 正式源：`skill/gpt/`（GPT 版）、`skill/dsh/`（DSH 版）；agent-task-packet / lifecycle-* / tavern-asset / tavern-creator-import-draft / 长上下文交接 |
| `Vibe-Coding` | AI 驱动开发方法论、Lifecycle/Harness、Agent 协作规则、复盘经验、酒馆项目产品/架构/Roadmap/Stage 裁定 |
| `sillytavern` | 酒馆游戏**实现事实源**（代码/测试/运行/Git 事实） |
| `sillytavern-assets` | 酒馆**语义资产内容**事实源（世界包/人物卡/拓展包/资产族/通用资产库） |
| `career` | 职业知识库（当前状态、战略、校招面试、能力建设） |
| `Zhang-Chenjia` | 「关于我」人生演化档案（时间线/决策/变化/轨迹/当前模型） |
| `the-world` | 酒馆新版游戏本体实现（G1 语义协议 + G2 内核 + 试玩外壳） |

本地克隆在 `D:\AI\deepseekharness\user-repos\`；各仓库总结在 `user-repos\_summaries\`；综合文档在 `D:\AI\deepseekharness\用户画像与AI协作模式.md`。

## 3. AI 协作哲学（10 条稳定偏好）

1. **GitHub main 是动态事实源**：聊天/附件/本地副本只是快照；正式任务开始前核对 GitHub current。
2. **权威优先级**：用户明确指令 > 项目正式裁定 > 可验证实现事实 > 跨项目治理规范 > Skill > 历史文档 > 模型记忆。
3. **冲突不得静默解决**：不得选更方便的一份、不得拼接第三套；列出冲突+影响+推荐裁定，交用户决定。
4. **最小充分上下文**：默认 3–7 个初始入口（AGENTS.md 链 → Task Spec → 实现入口 → 相关测试 → 契约）；但不得饥饿（目标/身份/状态/不变量/验收证据必须齐全）。
5. **Current-only 治理**：同文档族 active 只留一个 current，旧版进 `99_归档/`；禁止 `xxx_v2.md`/`xxx最新版.md` 并列。
6. **Freshness + Decision Propagation**：新事实必须判断影响哪些层（Stage Gate/Current Task/Schema/Owner…）；写回前核对 HEAD（`Task Base HEAD != Current HEAD ?`）。
7. **表达保真**：可能≠确定；当天讨论≠长期世界观；大概记得≠精确历史事实；允许访问≠允许无差别写入。
8. **隐私最小化**：不写 PII/客户敏感数据/Key；Never-Git（身份证件、银行凭证、医疗原件）。
9. **中文命名优先**：目录与普通文件用中文；只保留技术必需英文名。
10. **AI 承担维护劳动**：去重/断链/旧值搜索/传播/隐私初筛/一致性审核由 AI 完成；用户只做裁定（"本人不是 QA Bot"）。

## 4. 期望的 AI Agent 模式

### 4.1 三层任务模型
```text
Canonical Spec      = 完整产品/架构/需求事实（留在仓库）
Decision Digest     = 执行环境无法直接读取时随任务携带的关键裁定
Execution Envelope  = 本次运行的目标、范围、验收、命令、Git 与返回协议
```
禁止把全部历史/完整 Lifecycle/上游全文复制成超长 Prompt（约 4000 中文字符触发审计）；不得为变短删除 authority/security/acceptance/recovery 边界。

### 4.2 正式任务指令最低结构（agent-task-packet）
1. Task ID、类型、Owner、repo/branch/base HEAD；2. Outcome 与 Why Now；3. Authority/Source Manifest；4. Read First；5. 编号化 DEC/INV/AC/NON；6. Allowed/Prohibited Scope；7. Deliverables；8. Acceptance Gates；9. focused→full 验证命令；10. Git/集成责任；11. Stop/Return 条件；12. Final Report 格式。
默认**一个任务一个 Outcome 一个 Owner**；Implementation 与 Independent Review 分离。

### 4.3 角色分工
- lifecycle-dev-process＝为什么做/在哪个 Gate/依赖谁；agent-task-packet＝指令如何组织/含哪些上下文/如何验收。
- 用户：产品方向、不可逆决策、架构裁决、UAT、最终裁定（不写代码、不逐字段调试）。
- 规划 Agent：探索/事实整理/架构/路线图/任务拆分/审核/推进裁决。
- 实现 Agent：读任务与规则/写代码/测试/Git/自验证/证据。
- 独立审核 Agent：对照目标与合同审核真实 diff；**不得只依据实现者自述**。

### 4.4 生命周期阶段（DSH 版 v1.2 S0–S10；GPT 版 v1.6 为架构治理版，用户裁定 DSH 侧保持 v1.6）
S0 接管与事实恢复 → S1 产品探索 → S2 形态/原则冻结 → S3 架构/合同 → S4 路线图/阶段门 → S5 任务包 → S6 实施自验证 → S7 自动验证 → S8 独立审核/集中修订 → S9 真人 UAT → S10 发布/复盘/Harness 更新。既有项目必先过 S0。阶段关闭＝复盘+项目源回写+Skill 演进检查（不只是 PASS）。

### 4.5 Final Report 协议
```markdown
## Result     PASS | PARTIAL | BLOCKED
## Changed    文件与行为
## Evidence   命令与结果
## Git        base HEAD / final HEAD / commit / push / status
## Remaining  风险、未完成项、下一 Gate
```
**禁止只回复"完成"。**

### 4.6 架构不变量（酒馆项目）
L3→L2→L1→L0 分层；Canonical Ownership（UI/投影/缓存不得成为第二 live truth）；Source/Local Instance/Runtime 三分；No Phantom/Player Agency（Narrative 不得宣称未提交变化）；Declarative Boundary；测试四类 PASS（Fixture/Engineering/Real Provider/Owner UAT）分开报告。

## 5. DSH 落地映射

| 用户规则 | DSH 落地 |
|---|---|
| Freshness | git pull / GitHub API 核对 current；聊天记忆不算权威 |
| 最小充分读取 | `glob`/`grep` 定位 AGENTS.md 链与 Task Spec，`read` 按需 |
| 正式 Agent 指令 | 套 agent-task-packet；`subagent`/`subagent_fork` 派发 |
| 独立审核分离 | `subagent_fork` 隔离审核 |
| 用户裁定/冲突上报 | `ask_user_question` 一次一问、带推荐 |
| 多阶段跟踪 | `todo_write` 维护步骤与 Gate |
| Final Report | Result/Changed/Evidence/Git/Remaining |

## 6. 隐私与边界

- 不向 Git 写 PII/Key/客户数据；敏感内容最小化存储。
- 涉及"我是谁/我如何变化"的正式结论必须用户确认（沉默不等于批准）；冲突直接在聊天问，不留仓库。
- 用户领域：职业（career）、人生（Zhang-Chenjia）、酒馆（sillytavern / sillytavern-assets / the-world / Vibe-Coding）。
