# SillyTavern 项目治理入口

本目录是 **SillyTavern 核心治理与项目事实源**。这里只保存当前有效的正式架构裁定、阶段规格、Independent Review、路线与长期治理；代码 Agent 的具体施工任务包放在实现仓库。

## 1. Rolling current

- `酒馆游戏项目开发核心总纲_CURRENT.md`：当前项目解释层。
- `酒馆游戏新版主体重建总路线 v2.3.md`：当前总路线。
- `G9阶段启动与G9-02实施切片裁定_v1.0_2026-08-18.md`：G9 基础阶段顺序。

当前阶段：

```text
G1–G8                     PASS / CLOSED
G9-01                      PASS / CLOSED
G9-02A                     PASS / CLOSED
G9-02BC Shared Foundation  PASS / CLOSED
G9-02B                     PASS / CLOSED
G9-02C Core                PASS / CLOSED
G9-02C Breadth             PASS / CLOSED
G9-02 Integrated Closure   ACTIVE / NEXT
G9-03                      NOT AUTHORIZED
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
81bdbb7b321e796d8d623989a8eb1e10a0c11bee
```

当前下一项：

> **G9-02 Integrated Closure｜对 02A / 02BC / 02B / 02C 的组合轨道做最终 Stage Gate，确认 Source→Local→Runtime、Domain ownership、Player-known、Model-first Context、Save/Restore/Branch/Recovery 与真实 Provider 证据在同一实现主线上无回滚。**

G9-03 在 Integrated Closure PASS 前继续 `NOT AUTHORIZED`。

---

## 2. 当前 G9 / Runtime Authority

- `G9-02A_SourceBinding与GameLocalRevisionFoundation规格_v1.0_2026-08-18.md`
- `G9-02A_IndependentReview_SourceBinding与GameLocalRevision_v1.0_2026-08-19.md`
- `G9-02BC_SharedRuntimeFoundationConvergence规格_v1.0_2026-08-19.md`
- `G9-02BC_IndependentReview_共享运行时基础_v1.0_2026-08-19.md`
- `G9-02B_RuntimeDomainBreadth与PlayerKnownDirectory规格_v1.0_2026-08-19.md`
- `G9-02B_IndependentReview_PlayerKnownDirectory最终收口_v1.0_2026-08-19.md`
- `G9-02C_ModelFirstRouting与ContextOrchestration核心规格_v1.0_2026-08-19.md`
- `G9-02C_Agent资源分配增量裁定_v1.0_2026-08-19.md`
- `G9-02C_IndependentReview_Core最终收口_v1.0_2026-08-19.md`
- `G9-02C_Breadth_IndependentReview_最终收口_v1.0_2026-08-19.md`

历史阻塞记录：

- `G9-02C_Breadth_IndependentReview_真实Provider证据阻塞_v1.0_2026-08-19.md`：已由最终 Breadth Review supersede；保留为 evidence-gate 历史。

仍有效的编号核心：

- `11_酒馆游戏_G5长期连续性与玩家授权收口裁定_v1.0_2026-08-15.md`
- `12_酒馆游戏_G6_SaveContinueRestore收口裁定_v1.0_2026-08-15.md`
- `13_酒馆游戏_G7_CrashResumeRecovery收口裁定_v1.0_2026-08-16.md`
- `14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`
- `17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`
- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.2_2026-08-19.md`
- `18A_酒馆游戏_资料库资源层协议护栏增量裁定_v1.0_2026-08-19.md`
- `19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md`

---

## 3. G9-02C 最终冻结结果

```text
Enabled Package / Feature / Module
↓
bounded Program-owned Routing Catalog
↓
Model-first Package → Feature → Module refinement
↓
Program structural validation
+ state_mandatory
+ authoritative_continuation
↓
provenance-bearing Selection
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

Scale / real evidence 已证明：

```text
1,000 enabled leaves
→ 3 real Provider routing calls
→ max 10 profiles/request
→ max 4601 serialized chars/request
→ exact target module

1,000 Player-known
→ unrelated dossier load = 0

10,000 relationship edges
→ player-safe bounded relevant subgraph

100 deterministic background turns
→ Router = 0 / Domain Candidate model = 0

100-turn long session
→ routing context size stable
→ Save / Restore / Branch / Recovery exactly-once
```

整个 G9-02C Core + Breadth 已 PASS；当前只剩 G9-02 Integrated Closure。

---

## 4. #18 / #18A｜资料库资源层

```text
三类主资产
= 世界包 + 角色卡 + 拓展包

资料库
= 可复用 / 可绑定 / 可检索的资料资源层
!= 第四类主资产
!= Runtime Truth
```

时序保持：

```text
G9-03  协议同批冻结
G9-04  三类主资产完整实现 + 资料库最小协议验证
G9-05  三类主资产 Creator 首轮闭环
之后   资料库产品功能增量
```

关键护栏：

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

## 5. #19｜Creator Conversational Authoring

新版 Creator 正式定义：

```text
Structured Creator Workspace
+
Conversational AI Authoring
```

AI 通过 Program-owned Typed Creator Tools 修改 Draft，不直接操作 DOM。

```text
AI Chat
!= Creator Draft

Creator Draft
!= Saved Source Asset

AI can edit Draft
!= AI can Save / Publish Source Asset
```

无 Provider 时完整手工 Creator 仍必须可用。

跨多个资产的大规模自主操作、长时间自主 Agent、自动联网研究仍为 `DEFERRED OPTIONAL`，不是当前 Roadmap 承诺。

---

## 6. 执行治理

```text
Vibe-Coding/main/sillytavern
= 核心项目事实源

zhangchenjia21-dot/sillytavern
= 实现事实源与 Grok Build 施工材料
```

默认：

```text
main = protected integration line
agent/<task-id> + isolated worktree = construction line
GPT exact-SHA Review PASS
→ fast-forward main
→ cleanup old task worktree / branch
```

长期治理：

- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`

---

## 7. Discussion / Deferred optional

不是当前实现 Authority：

- `G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md`
- `Creator_AI后续可选增量备忘录_v1.0_2026-08-19.md`

---

## 8. Current Gate

```text
G9-02C Core PASS
+
G9-02C Breadth PASS
↓
G9-02 Integrated Closure ACTIVE / NEXT
↓
G9-03
```

G9-03 当前仍 `NOT AUTHORIZED`。
