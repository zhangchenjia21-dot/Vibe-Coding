# SillyTavern 项目治理入口

本目录是 **SillyTavern 的核心治理与项目事实源**。只保存仍有效的正式架构裁定、阶段规格、独立审核、路线与长期协作治理。

代码 Agent 的具体施工任务包不存放在本目录。

## 1. Rolling current

- `酒馆游戏项目开发核心总纲_CURRENT.md`：当前项目解释层。
- `酒馆游戏新版主体重建总路线 v2.3.md`：当前总路线。
- `G9阶段启动与G9-02实施切片裁定_v1.0_2026-08-18.md`：G9 基础实施顺序。

当前阶段：

```text
G1–G8                     PASS / CLOSED
G9-01                      PASS / CLOSED
G9-02A                     PASS / CLOSED
G9-02BC Shared Foundation  PASS / CLOSED
G9-02B                     PASS / CLOSED
G9-02C Core Design         FROZEN / IMPLEMENTATION NEXT
G9-02C Breadth             BLOCKED BY CORE REVIEW
G9-02 Integrated Closure   BLOCKED BY G9-02C
G9-03                      NOT AUTHORIZED
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
0ee847e1173ae8d17e643d5b838d238cf889031e
```

当前下一项：

> **G9-02C Core｜Model-first Routing / bounded routing working set / state-mandatory augmentation / authorized context anchors / outcome-gated continuation context boundary。**

#18 / #18A / #19 均不回开 G9-02C。

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

## 3. #18 / #18A｜资料库资源层

长期资产架构：

```text
三类主资产
= 世界包 + 角色卡 + 拓展包

资料库
= 独立可复用 / 可绑定 / 可检索的资料资源层
!= 第四类主资产
!= 第二世界包
!= Runtime Truth
```

时序：

```text
G9-03
→ 世界包 + 角色卡 + 拓展包 + 资料库协议同批冻结

G9-04
→ 三类主资产完整实现
+ 资料库协议最小 parse / validate / round-trip proof

G9-05
→ 先完成三类主资产 Creator

主资产端到端完整游玩 PASS
↓
资料库产品功能增量
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

G9-03 不提前冻结 embedding、chunk size、向量维度、排名算法、任意查询 DSL 或具体 RAG Provider API。

---

## 4. #19｜Creator Conversational Authoring

新版 Creator 已正式从“传统表单 + AI 按钮”升级为：

```text
Structured Creator Workspace
+
Conversational AI Authoring
```

### 首版核心

```text
主工作区 / 设定区
= 可见、可手工编辑的 Creator Draft

AI 创作对话区
= 需求理解 / 最少必要追问 / 解释 / 受控编辑入口
```

AI 必须通过 Program-owned Typed Creator Tools 修改 Draft，不直接操作 DOM。

正式 Authority：

```text
AI Chat
!= Creator Draft

Creator Draft
!= Saved Source Asset

AI can edit Draft
!= AI can Save / Publish Source Asset
```

G9-05 首版必须具备：

- 世界包 / 角色卡 / 拓展包结构化主工作区；
- 持续 AI 创作对话区；
- bounded current asset / section / focus context；
- 自然语言创作请求 + 最少必要追问；
- Typed Creator Tool / Patch Contract；
- AI 受控修改 Draft；
- 主工作区即时同步；
- 任务级变更摘要 / Undo；
- deterministic Validator；
- AI Review / Validator explanation；
- 用户显式 Save / Publish；
- 无 Provider 时完整手工 Creator；
- 导入 / 导出 / 返回“我的资产库”。

G9-03 只需要让资产字段 / refs 具有稳定可编辑身份；不得把 chat history、prompt、model provider、Creator tool transport 写进 Source Asset schema。

G9-04 parser / compiler 必须完全 AI-independent。

---

## 5. Creator 高级 AI｜Deferred optional

`Creator_AI后续可选增量备忘录_v1.0_2026-08-19.md` 只记录未来可能性：

```text
跨多个资产的大规模自主操作
长时间自主 Agent
自动联网研究
```

状态：

```text
DEFERRED OPTIONAL
NOT CURRENT PRODUCT TARGET
NOT ROADMAP COMMITMENT
```

只有真实 Creator UAT 后出现明确需求，才重新进入 Product Definition / Architecture Gate。

当前不得因此提前建设 autonomous-agent runtime、跨资产万能事务、长任务调度器、联网抓取平台、向量数据库或相关 external schema。

---

## 6. 代码 Agent 执行治理

```text
Vibe-Coding/main/sillytavern
= 核心项目事实源

zhangchenjia21-dot/sillytavern
= 实现事实源与施工材料
```

默认工作模式：

```text
main
= protected integration line

agent/<task-id>
+
D:\AI\Projects\.worktrees\sillytavern-agent
= isolated construction line

GPT exact-SHA Review PASS
→ fast-forward main
→ cleanup branch + worktree
```

长期治理：

- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`

---

## 7. Discussion / Deferred optional

不是当前实现 Authority：

- `G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md`
- `Creator_AI后续可选增量备忘录_v1.0_2026-08-19.md`

Opening Scenario 玩家端 Runtime 当前仍延后。

---

## 8. 文档版本规则

```text
1.8 → 1.9 → 2.0 → 2.1
```

不生成 `1.10 / 1.11 / 1.12`；高频滚动解释层优先固定 `*_CURRENT.md`。