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
  - 产品规格
  - 软件架构
  - 任务规划
  - 测试
  - 审核
  - UAT
status: active
version: 1.8
created: 2026-08-15
last_updated: 2026-08-23
scope: cross-project
supersedes: AI驱动项目全生命周期开发流程规范_v1.7_2026-08-19.md
---

# AI 驱动项目全生命周期开发流程规范 v1.8

> [!abstract] 文档定位
> v1.8 **SUPERSEDES v1.7**。
>
> v1.8 完整保留 v1.7 的 Stage 0 Product Definition、Stage Exit 分类、Host-before-Protocol、Production Vertical、声明式数据边界、Source / Owner Identity、Durable Preference、Supersession、Scope Right-sizing、Owner-aligned Execution、Canonical Spec / Decision Digest / Execution Envelope 与 Agent Task Packet 治理。
>
> v1.8 新增一条更上游、也更严格的原则：**产品的核心用途与核心价值必须成为所有架构、约束和验收的最高坐标系。** 支撑性能力、工程完整性、安全边界、扩展性和长期维护都不能在未明确产品裁定的情况下反过来牺牲核心体验。

---

## 0. Stage 0｜Product Purpose → Product Definition → Canonical Product Spec

新项目、重大重构、产品方向重置或核心用户承诺发生改变时，默认从 Stage 0 开始。

正确顺序升级为：

```text
Primary Purpose / Job To Be Done
↓
Core User Value / Core Experience
↓
Core Value Loop / Core User Journey
↓
Current Alternative / Simple Baseline
↓
Product Discovery / Discussion
↓
Canonical Product Spec
↓
Product Definition Gate
↓
Domain / Capability Survey
↓
Shared Foundation Identification
↓
Canonical Ownership
↓
Minimal Real Vertical
↓
Early Owner / User Reality Check
↓
Downstream Consumers
↓
Stage / Release Lifecycle
```

### 0.1 先回答“产品主要拿来干什么”

进入功能清单、架构和协议讨论前，必须先写清楚：

1. **Primary Purpose / Job To Be Done**：用户最主要拿这个产品来干什么；
2. **Core Value**：用户为什么愿意使用它，而不是不用或改用更简单方案；
3. **Core Experience / Core Loop**：用户怎样反复获得这个价值；
4. **Non-negotiable Core**：哪些体验一旦失去，即使其它能力全部正确，产品仍算失败；
5. **Support Capabilities**：哪些能力只是为了让核心体验更可靠、更持久、更安全、更可扩展；
6. **Current Alternative / Simple Baseline**：用户今天不用本产品时会怎么做，最简单的可替代方案是什么。

禁止把“功能很多”“架构先进”“长期可维护”“安全边界完整”本身当作 Primary Purpose。

### 0.2 “先讨论清楚”必须产生正式产物

聊天、头脑风暴、原型、零散笔记可以作为 Discovery Evidence，但不能成为长期产品事实源。

Stage 0 的正式产物是唯一 current 的 `Canonical Product Spec`，至少覆盖：

1. Primary Purpose / Job To Be Done；
2. Target User / Actors；
3. Product Promise；
4. Core Experience / Core Value Loop；
5. Core User Journey；
6. Current Alternative / Simple Baseline；
7. Functional Requirements；
8. Non-functional Requirements；
9. Domain Semantics；
10. Scope / Deferred / Non-scope；
11. Hard Constraints；
12. Support Capabilities / Guardrails；
13. Success / Acceptance；
14. Open Questions / Blockers；
15. Decision Ledger / Evidence。

### 0.3 Product Definition Gate

进入正式 Architecture Discovery 前至少确认：

- Primary Purpose 可以用一句话说明；
- 核心用户、核心价值、Core Experience / Core Loop 与 Core User Journey 明确；
- 已明确哪些能力属于核心价值，哪些只是支撑性能力；
- 已定义至少一个现实替代方案或简单基线；
- Must-have、Deferred 与 Non-scope 可区分；
- 关键 Domain Semantics 足够清晰，不会让架构基于不同含义各自实现；
- 主要硬约束已知；
- Success / Acceptance 可验证，而且至少包含核心价值是否真的成立；
- Open Questions 已分类为 blocking / non-blocking；
- 不存在会实质改变 Owner、Identity、State Model、Schema / Protocol、安全边界或主用户路径的 unresolved blocker；
- 已指定唯一 current Canonical Product Spec；
- 已计划最早何时用真实用户 / Owner 路径验证核心体验。

Gate 结果只有：

```text
NOT READY FOR ARCHITECTURE
READY FOR DOMAIN / CAPABILITY SURVEY
```

### 0.4 Gate 前允许探索，但禁止伪冻结

Product Definition Gate PASS 前允许：

- Prototype；
- UX exploration；
- Technical spike；
- Mock / Fixture；
- Feasibility test；
- Exploratory provider/model test；
- 与简单替代方案做快速体验对照。

但必须标记：

```text
exploration
NOT canonical architecture
NOT production commitment
```

Gate PASS 前默认禁止：

- 冻结 external schema / plugin protocol / public contract；
- 把未确认产品语义固化成长期 Domain Model / DB Schema；
- 大规模建设依赖未确认产品假设的高耦合下游模块；
- 把 Prototype / Demo / Fixture 反向宣布为正式产品规则；
- 以“代码已经写了很多”为理由跳过产品裁定；
- 因为某个安全、扩展性或工程方案“看起来更完整”，就默认接受其对核心体验造成的损失。

### 0.5 Guardrail 不得成为 Product Owner

安全、权限、幂等、恢复、类型、兼容、审计、扩展性、可维护性等都属于重要约束，但默认是**护栏或支撑能力**，不是产品目的本身。

每个会明显限制核心用户行为、模型能力、内容自由度或主路径效率的 Guardrail，都必须回答：

```text
它防止的真实风险是什么？
↓
它限制了哪一部分核心价值？
↓
有没有更窄的边界可以防住风险而不损伤核心体验？
↓
如果必须牺牲核心体验，是否已经由 Product Owner 明确接受？
```

禁止从：

```text
某项约束局部正确
```

直接推导：

```text
它可以无限扩大适用范围
```

特别警惕“护栏反客为主”：为了阻止错误行为，最终把产品本来应该发生的正确行为也一起禁止。

### 0.6 简单基线与反证门禁

复杂产品必须识别一个现实的简单基线，例如：

- 当前人工流程；
- 一个通用模型直接完成任务；
- 一个简单脚本；
- 现有竞品或手工替代路径。

正式产品不要求在所有维度都超过基线，但在 **Primary Purpose / Core Value** 上不能明显更差。

若真实使用出现：

```text
Complex System
< Simple Baseline
on Core Value
```

则不能用以下理由宣布产品成功：

- 架构更先进；
- 状态更一致；
- 测试更多；
- 长期维护能力更强；
- 协议更完整；
- 功能数量更多。

必须先判断是产品定义、架构、约束还是实现路线需要重开。

### 0.7 Product Spec 是 current truth，不是一次写死

架构、原型、UAT 或真实用户反馈可以促使 Product Spec 演化。

若新决策改变 Primary Purpose、Product Promise、Core Experience / Core Loop、Core User Journey、关键 Domain Semantics、Scope / Non-scope、Success / Acceptance 或会影响架构的硬约束，则必须：

```text
update Canonical Product Spec
↓
re-evaluate Product Definition Gate
↓
Decision Propagation
↓
recompute Stage / Task DAG / Architecture impact
```

禁止新产品决定只存在聊天中，而 Roadmap、Architecture 与代码继续使用旧产品事实。

---

## 1. Stage Exit 不等于“把计划表全部做完”

阶段退出前必须重新分类剩余事项，而不是机械执行初始清单。

### A｜Downstream Architecture Prerequisite
若延后会改变下一阶段的 Schema、Public Contract、Compiler、Plugin / Asset Protocol、Runtime authority、State model、Safety boundary 或 Core action semantics，则当前阶段 REQUIRED。

### B｜Stage Objective Minimum
属于当前阶段声称已成立的核心能力，即使不直接改变下一阶段协议，也必须完成或正式调整阶段目标。

典型错误：只有 fixture 却把 Host 标 CLOSED；只有内存却声称 durable；动态结构不随正式状态变化却声称 dynamic complete；production 仍保留第二 authority path 却宣称 migration closed；工程 Gate 全绿但 Primary Purpose 对用户仍未成立。

### C｜Product Consumption / UX Maturity
底层 semantics / authority 已证明，只缺完整消费体验。internal/personal 阶段可显式延后；Alpha 重新拉回关键用户路径；Release 则 resilience / accessibility / migration / operations 多数成为 Required。

### D｜Polish / Future Scale
深度 WCAG、全设备像素级适配、分布式、大规模性能、speculative platformization 默认不阻塞早期架构阶段。

---

## 2. 延后必须是正式裁定，不是遗忘

Deferred item 至少记录：Capability、Why deferred、Underlying semantics already proven by、Why it does not change current downstream contract、Revisit stage / trigger、Non-regression constraint。

若被延后的能力直接决定 Primary Purpose / Core Value 是否成立，不得仅以“可后续完善体验”归入 Deferred。

---

## 3. Host / Platform Before External Protocol

当下一阶段要设计 asset schema、plugin manifest、extension protocol、workflow DSL、Creator / Builder / Compiler，默认顺序：

```text
internal host/platform capability
↓
typed internal declaration
↓
real production-equivalent vertical proof
↓
freeze actual capability boundary
↓
external protocol / schema
↓
compiler / authoring tool
```

禁止先发明强大 Schema 再逼 Host / Runtime 实现其假设。

---

## 4. Fixture / Preview ≠ Production Closure

unit assembler、mock DTO、typed fixture、preview query、static screenshot、isolated component test 只能证明局部能力。若能力将成为下游正式上游，至少需要：

```text
internal typed declaration
→ real bootstrap / server
→ official projection
→ real consumer
```

---

## 5. Declarative Structure ≠ Live Data Access

声明式系统必须区分 Declaration 与 Materialized Safe Data：

```text
Authoritative State
→ bounded domain projection
→ server-side adapter / resolver
→ materialized safe DTO
→ declarative Host rendering
```

声明层默认不得拥有 arbitrary state path、JS callback、expression engine、eval 或 arbitrary query DSL。

---

## 6. Source / Owner Identity 不得在 Adapter 链中丢失

```text
Source
→ Compiler / Adapter
→ Assembly
→ Projection
→ UI / Runtime Consumer
```

必须保留谁声明、谁 owns、谁 depends on 谁、当前实例属于哪个 source，以及 disable/remove 后的责任。

---

## 7. Durable Preference 必须单独证明 durability

区分 in-memory UI state、session/browser state、durable product preference、business authoritative state、canonical snapshot。跨应用重启保留必须有真正 persistence owner。

---

## 8. Supersession 需要 production zero-reference audit

新消费者不用旧 API 不等于旧 authority 已退休。Migration Closure 搜索 old DTO、route、state owner、store、fallback、tests、adapter；无真实兼容义务时 retire old production path，只留 historical evidence。

---

## 9. Personal / Internal Development 的 Scope Right-sizing

个人或内部开发可以推迟消费层成熟度，但不能推迟会决定下游协议正确性的语义、authority、安全、数据一致性与不可逆架构问题，也不能把决定 Primary Purpose 是否成立的体验错误归入“以后再打磨”。

---

## 10. Minimal Semantic Slice Before Downstream Schema

```text
Core semantic rule
→ deterministic authority
→ atomic behavior
→ minimal real vertical proof
→ core-value reality check
```

不需要同时完成 polished UI、general planner、万能 DSL 与所有 edge features。

---

## 11. Owner-aligned Multi-Agent Execution

默认按能力 Owner 切任务：Executor implements → focused validation → task-owned integration；Independent Reviewer review canonical result。只有 cross-owner diff、conflict、unknown dirty、复杂迁移或高风险 Git 才引入 Integration Owner。

Product Owner 不应承担 routine Git、代码诊断或工程验收；其不可替代职责是产品方向、核心体验与真实使用裁定。

---

## 12. Architecture Audit 必查问题

在 Shared Foundation、Schema Freeze、Stage Closure 前检查：

- 当前架构明确服务哪个 Primary Purpose / Core Value？
- 每个高影响约束是在保护核心价值，还是已经开始压制核心价值？
- 是否存在“局部正确的护栏”被扩大成全局禁止规则？
- 如果去掉所有外围能力，核心用户旅程本身是否仍成立？
- 与 Current Alternative / Simple Baseline 相比，核心体验是否至少不明显更差？
- Product Spec 是否已经 PASS，当前设计是否与其一致；
- capability 是 fixture proof 还是 production vertical proof；
- external protocol 是否反向定义 Host；
- source/owner identity 是否穿过 adapter；
- durable 是否只证明同进程 reload；
- 剩余任务是否混淆 Architecture Prerequisite 与 Product UX；
- 是否因追求完整产品把 polish 错塞进关键路径。

若前五项不能回答，不得因为架构已经写得很完整而 Freeze。

---

## 13. Canonical Spec ≠ Agent Execution Prompt

```text
Repository Total Knowledge
!= Current Task Working Set
```

Canonical Product / Architecture Spec 负责完整产品、架构、需求与决策；Agent Prompt 负责当前原子执行。

正式任务默认采用：

```text
Canonical Spec
+
必要 Decision Digest
+
Execution Envelope
```

---

## 14. Decision Digest 与稳定编号

Decision Digest 只保留真正改变本次实现方式的裁定，通常使用 DEC / INV / AC / NON / RISK，每条只表达一个约束。

产品面任务至少应携带与当前增量直接相关的 Primary Purpose / Core Value 不变量，避免执行 Agent 只优化局部工程指标。

---

## 15. Formal Agent Task Packet Gate

正式 Codex / Grok / Agent 指令前确认：

```text
Freshness PASS
+
Decision Propagation PASS
+
Current Product Spec / Roadmap Consistent
+
Task Base HEAD Identified
+
No Known Superseding Decision Ignored
```

每份任务至少包含 Task ID/Owner/Base、Outcome、Why Now、Authority / Source Manifest、3–7 个 Read First、Decision Digest、Allowed / Prohibited Scope、Deliverables、Acceptance Gates、focused → full Validation、Git / Integration、Stop / Return、Final Report。

若任务直接改变用户主路径、核心体验或产品行为，Acceptance Gates 必须显式区分：

```text
Engineering Acceptance
!=
Product Value Acceptance
```

执行 Agent 可以证明工程实现完成，但不得替 Product Owner 宣布核心体验已经成立。

---

## 16. Repository `AGENTS.md` 与 Context Inclusion

长期稳定仓库规则写入 root / nested `AGENTS.md`；执行 Agent 默认读取 root AGENTS → nested AGENTS → Task Packet source manifest → implementation / tests。Dependency Graph != Context Inclusion Graph；Bounded Context 不得成为 Starved Context。

---

## 17. Validation、Git 与 Return Protocol

Validation 按 focused → full；真实 Provider / 联网 / 高成本 Gate 与离线 Gate 分开。开始记录 branch / base HEAD / status；authoritative write 前重查 HEAD；明确 commit / push / PR Owner；final report 给 final HEAD 与 status。

对于用户体验型产品，自动化 Validation 之后必须保留真实体验 Gate；工程自动化不能替代 Owner / User 对核心价值的裁定。

---

## 18. Prompt 长度治理与 A/B Regression

直接 Agent Prompt 超过约 4000 中文字符时触发重复度与 Canonicalization 审计：删除 superseded 历史 → 合并重复 → path + stable ID → 背景回 Canonical Spec → 拆跨阶段任务 → 稳定规则下沉 AGENTS → 保留当前增量与 Gate。

A/B Regression 不只比较 token / 错误率；对产品核心能力还要比较核心任务成功率、完成意愿、继续使用意愿或其它与 Primary Purpose 直接相关的指标。

---

## 19. Early Reality Check｜核心体验必须尽早真人验证

真实用户 / Owner 体验不得等到外围系统、协议、资产生态或长期可靠性全部完成后才第一次发生。

推荐节奏：

```text
最小核心循环可运行
→ 立即做一次短真实体验

核心 Product Vertical 成立
→ 1–3 次真实任务 / 1–3 回合

Real Creation / Import → Instance 成立
→ 5–10 步真实路径

Stage Exit 前
→ 10–20 步 Meaning / Product UAT
```

若短真实体验已经显示用户“不知道做什么”“不愿继续”“核心结果明显逊于简单替代方案”，应优先重开产品/架构问题，而不是继续堆外围系统。

---

## 20. AI Product Proposal 不得把审核责任全部转嫁给 Owner

AI / GPT 提出的产品方案、架构方案和“完整设计”都只是待验证方案，不因文本系统、专业或详尽而自动正确。

流程必须帮助 Product Owner 看见关键假设，而不是要求 Owner 在长讨论中自行发现所有隐患。

在产品定义或重大架构提案时，AI 至少应主动暴露：

- 它认为 Primary Purpose 是什么；
- 当前方案如何增强该价值；
- 哪些约束可能损害核心体验；
- 主要替代方案是什么；
- 最危险的反例 / 失败模式是什么；
- 最早如何用真人体验证伪。

若这些内容没有显式化，不能把后续偏航简单归咎于 Product Owner “当时没注意到”。

---

## 21. 与 current Skills 的关系

- `lifecycle-dev-process`：从 Product Purpose / Product Definition 到 Stage / Architecture / Migration / UAT 的生命周期规则；
- `lifecycle-templates`：包含 T00 Product Spec 在内的结构化模板；
- `agent-task-packet`：把项目事实转换成正式 Agent 执行指令，并对产品面任务保留 Product Value Gate。

项目执行时先读取项目 current facts，再读取 Skill current；项目专属裁定优先于通用规范。

---

## 22. v1.8 Change Log

相对 v1.7：

- 把 Stage 0 从“产品规格先于架构”进一步升级为“Primary Purpose / Core Value 先于产品规格与功能清单”；
- Canonical Product Spec 最小内容新增 Primary Purpose、Core Experience / Core Value Loop、Simple Baseline、Support Capabilities / Guardrails；
- 新增 Guardrail 不得成为 Product Owner 原则，防止局部正确约束无限扩大后吞噬核心体验；
- 新增 Current Alternative / Simple Baseline 反证门禁：复杂系统在核心价值上明显更差时不得以工程优势宣布成功；
- Product Definition Gate 新增最早真实用户 / Owner 验证计划；
- Architecture Audit 新增 Core Purpose、Guardrail、Baseline 与“去掉外围能力后核心旅程是否仍成立”检查；
- Agent Task Packet Gate 要求产品面任务区分 Engineering Acceptance 与 Product Value Acceptance；
- 新增 Early Reality Check，要求最小核心循环一旦可运行就尽早真人验证；
- 新增 AI Product Proposal 责任：AI 必须显式暴露产品假设、约束代价、替代方案、失败模式和最早证伪路径，不能把发现错误方案的责任全部转嫁给 Product Owner；
- 保留 v1.7 的 Product Definition、Stage Exit、Host-before-Protocol、Production Vertical、Canonical Spec / Decision Digest / Execution Envelope、Agent Task Packet、AGENTS 与 Prompt 治理。