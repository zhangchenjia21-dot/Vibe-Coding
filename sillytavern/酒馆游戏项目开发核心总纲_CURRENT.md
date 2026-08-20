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
G9-02                         PASS / CLOSED
G9-03 Semantics               FROZEN / PASS
G9-03 Implementation          ACTIVE / NEXT
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
ab09c7ce6960a99b062d22fd49c143f9ae876f4e
```

当前核心路线：`酒馆游戏新版主体重建总路线 v2.3.md`。

当前 Next：

> **G9-03 Unified Asset / Reference Protocol Implementation。** v1 machine semantics 已由 `G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md` 正式冻结；下一步由 Grok Build 在不重开 G9-02 Runtime authority 的前提下实现 TypeScript contract、validator、canonical serializer/integrity、四类 payload、dependency、Bundle / Game Asset Manifest 与现有 Runtime/UI rails 的 mapping proof。

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
- `G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md`

### Asset / Library / Creator

- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.2_2026-08-19.md`
- `18A_酒馆游戏_资料库资源层协议护栏增量裁定_v1.0_2026-08-19.md`
- `19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md`
- `酒馆游戏新版主体重建总路线 v2.3.md`

最新资产事实源：

```text
zhangchenjia21-dot/sillytavern-assets main
bed1b4c93d84df2b83723ddeb3ff479203bb6f52
```

G9-03 已吸收 `tavern-asset v1.0`、EP-KINSHIP-CORE / EP-POLITICS-CORE 与 Large Relation Graph / deterministic subgraph projection 结论。

### Execution Governance

- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`
- `AgentTaskPacket_GitHub原生交付增量裁定_v1.0_2026-08-20.md`
- `Skill/main/skill/gpt/agent-task-packet/SKILL.md` v1.1

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

Breadth final evidence：`81bdbb7b321e796d8d623989a8eb1e10a0c11bee`。

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

Tested composition implementation
c705ee240da70a77e804cca49821162c573f9bad

Final Evidence / integrated main
ab09c7ce6960a99b062d22fd49c143f9ae876f4e
```

```text
P0 = 0
P1 = 0
AC-01～AC-12 = PASS
Runtime production implementation changed = NO
```

因此：`G9-02 PASS / CLOSED`。

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

继续冻结：

```text
Library Source Update
!= Existing Game Silent Update

Retrieved Library Slice
= Reference Projection
!= Game-local Truth
!= Runtime State
```

G9-03 v1 已把 Library identity / version / hash / stable entry / provenance / 4-audience eligibility / Bundle / exact Game Manifest binding 纳入同一 machine protocol；完整检索产品仍后置。

---

## 10. #19｜Creator Conversational Authoring

G9-05 首版产品定义保持：

```text
Structured Creator Workspace
+
Conversational AI Authoring
```

AI 通过 Typed Creator Tools 受控修改 Draft；Chat != Draft；Draft != Saved Source Asset；Save / Publish 由用户显式触发并经过 deterministic Validator。

G9-03 Source schema 明确不包含 chat history、prompt、provider、AI action log 或 Creator tool transport。

---

## 11. G9-03｜SEMANTICS FROZEN / IMPLEMENTATION NEXT

正式规格：

`G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md`

已冻结：

```text
TavernAssetV1
= one Source Envelope
+ world | character | expansion | library typed payload
```

以及：

- `assetRef / assetType / version / sha256` exact Source Snapshot identity；
- typed Hard / Optional / Feature-conditional / Reference dependency；
- World default / recommended / optional composition；
- Expansion Package / Feature / Module / built-in runtime module binding；
- minimal routing profile / projection / handoff / safe UI declaration seams；
- Library stable entry / provenance / 4-audience eligibility / retrieval hints；
- `TavernBundleManifestV1`；
- `TavernGameAssetManifestV1` exact resolved creation binding；
- explicit migration / no silent source update；
- deterministic canonical serialization / integrity；
- stable fail-closed validation error classes。

正式链：

```text
Authoring Source
→ TavernAssetV1
→ validate / canonicalize / hash
→ resolved TavernGameAssetManifestV1
→ existing G9-02 SourceAssetDescriptor
→ Game-local Binding / lineage / revision
→ Runtime
```

G9-03 implementation 当前必须实现协议与验证 proof；完整 Markdown adapter / compiler 留给 G9-04。

---

## 12. 当前 DAG

```text
G9-02 Integrated Closure      PASS / CLOSED
↓
G9-03 Semantics               FROZEN / PASS
↓
G9-03 Implementation          ACTIVE / NEXT
↓
GPT exact-SHA Independent Review
↓
G9-03 PASS / CLOSED
↓
G9-04 Adapter / Compiler / Binding
↓
G9-05 Structured Creator Workspace + Conversational AI
↓
Primary Asset End-to-End Closure
↓
Library Product Increment
```
