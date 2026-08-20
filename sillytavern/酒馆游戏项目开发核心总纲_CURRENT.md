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
G9-02                         PASS / CLOSED
G9-03                         PASS / CLOSED
G9-04                         PASS / CLOSED
G9-05A Creator Foundation     PASS / FROZEN
G9-05B Creator Core Foundation
                              PASS / CLOSED
G9-05C World Creator Vertical AUTHORIZED / NEXT
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
25286b2517cb26520109e3d8738671e53d88c861
```

当前资产仓库主线：

```text
zhangchenjia21-dot/sillytavern-assets main
34f72dd1a32b84649f2b3973e98836ad68f3b65e
```

G9-04 真实资产 Gate 的冻结证据基线仍为：

```text
968175e6c3fb3545b7c2907b65089c7e1dbb40a0
```

后续资产仓库提交 `ffb3f15d...` 与 `34f72dd1...` 均为阶段导航同步，不改变 G9-04 真实样本证据或 canonical semantic asset 正文。

当前核心路线：`酒馆游戏新版主体重建总路线 v2.3.md`。

当前下一步：

> **G9-05C World Creator Vertical。** 在已经关闭的 G9-05B 共享 Creator Core 上，实现第一个真实世界包 Creator 产品纵向：结构化 World Draft 工作区、手工编辑、受控 AI 创作、外部 `.md/.txt` 创作稿导入整理、Validator / ChangeSet / Undo、显式发布到 Source Asset Library、重新打开与创建新版本。不得复制第二套 Draft / AI Patch / Source Store / 发布事务。

---

## 1. 当前正式 Authority

### Runtime / Asset

- `G9-02_IntegratedClosure_IndependentReview_最终收口_v1.0_2026-08-20.md`
- `G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md`
- `G9-03A_RuntimeModuleBinding与TypedConfig增量裁定_v1.0_2026-08-20.md`
- `G9-03_IndependentReview_最终收口_v1.0_2026-08-20.md`
- `G9-04_LegacyMarkdownAdapterCompilerBinding规格_v1.0_2026-08-20.md`
- `G9-04_IndependentReview_最终收口_v1.0_2026-08-20.md`

### Creator

- `19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md`
- `G9-05A_Creator基础模型与创作稿导入产品架构裁定_v1.0_2026-08-20.md`
- `G9-05B_CreatorCore草稿导入发布内部合同规格_v1.0_2026-08-20.md`
- `G9-05B_IndependentReview_最终收口_v1.0_2026-08-20.md`
- `酒馆游戏新版主体重建总路线 v2.3.md`

### 历史返修证据

以下仅为 historical evidence，不再是当前 Gate Authority：

- `G9-05B_IndependentReview_CreatorCore_correction-01_v1.0_2026-08-20.md`
- `G9-05B_IndependentReview_CreatorCore_correction-02_v1.0_2026-08-20.md`

### 执行治理

- `G9及后续阶段_Agent资源分配与Codex默认代码协作裁定_v1.1_2026-08-20.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`
- `AgentTaskPacket_GitHub原生交付增量裁定_v1.0_2026-08-20.md`
- `Skill/main/skill/gpt/agent-task-packet/SKILL.md` v1.1
- `Skill/main/skill/gpt/lifecycle-dev-process/SKILL.md` v2.1

默认代码执行 Agent = Codex；Grok 只有项目所有者明确指定时使用。面向项目所有者沟通中文优先，仅精确代码字段、路径、命令、协议名和提交号保留英文。

---

## 2. 永久架构边界

```text
Source Asset
!= Game-local Canonical Instance
!= Runtime State
```

```text
Creator Draft
!= Saved Source Asset
!= Game-local Canonical Instance
!= Runtime State
```

```text
AI Chat
!= Creator Draft

AI can edit Draft
!= AI can publish asset
```

```text
Model authors / proposes
Program / Domain Owner commits reality
```

继续保持程序最终结果权、玩家行动权、开放尝试、No Phantom、私密/公开边界、按需物化、Save / Restore / Branch、Crash / Resume / Recovery 与 exactly-once。

---

## 3. G9-03 / G9-04｜PASS / CLOSED

G9-03 最终实现：

```text
5da2294a9d21585665167e69307d9c693427582d
```

统一资产协议：

```text
TavernAssetV1
= world | character | expansion | library
```

G9-04 最终实现：

```text
c492ac4a0eb33ec055f582a2a023066853e2c323
```

已证明真实 Markdown → `TavernAssetV1` → exact Manifest → G9-02 Source Binding / lineage / Save-Restore；不猜资产身份、Source 新版本不静默更新旧游戏、Character Binding 不等于物化、Library 不进入本局主资产 lineage。

---

## 4. G9-05A｜Creator Foundation｜PASS / FROZEN

“我的资产库”顶层固定：

```text
我的资产库
├── Creator
├── 世界包
├── 角色卡
└── 拓展包
```

三种创作起点统一进入同一个 Draft：

```text
空白创建
外部创作稿导入
已有正式资产创建新版本
↓
Creator Draft
```

外部创作稿导入规则：

```text
明确 + 唯一 + 原文证据
→ 可以填入空白 Draft

信息不足 / 未提及 / 冲突 / 多种合理解释
→ 留空或保持原值
→ 保留原始证据
```

导入内容永远只是数据，不是 Creator 指令、工具授权或发布授权。

---

## 5. G9-05B｜Creator Core Foundation｜PASS / CLOSED

正式规格：

`G9-05B_CreatorCore草稿导入发布内部合同规格_v1.0_2026-08-20.md`

最终审核：

`G9-05B_IndependentReview_最终收口_v1.0_2026-08-20.md`

最终实现 / integrated main：

```text
25286b2517cb26520109e3d8738671e53d88c861
```

最终结论：

```text
P0 = 0
P1 = 0
G9-05B = PASS / CLOSED
```

### 5.1 两个长期 Owner

```text
Creator Draft Store
= 草稿 / Import Artifact / evidence / ChangeSet / Undo / publish work state

Source Asset Library Store
= 已发布、通过 G9-03 基础验证的本地正式 Source Asset
```

二者都不是 Runtime State，也不等于研发仓库 `sillytavern-assets`。

### 5.2 Draft identity / CAS

- Program-minted `targetAssetRef`；
- source revision 精确继承 base snapshot identity；
- revision/CAS；
- stale AI fail closed；
- published Draft 不原地编辑。

### 5.3 Import Artifact / blank-only

- `.md/.txt` exact 原文；
- SHA-256；
- stable segment refs；
- certain + evidence + blank target 才可填写；
- unresolved / conflict 保留；
- section 按 `sectionRef` 判空；
- Provider 不拥有 Creator node identity。

### 5.4 AI task authorization

Program-owned scope 约束：

- operation family；
- scalar targets；
- semantic section refs；
- dependency/list node refs；
- typed node exact `nodeKind + nodeRef + allowedOperations`。

AI 无默认全 Draft 写权限，`targetVersion` 也不默认授权。

### 5.5 Provider runtime gate / partial apply

Provider operation 视为 `unknown`；Program 对 complex payload 做完整运行时 shape / type / enum / nested key parsing。

非法 operation 局部忽略并记录；合法 sibling 保留；一次 AI task 仍形成一个 ChangeSet，最终只做一次 CAS persist。

### 5.6 Undo

Undo 使用 inverse mutation 创建新 revision；目标后来已经变化则 `CREATOR_UNDO_CONFLICT`，不覆盖后续修改。

### 5.7 Source Asset Library

```text
assetRef + version 不存在
→ append

存在且 digest 相同
→ idempotent success

存在但 digest 不同
→ SOURCE_ASSET_VERSION_CONFLICT
→ never overwrite
```

### 5.8 Publish / Recovery

```text
exact Draft revision
→ deterministic publication compiler
→ existing computeAssetDigest()
→ existing validateAndVerifyAsset()
→ SourceAssetLibraryStore
```

发布状态：

```text
editable → publishing → published
```

Source 已写入 / Draft 未 final 可 exact resume；确定性版本冲突会恢复 Draft 为 editable，让用户修改版本继续创作。

---

## 6. G9-05C｜World Creator Vertical｜AUTHORIZED / NEXT

G9-05C 是第一个真实 Creator 产品纵向，不再建设共享底座。

目标链：

```text
我的资产库
→ Creator
→ 新建世界包 / 导入创作稿 / 已有世界包新版本
→ World Creator Draft Workspace
→ 手工 + bounded AI authoring
→ import unresolved/conflict visibility
→ deterministic validation
→ task change summary / Undo
→ explicit Publish
→ Source Asset Library
→ 世界包资产详情 / 再打开 / 创建新版本
```

本阶段必须复用 G9-05B：

- `CreatorDraftV1`；
- Creator Draft Store；
- Import Artifact；
- typed operation / authoring scope；
- ChangeSet / Undo；
- Source Asset Library；
- publication compiler / recovery。

不得先复制角色卡/拓展包页面；先用世界包验证真实产品工作区与 AI 创作体验，再向后复用。

---

## 7. 当前 DAG

```text
G9-02 Runtime Foundation                 PASS / CLOSED
↓
G9-03 Unified Asset Protocol             PASS / CLOSED
↓
G9-04 Adapter / Compiler / Binding       PASS / CLOSED
↓
G9-05A Creator Foundation                PASS / FROZEN
↓
G9-05B Shared Creator Core               PASS / CLOSED
↓
G9-05C World Creator Vertical            AUTHORIZED / NEXT
↓
Character Creator Vertical
↓
Expansion Creator Vertical
↓
三类主资产“我的资产库 → 创建游戏 → 完整游玩”端到端闭环
↓
Primary Asset End-to-End Closure Gate
↓
Library Product Increment
```
