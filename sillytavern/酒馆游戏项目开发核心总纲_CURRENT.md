---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-18
current_path: sillytavern/酒馆游戏项目开发核心总纲_CURRENT.md
supersedes: 酒馆游戏项目开发核心总纲_Obsidian整合版_2026-08-18_第二版经验复盘增量版.md
---

# 酒馆游戏项目开发核心总纲｜CURRENT

> [!abstract] 当前解释层
> 本文件采用固定 `CURRENT` 路径。后续项目总纲更新默认直接更新本文件，不再为每次小增量生成新的时间戳“总纲副本”。历史解释层统一进入 `99_归档/` 与 Git history。

## 0. 当前状态

```text
G1–G7                   PASS / CLOSED
G8                       ACTIVE / UAT FIX
WEB-04 Host              PASS / CLOSED
WEB-05 Migration         PASS / CLOSED
WEB-08 Multi-action      PASS / CLOSED
Engineering Exit Gate    historical PASS
Stage UAT                FAIL / BLOCKED
Current Next             G8-UAT-01 implementation
G9                       NOT AUTHORIZED
```

当前实现基线仍由 `G8-UAT-01 v1.1` 约束；本次仓库整理与版本治理不扩展其工程 Scope。

---

## 1. 当前 active 正式来源

### 当前阶段 / 路线

- `G8_StageUAT_交互响应性与空壳世界阻塞发现_v1.1_2026-08-18.md`
- `G8-UAT-01_PlayableRuntimeSeed与NarrativeAuthority收口规格_v1.1_2026-08-18.md`
- `G8网页产品化启动规划_v1.8_2026-08-18.md`
- `G8阶段收口与G9前置裁定_v1.8_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.0.md`

### 编号核心

- `11_酒馆游戏_G5长期连续性与玩家授权收口裁定_v1.0_2026-08-15.md`
- `12_酒馆游戏_G6_SaveContinueRestore收口裁定_v1.0_2026-08-15.md`
- `13_酒馆游戏_G7_CrashResumeRecovery收口裁定_v1.0_2026-08-16.md`
- `14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`

### 通用开发治理

- `项目经验/AI驱动项目全生命周期开发流程规范_v1.5_2026-08-18.md`
- `项目经验/第一版_SillyTavern_项目构建经验复盘_Obsidian版.md`
- `项目经验/第二版_SillyTavern_项目构建经验复盘_Obsidian版_2026-08-18.md`
- `Skill/skill/gpt/lifecycle-dev-process/SKILL.md`：current v2.0
- `Skill/skill/gpt/lifecycle-templates/SKILL.md`：current v1.8

历史同族版本不再列入 current source；统一见 `../99_归档/`。

---

## 2. 当前产品与架构核心事实

### 2.1 Engineering Correctness != Playability Completeness

G8 Stage Close 必须同时证明：

```text
Engineering Correctness
+ Real Creation Instance
+ Playability Completeness
+ Integration of Meaning
+ Project Owner UAT
```

Rich Fixture 不能替代正式 Creation → Real Game Instance → Multi-turn UAT。

### 2.2 Narrative / UI Affordance 必须有真实 referent

```text
Visible as concrete interactable
→ authoritative referent + current capability
```

Narrative 不得宣称 Program 未提交的 durable world change；推荐输入不得引用不存在或隐藏的实体 / 地点。

### 2.3 Runtime Context

正式冻结：

```text
Asset Library
!= Package Included
!= Feature Enabled
!= Module Enabled
!= Runtime Relevant
!= Model Visible Working Set
```

以及：

```text
Full Asset Definition != Prompt Payload
Dependency Graph != Context Inclusion Graph
Domain Active != Full Definition Registry Visible
Background deterministic progression != Model Activation
Bounded != Starved
```

模型上下文目标是**最小但充分**，不是最短。

### 2.4 Source Asset / Game-local Asset / Runtime State

```text
Source Asset Library
↓ snapshot / bind
Game-local Canonical Assets
↓ instantiate / project
Authoritative Runtime State
```

- Source Asset 不被本局反写；
- Game-local Canonical Assets 是本局可持续演化的世界定义；
- Runtime State 管理位置、数值、持有、timer、Turn / Event 等运行状态；
- Model 可以 author candidate / typed patch；
- Program / Domain Owner 负责 validation、stable identity、atomic commit、persistence、recovery 与 disclosure boundary。

### 2.5 World Growth != Prompt Growth

当局游戏资产和 Runtime 世界可以持续增长；普通回合 Context 仍必须保持 current-relevant、bounded、purpose-built。

---

## 3. 当前 G8 blocker cluster

当前任务：

`G8-UAT-01｜Playable Runtime Seed + Narrative Authority + Dynamic Five Suggested Inputs`

必须收口：

1. P0 Narrative Authority；
2. Minimum Playable Runtime T0；
3. No Phantom Interactable；
4. Bounded Rich Narrative Context；
5. Ephemeral conversational freedom without durable unauthorized mutation；
6. exactly 5 context-sensitive SuggestedPlayerInput；
7. suggestion click only prefills composer；
8. suggestion provider failure non-blocking；
9. Real DeepSeek targeted Gate；
10. Project Owner Stage UAT re-run。

---

## 4. G9 当前 DAG

```text
G9-01 Compatibility Audit
→ G9-02 Runtime Asset Binding
      + Context Orchestration Foundation
      + Game-local Canonical Asset Layer
      + Runtime World Materialization Foundation
→ G9-03 asset-spec vNext Machine Contract
→ G9-04 Game Asset Adapter / Compiler
→ G9-05 Creator rebuild
```

G9-02 必须先用内部 / handwritten runtime profiles 证明真实能力，再冻结 G9-03 machine schema。

---

## 5. 当前 Next

```text
G8-UAT-01 implementation
↓
GitHub-first Independent Review
↓
Project Owner Stage UAT re-run
↓
G8 PASS / CLOSED
↓
G9-01 Compatibility Audit
```

---

## 6. 文档治理与版本规则

### Active-only

同一文档族在 active 目录只能保留一个 current。superseded 旧版移入：

`../99_归档/`

独立有效的编号核心不因日期旧而归档。

### Rolling current

高频滚动解释层优先固定路径，例如本文件 `*_CURRENT.md`，避免每个小变化生成新文件。

### 人类可读版本号

本项目治理文档不采用 SemVer 多位 minor，采用一位小版本：

```text
v1.8 → v1.9 → v2.0 → v2.1
```

`N` 只允许 `0–9`；不得再生成 `v1.10 / v1.11 / v1.12`。版本号只表示演进顺序，重大变化由 `status / supersedes / change_class / ADR` 说明。
