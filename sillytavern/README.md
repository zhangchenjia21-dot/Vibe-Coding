# SillyTavern 项目治理入口

本目录 active 层只保存**当前仍有效的项目事实源**。历史版本与已关闭过程材料统一进入 `../99_归档/sillytavern/`。

## 当前入口

### Rolling current

- `酒馆游戏项目开发核心总纲_CURRENT.md`：当前项目解释层，优先读取。
- `酒馆游戏新版主体重建总路线 v2.0.md`：当前总路线。
- `G9阶段启动与G9-02实施切片裁定_v1.0_2026-08-18.md`：当前 G9 阶段与实施顺序。

### 当前 G9 实施来源

- `G9-02A_SourceBinding与GameLocalRevisionFoundation规格_v1.0_2026-08-18.md`
- `G9-02A_IndependentReview_SourceBinding与GameLocalRevision_v1.0_2026-08-19.md`
- `G9-02BC_SharedRuntimeFoundationConvergence规格_v1.0_2026-08-19.md`
- `G9-02BC_IndependentReview_共享运行时基础_v1.0_2026-08-19.md`
- `G9-02B_RuntimeDomainBreadth与PlayerKnownDirectory规格_v1.0_2026-08-19.md`：当前实现规格。
- `G9-02B_GrokBuild_Worktree执行任务包_v1.0_2026-08-19.md`：当前 Grok Build 执行包。

### 当前 AI / Agent 协作治理

- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`：Sol / Grok Build 风险分工、中文沟通规则。
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`：代码 Agent 的隔离施工、精确提交审核、main 快进集成与临时分支清理规则。

默认代码 Agent 工作模式：

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

原则上任何时刻只保留 `main + 当前任务临时分支`；默认不使用长期 Grok 分支，不使用多层 feature branch，也不要求 PR。

### 仍有效的编号核心

- `11_酒馆游戏_G5长期连续性与玩家授权收口裁定_v1.0_2026-08-15.md`
- `12_酒馆游戏_G6_SaveContinueRestore收口裁定_v1.0_2026-08-15.md`
- `13_酒馆游戏_G7_CrashResumeRecovery收口裁定_v1.0_2026-08-16.md`
- `14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`
- `17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`

编号核心不会因为日期较早自动归档；同一核心出现 superseding current 时，旧版进入 `99_归档/sillytavern/版本历史/`。

### Discussion-only

- `G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md`

该文件不是实现 authority；Opening Scenario 玩家端 Runtime 当前仍延后。

## 当前阶段

```text
G1–G8                  PASS / CLOSED
G9-01                   PASS / CLOSED
G9-02A                  PASS / CLOSED
G9-02BC Shared Foundation PASS / CLOSED
G9-02B breadth          ACTIVE / NEXT
G9-02C                  BLOCKED BY G9-02B
G9-03                   NOT AUTHORIZED
```

当前代码基线：

```text
sillytavern/main
5962e6f5933f245693e090cbdfd2f79791820ef1
```

当前下一项：

> **G9-02B｜Player-known Character Directory，由 Grok Build 在隔离 worktree / 临时任务分支中实现。**

最后约一次 Sol 深任务优先保留给 G9-02C 的模型路由与上下文编排核心。

## 文档版本规则

```text
1.8 → 1.9 → 2.0 → 2.1
```

不生成 `1.10 / 1.11 / 1.12`；高频滚动解释层优先固定 `*_CURRENT.md`。
