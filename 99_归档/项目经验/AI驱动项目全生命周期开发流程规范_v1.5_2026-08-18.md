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
version: 1.5
created: 2026-08-15
last_updated: 2026-08-18
scope: cross-project
supersedes: AI驱动项目全生命周期开发流程规范_v1.4.md
---

# AI 驱动项目全生命周期开发流程规范 v1.5

> [!abstract] 文档定位
> v1.5 **SUPERSEDES v1.4**。
>
> v1.5 保留 v1.4 已冻结的完整生命周期、事实源恢复、产品/架构分层、真实纵向链、自动验证、独立审核、UAT、阶段止损、Git 安全、迁移与复盘原则；本版本重点补充近期多阶段 AI 项目中反复出现的四类问题：
>
> 1. 阶段剩余任务没有区分“下游架构前置”和“产品消费层成熟度”；
> 2. 外部 Schema / Creator / Plugin Protocol 在 Host 尚未真实验证时抢跑；
> 3. 声明式 UI / workflow 被误解为任意 live-state 查询或任意代码执行；
> 4. 多 Agent 协作中出现“实现者完成 → 第二 Agent 纯代提交”的无价值交接。
>
> 未在本版本明确修改的 v1.4 原则继续有效。

---

## 1. Stage Exit 不等于“把计划表全部做完”

阶段准备退出时，必须对剩余事项重新分类，而不是机械执行最初任务清单。

至少分为：

### A｜Downstream Architecture Prerequisite

如果延后会改变下一阶段：

- Schema；
- Public Contract；
- Compiler；
- Plugin / Asset Protocol；
- Runtime authority；
- State model；
- Safety boundary；
- Core action semantics；

则必须当前阶段完成。

### B｜Stage Objective Minimum

即使不直接影响下一阶段协议，如果属于当前阶段声称已经成立的核心能力，也必须完成或正式调整阶段目标。

典型错误：

- 只有 fixture，却把 Host 标为 CLOSED；
- 只有内存，却把 preference 标为 durable；
- UI 有动态结构，但正式状态变化不会更新数据，却宣称 dynamic complete；
- production 仍保留第二 authority path，却宣称 migration closed。

### C｜Product Consumption / UX Maturity

底层 semantics / authority 已经证明，只缺更完整玩家/用户消费体验，例如：

- 管理中心；
- undo UI；
- error wording；
- advanced browsing；
- copy / visual polish。

是否阻塞取决于当前成熟度：

```text
internal/personal development
→ 可以显式延后

Alpha
→ 重要用户路径重新拉回 Required

Release
→ product resilience / accessibility / migration / operations 大多必须完成
```

### D｜Polish / Future Scale

例如深度 WCAG、全设备像素级适配、分布式能力、大规模 performance tuning、speculative platformization，默认不阻塞早期架构阶段。

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

禁止只写：

> “以后做。”

Deferred 不等于取消。

如果后续进入 Alpha / Release，必须按 revisit gate 重新评估。

---

## 3. Host / Platform Before External Protocol

当下一阶段要设计：

- asset schema；
- plugin manifest；
- extension protocol；
- workflow DSL；
- Creator / Builder / Compiler；

默认必须先完成：

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

禁止反向：

```text
先发明一个强大 Schema
↓
再逼 Host/Runtime 实现 Schema 假设的能力
```

外部协议只能声明已经由 Host / Runtime 验证的能力。

如果真实消费者暴露新的 Host 需求：

> 回到 Host capability review，再扩协议。

不要在 Schema 中暗加执行权。

---

## 4. Fixture / Preview ≠ Production Closure

以下证据只能证明局部能力：

- unit assembler；
- mock DTO；
- typed fixture；
- preview query；
- static screenshot；
- isolated component test。

如果能力会成为下一阶段协议的正式上游，至少需要一条真实纵向证明：

```text
internal typed declaration
→ real bootstrap / server
→ official projection
→ real consumer
```

阶段审核必须明确标记：

- Foundation PASS；
- Presentation PASS；
- Production Vertical PASS；

不能混成一个模糊的 “DONE”。

---

## 5. Declarative Structure ≠ Live Data Access

声明式系统必须区分：

```text
Declaration
= what capability / structure is needed

Materialized Safe Data
= current values the consumer may see
```

默认正式链：

```text
Authoritative State
→ bounded domain projection
→ server-side adapter / resolver
→ materialized safe DTO
→ declarative Host rendering
```

声明层默认不得拥有：

- arbitrary state path；
- arbitrary object selector；
- JavaScript callback；
- expression engine；
- eval；
- arbitrary query DSL；
- browser-side hidden-state access。

如果确实需要动态绑定，优先从：

- typed data slot；
- bounded resolver vocabulary；
- purpose-built materializer；
- server-side projection port；

开始。

不要从通用表达式引擎开始。

---

## 6. Source / Owner Identity 不得在 Adapter 链中丢失

声明或贡献经过：

```text
Source
→ Compiler / Adapter
→ Assembly
→ Projection
→ UI / Runtime Consumer
```

时，必须保留足以回答：

- 谁声明；
- 谁 owns；
- 谁 depends on 谁；
- 当前实例属于哪个 source；
- disable/remove 后谁负责 dormant / cleanup。

禁止运行时通过：

- displayName；
- label；
- topic；
-自然语言相似度；

重新猜 Owner。

---

## 7. Durable Preference 必须单独证明 durability

需要明确区分：

```text
in-memory UI state
session/browser state
durable product preference
business authoritative state
canonical snapshot
```

如果产品语义声称“跨应用重启保留”，只证明组件 remount / 页面 reload 不够。

同时：

- Product Preference 不应因为 durable 就塞入 business state；
- Restore business snapshot 不应自动回滚独立 UI preference；
- preference 应有自己的 persistence owner。

---

## 8. Supersession 需要 production zero-reference audit

新架构替代旧实现时：

> 新消费者已经不使用旧 API，不代表旧 authority 已退休。

Migration Closure 应搜索：

- old DTO；
- old route；
- old state owner；
- old store；
- legacy fallback；
- obsolete tests；
-旧 adapter。

无真实兼容义务时：

```text
retire old production path
→ keep historical evidence only
```

避免永久 dual authority。

---

## 9. Personal / Internal Development 的 Scope Right-sizing

个人或内部开发阶段可以主动减少产品级工作量。

允许后置：

- 完整管理中心；
- 高级 undo UX；
- 深度 error center；
- 全设备 visual polish；
- 完整 WCAG；
- 运维级 observability；

前提是：

1. 底层 semantics / authority 已经独立证明；
2. 延后不会改变当前下游 Schema / Contract；
3. 有明确 revisit stage；
4. 不会借“内部开发”跳过安全、数据一致性或不可逆架构问题。

一句话：

> **个人开发可以推迟用户消费层成熟度，不能推迟决定未来协议正确性的上游事实。**

---

## 10. Minimal Semantic Slice Before Downstream Schema

如果某项能力未来会被外部 Extension / Asset / Plugin 声明，但完整产品化很大，可以只在当前阶段做最小语义竖切。

例如：

```text
Core semantic rule
→ deterministic authority
→ atomic behavior
→ minimal real vertical proof
```

而不必同时完成：

- polished UI；
- general planner；
-万能 DSL；
-所有 edge features。

这样既避免协议抢跑，也避免把产品工程量全部塞到架构阶段。

---

## 11. Owner-aligned Multi-Agent Execution

默认按能力 Owner 切任务：

```text
Frontend/Product owner
→ implementation + focused validation + task-owned integration

Runtime/Contract owner
→ implementation + focused validation + task-owned integration

Independent Reviewer
→ remote/canonical result review
```

如果执行者已经具备安全 Git 能力，不默认要求第二 Agent 纯代 commit/push。

纯代交接容易造成：

- context reload；
- 重复 review；
- ownership ambiguity；
- scope creep；
-第二次不必要开发周期。

只有 cross-owner diff、conflict、unknown dirty state、复杂迁移、高风险 Git 操作时才引入专门 Integration Owner。

---

## 12. Architecture Audit 新增必查问题

在 Shared Foundation、Schema Freeze、Stage Closure 前，新增以下问题：

- 当前 capability 是 fixture proof 还是 production vertical proof？
- 声明式结构是否偷偷拥有 live-state query 权？
- external protocol 是否在反向定义 Host？
- source/owner identity 是否穿过 adapter 链？
- “durable” 是否只证明同进程 reload？
- 当前剩余任务是否混淆了 Architecture Prerequisite 与 Product UX？
- 是否因为着急漏掉阶段真正目标？
- 是否因为追求完整产品把未来 polish 错塞进关键路径？

审计结果必须闭环到：

- current project source；
- Decision / Deferred Ledger；
- Lifecycle Skill；
- Template / Harness。

---

## 13. 与 lifecycle-dev-process v1.8 的关系

本规范的机器可执行 Skill 版本为：

`zhangchenjia21-dot/Skill/main/skill/gpt/lifecycle-dev-process/SKILL.md`

当前对应：

> `lifecycle-dev-process v1.8`

结构化模板对应：

> `lifecycle-templates v1.7`

项目执行时：

1. 先读取项目最新事实；
2. 再读取 Skill 最新实际版本；
3. 项目专属裁定优先于通用规范；
4. 通用 Skill 不覆盖项目业务事实。

---

## 14. v1.5 Change Log

相对 v1.4 新增/强化：

- Stage Exit A/B/C/D critical-path classification；
- maturity-aware scope right-sizing；
- Deferred item revisit gate；
- Host / Platform Before External Protocol；
- fixture/preview vs production closure boundary；
- Declarative Structure ≠ Live Data Access；
- arbitrary state-path / expression DSL 默认禁止；
- source/owner identity through adapter chain；
- durable preference 与 business state 分离；
- Supersession production zero-reference audit；
- Minimal Semantic Slice before downstream Schema；
- Owner-aligned execution / task-owned Git integration；
- Architecture Audit 对阶段任务量与协议前置的新增检查。
