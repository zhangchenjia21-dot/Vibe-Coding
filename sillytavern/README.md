# SillyTavern 项目治理入口

本目录是 **SillyTavern 的核心治理与项目事实源**。只保存仍有效的正式架构裁定、阶段规格、独立审核、路线与长期协作治理。

Grok Build / Codex 等代码 Agent 的具体施工任务包，不再存放在本目录。

## 1. 当前核心入口

### Rolling current

- `酒馆游戏项目开发核心总纲_CURRENT.md`：当前项目解释层。
- `酒馆游戏新版主体重建总路线 v2.0.md`：当前总路线。
- `G9阶段启动与G9-02实施切片裁定_v1.0_2026-08-18.md`：当前 G9 阶段与实施顺序。

### 当前 G9 正式来源

- `G9-02A_SourceBinding与GameLocalRevisionFoundation规格_v1.0_2026-08-18.md`
- `G9-02A_IndependentReview_SourceBinding与GameLocalRevision_v1.0_2026-08-19.md`
- `G9-02BC_SharedRuntimeFoundationConvergence规格_v1.0_2026-08-19.md`
- `G9-02BC_IndependentReview_共享运行时基础_v1.0_2026-08-19.md`
- `G9-02B_RuntimeDomainBreadth与PlayerKnownDirectory规格_v1.0_2026-08-19.md`
- `G9-02B_IndependentReview_玩家已知人物目录阻塞发现_v1.0_2026-08-19.md`
- `G9-02B_Correction01_IndependentReview_剩余阻塞_v1.0_2026-08-19.md`：**当前 Review Authority；FAIL / CORRECTION-02 REQUIRED。**

### 当前 AI / Agent 长期治理

- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`

## 2. Grok Build 执行材料位置

正式分工：

```text
Vibe-Coding/main/sillytavern
= 核心项目事实源
= 架构裁定 / 阶段规格 / Independent Review / 长期治理

zhangchenjia21-dot/sillytavern/grok build
= Grok Build 施工材料
= Execution Packet / Correction Packet / 临时实现交接
```

当前 G9-02B 仍在任务分支施工：

```text
repo:   zhangchenjia21-dot/sillytavern
branch: agent/g9-02b
folder: grok build/
```

当前主要执行文件：

`grok build/G9-02B_GrokBuild_Correction02_Worktree任务包_v1.0_2026-08-19.md`

**Vibe-Coding 不保存 Grok Build 执行任务包副本。**

## 3. 默认代码 Agent 工作模式

```text
main
= 受保护正式主线

当前唯一 agent/<task-id> 临时分支
+
D:\AI\Projects\.worktrees\sillytavern-agent
= Agent 隔离施工区

GPT exact-SHA Review PASS
→ fast-forward main
→ 删除 worktree + 临时分支
```

原则上任何时刻只保留 `main + 当前任务临时分支`；Review FAIL 后继续原任务分支，不新建 fix branch。

## 4. 仍有效的编号核心

- `11_酒馆游戏_G5长期连续性与玩家授权收口裁定_v1.0_2026-08-15.md`
- `12_酒馆游戏_G6_SaveContinueRestore收口裁定_v1.0_2026-08-15.md`
- `13_酒馆游戏_G7_CrashResumeRecovery收口裁定_v1.0_2026-08-16.md`
- `14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`
- `17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`

## 5. Discussion-only

- `G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md`

该文件不是实现 authority；Opening Scenario 玩家端 Runtime 当前仍延后。

## 6. 当前阶段

```text
G1–G8                     PASS / CLOSED
G9-01                      PASS / CLOSED
G9-02A                     PASS / CLOSED
G9-02BC Shared Foundation  PASS / CLOSED
G9-02B Correction-01       REVIEW FAIL
P0                         0
P1                         3
G9-02B Correction-02       ACTIVE / NEXT
G9-02C                     BLOCKED BY G9-02B
G9-03                      NOT AUTHORIZED
```

当前实现 Git 状态：

```text
sillytavern/main
08f9dcf0a2f9e30b29d6d8391fa91701e57b6e23

Correction-01 reviewed SHA
76c7440e38a7ccdffd5c2dae4cf24012435e995c

agent/g9-02b
继续用于 Correction-02

Main integration
NOT AUTHORIZED
```

当前下一项：

> **G9-02B Correction-02：继续由 Grok Build 在同一 `agent/g9-02b` worktree 修复 3 个 P1；不得另开分支，不得修改 main。**

最后约一次 Sol 深任务继续保留给 G9-02C 的模型路由与上下文编排核心。

## 7. 文档版本规则

```text
1.8 → 1.9 → 2.0 → 2.1
```

不生成 `1.10 / 1.11 / 1.12`；高频滚动解释层优先固定 `*_CURRENT.md`。
