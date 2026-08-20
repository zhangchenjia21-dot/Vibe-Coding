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
G9-03 Semantics               PASS / FROZEN
G9-03 Implementation          PASS / CLOSED
G9-03                         PASS / CLOSED
G9-04                         AUTHORIZED / NEXT
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
5da2294a9d21585665167e69307d9c693427582d
```

当前核心路线：`酒馆游戏新版主体重建总路线 v2.3.md`。

当前 Next：

> **G9-04 Adapter / Compiler / Binding。** 使用已经冻结并实现的 `TavernAssetV1 / TavernBundleManifestV1 / TavernGameAssetManifestV1`，对现有 World / Character / Expansion canonical Markdown 做 AI-independent adapter / compiler，并把 exact snapshot manifest 接到现有 G9-02 Source Binding / Game-local rails；资料库在本阶段仅做最小 parse / validate / round-trip / cross-reference proof，不实现 Runtime retrieval 或产品 UI。

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
- `G9-03A_RuntimeModuleBinding与TypedConfig增量裁定_v1.0_2026-08-20.md`
- `G9-03_IndependentReview_最终收口_v1.0_2026-08-20.md`

### Asset / Library / Creator

- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.2_2026-08-19.md`
- `18A_酒馆游戏_资料库资源层协议护栏增量裁定_v1.0_2026-08-19.md`
- `19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md`
- `酒馆游戏新版主体重建总路线 v2.3.md`

最新资产事实源：

```text
zhangchenjia21-dot/sillytavern-assets main
d2dc31c1b80e514285021a6ed1e37f0c9e733f3a
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

## 2. 不可回滚 Runtime / Asset Authority

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

G9-03 / G9-04 不得建立第二套 Game-local identity、Runtime state、Router、mutation 或 persistence authority。

---

## 3. G9-02｜PASS / CLOSED

Final integrated code before G9-03：

```text
ab09c7ce6960a99b062d22fd49c143f9ae876f4e
```

组成：

```text
G9-02A Source Binding / Game-local Revision       PASS / CLOSED
G9-02BC Shared Runtime Foundation                 PASS / CLOSED
G9-02B Player-known / Domain Breadth              PASS / CLOSED
G9-02C Core                                        PASS / CLOSED
G9-02C Breadth                                     PASS / CLOSED
G9-02 Integrated Closure                           PASS / CLOSED
```

长期 rails：

```text
Source Asset Descriptor
→ per-game binding / lineage
→ Game-local Canonical Instance / definitionRevision
→ Domain Module Host / owner-scoped records + state
→ Model-first bounded routing / selected-only projection
→ Formal Turn / Program Outcome authority
→ Save / Restore / Branch / Recovery exactly-once
```

G9-03 及以后全部复用这套 rails。

---

## 4. G9-03｜PASS / CLOSED

正式规格：

- `G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md`
- `G9-03A_RuntimeModuleBinding与TypedConfig增量裁定_v1.0_2026-08-20.md`
- `G9-03_IndependentReview_最终收口_v1.0_2026-08-20.md`

Final implementation / integrated main：

```text
5da2294a9d21585665167e69307d9c693427582d
```

Final Review：

```text
P0 = 0
P1 = 0
G9-03 = PASS / CLOSED
```

### 4.1 Frozen Source protocol

```text
TavernAssetV1
= one Source Envelope
+ world | character | expansion | library typed payload
```

Source Snapshot identity：

```text
assetRef
+ assetType
+ version
+ sha256 contentHash
```

该 identity 直接映射既有 G9-02：

```text
SourceAssetDescriptor.stableRef
SourceAssetDescriptor.assetType
SourceAssetDescriptor.version
SourceAssetDescriptor.contentHash
```

不建立第二套 Runtime Source identity。

### 4.2 Frozen protocol capabilities

已实现并验证：

- deterministic canonical JSON + SHA-256；
- same `assetRef + version` conflicting digest fail closed；
- Hard / Optional / Feature-conditional / Reference dependency；
- hard dependency cycle fail closed；
- World default / recommended / optional composition；
- Expansion Package / Feature / Source Module declarations；
- Source `runtimeModuleRef` → Program `RuntimeDomainModuleDescriptor.moduleRef`；
- resolved active binding → existing `RuntimeDomainModuleBinding`；
- exact Program-owned Typed Config validator gate；
- minimal routing profile / projection / handoff seams；
- G8 Host-safe UI capability validation；
- Library stable entry / provenance / four-audience eligibility；
- `TavernBundleManifestV1`；
- `TavernGameAssetManifestV1` exact snapshot pinning；
- Source v2 publication does not silently update an existing v1 Game Manifest；
- no executable Source fields / arbitrary query DSL / Creator transport。

### 4.3 G9-03A identity clarification

永久区分：

```text
ExpansionModuleV1.moduleRef
= Source declaration identity

ExpansionModuleV1.runtimeModuleRef
= Program Runtime module identity

RuntimeDomainModuleBinding.moduleRef
= runtimeModuleRef
```

Source `ownerNamespace` 不覆盖 Program descriptor owner。

同一 resolved active set 中两个 Source modules 指向同一 `runtimeModuleRef` 必须 fail closed。

### 4.4 Final correction evidence

```text
g9:03:test              36 / 36 PASS
G9-02 closure            2 / 2 PASS
G5                       207 / 207 PASS
G6                        17 / 17 PASS
G7                        20 / 20 PASS
G8                       208 / 208 PASS
full npm test            789 / 789 PASS
typecheck                PASS
lint                     PASS
product:build            PASS
launcher:smoke           PASS
g2:disclosure            PASS
git diff --check         PASS
```

Correction did not modify G9-02 / G8 production contracts or `tavern.asset.v1` wire fields.

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

继续冻结：

```text
Library Source Update
!= Existing Game Silent Update

Retrieved Library Slice
= Reference Projection
!= Game-local Truth
!= Runtime State
```

G9-03 已冻结并实现 Library identity / version / hash / stable entry / provenance / four-audience eligibility / Bundle / exact Game Manifest binding。

G9-04 对 Library 仅做最小协议 proof；完整检索产品继续后置。

---

## 6. #19｜Creator Conversational Authoring

G9-05 首版产品定义保持：

```text
Structured Creator Workspace
+
Conversational AI Authoring
```

AI 通过 Typed Creator Tools 受控修改 Draft；Chat != Draft；Draft != Saved Source Asset；Save / Publish 由用户显式触发并经过 deterministic Validator。

Source Asset schema 不包含 chat history、prompt、provider、AI action log 或 Creator tool transport。

---

## 7. G9-04｜AUTHORIZED / NEXT

G9-04 的唯一主 Outcome：

> **让当前 canonical World Pack / Character Card / Expansion Pack 通过 AI-independent Adapter / Compiler 转换成已冻结的 `TavernAssetV1`，再由 exact `TavernGameAssetManifestV1` 接到既有 G9-02 Game-local Source Binding；同时对 Library 做最小 parse / validate / round-trip / cross-reference proof。**

正式链：

```text
current canonical Markdown Source
↓ deterministic adapter / parser
TavernAssetV1
↓ validate / canonicalize / hash
resolved TavernGameAssetManifestV1
↓ compiler / binding
existing SourceAssetDescriptor
↓
G9-02 Game-local Binding / lineage / revision
```

### 7.1 三类主资产必须证明

- World Pack current sample 可转换；
- Character Card current sample 可转换；
- Expansion current sample 可转换；
- stable identities / versions / ownership / dependencies 不因 adapter 丢失；
- Expansion Feature / Module / runtimeModuleRef mapping 使用 G9-03A；
- exact snapshot manifest 可进入 G9-02 Source binding；
- source update 不静默改变已绑定 Game；
- parse / compile 全流程 AI-independent。

### 7.2 Library 最小 proof

允许：

```text
Library sample
→ parse
→ validate
→ canonical serialize / round-trip
→ cross-reference validation
```

禁止借 G9-04 提前实现：

- Library product UI；
- Runtime indexing / retrieval；
- model Reference Provider；
- Creator Library editor。

### 7.3 Non-scope

G9-04 不重新设计 `tavern.asset.v1`；如真实 current asset 暴露 protocol blocker，必须回到 GPT architecture review，不允许 adapter 私自发明第二 wire。

---

## 8. 当前 DAG

```text
G9-02                         PASS / CLOSED
↓
G9-03 Unified Asset / Reference Protocol
                              PASS / CLOSED
↓
G9-04 Adapter / Compiler / Binding
                              AUTHORIZED / NEXT
↓
G9-05 Structured Creator Workspace + Conversational AI
↓
Primary Asset End-to-End Closure
↓
Library Product Increment
```
