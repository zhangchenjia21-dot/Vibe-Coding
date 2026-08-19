---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-19
current_path: sillytavern/酒馆游戏项目开发核心总纲_CURRENT.md
---

# 酒馆游戏项目开发核心总纲｜CURRENT

## 0. 当前状态

```text
G1–G8                         PASS / CLOSED
G8-UAT-02 final SHA            cdbd9cd7ff0b5b9a5672156066478b57f732307c
Project Owner Stage UAT        PASS WITH NON-BLOCKING UX FINDINGS
P0                             0
P1                             0
G9                             ACTIVE / G9-02BC Shared Foundation
G9-01 Compatibility Audit      PASS / CLOSED
G9-02A                         PASS / CLOSED
G9-02A final SHA               04603e1e4a3270e9f5740b5957cf545a2bd001d0
G9-02A Independent Review      PASS / CLOSED
G9-02BC Shared Foundation      ACTIVE / NEXT
G9-02B breadth                 BLOCKED BY G9-02BC
G9-02C breadth                 BLOCKED BY G9-02BC + G9-02B breadth
G9-03                          NOT AUTHORIZED
Current Next                   G9-02BC Shared Runtime Foundation Convergence
```

G9-01 已确认：现有 Semantic Assets 总体兼容，真正缺口在 Runtime 资产承接与 Context Orchestration。

G9-02A 已在 exact SHA `04603e1e4a3270e9f5740b5957cf545a2bd001d0` 完成 Source Binding、Game-local Revision、typed existing-asset mutation、Save / Restore / Branch / Recovery，并通过 Independent Review。当前没有 P0/P1 blocker。

#17 继续冻结：**Current Scene Visible Characters != Player-known Character Directory**。People Surface 必须是长期玩家已知人物目录，而不是当前场景 presence 列表。

---

## 1. 当前 active 正式来源

- `G9-01_资产兼容性审计与G9-02基础门禁_v1.0_2026-08-18.md`
- `G9阶段启动与G9-02实施切片裁定_v1.0_2026-08-18.md`
- `G9-02A_SourceBinding与GameLocalRevisionFoundation规格_v1.0_2026-08-18.md`
- `G9-02A_IndependentReview_SourceBinding与GameLocalRevision_v1.0_2026-08-19.md`
- `G9-02BC_SharedRuntimeFoundationConvergence规格_v1.0_2026-08-19.md`
- `17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`
- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `G8_StageUAT_最终收口与体验Backlog_v1.0_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.0.md`

Discussion-only, not implementation authority:
- `G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md`

---

## 2. G9-01 最终结论

```text
Semantic Asset Architecture       COMPATIBLE
Existing Assets Rewrite           NOT REQUIRED
Current Runtime Asset Foundation  PARTIAL
G9-02 Foundation                  REQUIRED
G9-03 Schema Freeze               BLOCKED BY G9-02
```

继续保留：

- World Pack / Character Card / Expansion 分权；
- Source Definition != Game-local Instance != Runtime State；
- Program-owned RNG / Judge / Formal Outcome / Atomic Commit / Save；
- Open Attempt；
- Canonical Owner / typed dependency / handoff / contribution；
- Package / Feature / Module activation semantics；
- Runtime Context Contract 18 项语义；
- UI Surface Owner / Contributor 语义。

G9 不根据现有 Markdown frontmatter 猜 final machine schema。

---

## 3. G9-02A 已完成能力

已正式证明：

```text
Source Asset Descriptor / Snapshot Identity
↓ bind
Game-local Canonical Identity + Lineage
↓ typed definition mutation
Game-local Definition Revision
↓ Runtime / Product projection
↓ Save / Restore / Branch / Recovery
```

关键 reference case：

```text
已有“重要信件”
→ 玩家检查并发现长期公开细节
→ Semantic AI 提出 typed mutation need
→ bounded Asset Mutator author candidate
→ Program validation / atomic local revision
→ Narrative / Product 读取 committed canonical value
→ Save / Restore / Branch / Recovery 保持一致
→ Source Asset 不变
```

Independent Review 结论：

```text
Implementation Review             PASS
P0                                0
P1                                0
Source immutability               PASS
Two-game isolation                PASS
Atomic revision                   PASS
Save / Restore / Branch           PASS
Recovery exactly-once             PASS
Hidden disclosure                 PASS
Ordinary-turn mutation call       0 when no semantic need
```

非阻塞历史迁移说明：pre-G9 数据没有 exact `createdRevision`，0011 migration 只能使用既有 `createdTurn` 作为历史回填种子；future consumer 不得将该 legacy 回填值视为可证明的历史 Event revision。

---

## 4. 当前 G9-02BC Shared Runtime Foundation

这是 G9-02B / G9-02C 共同依赖的 cross-slice shared foundation，**不新增生命周期阶段**。

当前必须 production-proof：

```text
Program-owned Built-in Domain Module Registry / Host
↓
Game-local Package / Feature / Module Binding + Activation
↓
owner-scoped Canonical Record / Runtime State extension seam
↓
typed Candidate / Change / Event / Handoff / Projection seam
↓
Routing Directory boundary
↓
validated selection
↓
JIT Context Projection Host
↓
bounded owner-preserving context join boundary
```

核心不变量：

- Asset 只提供 data / identity / configuration，不能注入 JS / callback / eval / script / arbitrary query；
- G9-02A common identity / lineage / revision 是唯一 Game-local metadata seam，不另建第二套 local identity；
- Domain canonical record 与 Domain runtime state 分离；
- owner/module identity 必须显式保留；
- `Package Included != Feature Enabled != Module Enabled`；
- disabled module 对 candidate/change/router/projection/handoff 全部 fail closed；
- `Dependency Graph != Context Inclusion Graph`；
- Handoff 只传 bounded typed payload / refs，不共享 whole state；
- Routing Directory 只暴露选择模块所需最小 metadata；
- JIT projection 必须在 validated selection 后发生；
- Context Projection 是 read-only / bounded / owner-preserving；
- `Bounded != Starved`。

本轮只做 shared primitives + one production reference vertical，不扩完所有真实 Domain，不做完整 Router，不冻结 external schema。

---

## 5. G9-02B / 02C 后续 breadth

Shared Foundation Independent Review PASS 后，由 Grok Build 默认主导。

### G9-02B breadth

- package / feature / module registry breadth；
- concrete built-in domain modules against frozen Host；
- Player-known Entity / Character Directory；
- G8 UI Host contribution binding；
- persistence / migration / test breadth。

必须保持：

```text
Current Scene Visible Characters
!= Player-known Character Directory
```

People Surface 只消费长期 player-known safe projection；未认识 NPC 不泄露，角色离场不从目录消失。

### G9-02C breadth

- Model-first Router wiring；
- state-mandatory augmentation；
- JIT owner projections；
- bounded owner-preserving join；
- outcome-gated continuation；
- disabled fail-closed；
- deterministic background zero-model-call；
- large registry bounded projection；
- People Directory scale stress。

必须证明：

```text
Known Character Directory size ↑↑↑
ordinary unrelated Turn context ≈ bounded
```

---

## 6. G9-03 Freeze Gate

只有 G9-02 Shared Foundation + 02B breadth + 02C breadth + Integrated Closure 全部 PASS，才能冻结：

- source asset machine identity/version/hash；
- source → local binding wire；
- immutable/evolvable/private field policy；
- owner / namespace；
- dependency / conditional / handoff / contribution；
- package / feature / module activation；
- routing / context contract；
- UI declaration；
- materialization / mutation eligibility；
- runtime binding metadata；
- compatibility / migration metadata；
- Manifest / Bundle relationship。

禁止：

- 把 Player-known Character Directory 做成 Source Character Card 字段；
- 把 current visibility 与 long-lived acquaintance/knowledge lifecycle 合并；
- 提前冻结 People / Objective / Information UI taxonomy；
- 把 Discussion-only Prologue Scenario wire 提前塞进 external schema。

---

## 7. Agent 执行策略｜Sol 稀缺、Grok Build 常态化

正式治理：`G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`。

```text
GPT
= Product / Architecture Lead
+ Freshness / Decision Propagation
+ Canonical Spec / Task Packet
+ exact-SHA Independent Review

Sol
= scarce high-risk shared-foundation executor

Grok Build
= default bounded implementation executor after contract freeze
```

当前最后一次高价值 Sol 深任务已正式授权给：

> `G9-02BC Shared Runtime Foundation Convergence`

Sol 只负责冻结并 production-proof shared rails，不负责扩完所有 Domain / UI / Creator / Adapter。

Shared Foundation PASS 后，G9 后续默认 Grok Build 主导 implementation；Grok 可在冻结合同下承担后端、测试、Adapter/Compiler 与 Creator，但不得自行重定义 canonical owner、Save/Restore/Recovery、Context authority 或 external schema。

同一 repo 默认 serialized writer；Executor != Independent Reviewer。

---

## 8. 不可回滚 Authority

继续保持：

- Semantic AI judges open semantics；Program 不重复 NLP；
- Program Final Outcome authority；
- No Phantom Interactable；
- Player Agency / Open Attempt；
- World Materializer need-gated；
- Source 不被 Runtime 反写；
- Save / Restore / Branch；
- Crash / Recovery / Idempotency；
- private / public disclosure boundary；
- Runtime Relevant != Model Visible；
- Dependency Graph != Context Inclusion Graph；
- Bounded != Starved。

以及：

```text
Canonical Character Truth
!= Player-known Character Directory
!= Current Scene Visible Character Set
```

---

## 9. Owner UAT / Product Backlog 路由

```text
Item durable known-description evolution
→ G9-02A PASS / CLOSED

People persistent known-character directory
→ G9-02B breadth
→ G9-02C bounded-context proof

DeepSeek model selector
→ G10 Provider Expansion

Game delete lifecycle
→ G11 Alpha

carried Item description display polish
→ G11 Product polish

People / Information / Objective classification/search/filter/history organization
→ G11 Product information architecture maturity

full Objective / Task vertical
→ dedicated later vertical，最迟 G11 前
```

Opening Scenario 保持 Discussion Draft：玩家端入口与 Prologue Runtime 暂不开放；未来先做 Creator 最小 authoring prototype + 首轮真实创作复盘，再决定正式 Runtime / asset-spec。

---

## 10. 长期资料 Surface 原则

会长期累积的资料 Surface 不得通过“只保留当前可见项”规避规模问题。

```text
People
Information / Knowledge
Objective / Task
future long-lived dossier surfaces
```

G9 保留 stable identity、status/source/player-safe facet metadata 与正确 owner；G11 再实现 grouping / filter / sort / search / active-history separation / pagination 等交互。

---

## 11. 当前 Next

> **G9-02BC｜Shared Runtime Foundation Convergence implementation。**

Base implementation HEAD：`04603e1e4a3270e9f5740b5957cf545a2bd001d0`。

G9-03 当前 `NOT AUTHORIZED`。
