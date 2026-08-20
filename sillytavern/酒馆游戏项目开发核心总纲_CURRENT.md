---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-20
current_path: sillytavern/酒馆游戏项目开发核心总纲_CURRENT.md
---

# 酒馆游戏项目开发核心总纲｜CURRENT

## 0. 当前状态

```text
G1–G8                         PASS / CLOSED
G9-01                         PASS / CLOSED
G9-02A                        PASS / CLOSED
G9-02BC                       PASS / CLOSED
G9-02B                        PASS / CLOSED
G9-02C Core                   PASS / CLOSED
G9-02C Breadth                PASS / CLOSED
G9-02 Integrated Closure      PASS / CLOSED
G9-02                          PASS / CLOSED
G9-03                          AUTHORIZED / NEXT
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
ab09c7ce6960a99b062d22fd49c143f9ae876f4e
```

当前核心路线：`酒馆游戏新版主体重建总路线 v2.3.md`。

当前 Next：

> **G9-03 Unified Asset / Reference Protocol。** G9-02 Runtime foundation 已正式关闭；下一步由 GPT 先冻结世界包 / 角色卡 / 拓展包 / 资料库的统一 machine contract 语义，再交给实现 Agent。G9-03 开始前仍需重新执行 Freshness Preflight。

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
- `G9-02C_IndependentReview_Core最终收口_v1.0_2026-08-19.md`
- `G9-02C_Breadth_IndependentReview_最终收口_v1.0_2026-08-19.md`
- `G9-02_IntegratedClosure_IndependentReview_最终收口_v1.0_2026-08-20.md`

### Asset / Library / Creator

- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.2_2026-08-19.md`
- `18A_酒馆游戏_资料库资源层协议护栏增量裁定_v1.0_2026-08-19.md`
- `19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md`
- `酒馆游戏新版主体重建总路线 v2.3.md`

### Execution Governance

- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`
- `AgentTaskPacket_GitHub原生交付增量裁定_v1.0_2026-08-20.md`

Discussion / deferred optional：

- `G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md`
- `Creator_AI后续可选增量备忘录_v1.0_2026-08-19.md`

---

## 2. 不可回滚 Runtime Authority

```text
Source Asset
!= Game-local Canonical Instance
!= Runtime State
```

```text
Model authors / proposes
Program / Domain Owner commits reality
```

```text
Dependency Graph
!= Context Inclusion Graph
```

```text
Player-known
!= Runtime Relevant
!= Model Visible
```

```text
Bounded
!= Starved
```

继续保持 Program Final Outcome、Player Agency / Open Attempt、No Phantom、private/public disclosure、Save / Restore / Branch、Crash / Resume / Recovery / exactly-once、need-gated Materialization 与 Source 不被 Runtime 反写。

G9-03 不得建立第二套 Game-local identity、Runtime state、Router、mutation 或 persistence authority。

---

## 3. G9-02A｜PASS / CLOSED

Final code：`04603e1e4a3270e9f5740b5957cf545a2bd001d0`。

已证明：

```text
Source Asset
→ per-game binding + lineage
→ Game-local typed definition mutation
→ definition revision
→ Product canonical projection
→ Save / Restore / Branch / Recovery
```

G9-03 必须复用这套 stable identity / version / lineage / revision，不建立第二套 local identity authority。

---

## 4. G9-02BC｜PASS / CLOSED

Final code：`5962e6f5933f245693e090cbdfd2f79791820ef1`。

已建立：

```text
Program-built Domain Module Host
→ Package / Feature / Module activation
→ owner-scoped Canonical Record / Runtime State
→ typed Candidate / Change / Event / Handoff
→ selected-only JIT Projection
→ bounded owner-preserving Context
```

Disabled fail-closed、hard dependency 不递归扩大 Context、Save / Restore / Branch / Recovery 与 migration rollback 已验证。

---

## 5. G9-02B｜PASS / CLOSED

Final code：`0ee847e1173ae8d17e643d5b838d238cf889031e`。

正式冻结：

```text
Canonical Character Truth
!= Player-known Character Directory
!= Current Scene Visible Character Set
```

People Surface 使用长期 player-known / last-known safe projection；未认识人物不泄露，角色离场不删除 membership，off-scene 不读取实时关系或实时角色状态。

---

## 6. G9-02C Core｜PASS / CLOSED

Core final code：`182740801b48c2edc2399e4e4dd8b6ae5a43ccaa`。

正式轨道：

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
authorized typed Domain Candidate
↓
existing Formal Turn authority
```

核心 authority：Router = Context Selection，不是 Player Authorization；Program 不用关键词复制 NLP；routing capacity 不把合法 Core Attempt 判非法；Recovery 不重复 model authoring。

---

## 7. G9-02C Breadth｜PASS / CLOSED

Reviewed code：`8a481ef16737e2c36310668b61b40e29b82ee1f7`。

Breadth final evidence / former integrated main：`81bdbb7b321e796d8d623989a8eb1e10a0c11bee`。

已证明：

```text
1,000 enabled leaves
→ bounded Package → Feature → Module routing

1,000 Player-known entries
→ unrelated dossier load = 0

10,000 relationship edges
→ deterministic player-safe bounded subgraph

4-owner join
→ selected-only / owner-preserving / < 4,000 chars

100 deterministic background turns
→ Router = 0 / Candidate model = 0

100-turn long session
→ context size stable
→ Save / Restore / Branch / Recovery exactly-once
```

Real Provider Gate：

```text
model                      deepseek-v4-pro
enabledLeaves              1000
providerCalls              3
maxProfilesPerRequest      10 / limit 16
maxSerializedChars/request 4601 / limit 8000
selectedModule             builtin:smoke.inventory-state.v1
verdict                    PASS
```

---

## 8. G9-02 Integrated Closure｜PASS / CLOSED

Independent Review：`G9-02_IntegratedClosure_IndependentReview_最终收口_v1.0_2026-08-20.md`。

```text
Formal Code Base
81bdbb7b321e796d8d623989a8eb1e10a0c11bee

Task Packet
f415659361a640fc6eb98d2c61c73e25bccf6853

Tested composition implementation
c705ee240da70a77e804cca49821162c573f9bad

Final Evidence / integrated main
ab09c7ce6960a99b062d22fd49c143f9ae876f4e
```

Review 结论：

```text
P0 = 0
P1 = 0
AC-01～AC-12 = PASS
Runtime production implementation changed = NO
```

组合闭环在同一局、同一 Store、同一 Host、同一 Formal Turn / Orchestrator 上证明：

```text
Source identity / lineage
+
Game-local definition / revision
+
Domain canonical + runtime ownership
+
Player-known / disclosure
+
Model-first selected-only routing / context
+
Authorized Candidate / provenance
+
Save / Restore / Branch / Recovery
↓
ONE coherent Runtime foundation
```

额外确认：

- routing miss 不改写合法 Player Attempt；
- `state_mandatory` 与 `authoritative_continuation` provenance 都有现行自动回归证据；
- hidden character / relation / private runtime marker 不泄漏；
- deterministic wait 不调用 Router / Domain Candidate model；
- `semantic_ready` recovery 不重复 model authoring；
- G5/G6/G7/G8/G9 与 full suite 全部通过；
- Closure 没有 `src` drift，因此合法继承已审核的真实 Provider evidence；
- 没有进入 G9-03 / Creator / Library Runtime。

因此：

```text
G9-02 PASS / CLOSED
```

---

## 9. #18 / #18A｜资料库资源层

```text
三类主资产
= 世界包 + 角色卡 + 拓展包

资料库
= 可复用 / 可绑定 / 可检索的资料资源层
!= 第四类主资产
!= Runtime Truth
```

时序：

```text
G9-03
→ 三类主资产 + 资料库协议同批冻结

G9-04
→ 三类主资产完整 Adapter / Compiler / Binding
+ 资料库最小协议 proof

G9-05
→ 三类主资产 Creator 首轮闭环

主资产端到端 PASS
→ 资料库产品功能后置增量
```

继续冻结：`Library Source Update != Existing Game Silent Update`；Retrieved Library Slice 只是 Reference Projection，不是 Game-local / Runtime Truth。

---

## 10. #19｜Creator Conversational Authoring

G9-05 首版产品定义保持：

```text
Structured Creator Workspace
+
Conversational AI Authoring
```

AI 通过 Typed Creator Tools 受控修改 Draft；Chat != Draft；Draft != Saved Source Asset；Save / Publish 由用户显式触发并经过 deterministic Validator。

跨多个资产的大规模自主操作、长时间自主 Agent、自动联网研究继续 `DEFERRED OPTIONAL`。

---

## 11. G9-03 Gate｜AUTHORIZED / NEXT

```text
G9-02 Integrated Closure PASS
↓
G9-02 PASS / CLOSED
↓
G9-03 AUTHORIZED / NEXT
```

G9-03 下一步统一冻结：

- 世界包；
- 角色卡；
- 拓展包；
- 资料库 Reference Resource Layer；

的 machine-readable identity / version / refs / dependency / contribution / routing / binding / compatibility / migration contract。

必须继续保持：

```text
Asset Contract
!= Creator Tool Contract
!= Conversation Protocol
```

Source Asset schema 不得包含 chat history、prompt、model provider、AI action history 或 Creator tool transport。

G9-03 开始正式语义设计前必须重新执行 Freshness Preflight。
