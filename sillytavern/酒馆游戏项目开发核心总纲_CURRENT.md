---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-21
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
G9-05C World Creator Vertical PASS / CLOSED
G9-05D0 Character Field Seam  PASS / CLOSED
G9-05D Character Creator      PASS / CLOSED
G9-05E Use My Assets Game Creation
                              AUTHORIZED / SPEC NEXT
G9-05F Expansion Creator      DEFERRED / NOT AUTHORIZED
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
dd67bd9c02f42717dab139e9c87b4fe7e25f0fc6
```

G9-05D 最终实现 / 集成主线：

```text
dd67bd9c02f42717dab139e9c87b4fe7e25f0fc6
P0 = 0
P1 = 0
```

当前资产仓库主线：

```text
zhangchenjia21-dot/sillytavern-assets main
1d9872dccdb2caeff141e959fd533512c9de384a
```

G9-04 真实资产 Gate 的冻结证据基线仍为：

```text
968175e6c3fb3545b7c2907b65089c7e1dbb40a0
```

后续资产仓库提交仅为阶段导航同步，不改变 G9-04 真实样本证据或 canonical semantic asset 正文。

当前核心路线：`酒馆游戏新版主体重建总路线 v2.3.md`。

当前下一步：

> **进入 G9-05E【使用我的资产库】创建游戏纵向的详细规格冻结。** World Creator 与 Character Creator 已经 PASS/CLOSED，下一步优先验证“已发布 Source Asset exact snapshot → TavernGameAssetManifestV1 → Game-local binding → 创建游戏 → Session / Save / Continue / Restore / Recovery”的真实闭环。G9-05F Expansion Creator 在 G9-05E PASS/CLOSED 前保持未授权。

---

## 1. 当前正式 Authority

### Runtime / Asset

- `G9-02_IntegratedClosure_IndependentReview_最终收口_v1.0_2026-08-20.md`
- `G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md`
- `G9-03A_RuntimeModuleBinding与TypedConfig增量裁定_v1.0_2026-08-20.md`
- `G9-03_IndependentReview_最终收口_v1.0_2026-08-20.md`
- `G9-04_LegacyMarkdownAdapterCompilerBinding规格_v1.0_2026-08-20.md`
- `G9-04_IndependentReview_最终收口_v1.0_2026-08-20.md`

### Creator / Asset Game Creation

- `19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md`
- `G9-05A_Creator基础模型与创作稿导入产品架构裁定_v1.0_2026-08-20.md`
- `G9-05B_CreatorCore草稿导入发布内部合同规格_v1.0_2026-08-20.md`
- `G9-05B_IndependentReview_最终收口_v1.0_2026-08-20.md`
- `G9-05C_WorldCreator产品纵向规格_v1.0_2026-08-20.md`
- `G9-05C_IndependentReview_最终收口_v1.0_2026-08-21.md`
- `G9-05D0_CharacterProfileFields增量裁定_v1.0_2026-08-21.md`
- `G9-05D_CharacterCreator产品纵向规格_v1.0_2026-08-21.md`
- `G9-05D_IndependentReview_最终收口_v1.0_2026-08-21.md`
- `G9-05_阶段重排_先资产建局后ExpansionCreator裁定_v1.0_2026-08-21.md`
- `酒馆游戏新版主体重建总路线 v2.3.md`

### 历史返修证据

以下只作为 historical evidence，不再是当前 Gate Authority：

- `G9-05B_IndependentReview_CreatorCore_correction-01_v1.0_2026-08-20.md`
- `G9-05B_IndependentReview_CreatorCore_correction-02_v1.0_2026-08-20.md`
- `G9-05C_IndependentReview_WorldCreator_correction-01_v1.0_2026-08-20.md`
- `G9-05C_IndependentReview_WorldCreator_correction-02_v1.0_2026-08-21.md`
- `G9-05D_IndependentReview_CharacterCreator_correction-01_v1.0_2026-08-21.md`

### 执行治理

- `G9及后续阶段_Agent资源分配与Codex默认代码协作裁定_v1.1_2026-08-20.md`
- `G9及后续阶段_Grok临时主力执行切换记录_v1.0_2026-08-21.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`
- `AgentTaskPacket_GitHub原生交付增量裁定_v1.0_2026-08-20.md`
- `Skill/main/skill/gpt/agent-task-packet/SKILL.md` v1.1
- `Skill/main/skill/gpt/lifecycle-dev-process/SKILL.md` v2.1

当前因 Codex 额度不可用，Project Owner 已明确选择 Grok Build 作为临时优先实现 Agent；GPT 继续负责产品/架构、Task Packet、exact-SHA Independent Review 与 main 集成 Gate。面向项目所有者沟通中文优先，仅精确代码字段、路径、命令、协议名和提交号保留英文。

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

AI can edit authorized Draft scope
!= AI can publish asset
```

```text
Model authors / proposes
Program / Domain Owner commits reality
```

Character 特别边界：

```text
Character Source Definition
!= materialized Character
!= current player character
!= current position / relationship / knowledge / injury / allegiance
```

`playerCharacterSupported=true` 只表示 Source capability，不执行选择或 materialization。

资产建局特别边界：

```text
Creator Draft
!= selectable game asset

Saved Source Asset exact snapshot
→ TavernGameAssetManifestV1
→ Game-local binding / creation
```

【使用我的资产库】只能选择已发布 Source Asset；Draft 可提示“未发布”并跳回 Creator，但不得直接进入 Manifest、不得自动发布、不得偷偷形成临时 Source。

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

正式规格：`G9-05B_CreatorCore草稿导入发布内部合同规格_v1.0_2026-08-20.md`。

最终审核：`G9-05B_IndependentReview_最终收口_v1.0_2026-08-20.md`。

最终实现：

```text
25286b2517cb26520109e3d8738671e53d88c861
P0 = 0
P1 = 0
```

共享 Core 已冻结：Program-minted identity、Draft Store / Source Store 分离、revision/CAS、Import Artifact/evidence、task-level AI scope、typed runtime parsing、partial apply、ChangeSet/Undo、Source append-only、deterministic publish 与 recovery、No-Provider 手工路径。

---

## 6. G9-05C｜World Creator Vertical｜PASS / CLOSED

正式产品规格：`G9-05C_WorldCreator产品纵向规格_v1.0_2026-08-20.md`。

最终审核：`G9-05C_IndependentReview_最终收口_v1.0_2026-08-21.md`。

最终实现：

```text
1b79323bb53b5fb243465294a50c9d0b3f63dac8
P0 = 0
P1 = 0
```

已成立：四入口、World 三种创作起点、结构化 workspace、sections/composition/dependency 完整编辑、exact AI scope、Import evidence/continuation、ChangeSet/Undo、显式 Publish、Source list/detail/version history、No-Provider manual path、Runtime isolation 与 `validateAssetCatalog()` 正向兼容证明。

---

## 7. G9-05D0 / G9-05D｜Character Creator｜PASS / CLOSED

正式规格：

- `G9-05D0_CharacterProfileFields增量裁定_v1.0_2026-08-21.md`
- `G9-05D_CharacterCreator产品纵向规格_v1.0_2026-08-21.md`

最终审核：`G9-05D_IndependentReview_最终收口_v1.0_2026-08-21.md`。

最终实现 / integrated main：

```text
dd67bd9c02f42717dab139e9c87b4fe7e25f0fc6
P0 = 0
P1 = 0
```

已成立：

- exact `metadata.aliases` mutation / import / Undo；
- `playerCharacterSupported` 未声明 / true / false 三态；
- Character-specific Provider adapter，author / organizer exact type+revision preflight；
- blank / `.md/.txt` import / exact Source revision；
- Character dossier workspace；
- sections / referenceSources / dependencies 完整编辑；
- Character dependency 只允许 `hard | optional | reference`；
- exact AI scope、partial ignore、ChangeSet / Undo；
- evidence / unresolved / conflict；
- stale CAS 刷新保留玩家失败的本地基础字段输入，不削弱 CAS；
- explicit Publish；
- Character Source list/detail/version history；
- SQLite reopen；
- No-Provider manual path；
- `validateAssetCatalog()` 正向证明；
- Runtime No-Phantom / session revision-turn isolation。

永久保持：

```text
Character Source Definition
!= materialized Character
!= current player character
!= Runtime State
```

---

## 8. G9-05E｜Use My Assets Game Creation｜AUTHORIZED / SPEC NEXT

正式顺序裁定：`G9-05_阶段重排_先资产建局后ExpansionCreator裁定_v1.0_2026-08-21.md`。

首轮允许：

```text
1 exact published World
+
0..N exact published Characters
+
0 published Expansions
```

目标闭环：

```text
Published Source exact snapshots
↓
TavernGameAssetManifestV1
↓
existing catalog / manifest validation
↓
G9-04 binding semantics
↓
G9-02 lineage / game-local instances
↓
创建游戏
↓
Session / Save / Continue / Restore / Recovery
```

Draft 不可直接选择；不得自动发布；不得形成临时 Source；不得因为 Character 被选入 Manifest 就绕过既有 materialization / No-Phantom 边界。

G9-05E 详细规格和 Task Packet 现在允许基于 `sillytavern/main@dd67bd9c...` 冻结。规格冻结前不进入实现。

G9-05F Expansion Creator 在 G9-05E PASS/CLOSED 前保持未授权。

---

## 9. 当前 DAG

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
G9-05C World Creator Vertical            PASS / CLOSED
↓
G9-05D0 Character Field Seam             PASS / CLOSED
↓
G9-05D Character Creator Vertical        PASS / CLOSED
↓
G9-05E Use My Assets Game Creation       AUTHORIZED / SPEC NEXT
↓
G9-05F Expansion Creator Vertical        DEFERRED / NOT AUTHORIZED
↓
三类主资产完整组合建局与游玩闭环
↓
Primary Asset End-to-End Closure Gate
↓
Library Product Increment
```
