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
status: archived
version: 1.6
created: 2026-08-15
last_updated: 2026-08-18
scope: cross-project
superseded_by: AI驱动项目全生命周期开发流程规范_v1.7_2026-08-19.md
---

# AI 驱动项目全生命周期开发流程规范 v1.6

> [!abstract] 文档定位
> v1.6 **SUPERSEDED BY v1.7**。
>
> 本文件为历史版本，仅供追溯。current 规则请读取 active 目录中的 v1.7。

---

## 1. Stage Exit 不等于“把计划表全部做完”

阶段退出前必须重新分类剩余事项，而不是机械执行初始清单。

### A｜Downstream Architecture Prerequisite
若延后会改变下一阶段的 Schema、Public Contract、Compiler、Plugin / Asset Protocol、Runtime authority、State model、Safety boundary 或 Core action semantics，则当前阶段 REQUIRED。

### B｜Stage Objective Minimum
属于当前阶段声称已成立的核心能力，即使不直接改变下一阶段协议，也必须完成或正式调整阶段目标。

### C｜Product Consumption / UX Maturity
底层 semantics / authority 已证明，只缺完整消费体验。是否阻塞取决于成熟度。

### D｜Polish / Future Scale
深度 WCAG、全设备像素级适配、分布式能力、大规模性能、speculative platformization 默认不阻塞早期架构阶段。

---

## 2. 延后必须是正式裁定，不是遗忘
任何 Deferred item 至少记录 Capability、Why deferred、Underlying semantics、Downstream contract impact、Revisit trigger 与 Non-regression constraint。

---

## 3. Host / Platform Before External Protocol

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

---

## 4. Fixture / Preview ≠ Production Closure
unit assembler、mock DTO、typed fixture、preview query、static screenshot、isolated component test 只能证明局部能力。若能力将成为下一阶段协议的正式上游，至少需要 internal typed declaration → real bootstrap / server → official projection → real consumer。

---

## 5. Declarative Structure ≠ Live Data Access
声明式系统必须区分 Declaration 与 Materialized Safe Data；正式链为 Authoritative State → bounded domain projection → server-side adapter / resolver → materialized safe DTO → declarative Host rendering。

---

## 6. Source / Owner Identity 不得在 Adapter 链中丢失
Source → Compiler / Adapter → Assembly → Projection → UI / Runtime Consumer 必须保留声明者、Owner、依赖和实例来源。

---

## 7. Durable Preference 必须单独证明 durability
区分 in-memory UI state、session/browser state、durable product preference、business authoritative state 与 canonical snapshot。

---

## 8. Supersession 需要 production zero-reference audit
新消费者不再使用旧 API，不等于旧 authority 已退休。无真实兼容义务时应 retire old production path → keep historical evidence only。

---

## 9. Personal / Internal Development 的 Scope Right-sizing
个人或内部开发阶段允许后置消费层成熟度，但不能推迟决定未来协议正确性的上游事实。

---

## 10. Minimal Semantic Slice Before Downstream Schema

```text
Core semantic rule
→ deterministic authority
→ atomic behavior
→ minimal real vertical proof
```

---

## 11. Owner-aligned Multi-Agent Execution
默认按能力 Owner 切任务；Task Executor implements → validates → precise commit/push；Independent Reviewer reviews canonical result。

---

## 12. Architecture Audit 必查问题
在 Shared Foundation、Schema Freeze、Stage Closure 前检查 production proof、声明式数据边界、Host/protocol 顺序、source identity、durability、阶段目标、polish 与 prerequisite 分类。

---

## 13. Canonical Spec ≠ Agent Execution Prompt

```text
Repository Total Knowledge
!= Current Task Working Set
```

Canonical Spec 负责完整产品、架构、需求和决策；Agent Prompt 负责当前原子执行。正式任务默认采用 Canonical Spec + 必要 Decision Digest + Execution Envelope。

---

## 14. Decision Digest 与稳定编号
Decision Digest 只保留真正改变本次实现方式的裁定，通常使用 DEC / INV / AC / NON / RISK。

---

## 15. Formal Agent Task Packet Gate
正式 Agent 指令前确认 Freshness PASS + Decision Propagation PASS + Current Roadmap Consistent + Task Base HEAD Identified + No Known Superseding Decision Ignored。

---

## 16. Repository `AGENTS.md` 与 Context Inclusion
长期稳定仓库规则写入 `AGENTS.md`；执行 Agent 默认读取 root AGENTS → nested AGENTS → Task Packet source manifest → implementation/tests。Dependency Graph != Context Inclusion Graph；Bounded Context 不得变成 Starved Context。

---

## 17. Validation、Git 与 Return Protocol
Validation 按 focused → full；开始记录 branch/base HEAD/status；authoritative write 前重查 HEAD；明确 commit/push/PR Owner；final report 给 final HEAD/status。

---

## 18. Prompt 长度治理与 A/B Regression
约 4000 中文字符以上的直接 Agent Prompt 触发重复度与 Canonicalization 审计；压缩顺序为删除 superseded 历史 → 合并重复 → path + stable ID → 背景回 Canonical Spec → 拆任务 → 稳定规则下沉 AGENTS → 保留当前 Gate。

---

## 19. 与 current Skills 的关系
- lifecycle-dev-process：阶段、架构、迁移、Owner、Gate 与 UAT；
- lifecycle-templates：结构化审计与模板；
- agent-task-packet：把项目事实转换成正式 Agent 执行指令。

---

## 20. v1.6 Change Log
相对 v1.5：新增 Canonical Spec / Decision Digest / Execution Envelope 三层模型、Formal Agent Task Packet Gate、Authority / Source Manifest、Read First、Scope、Acceptance、Validation、Git、Return、Final Report、AGENTS 分层治理、Prompt 长度治理与 A/B regression。