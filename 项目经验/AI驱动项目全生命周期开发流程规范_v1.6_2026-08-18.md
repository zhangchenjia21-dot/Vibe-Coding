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
> v1.6 完整保留 v1.5 的 Stage Exit 分类、Host-before-Protocol、Production Vertical、声明式数据边界、Source / Owner Identity、Durable Preference、Supersession、Scope Right-sizing 与 Owner-aligned Execution；新增正式 Agent 指令与长上下文治理：
>
> 1. Canonical Spec 不再全文复制为执行 Prompt；
> 2. 固定 `Canonical Spec + Decision Digest + Execution Envelope` 三层模型；
> 3. 正式 Codex / Grok / Agent 指令必须包含 Authority、Scope、Acceptance、Validation、Git 与 Return；
> 4. 仓库稳定规则下沉至 `AGENTS.md`，任务上下文采用最小充分工作集；
> 5. 对高频 Prompt 建立 A/B regression，而不是凭感觉压缩。
>
> 未在本版本明确修改的 v1.5 原则继续有效。

---

## 1. Stage Exit 不等于“把计划表全部做完”

阶段退出前必须重新分类剩余事项，而不是机械执行初始清单。

### A｜Downstream Architecture Prerequisite

若延后会改变下一阶段的 Schema、Public Contract、Compiler、Plugin / Asset Protocol、Runtime authority、State model、Safety boundary 或 Core action semantics，则当前阶段 REQUIRED。

### B｜Stage Objective Minimum

属于当前阶段声称已成立的核心能力，即使不直接改变下一阶段协议，也必须完成或正式调整阶段目标。

典型错误：

- 只有 fixture，却把 Host 标为 CLOSED；
- 只有内存，却把 preference 标为 durable；
- UI 有动态结构，但正式状态变化不更新数据，却宣称 dynamic complete；
- production 仍保留第二 authority path，却宣称 migration closed。

### C｜Product Consumption / UX Maturity

底层 semantics / authority 已证明，只缺完整消费体验，例如管理中心、undo UI、error wording、advanced browsing、copy / visual polish。是否阻塞取决于成熟度：

```text
internal / personal → 可显式延后
Alpha              → 重要用户路径重新拉回 Required
Release            → resilience / accessibility / migration / operations 多数 Required
```

### D｜Polish / Future Scale

深度 WCAG、全设备像素级适配、分布式能力、大规模性能、speculative platformization 默认不阻塞早期架构阶段。

---

## 2. 延后必须是正式裁定，不是遗忘

任何 Deferred item 至少记录：

```text
Capability
Why deferred
Underlying semantics already proven by
Why it does not change current downstream contract
Revisit stage / trigger
Non-regression constraint
```

禁止只写“以后做”。Deferred 不等于取消；进入 Alpha / Release 后按 revisit gate 重新评估。

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

禁止先发明强大 Schema，再逼 Host / Runtime 实现 Schema 假设。外部协议只能声明已由 Host / Runtime 验证的能力；真实消费者暴露新 Host 需求时，回到 Host capability review，再扩协议。

---

## 4. Fixture / Preview ≠ Production Closure

unit assembler、mock DTO、typed fixture、preview query、static screenshot、isolated component test 只能证明局部能力。

若能力将成为下一阶段协议的正式上游，至少需要：

```text
internal typed declaration
→ real bootstrap / server
→ official projection
→ real consumer
```

阶段审核必须分别标记 Foundation PASS、Presentation PASS、Production Vertical PASS，不能混成模糊的 DONE。

---

## 5. Declarative Structure ≠ Live Data Access

声明式系统必须区分：

```text
Declaration = what capability / structure is needed
Materialized Safe Data = current values the consumer may see
```

正式链：

```text
Authoritative State
→ bounded domain projection
→ server-side adapter / resolver
→ materialized safe DTO
→ declarative Host rendering
```

声明层默认不得拥有 arbitrary state path / object selector、JavaScript callback、expression engine、eval、arbitrary query DSL 或 browser-side hidden-state access。

需要动态绑定时，优先 typed data slot、bounded resolver vocabulary、purpose-built materializer、server-side projection port，不从通用表达式引擎开始。

---

## 6. Source / Owner Identity 不得在 Adapter 链中丢失

```text
Source
→ Compiler / Adapter
→ Assembly
→ Projection
→ UI / Runtime Consumer
```

必须保留：谁声明、谁 owns、谁 depends on 谁、当前实例属于哪个 source、disable/remove 后谁负责 dormant / cleanup。

禁止运行时通过 displayName、label、topic 或自然语言相似度重新猜 Owner。

---

## 7. Durable Preference 必须单独证明 durability

明确区分：

```text
in-memory UI state
session/browser state
durable product preference
business authoritative state
canonical snapshot
```

若产品声称跨应用重启保留，只证明组件 remount / 页面 reload 不够。Product Preference 不应因 durable 就塞入 business state；Restore business snapshot 不应自动回滚独立 UI preference；preference 应有自己的 persistence owner。

---

## 8. Supersession 需要 production zero-reference audit

新消费者不再使用旧 API，不等于旧 authority 已退休。Migration Closure 应搜索 old DTO、old route、old state owner、old store、legacy fallback、obsolete tests、old adapter。

无真实兼容义务时：

```text
retire old production path
→ keep historical evidence only
```

避免永久 dual authority。

---

## 9. Personal / Internal Development 的 Scope Right-sizing

个人或内部开发阶段允许后置完整管理中心、高级 undo UX、深度 error center、全设备 visual polish、完整 WCAG、运维级 observability，前提是：

1. 底层 semantics / authority 已独立证明；
2. 延后不改变下游 Schema / Contract；
3. 有明确 revisit stage；
4. 不借“内部开发”跳过安全、数据一致性或不可逆架构问题。

> 个人开发可以推迟用户消费层成熟度，不能推迟决定未来协议正确性的上游事实。

---

## 10. Minimal Semantic Slice Before Downstream Schema

若能力未来会被 Extension / Asset / Plugin 声明，但完整产品化很大，可以先做：

```text
Core semantic rule
→ deterministic authority
→ atomic behavior
→ minimal real vertical proof
```

而不同时完成 polished UI、general planner、万能 DSL 和所有 edge features。这样既避免协议抢跑，也避免把全部产品工程量塞进架构阶段。

---

## 11. Owner-aligned Multi-Agent Execution

默认按能力 Owner 切任务：

```text
Frontend / Product owner
→ implementation + focused validation + task-owned integration

Runtime / Contract owner
→ implementation + focused validation + task-owned integration

Independent Reviewer
→ canonical result review
```

执行者具备安全 Git 能力时，不默认要求第二 Agent 纯代 commit/push。只有 cross-owner diff、conflict、unknown dirty state、复杂迁移或高风险 Git 操作时才引入 Integration Owner。

---

## 12. Architecture Audit 必查问题

在 Shared Foundation、Schema Freeze、Stage Closure 前检查：

- capability 是 fixture proof 还是 production vertical proof？
- 声明式结构是否偷偷拥有 live-state query 权？
- external protocol 是否在反向定义 Host？
- source/owner identity 是否穿过 adapter 链？
- durable 是否只证明同进程 reload？
- 剩余任务是否混淆 Architecture Prerequisite 与 Product UX？
- 是否因为着急漏掉阶段目标？
- 是否因追求完整产品把未来 polish 错塞进关键路径？

审计结果闭环到 current project source、Decision / Deferred Ledger、Lifecycle Skill、Template / Harness。

---

## 13. Canonical Spec ≠ Agent Execution Prompt

项目事实越多，不代表每次 Agent 执行都应看到全部事实。

```text
Repository Total Knowledge
!= Current Task Working Set
```

Canonical Spec 负责完整产品、架构、需求和决策；Agent Prompt 负责当前原子执行。把所有上游全文复制进一条 Prompt 会制造重复、权威竞争、历史噪声和多任务混装。

正式任务默认采用：

```text
Canonical Spec
+
必要 Decision Digest
+
Execution Envelope
```

路径引用只有在执行 Agent 可读取时才有效；不可读取时提供必要摘要，不得只给无效链接。

---

## 14. Decision Digest 与稳定编号

Decision Digest 只保留真正改变本次实现方式的裁定，通常 5–12 条。使用：

```text
DEC-xx  决策
INV-xx  不变量
AC-xx   验收
NON-xx  非范围
RISK-xx 风险
```

每条只表达一个约束。能引用 Canonical Spec 中稳定编号时，不复制长段落。

---

## 15. Formal Agent Task Packet Gate

GPT 网页端、Planner 或 Reviewer生成正式 Codex / Grok / Agent 指令前必须确认：

```text
Freshness PASS
+
Decision Propagation PASS
+
Current Roadmap Consistent
+
Task Base HEAD Identified
+
No Known Superseding Decision Ignored
```

每份正式任务至少包含：

1. Task ID / Type / Primary Owner / Base；
2. Outcome；
3. Why Now；
4. Authority / Source Manifest；
5. 3–7 个初始 Read First 入口；
6. Decision Digest / Invariants；
7. Allowed / Prohibited Scope；
8. Deliverables；
9. 可证伪 Acceptance Gates；
10. focused → full Validation；
11. Git / Integration；
12. Stop / Return Conditions；
13. Final Report。

不能用“请完整实现”“保证质量”“跑所有测试”等模糊语言替代具体 Gate。

---

## 16. Repository `AGENTS.md` 与 Context Inclusion

长期稳定的仓库规则应写入目标仓库根级 `AGENTS.md`；高风险 Owner 目录可添加局部 `AGENTS.md`。快速变化的 current task、临时 SHA 和阶段 blocker 不写入稳定 `AGENTS.md`。

执行 Agent 的默认读取顺序：

```text
root AGENTS.md
→ relevant nested AGENTS.md
→ Task Packet source manifest
→ implementation / tests
```

默认初始读取集为 3–7 个入口，只有证据不足时扩大并说明原因。禁止默认阅读整个仓库或所有文档。

```text
Dependency Graph
!= Context Inclusion Graph
```

同时 Bounded Context 不得变成 Starved Context：identity、目标、当前状态、refs、authority、验收和完成证据必须齐全。

---

## 17. Validation、Git 与 Return Protocol

Validation 按 focused → full 排列；真实 Provider、联网或高成本 Gate 与离线 Gate 分开，离线失败时停止浪费真实调用。

Git 至少要求：

- 开始时记录 branch / base HEAD / status；
- 不覆盖未知 dirty worktree；
- authoritative write 前重新检查 HEAD；
- 明确 commit / push / PR Owner；
- final report 给出 final HEAD 与 status。

只有 current source 冲突、需要产品裁定、缺少必要 secret / artifact、发现 P0 或不可逆迁移风险、任务必须越过 Prohibited Scope、worktree 无法安全识别时才停止并返回。普通局部不确定性应先调查并作最小安全实现。

---

## 18. Prompt 长度治理与 A/B Regression

以下为治理触发器，不是模型硬限制：

- 单模块 Execution Envelope：约 800–1800 中文字符；
- 跨模块任务：约 1500–3500 中文字符；
- Decision Digest：通常 5–12 条；
- 初始读取集：通常 3–7 个文件；
- 直接任务 Prompt 超过约 4000 中文字符时，执行重复度与 Canonicalization 审计。

压缩顺序：删除 superseded 历史 → 合并重复 → 用 path + stable ID 替代全文 → 背景回归 Canonical Spec → 拆分跨阶段目标 → 稳定规则下沉 AGENTS → 保留当前增量与 Gate。

对高频任务建立 A/B regression，比较 Acceptance PASS、错误路径修改、scope creep、Review 新发现数、读取文件 / 工具调用、token / 时间 / 成本、关键不变量遗漏。只有正确率不下降时才采用更短版本。

---

## 19. 与 current Skills 的关系

- `lifecycle-dev-process`：阶段、架构、迁移、Owner、Gate 与 UAT；
- `lifecycle-templates`：结构化审计与 Task Packet 骨架；
- `agent-task-packet`：把项目事实转换成正式 Agent 执行指令。

项目执行时先读取项目 current facts，再读取 Skill current；项目专属裁定优先于通用规范。

---

## 20. v1.6 Change Log

相对 v1.5：

- 新增 Canonical Spec / Decision Digest / Execution Envelope 三层模型；
- 新增 Formal Agent Task Packet Gate；
- 新增 Authority / Source Manifest、Read First、Scope、Acceptance、Validation、Git、Return 与 Final Report 要求；
- 新增 repository `AGENTS.md` 分层治理；
- 明确 Dependency Graph ≠ Context Inclusion Graph，Bounded ≠ Starved；
- 新增 Prompt 长度触发器、压缩顺序与 A/B regression；
- 更新 current Skill 协作关系。
