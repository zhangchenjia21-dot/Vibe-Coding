---
name: agent-task-packet
description: Generate and audit bounded, source-grounded execution instructions for Codex, Grok, and other agents, separating canonical specifications from decision digests and execution envelopes, with explicit product-value acceptance for user-facing tasks. Use when GPT must create, review, shorten, split, or hand off a formal agent task with explicit scope, evidence, validation, Git, product-value, and return requirements.
---

# agent-task-packet v1.2

> [!abstract]
> 将复杂项目事实转换成**最小充分、权威明确、可执行、可验证**的 Agent Task Packet。
>
> 本 Skill 不追求最短 Prompt，也不复制全部项目文档。目标是：让执行 Agent 在不丢失关键边界的前提下，只读取完成当前职责所需的工作集。
>
> v1.2 新增产品面任务的强制要求：**Engineering Acceptance 不得替代 Product Value Acceptance。** 当任务直接改变用户主路径、核心体验或产品行为时，Task Packet 必须携带与当前增量直接相关的 Primary Purpose / Core Value，并明确哪些结果只能由真实用户 / Product Owner 验收。

---

## 1. 核心模型

正式 Agent 指令分为三层：

```text
Canonical Spec
= 完整产品 / 架构 / 需求 / 决策事实

Decision Digest
= 当前任务必须携带、但执行环境无法可靠读取的关键裁定

Execution Envelope
= 本次运行的目标、范围、验收、命令、Git 与返回协议
```

规则：

- Canonical Spec 可以长，但必须唯一、可版本化、可 supersede；
- Decision Digest 只保留真正改变实现方式的裁定；
- Execution Envelope 必须聚焦本次增量；
- 已在仓库 `AGENTS.md`、Skill 或 Canonical Spec 中稳定表达的规则，不在 Prompt 中全文重复；
- 路径引用只有在执行 Agent 能读取时才有效。不能读取时，提供必要摘要或随任务附带内容；
- 对产品面任务，Execution Envelope 必须知道“这个改动最终服务哪个用户核心价值”，避免 Agent 只优化局部工程指标。

---

## 1A. Repository-native Delivery｜仓库原生任务交付

当 GPT、目标仓库与执行 Agent 已经通过 GitHub / Git 仓库打通时，正式 Task Packet 的默认交付介质是**目标仓库中的 Markdown 文件**，而不是聊天中的超长可复制 Prompt。

默认模型：

```text
Canonical Spec
= GitHub current sources

Execution Task Packet
= target repository 中已提交的 .md 文件

Chat handoff
= repo + branch/ref + task packet path + 最少启动说明
```

若同时满足：

1. GPT 对目标仓库具有合法写权限；
2. 执行 Agent 能读取同一仓库；
3. 本次属于正式 implementation / review / migration / planning / UAT-support；

则：

> **正式 Task Packet 默认必须落为 `.md` 文件并提交到目标仓库。**

交付规则：

- 优先遵守目标仓库既有 Task Packet 目录和命名约定；
- 若项目采用 `main = Protected Integration Line` 与 `agent/<task-id>` 临时分支，Task Packet 默认优先提交到对应 task branch，避免仅为派任务污染 main；
- Task Packet commit 必须可追溯，并记录 `Formal Code Base SHA`、`Task Branch`、`Task Packet Path`、`Task Packet Commit SHA`；
- Task Packet 的纯文档 commit 不得冒充实现完成证据；Independent Review 应区分 packet-only commit 与 implementation commits；
- Chat 中不重复粘贴完整任务正文，只发送仓库、ref/branch、文件路径、Task ID 与必要的 stop/base note；
- Agent 启动时先读目标仓库 `AGENTS.md` 链和 Task Packet，再按 Source Manifest 读取 Canonical Specs；
- 若任务指令更新，优先更新仓库中的同一 active packet 或明确 superseding packet，不在聊天里偷偷追加与仓库文件不一致的新约束。

只有以下情况才回退到聊天全文 Task Packet：

- GitHub / 目标仓库当前不可写；
- 执行 Agent 无法读取仓库；
- 用户明确要求直接聊天 Prompt；
- 临时连接故障使 repository-native handoff 不可用。

原则：

```text
Chat
= 协调 / 通知 / 回包

Repository Task Packet
= 正式执行指令

Canonical Sources
= 产品 / 架构 / Stage / Contract 动态事实
```

---

## 2. 触发前置 Gate

正式输出任务前必须完成：

```text
Freshness PASS
+
Decision Propagation PASS
+
Current Roadmap / Stage Consistent
+
No Known Superseding Decision Ignored
+
Task Base Identified
```

至少确认：

1. 当前项目事实来自哪个仓库、分支与 current source；
2. 当前实现事实来自哪个 commit / HEAD；
3. 新决策是否改变 Stage Gate、Current Task、Prerequisite 或 Task DAG；
4. 已经发出的 Agent 任务是否仍然有效；
5. 本次是 implementation、review、UAT support、migration 还是 planning；
6. 是否存在 dirty worktree、并行改动或不可覆盖内容；
7. **若任务直接改变用户主路径 / 核心体验：当前 Primary Purpose / Core Value 来自哪个 current 产品事实源，本任务是否与其一致。**

未通过时，不生成看似完整但建立在旧事实上的正式指令。

---

## 3. Authority / Source Manifest

每份 Task Packet 必须给出权威顺序，建议使用：

```markdown
## Authority / Source Manifest
1. 用户当前明确指令
2. `<canonical product path>` — status / version / ref / SHA
3. `<canonical architecture / supporting decision>` — status / version / ref / SHA
4. 当前代码与测试 — repo / branch / base HEAD
5. 通用 Skill — exact current path

Not authoritative unless explicitly referenced:
- archive / legacy-reference
- superseded roadmap
- old handoff
- historical chat summary
- model memory
```

必须区分：

- 产品“应该是什么”；
- 代码“现在实际上是什么”；
- Skill“应如何执行”；
- 历史资料“曾经是什么”。

两个 current source 冲突时，不得自行融合成第三套规则。

产品面任务必须优先包含真正定义 Primary Purpose / Core Value 的 current 产品源，不能只引用工程规格。

---

## 4. Context Inclusion Algorithm

```text
1. 从目标与验收反推所需事实
2. 确定 canonical owner
3. 选择初始读取集
4. 只在证据不足时扩大搜索
5. 删除重复、历史和与本任务无关的上下文
6. 检查是否仍足以完成职责
```

默认初始读取集：3–7 个文件或入口，包括：

- 最近的 `AGENTS.md` 链；
- 当前任务规格；
- 当前增量直接相关的产品/架构事实；
- 主要实现入口；
- 直接相关测试；
- 必要的公开契约 / DTO。

禁止默认要求：

- “阅读整个仓库”；
- “阅读所有文档”；
- 把依赖图等同于 Prompt inclusion graph；
- 因为某模块可访问很多数据就把全部数据交给模型。

```text
System Total State
!= Enabled Capability Set
!= Current Relevant Set
!= Agent Working Set
```

但 bounded context 不得变成 starved context。identity、目标、状态、refs、关键约束和完成证据必须齐全；产品面任务还必须保留当前增量所服务的核心价值，不能只留下技术边界。

---

## 5. Task Atomicity 与拆分

一份 Task Packet 默认只拥有一个主要 Outcome 和一个主要 Owner。

出现以下任一情况，优先拆分或先发 Plan-only 任务：

- 同时跨越多个生命周期阶段；
- implementation 与 independent review 混在一起；
- 需要多个互不相干的 Owner；
- 既要求架构裁定又要求大规模实现；
- 需要不可逆 migration，但迁移边界尚未冻结；
- 验收标准无法用一个清晰 Gate 表达；
- Prompt 很长主要因为复制了多个上游全文。

允许单任务跨模块，但必须存在一个清晰的纵向 Outcome，并明确 cross-owner integration。

如果产品核心用途本身仍未冻结，禁止用 implementation task 暗中替代 Product Definition。

---

## 6. 标准 Agent Task Packet

正式指令默认包含以下结构。

### 6.1 Task Identity

```markdown
# TASK｜<ID>｜<Name>
Type: implementation | review | migration | planning | UAT-support
Owner: <primary owner>
Base: <repo / branch / HEAD>
```

### 6.2 Outcome

用 1–3 句话描述完成后可观察到的结果，不写宽泛愿景。

对产品面任务，Outcome 要表达**用户/产品可观察变化**，而不是只写“新增接口 / 表 / DTO”。

### 6.3 Why Now

只说明当前 Gate、blocker 或依赖关系。项目历史压缩为必要因果，不复制全过程。

若当前 blocker 是核心价值失败，明确写出真实用户症状；不要把它降格成局部工程缺口。

### 6.4 Authority / Source Manifest

列出 exact source、版本、状态和 SHA；明确 legacy / archive 不构成权威。

### 6.5 Read First

按顺序列出初始工作集，并写：

> 只有在现有证据不足时才扩大读取范围；扩大后说明原因。

### 6.6 Decision Digest / Invariants

使用稳定编号：

```text
DEC-xx  决策
INV-xx  不变量
AC-xx   验收
NON-xx  非范围
RISK-xx 风险
```

每条只表达一个约束。能引用原编号时不重写长段落。

产品面任务至少应包含一条可追踪的 Core Value / Primary Purpose 不变量，例如：

```text
INV-PRODUCT-01
本增量不得为了满足支撑性 Guardrail 而破坏 <核心用户价值>。
```

### 6.7 Scope

```markdown
Allowed:
- 可修改目录 / 文件族
- 必要的测试与文档同步

Prohibited:
- 不得修改的 Owner / 模块
- 不得提前实现的阶段
- 不得恢复的 legacy path
- 不得进行的 destructive Git 操作
```

若 current 产品裁定允许退休旧限制，应明确写出“可以移除哪些被 supersede 的约束”，避免 Agent 为了旧测试恢复错误路线。

### 6.8 Deliverables

列出必须交付的代码、迁移、测试、文档或审核证据，不指定无必要的微观编码步骤。

产品面任务若需要 Owner 真人体验，必须包含“让真实产品路径可直接体验”的交付，而不是只提供内部 CLI / fixture。

### 6.9 Acceptance Gates

验收必须可证伪：

- 行为结果；
- authority / persistence / security；
- regression；
- real vertical / UAT；
- 具体命令和预期结果。

不能用“测试很多”“代码整洁”“看起来正确”代替 Gate。

#### 6.9A Product Value Acceptance｜产品面任务强制

当任务直接影响主用户路径、核心体验、交互、生成质量、游戏性、创作性或其它 Product Promise 时，必须显式拆分：

```text
Engineering Acceptance
+
Product Value Acceptance
```

Engineering Acceptance 可由 Agent / 测试证明。

Product Value Acceptance 至少回答：

- 当前 Primary Purpose / Core Value 是什么；
- 真实用户路径出现了什么可观察变化；
- 是否存在适用的 Current Alternative / Simple Baseline；
- 哪些体验结果只能由 Product Owner /真实用户裁定；
- 哪些现象出现就应判产品 Gate 失败，而不是归为“后续 polish”。

如果需要 Owner UAT，Agent 的最高可宣布状态通常是：

```text
READY FOR OWNER UAT
```

而不是：

```text
PRODUCT PASS
FUN PASS
PLAYABILITY PASS
```

### 6.10 Validation Commands

按 focused → full 的顺序列出。真实 Provider、联网或高成本 Gate 与离线 Gate 分开，禁止未通过离线 Gate 就浪费真实调用。

自动化测试通过不能替代真实产品价值 Gate。

### 6.11 Git / Integration

至少说明：

- 开始时记录 base HEAD 与 status；
- 不覆盖未知 dirty worktree；
- authoritative write 前重新检查 HEAD；
- commit / push / PR 责任；
- 若 HEAD 改变，如何吸收或返回。

### 6.12 Stop / Return Conditions

只有以下情况才停止并返回，而不是自行扩大范围：

- current source 实质冲突；
- 需要用户产品裁定；
- 必需 secret / provider / artifact 缺失；
- 发现 P0 或不可逆迁移风险；
- 任务必须跨越明确禁止的范围；
- 当前 worktree 无法安全识别；
- 实现证据表明 current architecture / guardrail 与 Primary Purpose 发生实质冲突，继续机械实现会固化产品错误。

普通局部不确定性应先调查并作最小安全实现，不要把可解决问题全部抛回用户。

### 6.13 Final Report

固定要求：

```markdown
## Result
PASS | PARTIAL | BLOCKED | READY FOR OWNER UAT

## Changed
- 文件与行为变化

## Evidence
- 命令
- 结果
- 关键测试 / UAT / diff

## Product Value Evidence（产品面任务）
- primary purpose / core value addressed
- real user-visible behavior
- baseline comparison status if applicable
- owner UAT status / remaining product decision

## Git
- branch
- base HEAD
- final HEAD
- commit / push / PR
- final status

## Remaining
- 未完成项、风险、下一 Gate
```

禁止只回复“完成了”。

产品面任务不得把“自动化全绿”写成 Product Owner 已接受的证据。

---

## 7. Agent-specific Adaptation

### Codex / Coding Agent

强调：真实仓库、入口文件、允许路径、测试命令、Git 状态、迁移与回归。不要把具体实现写死到失去代码调查空间。产品面任务需保留可体验纵向，不得只交付内部 contract。

### Grok / UI / Mock Agent

强调：产品目标、正式 DTO / contract、可视层边界、交付文件格式、不可虚构能力、如何交给工程 Owner 集成。Mock 必须标明不是 production truth。

### Independent Reviewer

只读或审查优先；明确 base/head、diff scope、Gate 与证据。不得顺手实现再审核自己的改动，除非任务明确改为 return-fix。

对于产品面任务，Reviewer 可判 Engineering / Architecture Gate，但不能替真实用户宣布主观体验已通过。

### Planner

输出 Task DAG、Owner、依赖、Gate 与 source manifest；不把规划文本冒充已实现事实。若 Product Definition 未关闭，先规划产品/架构验证而不是直接生成大规模实现 DAG。

---

## 8. 长度与复杂度启发式

以下是治理触发器，不是模型硬限制：

- 日常单模块 Execution Envelope：约 800–1800 中文字符；
- 跨模块任务：约 1500–3500 中文字符；
- Decision Digest：通常 5–12 条；
- 初始读取集：通常 3–7 个文件；
- 直接任务 Prompt 超过约 4000 中文字符时，执行重复度与 Canonicalization 审计。

超过阈值并不自动失败。必须检查长度来自：

```text
真实任务复杂度
or
重复规则 / 历史 / 多任务混装 / 全文复制
```

不能为了变短删除 authority、acceptance、security、product-value 或 recovery 边界。

---

## 9. Prompt 压缩顺序

压缩时按以下顺序处理：

1. 删除已经 superseded 的历史；
2. 合并重复规则；
3. 用 source path + stable ID 替代全文复制；
4. 把完整背景移回 Canonical Spec；
5. 把跨阶段目标拆成独立任务；
6. 把稳定仓库规则移入 `AGENTS.md`；
7. 保留当前任务的差异、核心价值、Gate 与证据要求。

禁止先删验收和禁止范围。

---

## 10. A/B Regression

对高频正式任务模板建立小型回归集，至少覆盖：

- 单文件 Bug；
- 跨模块实现；
- Migration；
- Independent Review；
- UI / Runtime 接线；
- 文档与代码同步；
- 产品核心体验改动。

比较旧长 Prompt 与新 Task Packet：

- Acceptance PASS；
- 错误路径修改；
- scope creep；
- Review 新发现数；
- 读取文件与工具调用；
- token / 时间 / 成本；
- 关键不变量遗漏；
- 对产品面任务：Primary Purpose / Core Value 是否被遗漏或被 Guardrail 误伤。

只有在正确率和产品核心结果不下降时，才正式采用更短版本。

---

## 11. 输出前检查

```text
[ ] Freshness / Propagation PASS
[ ] Base HEAD / repo / branch 明确
[ ] 一个主要 Outcome / Owner
[ ] Authority 与 implementation truth 分开
[ ] 初始读取集有界但充分
[ ] Allowed / Prohibited 明确
[ ] INV / AC / NON 可追踪
[ ] 验收可执行、可证伪
[ ] 产品面任务已识别 Primary Purpose / Core Value
[ ] Engineering Acceptance 与 Product Value Acceptance 已分开
[ ] 需要 Owner UAT 时 Agent 不会自宣 Product / Fun / Playability PASS
[ ] 离线与真实 Provider Gate 分开
[ ] Git / stop / return / final report 完整
[ ] Repository-native delivery 已判断；GitHub 可读写时优先 .md Task Packet
[ ] 未复制无关历史或整个上游正文
```

任一关键项缺失时，先修正 Task Packet，再交给执行 Agent。

---

## 12. v1.2 Change Log

相对 v1.1：

- 正式任务前置 Gate 新增产品面任务的 Primary Purpose / Core Value 一致性检查；
- Authority Manifest 要求产品面任务引用真正定义核心价值的 current 产品源；
- Outcome / Why Now / Invariants 增加用户可观察结果与 Core Value 不变量；
- 新增 `6.9A Product Value Acceptance`，强制区分 Engineering Acceptance 与 Product Value Acceptance；
- 明确需要真实用户 / Owner 验收时，Agent 最高只可宣布 `READY FOR OWNER UAT`，不得自宣 Product / Fun / Playability PASS；
- Final Report 新增 Product Value Evidence；
- Stop Conditions 增加“current guardrail / architecture 与 Primary Purpose 实质冲突”情形；
- 输出前检查新增产品价值、基线和 Owner UAT 边界；
- 保留 v1.1 的 Repository-native Delivery、Canonical Spec / Decision Digest / Execution Envelope、Freshness、Authority、Context、Scope、Git 与 Final Report 治理。
