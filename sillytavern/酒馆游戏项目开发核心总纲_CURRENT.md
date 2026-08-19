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

G9                             ACTIVE / G9-02C
G9-01                          PASS / CLOSED
G9-02A                         PASS / CLOSED
G9-02A final SHA               04603e1e4a3270e9f5740b5957cf545a2bd001d0
G9-02BC                        PASS / CLOSED
G9-02BC final SHA              5962e6f5933f245693e090cbdfd2f79791820ef1
G9-02B                         PASS / CLOSED
G9-02B final SHA               0ee847e1173ae8d17e643d5b838d238cf889031e
G9-02C Core Design             FROZEN / IMPLEMENTATION NEXT
G9-02C Breadth                 BLOCKED BY CORE REVIEW
G9-02 Integrated Closure       BLOCKED BY G9-02C
G9-03                          NOT AUTHORIZED

Current Next
G9-02C Context Orchestration Core implementation
```

当前实现基线：

```text
zhangchenjia21-dot/sillytavern main
0ee847e1173ae8d17e643d5b838d238cf889031e
```

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

### Asset / Library / Creator

- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.2_2026-08-19.md`
- `18A_酒馆游戏_资料库资源层协议护栏增量裁定_v1.0_2026-08-19.md`
- `19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md`
- `酒馆游戏新版主体重建总路线 v2.3.md`

### Execution Governance

- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`

Discussion / deferred optional，不是当前实现 Authority：

- `G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md`
- `Creator_AI后续可选增量备忘录_v1.0_2026-08-19.md`

---

## 2. 不可回滚 Runtime Authority

继续冻结：

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
Runtime Relevant
!= Model Visible
```

```text
Bounded
!= Starved
```

以及：

- Program Final Outcome authority；
- Player Agency / Open Attempt；
- No Phantom Interactable；
- private / public disclosure boundary；
- Save / Restore / Branch；
- Crash / Resume / Recovery / exactly-once；
- Materialization need-gated；
- Source 不被 Runtime 反写。

---

## 3. G9-02A｜PASS / CLOSED

Final code：`04603e1e4a3270e9f5740b5957cf545a2bd001d0`。

已证明：

```text
Source Asset
→ per-game bind + lineage
→ Game-local typed definition mutation
→ definition revision
→ Product canonical projection
→ Save / Restore / Branch / Recovery
```

G9-03 后续必须复用这一套 stable identity / version / lineage / revision 语义，不建立第二套 local identity authority。

---

## 4. G9-02BC｜PASS / CLOSED

Final code：`5962e6f5933f245693e090cbdfd2f79791820ef1`。

正式轨道：

```text
Program-built Domain Module Host
→ Package / Feature / Module activation
→ owner-scoped Canonical Record / Runtime State
→ typed Candidate / Change / Event / Handoff
→ minimal Routing Directory
→ validated selection
→ selected-only JIT Projection
→ bounded owner-preserving Context
```

已证明 disabled fail-closed、hard dependency 不递归扩大 Context、100-module selected-only projection、Save / Restore / Branch / Recovery 与 migration rollback。

---

## 5. G9-02B｜PASS / CLOSED

Final code：`0ee847e1173ae8d17e643d5b838d238cf889031e`。

正式冻结：

```text
Canonical Character Truth
!= Player-known Character Directory
!= Current Scene Visible Character Set
```

以及：

```text
Player-known
!= Runtime Relevant
!= Model Visible
```

People Surface 使用长期 player-known / last-known safe projection；未认识人物不泄露，角色离场不删除 membership，off-scene 不读取实时关系或角色状态。

---

## 6. G9-02C｜ACTIVE / NEXT

Core Authority：`G9-02C_ModelFirstRouting与ContextOrchestration核心规格_v1.0_2026-08-19.md`。

正式目标：

```text
已启用 Package / Feature / Module
↓
Program 构建 bounded Routing Working Set
↓
Model-first immediate routing
↓
Program structural validation
+ state-mandatory augmentation
+ authoritative continuation activation
↓
provenance-aware Module Selection
↓
Authorized Turn / Context Anchors
↓
selected-only JIT Projection
↓
owner-preserving bounded Context
↓
typed Domain Candidate
↓
existing Formal Turn authority
```

重点 Gate：

- Router = Context Selection，不是行为白名单；
- Model-first，不用 Program 关键词复制 NLP；
- enabled modules 增长不让 Router 输入无界增长；
- selection provenance：`model_immediate / state_mandatory / authoritative_continuation`；
- Domain path 不获得新的玩家授权权；
- People 目录增长不扩大无关 Turn Context；
- Large Relation Graph 只投影 current-relevant subgraph；
- outcome-gated continuation；
- deterministic background zero-model-call；
- recovery / replay exactly-once。

#18 / #18A / #19 均不回开 02C；资料库导入 / 检索和 Creator AI 不进入当前 02C 实现。

---

## 7. #18 / #18A｜资料库资源层

正式资产架构：

```text
三类主资产
= 世界包 + 角色卡 + 拓展包

资料库
= 可复用 / 可绑定 / 可检索的资料资源层
!= 第四类主资产
!= 第二世界包
!= Runtime Truth
```

时序：

```text
G9-03
→ 三类主资产 + 资料库协议同批冻结

G9-04
→ 三类主资产完整 Adapter / Compiler / Binding
+ 资料库协议最小 parse / validate / round-trip proof

G9-05
→ 先完成三类主资产 Creator

主资产完整游玩闭环 PASS
↓
资料库产品功能增量
```

额外护栏：

```text
Library Source Update
!= Existing Game Silent Update
```

```text
Retrieved Library Slice
= Reference Projection
!= Game-local Truth
!= Runtime Truth
```

```text
Creator Reference
!= Model Worldbuilding Reference
!= Player-visible Knowledge
!= Character-known Knowledge
```

G9-03 冻结 identity / version / integrity / provenance / binding / audience 挂点，但不提前冻结 embedding、chunk size、向量维度、排名算法、任意查询 DSL 或具体 RAG Provider API。

---

## 8. #19｜Creator Conversational Authoring

新版 Creator 正式产品定义：

```text
Structured Creator Workspace
+
Conversational AI Authoring
```

不是：

```text
traditional form
+ a few AI buttons
```

### 双工作面

```text
主工作区 / 设定区
= 当前结构化 Creator Draft

AI 创作对话区
= 需求理解 / 最少必要追问 / 解释 / 受控编辑入口
```

正式 Authority：

```text
AI Chat
!= Creator Draft

Creator Draft
!= Saved Source Asset

AI can edit Draft
!= AI can Save / Publish Source Asset
```

AI 必须通过 Program-owned Typed Creator Tools 修改 Draft，不直接操作 DOM，不获得 arbitrary state path / query / script 权限。

AI 可在用户明确任务范围内进行任务级修改；修改必须即时反映到主工作区，并提供变更摘要与任务级 Undo。

确定性 Validator 继续拥有正式合法性判断；AI Review 只提供语义建议、完整性检查和 Validator 解释。

无 API / Provider unavailable 时：

```text
full manual Creator remains usable
```

打开、手工编辑、Validator、保存、导入导出不得自动调用模型。

---

## 9. G9-03｜统一资产 / 资料资源协议

只有 G9-02C + Integrated Closure PASS 后授权。

必须统一冻结世界包、角色卡、拓展包与资料库的 machine identity / version / refs / ownership / binding / compatibility / Bundle / Manifest 等长期协议。

#19 对 G9-03 的新增约束：

```text
Asset Contract
!= Creator Tool Contract
!= Conversation Protocol
```

资产字段与 stable refs 必须足够可靠，让 Creator Tool 可以 typed edit；但 Source Asset schema 不包含：

- prompt；
- chat history；
- model provider；
- AI action history；
- Creator tool transport。

---

## 10. G9-04｜AI-independent Adapter / Compiler / Binding

三类主资产：

```text
Source
→ parse / validate
→ adapter / compiler
→ unified asset contract
→ Game-local Binding
```

必须证明：

```text
Manual Creator
and
AI-assisted Creator
→ same valid Source Asset contract
```

Parser / Compiler 不依赖模型。

资料库只做最小协议 parse / validate / round-trip / cross-reference proof，不上线资料库产品功能。

---

## 11. G9-05｜Conversational Authoring Creator｜REQUIRED

G9-05 首版必须交付：

1. 世界包 / 角色卡 / 拓展包结构化主工作区；
2. 持续 AI 创作对话区；
3. bounded current asset / section / focus context；
4. 自然语言创作请求；
5. 最少必要追问；
6. Typed Creator Tool / Patch Contract；
7. AI 受控修改 Draft；
8. 主工作区即时同步；
9. 任务级变更摘要 / Undo；
10. deterministic Validator；
11. AI Review / Validator explanation；
12. 用户显式 Save / Publish；
13. 无 Provider 时完整手工创作；
14. 导入 / 导出 / 返回“我的资产库”。

### 首个真实纵向

优先世界包：

```text
用户自然描述世界
→ AI 少量关键追问
→ AI 通过 Creator Tools 填充结构化 Draft
→ 主工作区显示
→ 用户手工 / 对话继续迭代
→ Validator
→ Undo
→ 用户保存
→ 导入 / 导出 / 再打开
```

角色卡与拓展包复用同一 Creator rails；G9-05 关闭前，三类主资产都必须完成基础创作链。

---

## 12. Creator AI 高级能力｜DEFERRED OPTIONAL

以下不是当前目标，也不是 Roadmap 承诺：

```text
跨多个资产的大规模自主操作
长时间自主 Agent
自动联网研究
```

来源：`Creator_AI后续可选增量备忘录_v1.0_2026-08-19.md`。

只有真实 Creator UAT 后出现明确需求，才重新进入 Product Definition / Architecture Gate。

当前不得因此预造：

- autonomous-agent runtime；
- 跨资产万能事务；
- 长任务调度器；
- 通用联网抓取平台；
- speculative vector/RAG platform；
- 相关 external schema。

---

## 13. Owner UAT / Product Backlog 路由

```text
Item durable known-description evolution
→ G9-02A PASS / CLOSED

People persistent known-character directory
→ G9-02B PASS / CLOSED
→ G9-02C bounded-context / scale proof

三类主资产 + 资料库统一机器协议
→ G9-03

三类主资产 Adapter / Compiler / Game-local Binding
→ G9-04

资料库协议最小 parse / validate / round-trip proof
→ G9-04
→ 不等于资料库产品功能上线

Conversational Authoring Creator
→ G9-05 REQUIRED

“我的资产库 → 创建游戏 → 完整游玩”三类主资产纵向闭环
→ 必须早于资料库产品功能开发

资料库 Creator / Runtime retrieval
→ Primary Asset End-to-End Closure Gate 后的增量阶段

Creator 高级 autonomous 能力
→ DEFERRED OPTIONAL
→ only reconsider after real Creator UAT

DeepSeek model selector
→ G10 Provider Expansion

Game delete lifecycle
→ G11 Alpha

People / Information / Objective information architecture maturity
→ G11
```

Opening Scenario 保持 Discussion Draft；玩家端 Prologue Runtime 当前仍延后。

---

## 14. 当前 Next

> **G9-02C Context Orchestration Core implementation。**

Implementation Base：`0ee847e1173ae8d17e643d5b838d238cf889031e`。

当前 02C Core Spec 已冻结；由 Sol 在隔离 worktree 中实现，完成后执行 exact-SHA Independent Review；PASS 后再开放其余 Grok Build breadth。

#18 / #18A / #19 不改变当前 G9-02C Task DAG。

G9-03 当前 `NOT AUTHORIZED`。