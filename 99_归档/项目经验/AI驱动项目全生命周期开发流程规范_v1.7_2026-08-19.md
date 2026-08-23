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
version: 1.7
created: 2026-08-15
last_updated: 2026-08-19
scope: cross-project
supersedes: AI驱动项目全生命周期开发流程规范_v1.6_2026-08-18.md
---

# AI 驱动项目全生命周期开发流程规范 v1.7

> [!abstract] 文档定位
> v1.7 **SUPERSEDES v1.6**。
>
> v1.7 保留 v1.6 的 Stage Exit 分类、Host-before-Protocol、Production Vertical、声明式数据边界、Source / Owner Identity、Durable Preference、Supersession、Scope Right-sizing、Owner-aligned Execution、Canonical Spec / Decision Digest / Execution Envelope 与 Agent Task Packet 治理。
>
> v1.7 新增生命周期最上游：**Stage 0｜Product Definition & Canonical Product Spec**。以后“先讨论清楚”必须收敛成唯一 current 的产品规格事实源；Product Definition Gate 未通过前，不进入需要长期承诺的正式架构冻结与高耦合实现。

---

## 0. Stage 0｜Product Definition & Canonical Product Spec

新项目、重大重构、产品方向重置或核心用户承诺发生改变时，默认从 Stage 0 开始。

正确顺序：

```text
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
Downstream Consumers
↓
Stage / Release Lifecycle
```

### 0.1 “先讨论清楚”必须产生正式产物

聊天、头脑风暴、原型、零散笔记可以作为 Discovery Evidence，但不能成为长期产品事实源。

Stage 0 的正式产物是唯一 current 的 `Canonical Product Spec`，至少覆盖：

1. Problem / Motivation；
2. Target User / Actors；
3. Product Promise；
4. Core User Journey；
5. Functional Requirements；
6. Non-functional Requirements；
7. Domain Semantics；
8. Scope / Deferred / Non-scope；
9. Hard Constraints；
10. Success / Acceptance；
11. Open Questions / Blockers；
12. Decision Ledger / Evidence。

### 0.2 Product Definition Gate

进入正式 Architecture Discovery 前至少确认：

- 核心用户、核心价值与 Core User Journey 明确；
- Must-have、Deferred 与 Non-scope 可区分；
- 关键 Domain Semantics 足够清晰，不会让架构基于不同含义各自实现；
- 主要硬约束已知；
- Success / Acceptance 可验证；
- Open Questions 已分类为 blocking / non-blocking；
- 不存在会实质改变 Owner、Identity、State Model、Schema / Protocol、安全边界或主用户路径的 unresolved blocker；
- 已指定唯一 current Canonical Product Spec。

Gate 结果只有：

```text
NOT READY FOR ARCHITECTURE
READY FOR DOMAIN / CAPABILITY SURVEY
```

### 0.3 Gate 前允许探索，但禁止伪冻结

Product Definition Gate PASS 前允许：

- Prototype；
- UX exploration；
- Technical spike；
- Mock / Fixture；
- Feasibility test；
- Exploratory provider/model test。

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
- 以“代码已经写了很多”为理由跳过产品裁定。

### 0.4 Product Spec 是 current truth，不是一次写死

架构、原型、UAT 或真实用户反馈可以促使 Product Spec 演化。

若新决策改变 Product Promise、Core User Journey、关键 Domain Semantics、Scope / Non-scope、Success / Acceptance 或会影响架构的硬约束，则必须：

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

典型错误：只有 fixture 却把 Host 标 CLOSED；只有内存却声称 durable；动态结构不随正式状态变化却声称 dynamic complete；production 仍保留第二 authority path 却宣称 migration closed。

### C｜Product Consumption / UX Maturity
底层 semantics / authority 已证明，只缺完整消费体验。internal/personal 阶段可显式延后；Alpha 重新拉回关键用户路径；Release 则 resilience / accessibility / migration / operations 多数成为 Required。

### D｜Polish / Future Scale
深度 WCAG、全设备像素级适配、分布式、大规模性能、speculative platformization 默认不阻塞早期架构阶段。

---

## 2. 延后必须是正式裁定，不是遗忘

Deferred item 至少记录：Capability、Why deferred、Underlying semantics already proven by、Why it does not change current downstream contract、Revisit stage / trigger、Non-regression constraint。

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

个人或内部开发可以推迟消费层成熟度，但不能推迟会决定下游协议正确性的语义、authority、安全、数据一致性与不可逆架构问题。

---

## 10. Minimal Semantic Slice Before Downstream Schema

```text
Core semantic rule
→ deterministic authority
→ atomic behavior
→ minimal real vertical proof
```

不需要同时完成 polished UI、general planner、万能 DSL 与所有 edge features。

---

## 11. Owner-aligned Multi-Agent Execution

默认按能力 Owner 切任务：Executor implements → focused validation → task-owned integration；Independent Reviewer review canonical result。只有 cross-owner diff、conflict、unknown dirty、复杂迁移或高风险 Git 才引入 Integration Owner。

---

## 12. Architecture Audit 必查问题

在 Shared Foundation、Schema Freeze、Stage Closure 前检查：

- Product Spec 是否已经 PASS，当前设计是否与其一致；
- capability 是 fixture proof 还是 production vertical proof；
- external protocol 是否反向定义 Host；
- source/owner identity 是否穿过 adapter；
- durable 是否只证明同进程 reload；
- 剩余任务是否混淆 Architecture Prerequisite 与 Product UX；
- 是否因追求完整产品把 polish 错塞进关键路径。

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

---

## 16. Repository `AGENTS.md` 与 Context Inclusion

长期稳定仓库规则写入 root / nested `AGENTS.md`；执行 Agent 默认读取 root AGENTS → nested AGENTS → Task Packet source manifest → implementation / tests。Dependency Graph != Context Inclusion Graph；Bounded Context 不得成为 Starved Context。

---

## 17. Validation、Git 与 Return Protocol

Validation 按 focused → full；真实 Provider / 联网 / 高成本 Gate 与离线 Gate 分开。开始记录 branch / base HEAD / status；authoritative write 前重查 HEAD；明确 commit / push / PR Owner；final report 给 final HEAD 与 status。

---

## 18. Prompt 长度治理与 A/B Regression

直接 Agent Prompt 超过约 4000 中文字符时触发重复度与 Canonicalization 审计：删除 superseded 历史 → 合并重复 → path + stable ID → 背景回 Canonical Spec → 拆跨阶段任务 → 稳定规则下沉 AGENTS → 保留当前增量与 Gate。

---

## 19. 与 current Skills 的关系

- `lifecycle-dev-process`：从 Product Definition 到 Stage / Architecture / Migration / UAT 的生命周期规则；
- `lifecycle-templates`：包含 T00 Product Spec 在内的结构化模板；
- `agent-task-packet`：把项目事实转换成正式 Agent 执行指令。

项目执行时先读取项目 current facts，再读取 Skill current；项目专属裁定优先于通用规范。

---

## 20. v1.7 Change Log

相对 v1.6：

- 新增 Stage 0 Product Definition & Canonical Product Spec；
- 把“先讨论清楚”升级为必须形成唯一 current 产品规格事实源；
- 明确 Canonical Product Spec 12 类最小内容；
- 新增 Product Definition Gate PASS 条件；
- Gate 前允许 Prototype / Spike / Mock / Feasibility，但必须标记 exploration；
- Gate PASS 前默认禁止冻结 external schema / protocol / 长期 Domain Model 与批量高耦合实现；
- Product Spec 发生实质变化时必须 re-evaluate Gate + Decision Propagation；
- Architecture Audit 与 Formal Agent Task Packet Gate 增加 Product Spec 一致性检查；
- 保留 v1.6 的 Stage Exit、Host-before-Protocol、Production Vertical、Canonical Spec / Decision Digest / Execution Envelope、Agent Task Packet、AGENTS 与 Prompt 治理。