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
G9-01                          PASS / CLOSED
G9-02A                         PASS / CLOSED
G9-02BC                        PASS / CLOSED
G9-02B                         PASS / CLOSED
G9-02C Core                    PASS / CLOSED
G9-02C Core final SHA          182740801b48c2edc2399e4e4dd8b6ae5a43ccaa
G9-02C Breadth                 ACTIVE / NEXT
G9-02 Integrated Closure       BLOCKED BY G9-02C BREADTH
G9-03                          NOT AUTHORIZED

Current Next
G9-02C Breadth｜Grok Build
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
182740801b48c2edc2399e4e4dd8b6ae5a43ccaa
```

Core Final Review：

`G9-02C_IndependentReview_Core最终收口_v1.0_2026-08-19.md`

当前核心路线：`酒馆游戏新版主体重建总路线 v2.3.md`。

---

## 1. 当前正式 Authority

### Runtime / G9

- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`
- `17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`
- `G9-02A_SourceBinding与GameLocalRevisionFoundation规格_v1.0_2026-08-18.md`
- `G9-02A_IndependentReview_SourceBinding与GameLocalRevision_v1.0_2026-08-19.md`
- `G9-02BC_SharedRuntimeFoundationConvergence规格_v1.0_2026-08-19.md`
- `G9-02BC_IndependentReview_共享运行时基础_v1.0_2026-08-19.md`
- `G9-02B_RuntimeDomainBreadth与PlayerKnownDirectory规格_v1.0_2026-08-19.md`
- `G9-02B_IndependentReview_PlayerKnownDirectory最终收口_v1.0_2026-08-19.md`
- `G9-02C_ModelFirstRouting与ContextOrchestration核心规格_v1.0_2026-08-19.md`
- `G9-02C_Agent资源分配增量裁定_v1.0_2026-08-19.md`
- `G9-02C_IndependentReview_Core最终收口_v1.0_2026-08-19.md`

### Asset / Library / Creator

- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.2_2026-08-19.md`
- `18A_酒馆游戏_资料库资源层协议护栏增量裁定_v1.0_2026-08-19.md`
- `19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md`
- `酒馆游戏新版主体重建总路线 v2.3.md`

### Execution Governance

- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`

Discussion / deferred optional：

- `G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md`
- `Creator_AI后续可选增量备忘录_v1.0_2026-08-19.md`

---

## 2. 不可回滚 Runtime Authority

```text
Source Asset
!= Game-local Canonical Instance
!= Runtime State

Model authors / proposes
Program / Domain Owner commits reality

Dependency Graph
!= Context Inclusion Graph

Runtime Relevant
!= Model Visible

Bounded
!= Starved
```

继续保持 Program Final Outcome、Player Agency / Open Attempt、No Phantom、private/public disclosure、Save / Restore / Branch、Crash / Resume / Recovery / exactly-once、need-gated Materialization、Source immutable 等既有 authority。

---

## 3. G9-02C Core｜PASS / CLOSED

Final code：

```text
182740801b48c2edc2399e4e4dd8b6ae5a43ccaa
```

正式收口：

```text
Enabled Package / Feature / Module
↓
Program-owned bounded Routing Catalog
↓
Model-first Package → Feature → Module refinement
↓
Program structural validation
+ state_mandatory
+ authoritative_continuation
↓
provenance-bearing selection
↓
Authorized Turn Anchors
↓
selected-only JIT Projection
↓
owner-preserving bounded Context
↓
authorized typed Candidate
↓
existing Formal Turn authority
```

核心已证明：

- 每次 Routing Working Set 有 profile-count + serialized-size 双上限；
- 大 fan-out 使用 Program pagination；
- Router global call bound 达到且仍有未访问页时显式 `capacity_exhausted`，不伪装普通 miss；
- Package / Feature group 使用 Program-owned bounded semantic descriptor，parent profile 不再随 child count 线性增长；
- Router = Context Selection，不获得 Player Authorization authority；
- `model_immediate / state_mandatory / authoritative_continuation` provenance；
- Candidate 回显 exact Authorized Turn + refs，并由 owner module 再验证；
- selected-only JIT，Hard Dependency 不递归进入 Context；
- 1,000 Player-known unrelated Turn 不输出 dossiers；
- targeted known character 输出 bounded last-known dossier；
- Large Relation Graph 使用 deterministic bounded relevant subgraph；
- relationship traversal 受 Player-known/current-visible eligibility 限制，不泄漏未认识人物；
- deterministic background = zero Router / zero Domain Candidate model call；
- semantic-ready Recovery 复用 durable Domain plan/orchestration，不重复 authoring；
- one-domain Formal Change cardinality 未改变；
- 无 G9-03 / Creator / Library scope drift。

---

## 4. G9-02C Breadth｜ACTIVE / NEXT

Core PASS 后，Grok Build 沿 frozen rails 扩展：

### B1｜Routing Scale

- 更大 Package / Feature / Module registry；
- 多页 refinement / branch breadth；
- disabled / unknown / wrong-parent / duplicate / malformed / clarification；
- routing exhaustion metrics；
- group descriptor breadth 与 anti-starvation。

### B2｜Context Scale

- 1,000+ People；
- targeted dossier；
- large Relation Graph；
- large Definition Registry；
- cross-owner bounded join；
- Bounded != Starved。

### B3｜Continuation / Background

- outcome-gated continuation branch matrix；
- miss / disabled recipient no-load；
- deterministic background zero model call；
- Handoff activation / recipient projection。

### B4｜Integrated Regression

- G5–G9 regressions；
- long session；
- Save / Restore / Branch / Recovery；
- context-size / model-call-count instrumentation；
- production Catalog → real Model-first Router smoke（有凭据时）。

整个 G9-02C 仍未 CLOSED。Breadth + Integrated Closure + 至少一次可核验真实 Model-first routing evidence 完成后，才允许 G9-03。

---

## 5. #18 / #18A｜资料库资源层

```text
三类主资产
= 世界包 + 角色卡 + 拓展包

资料库
= 可复用 / 可绑定 / 可检索的资料资源层
!= 第四类主资产
!= Runtime Truth
```

时序继续：

```text
G9-03
→ 三类主资产 + 资料库协议同批冻结

G9-04
→ 三类主资产完整 Adapter / Compiler / Binding
+ 资料库最小 parse / validate / round-trip proof

G9-05
→ 三类主资产 Creator 首轮闭环

主资产端到端 PASS
→ 资料库产品功能后置增量
```

继续冻结：

```text
Library Source Update
!= Existing Game Silent Update

Retrieved Library Slice
= Reference Projection
!= Game-local Truth
!= Runtime Truth

Creator Reference
!= Model Worldbuilding Reference
!= Player-visible Knowledge
!= Character-known Knowledge
```

---

## 6. #19｜Creator Conversational Authoring

G9-05 首版产品定义保持：

```text
Structured Creator Workspace
+
Conversational AI Authoring
```

AI 通过 Typed Creator Tools 受控修改 Draft；Chat != Draft；Draft != Saved Source Asset；Save / Publish 仍由用户显式触发并经过 deterministic Validator。

无 Provider 时完整手工 Creator 必须继续可用。

跨多个资产的大规模自主操作、长时间自主 Agent、自动联网研究继续 `DEFERRED OPTIONAL`，不是当前 Roadmap 承诺。

---

## 7. G9-03 Gate

```text
G9-02C Breadth PASS
+
G9-02 Integrated Closure PASS
+
real Model-first routing evidence available
↓
G9-03 AUTHORIZED
```

当前仍：

```text
G9-03 NOT AUTHORIZED
```

G9-03 才统一冻结世界包 / 角色卡 / 拓展包 / 资料库 machine contract；不得提前把 Creator chat/tool transport 写入 Source Asset schema。

---

## 8. 当前 Next

> **G9-02C Breadth｜Grok Build。**

Core Review 已 PASS 并 fast-forward `main@182740801b48c2edc2399e4e4dd8b6ae5a43ccaa`。

下一任务只允许沿 Core frozen rails 扩 breadth / scale / regression / real-provider evidence；不得回开 Core authority，也不得进入 G9-03 external schema。