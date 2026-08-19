# SillyTavern 项目治理入口

本目录是 **SillyTavern 的核心治理与项目事实源**。只保存仍有效的正式架构裁定、阶段规格、独立审核、路线与长期协作治理。

Grok Build / Codex 等代码 Agent 的具体施工任务包，不再存放在本目录。

## 1. 当前核心入口

### Rolling current

- `酒馆游戏项目开发核心总纲_CURRENT.md`：当前项目解释层。
- `酒馆游戏新版主体重建总路线 v2.1.md`：当前总路线。
- `G9阶段启动与G9-02实施切片裁定_v1.0_2026-08-18.md`：当前 G9 阶段与实施顺序。

### 当前 G9 正式来源

- `G9-02A_SourceBinding与GameLocalRevisionFoundation规格_v1.0_2026-08-18.md`
- `G9-02A_IndependentReview_SourceBinding与GameLocalRevision_v1.0_2026-08-19.md`
- `G9-02BC_SharedRuntimeFoundationConvergence规格_v1.0_2026-08-19.md`
- `G9-02BC_IndependentReview_共享运行时基础_v1.0_2026-08-19.md`
- `G9-02B_RuntimeDomainBreadth与PlayerKnownDirectory规格_v1.0_2026-08-19.md`
- `G9-02B_IndependentReview_玩家已知人物目录阻塞发现_v1.0_2026-08-19.md`：历史第一轮阻塞审核。
- `G9-02B_Correction01_IndependentReview_剩余阻塞_v1.0_2026-08-19.md`：历史第二轮阻塞审核。
- `G9-02B_IndependentReview_PlayerKnownDirectory最终收口_v1.0_2026-08-19.md`：02B 最终 Review Authority；`PASS / CLOSED`。
- `G9-02C_ModelFirstRouting与ContextOrchestration核心规格_v1.0_2026-08-19.md`：**当前 02C Core Authority；CORE DESIGN FROZEN / IMPLEMENTATION NEXT。**
- `G9-02C_Agent资源分配增量裁定_v1.0_2026-08-19.md`：**当前 02C Core / Breadth 执行者分工。**

### 当前 AI / Agent 长期治理

- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`

## 2. 代码 Agent 执行材料位置

正式分工：

```text
Vibe-Coding/main/sillytavern
= 核心项目事实源
= 架构裁定 / 阶段规格 / Independent Review / 长期治理

zhangchenjia21-dot/sillytavern
= 实现与施工材料
```

Grok Build 施工任务继续放在 `sillytavern/grok build/`。

02C Core 当前只完成正式规格设计，**尚未创建新的代码 worktree / agent branch**。Sol 执行包在正式启动时必须遵守同一 worktree 隔离治理，不直接写 main。

**Vibe-Coding 不保存代码 Agent 的具体执行任务包副本。**

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
- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.1_2026-08-19.md`：**资料库长期架构已批准，但实现后置；只有三类主资产从协议、Creator、“我的资产库”、创建游戏到完整游玩全部闭环后才允许开工。**

## 5. #18｜资料库资源层

长期资产架构语义：

```text
三类主资产
= 世界包 + 角色卡 + 拓展包

资料库
= 可复用 / 可绑定 / 可检索的资料资源层
!= 第四类主资产
!= Runtime State
!= Player-known Truth
```

当前开发状态：

```text
ARCHITECTURE APPROVED
IMPLEMENTATION DEFERRED
```

资料库不是 G9-02 / G9-03 / G9-04 / G9-05 首轮 blocker。

正式开工前必须先完成：

- 三类主资产 `tavern-asset` 语义 / 治理闭环；
- 三类主资产首轮机器协议；
- 三类主资产适配 / 编译 / 当局游戏绑定；
- Creator 三类主资产基础创作链；
- “我的资产库”对三类主资产的导入 / 管理 / 选择；
- 从“我的资产库”选择资产创建新游戏；
- 真实三类资产组合可以初始化并完整游玩；
- Save / Continue / Restore / Branch / Recovery 保持成立；
- 主资产链没有 P0/P1 blocker。

全部 PASS 后，才进入资料库增量阶段；阶段编号到时再定。

长期产品方向仍保持：创建游戏以世界包为入口，Creator 中资料入口靠近世界包，Runtime 只按当前职责检索最小充分资料切片，禁止整库 Prompt。

## 6. Discussion-only

- `G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md`

该文件不是实现 authority；Opening Scenario 玩家端 Runtime 当前仍延后。

## 7. 当前阶段

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

当前实现 Git 状态：

```text
sillytavern/main
0ee847e1173ae8d17e643d5b838d238cf889031e

active agent/* branches
0

G9-02B Independent Review
PASS / CLOSED
```

当前 Skill freshness：

```text
Skill/main
ac48934beae20a938ce126014cfee6a20642c1b2

lifecycle-dev-process
v2.1

agent-task-packet
v1.0
```

当前下一项：

> **G9-02C Core｜由 Sol 在隔离 worktree 中实现 Model-first Routing / bounded routing working set / state-mandatory augmentation / authorized context anchors / outcome-gated continuation context boundary。完成后由 GPT exact-SHA Independent Review；PASS 后再开放 Grok Build breadth。**

#18 v1.1 不改变当前 02C 执行 DAG，也不进入 G9-03/04/05 首轮范围。

## 8. 文档版本规则

```text
1.8 → 1.9 → 2.0 → 2.1
```

不生成 `1.10 / 1.11 / 1.12`；高频滚动解释层优先固定 `*_CURRENT.md`。
