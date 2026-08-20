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
G9-05B Creator Core Contract  PASS / FROZEN
G9-05B Core Implementation    CORRECTION-02 ACTIVE
G9-05C World Creator          NOT AUTHORIZED
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
c492ac4a0eb33ec055f582a2a023066853e2c323
```

当前 G9-05B 任务分支：

```text
agent/g9-05b-creator-core
reviewed correction-01 = f789150d584f8f2e538558c0129e0b25e5bbb73e
correction-02 packet   = eb9502b177f289bf5ee8956a454dca4a63e8cd2c
```

当前资产仓库主线：

```text
zhangchenjia21-dot/sillytavern-assets main
ffb3f15d959249ff2115edb99bf8cc5ca10bbe9d
```

G9-04 真实资产 Gate 仍以 `968175e6c3fb3545b7c2907b65089c7e1dbb40a0` 为冻结证据基线；后续资产仓库提交只同步阶段导航，不改变该证据。

当前下一步：

> **G9-05B correction-02。** Codex 继续原任务分支，只关闭复杂 typed node 的精确任务授权与复杂 Provider operation 的完整运行时类型/取值校验两个残余 P1。新精确提交通过独立审核之前，不合并 main、不进入 G9-05C。

---

## 1. 当前正式 Authority

### G9 / Runtime / Asset

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
- `G9-05B_IndependentReview_CreatorCore_correction-01_v1.0_2026-08-20.md`
- `G9-05B_IndependentReview_CreatorCore_correction-02_v1.0_2026-08-20.md`
- `酒馆游戏新版主体重建总路线 v2.3.md`

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
Model authors / proposes
Program / Domain Owner commits reality
```

```text
AI Chat
!= Creator Draft

AI can edit Draft
!= AI can publish asset
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
Creator
世界包
角色卡
拓展包
```

三种创作起点统一进入同一个 Draft：

```text
空白创建
外部创作稿导入
已有正式资产创建新版本
↓
Creator Draft
```

外部稿导入规则：

```text
明确 + 唯一 + 原文证据
→ 可以填入空白 Draft

信息不足 / 未提及 / 冲突 / 多种解释
→ 留空或保持原值
→ 保留原始证据
```

导入内容永远只是数据，不是 Creator 指令、工具授权或发布授权。

---

## 5. G9-05B｜Creator Core Contract｜PASS / FROZEN

两个长期 Owner：

```text
Creator Draft Store
= 草稿、导入证据、ChangeSet、Undo 工作状态

Source Asset Library Store
= 已通过 G9-03 校验的正式本地 Source Asset
```

核心合同：

- Program 生成稳定 `targetAssetRef`；
- Draft 变更使用 revision/CAS；
- stale AI result 丢弃；
- `.md/.txt` 原件 exact 保存 + SHA-256 + stable segment refs；
- AI 通过 typed operations 修改 Draft，不允许任意路径；
- ChangeSet + inverse Undo；
- Draft 直接经现有 `computeAssetDigest()` / `validateAndVerifyAsset()` 发布；
- Source Store `assetRef + version` append-only；
- 发布生命周期 `editable → publishing → published`，支持 exact crash recovery；
- Provider 未配置时仍可完整手工创作、导入、保存、恢复和发布。

---

## 6. G9-05B Independent Review｜CORRECTION-02 ACTIVE

最新审核对象：

```text
Formal Base              c492ac4a0eb33ec055f582a2a023066853e2c323
Reviewed correction-01   f789150d584f8f2e538558c0129e0b25e5bbb73e
P0                       0
P1                       2
```

correction-01 已关闭：

1. **导入 section identity：PASS**。blank-only 按 `sectionRef`，Program 重新生成内部 `nodeRef`，两类碰撞均失败关闭。
2. **Source 版本冲突恢复：PASS**。确定性版本冲突会 CAS 恢复 Draft 为 `editable`，保留内容和当前版本，用户可改新版本后重发。

仍阻断：

1. **复杂 typed node 授权仍不够精确**：`nodeRefs` 没有绑定 `nodeKind`，一个被授权 Feature 的 nodeRef 仍可能被模型拿去构造 Module 等另一语义种类。
2. **复杂 Provider operation 的运行时校验仍过浅**：Provider 已是 `unknown[]`，但 dependency / feature / module / UI 等复杂 payload 尚未逐字段验证 enum、boolean、nested shape、array 成员和未知字段；畸形模型输出仍可能先污染 Draft，直到发布时才被 G9-03 拒绝。

当前返修：

```text
repo: zhangchenjia21-dot/sillytavern
branch: agent/g9-05b-creator-core
packet: agent tasks/G9-05B_Codex_correction-02_复杂操作类型校验与TypedNode授权收口_v1.0_2026-08-20.md
packet commit: eb9502b177f289bf5ee8956a454dca4a63e8cd2c
```

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
G9-05B Creator Core Contract             PASS / FROZEN
↓
G9-05B Shared Creator Core Implementation
                                        CORRECTION-02 ACTIVE
↓
GPT exact-SHA Independent Review
↓ PASS only
G9-05C World Creator Vertical
```

G9-05C 当前明确未授权。
