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
G9-05B Core Implementation    AUTHORIZED / NEXT
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
c492ac4a0eb33ec055f582a2a023066853e2c323
```

当前资产仓库主线：

```text
zhangchenjia21-dot/sillytavern-assets main
ffb3f15d959249ff2115edb99bf8cc5ca10bbe9d
```

G9-04 真实资产验证使用的冻结基线仍为：

```text
968175e6c3fb3545b7c2907b65089c7e1dbb40a0
```

后续 `ffb3f15d...` 仅为阶段导航同步，不改变 G9-04 真实样本证据。

当前核心路线：`酒馆游戏新版主体重建总路线 v2.3.md`。

当前下一步：

> **G9-05B Creator Core Implementation。** 由 Codex 在不先做完整 UI 的前提下实现共享 Creator Draft、导入原件、任务级变更集/撤销、正式 Source Asset Library、确定性发布编译与发布恢复基础；首个发布证明使用 synthetic World Draft，Provider 调用为 0。通过独立审核后才进入 G9-05C 世界包 Creator 真实产品纵向。

---

## 1. 当前正式 Authority

### Runtime / G9

- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`
- `17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`
- `G9-02_IntegratedClosure_IndependentReview_最终收口_v1.0_2026-08-20.md`
- `G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md`
- `G9-03A_RuntimeModuleBinding与TypedConfig增量裁定_v1.0_2026-08-20.md`
- `G9-03_IndependentReview_最终收口_v1.0_2026-08-20.md`
- `G9-04_LegacyMarkdownAdapterCompilerBinding规格_v1.0_2026-08-20.md`
- `G9-04_IndependentReview_最终收口_v1.0_2026-08-20.md`

### Asset / Library / Creator

- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.2_2026-08-19.md`
- `18A_酒馆游戏_资料库资源层协议护栏增量裁定_v1.0_2026-08-19.md`
- `19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md`
- `G9-05A_Creator基础模型与创作稿导入产品架构裁定_v1.0_2026-08-20.md`
- `G9-05B_CreatorCore草稿导入发布内部合同规格_v1.0_2026-08-20.md`
- `酒馆游戏新版主体重建总路线 v2.3.md`

### 执行治理

- `G9及后续阶段_Agent资源分配与Codex默认代码协作裁定_v1.1_2026-08-20.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`
- `AgentTaskPacket_GitHub原生交付增量裁定_v1.0_2026-08-20.md`
- `Skill/main/skill/gpt/agent-task-packet/SKILL.md` v1.1

默认代码执行 Agent = Codex；只有项目所有者明确指定时才使用 Grok。

面向项目所有者沟通时中文优先；只有代码字段名、接口名、文件路径、命令、提交号等必须精确引用的内容保留英文。

---

## 2. 不可回滚架构边界

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
Player-known
!= Runtime Relevant
!= Model Visible
```

继续保持：程序拥有最终结果权、玩家行动权、开放尝试、No Phantom、私密/公开边界、按需物化、Save / Restore / Branch、Crash / Resume / Recovery 与 exactly-once。

---

## 3. G9-02｜PASS / CLOSED

G9-02 建立并关闭长期运行时基础：

```text
Source Asset Descriptor
→ per-game binding / lineage
→ Game-local Canonical Instance / definitionRevision
→ Domain Module Host
→ bounded routing / selected-only projection
→ Formal Turn authority
→ Save / Restore / Branch / Recovery
```

G9-03 及以后全部复用这套轨道，不建立第二套身份、状态、路由、变更或持久化权威。

---

## 4. G9-03｜PASS / CLOSED

最终实现：

```text
5da2294a9d21585665167e69307d9c693427582d
```

G9-03 已冻结并实现统一资产机器协议：

```text
TavernAssetV1
= world | character | expansion | library
```

包括稳定 `assetRef`、版本、SHA-256、依赖、Bundle、精确 Game Asset Manifest、拓展包 Feature / Module / `runtimeModuleRef` 接线、Typed Config 校验和资料库协议边界。

---

## 5. G9-04｜PASS / CLOSED

最终实现 / integrated main：

```text
c492ac4a0eb33ec055f582a2a023066853e2c323
```

最终审核：

```text
P0 = 0
P1 = 0
G9-04 = PASS / CLOSED
```

已证明：

```text
真实 canonical Markdown
→ 确定性 Adapter / Parser
→ TavernAssetV1
→ G9-03 validate / canonical SHA-256
→ exact TavernGameAssetManifestV1
→ SourceAssetDescriptor
→ Game-local sourceLineage
→ Save / Restore
```

并保持：不猜资产身份、完整旧正文默认 private、Character Binding 不等于物化、Source 新版本不静默改旧游戏、Library 不进入本局主资产 lineage、重复主资产失败关闭。

---

## 6. 资料库边界

```text
三类主资产
= 世界包 + 角色卡 + 拓展包

资料库
= Reference Resource Layer
!= 第四类主资产
!= Runtime Truth
```

资料库完整产品页面、Runtime 检索/索引、模型资料提供器、Creator 资料库编辑器继续后置到三类主资产端到端闭环之后。

---

## 7. G9-05A｜Creator Foundation｜PASS / FROZEN

顶层产品入口冻结：

```text
我的资产库
├── Creator
├── 世界包
├── 角色卡
└── 拓展包
```

三种创作起点统一进入同一 Draft：

```text
从空白开始
外部创作稿导入
已有正式资产创建新版本
↓
Creator Draft
```

永久保持：

```text
AI Chat
!= Creator Draft

Creator Draft
!= Saved Source Asset

AI can edit Draft
!= AI can publish asset
```

导入 AI 规则：

```text
明确、唯一、由原文支持
→ 可填入空白 Draft 字段

信息不足 / 未提及 / 冲突 / 多种合理解释
→ 字段留空或原值不变
→ 原始证据保留
```

导入阶段只整理已有材料，不自动创作缺失内容。

---

## 8. G9-05B｜Creator Core Contract｜PASS / FROZEN

正式规格：

`G9-05B_CreatorCore草稿导入发布内部合同规格_v1.0_2026-08-20.md`

### 8.1 两个长期数据 Owner

```text
Creator Draft Store
= 可编辑草稿 / 导入证据 / ChangeSet / Undo 工作状态 Owner

Source Asset Library Store
= 已通过 G9-03 校验的正式本地 Source Asset Owner
```

两者都不是 Runtime State。

### 8.2 CreatorDraftV1

核心身份：

```text
draftRef
assetType
targetAssetRef
targetVersion?
origin
lifecycle
revision
content
workState
```

新资产的 `targetAssetRef` 由 Program 创建一次并保持稳定，禁止从标题、文件名或 AI 输出猜测；已有资产新版本精确继承基础快照 `assetRef`。

生命周期：

```text
editable → publishing → published
```

所有变更使用 revision/CAS 自动保存，迟到 AI 结果不得覆盖新修订。

### 8.3 导入原件与确定项填写

`.md` / `.txt` 原件保存 exact hash、可恢复内容和稳定 `segmentRef`。

AI 导入输出分为：

```text
certainAssignments
unresolvedItems
conflicts
```

只有有真实 evidence segment、目标为空且允许导入填写的确定项可落入 Draft。已有用户内容不会被导入整理静默覆盖。

### 8.4 ChangeSet / Undo

每个用户或 AI 创作任务形成 Program-owned change set。Undo 通过 inverse change 创建新 revision，不回退 revision 数字；若目标后来已被修改则失败关闭，避免抹掉新内容。

### 8.5 正式 Source Asset Library

只接受通过 G9-03 验证的 `TavernAssetV1`。

```text
assetRef + version 不存在
→ append

已存在且 digest 相同
→ idempotent success

已存在但 digest 不同
→ SOURCE_ASSET_VERSION_CONFLICT
```

绝不覆盖旧版本。

### 8.6 发布

```text
exact Draft revision
→ deterministic publication compiler
→ existing computeAssetDigest()
→ existing validateAndVerifyAsset()
→ SourceAssetLibraryStore.appendValidated()
```

发布使用可恢复状态机，支持“Source 已写入但 Draft 尚未确认”的崩溃恢复与精确认领。

发布 Source 不自动绑定现有游戏。

---

## 9. G9-05B｜Core Implementation｜AUTHORIZED / NEXT

首次实现只做共享基础，不先做四入口最终视觉页面。

Codex 必须实现：

1. `src/资产库/` 正式 Source Asset Store 合同 + 内存/SQLite 实现；
2. `src/资产创作/` Draft / Import / ChangeSet / Publish 合同；
3. Draft revision/CAS 与持久化；
4. Markdown / text 原件提取与持久化；
5. AI 导入结构化结果的 Program 校验和局部应用；
6. blank-only import / protected target / stale result；
7. task-level Undo conflict gate；
8. Draft → `TavernAssetV1` 确定性发布；
9. Source Asset append-only collision gate；
10. publish retry/recovery；
11. no-Provider 手工路径；
12. synthetic World Draft 发布证明；
13. Provider 调用 = 0。

通过 exact-SHA Independent Review 后：

```text
G9-05B Creator Core Foundation = PASS / CLOSED
G9-05C World Creator Vertical = AUTHORIZED / NEXT
```

---

## 10. 当前 DAG

```text
G9-02 Runtime Foundation
PASS / CLOSED
↓
G9-03 Unified Asset Protocol
PASS / CLOSED
↓
G9-04 Adapter / Compiler / Binding
PASS / CLOSED
↓
G9-05A Creator Foundation
PASS / FROZEN
↓
G9-05B Creator Core Contract
PASS / FROZEN
↓
G9-05B Shared Creator Core Implementation
AUTHORIZED / NEXT
↓
G9-05C World Creator Vertical
↓
Character → Expansion Creator 纵向
↓
三类主资产“我的资产库 → 创建游戏 → 完整游玩”端到端闭环
↓
Primary Asset End-to-End Closure Gate
↓
Library Product Increment
```
