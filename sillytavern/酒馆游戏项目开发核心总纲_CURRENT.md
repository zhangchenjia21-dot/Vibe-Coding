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
Character Creator Vertical    AUTHORIZED / NEXT
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
1b79323bb53b5fb243465294a50c9d0b3f63dac8
```

G9-05C 最终审核实现：

```text
1b79323bb53b5fb243465294a50c9d0b3f63dac8
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

> **Character Creator Vertical。** 复用 G9-05B Shared Creator Core 和 G9-05C 已验证的产品纵向模式，先冻结 Character-specific structured workspace / Source schema 映射，再由当前临时主力 Grok Build 执行 bounded implementation。不得把 World-only composition/dependency UI 生搬硬套到 Character Creator，也不得重开 Creator Core。

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
- `G9-05C_WorldCreator产品纵向规格_v1.0_2026-08-20.md`
- `G9-05C_IndependentReview_最终收口_v1.0_2026-08-21.md`
- `酒馆游戏新版主体重建总路线 v2.3.md`

### 历史返修证据

以下只作为 historical evidence，不再是当前 Gate Authority：

- `G9-05B_IndependentReview_CreatorCore_correction-01_v1.0_2026-08-20.md`
- `G9-05B_IndependentReview_CreatorCore_correction-02_v1.0_2026-08-20.md`
- `G9-05C_IndependentReview_WorldCreator_correction-01_v1.0_2026-08-20.md`
- `G9-05C_IndependentReview_WorldCreator_correction-02_v1.0_2026-08-21.md`

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

最终实现 / integrated main：

```text
1b79323bb53b5fb243465294a50c9d0b3f63dac8
P0 = 0
P1 = 0
```

### 6.1 已成立产品纵向

- “我的资产库”四入口；
- Creator / 世界包真实可用，角色卡 / 拓展包仍为后续入口；
- blank / `.md/.txt` import / exact Source revision 三起点；
- World structured workspace；
- manual CAS autosave；
- section create/edit/delete，existing `sectionRef` 只读稳定 identity；
- composition/dependency 完整 create/edit/delete；
- World dependency kind 仅 `hard | optional | reference`；
- exact AI scope / Provider runtime parsing / ChangeSet / Undo；
- raw evidence / mappings / unresolved / conflict visibility；
- formal target → exact field/section continuation navigation；
- explicit Publish；
- Source world list / exact version history；
- SQLite restart persistence；
- No-Provider manual path；
- Runtime isolation。

### 6.2 Catalog 语义证明

最终 correction-02 已证明：Creator 发布的 World Source，带合法 `hard / optional / reference` dependency 后，可以进入 G9-03 `validateAssetCatalog()`：

- hard target present → PASS；
- optional target absent → gap only；
- reference target absent → allowed；
- 无 `CONDITIONAL_DEPENDENCY_SCOPE_MISSING`；
- 无 `DEPENDENCY_MISSING`。

因此 G9-05C 不再存在“Source 可发布但 catalog 不可用”的已知 P1。

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
G9-05C World Creator Vertical            PASS / CLOSED
↓
Character Creator Vertical               AUTHORIZED / NEXT
↓
Expansion Creator Vertical
↓
三类主资产“我的资产库 → 创建游戏 → 完整游玩”端到端闭环
↓
Primary Asset End-to-End Closure Gate
↓
Library Product Increment
```
