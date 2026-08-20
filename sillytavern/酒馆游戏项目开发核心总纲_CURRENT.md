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
G9-05 Core Contract Design    AUTHORIZED / NEXT
```

当前实现主线：

```text
zhangchenjia21-dot/sillytavern main
c492ac4a0eb33ec055f582a2a023066853e2c323
```

当前资产事实源：

```text
zhangchenjia21-dot/sillytavern-assets main
968175e6c3fb3545b7c2907b65089c7e1dbb40a0
```

当前核心路线：`酒馆游戏新版主体重建总路线 v2.3.md`。

当前下一步：

> **G9-05 Creator Core Contract Design。** 在已经冻结的“我的资产库四入口 + Creator Draft + 外部创作稿导入 + 用户显式发布”产品边界上，继续冻结共享 Draft、导入证据、冲突/未整理项、Typed Creator Tools、Draft→TavernAssetV1 发布转换与草稿持久化内部合同；随后再由 Codex 实现共享 Creator Core，而不是先做三个独立页面。

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

G9-02 建立并关闭了长期运行时基础：

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

G9-03 冻结并实现了统一资产机器协议：

```text
TavernAssetV1
= world | character | expansion | library
```

关键能力包括：稳定 `assetRef`、版本、SHA-256、依赖、Bundle、精确 Game Asset Manifest、拓展包 Feature / Module / `runtimeModuleRef` 接线、Typed Config 校验、资料库稳定条目/来源/四类受众资格以及失败关闭验证。

永久区分：

```text
ExpansionModuleV1.moduleRef
= Source declaration identity

ExpansionModuleV1.runtimeModuleRef
= Program Runtime module identity

RuntimeDomainModuleBinding.moduleRef
= runtimeModuleRef
```

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

G9-04 已证明真实资产链：

```text
真实 canonical Markdown
→ 显式 Legacy Adapter Profile
→ 确定性 YAML / Markdown 解析
→ TavernAssetV1
→ G9-03 validator + canonical SHA-256
→ exact TavernGameAssetManifestV1
→ binding compiler
→ SourceAssetDescriptor
→ hidden Game-local binding anchors
→ SQLiteRuntimeStore.bootstrap()
→ exact sourceLineage
→ Save / Restore
```

关键边界：

- `assetRef` 必须由显式适配档案提供，禁止从文件名、标题、别名或 Wikilink 模糊猜测；
- 旧 Markdown 完整正文默认 private，public 只能由精确章节选择器放行；
- Character Card 的绑定不等于角色实体物化，No Phantom 已验证；
- Source v2 不静默改写旧 Manifest / 旧游戏 lineage；
- Library 在 G9-04 只做协议与交叉引用完整性证明，不进入本局主资产 `sourceLineage`；
- Library `relatedRefs` 只做精确身份解析；
- Character / Expansion 重复主绑定必须失败关闭；
- Provider 调用 = 0。

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

G9-03 已完成资料库协议；G9-04 已完成最小 parse / validate / canonical round-trip / cross-reference proof。

资料库产品页面、Runtime 检索/索引、模型资料提供器、Creator 资料库编辑器继续后置到三类主资产端到端闭环之后。

---

## 7. G9-05A｜Creator Foundation｜PASS / FROZEN

G9-05A 正式冻结：

```text
我的资产库
├── Creator
├── 世界包
├── 角色卡
└── 拓展包
```

Creator 内置于酒馆本体，但不是 Runtime authority。世界包、角色卡、拓展包页面只管理正式资产并把“新建 / 创建新版本”路由到同一个 Creator，不建立三套编辑器。

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

外部创作稿导入规则：

```text
明确、唯一、由原文支持
→ AI 可填入 Draft

信息不足 / 材料未提及 / 存在冲突 / 多种合理解释
→ 正式字段留空
→ 原始证据保留在 Creator 工作区
```

导入阶段只整理已有材料，不自动创作缺失内容。用户明确要求继续创作后，才进入 #19 的正常 AI 创作模式。

外部文件始终只是数据：

```text
Imported Content
!= Creator Instruction
!= Tool Authorization
!= Publish Authorization
```

发布链必须复用 G9-03：

```text
Creator Draft
→ Program-owned deterministic conversion
→ G9-03 validate / canonicalize / integrity
→ TavernAssetV1
→ 用户显式发布到“我的资产库”
```

发布新 Source 不自动绑定现有游戏。

---

## 8. G9-05｜当前下一步

现在不先做页面，而先冻结共享 Creator Core 的内部合同：

1. Draft identity / lifecycle / revision；
2. 三种 origin；
3. 部分结构化资产内容；
4. 导入来源、已映射证据、待整理项、冲突项；
5. Typed Creator Tools 与授权边界；
6. Undo/change-set；
7. 草稿持久化与恢复；
8. Draft → `TavernAssetV1` 确定性发布转换；
9. 无 Provider 时的完整手工路径。

合同冻结后才由 Codex 实现共享 Creator Core，并优先用世界包跑第一个真实纵向；角色卡和拓展包随后复用同一套核心。

---

## 9. 当前 DAG

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
G9-05 Creator Core Contract Design
AUTHORIZED / NEXT
↓
共享 Creator Core 实现
↓
World → Character → Expansion Creator 纵向闭环
↓
三类主资产“我的资产库 → 创建游戏 → 完整游玩”端到端闭环
↓
Primary Asset End-to-End Closure Gate
↓
Library Product Increment
```
