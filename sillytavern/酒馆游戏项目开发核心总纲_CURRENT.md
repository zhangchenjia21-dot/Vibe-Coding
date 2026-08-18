---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-18
current_path: sillytavern/酒馆游戏项目开发核心总纲_CURRENT.md
---

# 酒馆游戏项目开发核心总纲｜CURRENT

## 0. 当前状态

```text
G1–G7                   PASS / CLOSED
G8                       ACTIVE / UAT FIX
WEB-04 Host              PASS / CLOSED
WEB-05 Migration         PASS / CLOSED
WEB-08 Multi-action      PASS / CLOSED
G8-UAT-01                PASS / CLOSED
Stage UAT rerun          FAIL / BLOCKED
Current Code Baseline    52d0421bc58449ac8763681816bc7a84de93b385
Current Next             G8-UAT-02
G9                       NOT AUTHORIZED
```

项目所有者在 `52d0421...` 的第二轮真实 Stage UAT 证明：G8-UAT-01 已关闭 Narrative Authority / Phantom / Dynamic Five 第一层问题，但 Creation → Runtime 仍使用 generic deterministic placeholder 冒充 semantic materialization。

---

## 1. 当前 active 正式来源

### 当前阶段 / UAT

- `G8_StageUAT_交互响应性与空壳世界阻塞发现_v1.1_2026-08-18.md`
- `G8-UAT-01_PlayableRuntimeSeed与NarrativeAuthority收口规格_v1.1_2026-08-18.md`
- `G8_StageUAT_语义物化与活世界阻塞发现_v1.0_2026-08-18.md`
- `G8-UAT-02_GameLocalSemanticMaterialization与活世界收口规格_v1.0_2026-08-18.md`
- `G8阶段收口与G9前置裁定_v1.8_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.0.md`

### 编号核心

- `11_酒馆游戏_G5长期连续性与玩家授权收口裁定_v1.0_2026-08-15.md`
- `12_酒馆游戏_G6_SaveContinueRestore收口裁定_v1.0_2026-08-15.md`
- `13_酒馆游戏_G7_CrashResumeRecovery收口裁定_v1.0_2026-08-16.md`
- `14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`

### 通用治理

- `项目经验/第二版_SillyTavern_项目构建经验复盘_Obsidian版_2026-08-18.md`
- `Skill/skill/gpt/lifecycle-dev-process/SKILL.md`：current v2.0
- `Skill/skill/gpt/lifecycle-templates/SKILL.md`：current v1.8

---

## 2. 本轮新确认的根因

### 2.1 Minimum Count != Semantic Fidelity

G8-UAT-01 证明了：

```text
NPC >= 1
Destination >= 1
optional Item >= 1
```

但没有证明这些实体正确对应 Creation 语义。

当前 production compiler 实际可把：

```text
导师 / 同行者 / 对手
→ 1 个“附近的人”

多个工具 / 日用品 / 钱币
→ 1 个“随身物品”

需要可探索地点
→ 1 个“附近区域”
```

因此数量 Gate 通过，但真实玩家世界仍是语义占位壳。

### 2.2 Creation Field != Entity Record

Creation 自由文本必须先区分：scalar definition、structured list、semantic guidance、world constraint、materialization brief、runtime state seed、product metadata。

禁止继续：

```text
free-text string
→ 截取第一段
→ 创建一个 Character / Item / Scene
```

### 2.3 AI-assisted Creation 的 zero-call 约束重新解释

Manual / no-key path 继续允许 deterministic Final Create。

但 configured AI-assisted Creation 可以使用 bounded typed semantic materialization call；不能为了保持零调用而生成语义错误 placeholder。

### 2.4 Living World 已成为 G8 Stage Playability prerequisite

完整 Asset Ecosystem 仍留 G9；但以下最小能力必须提前：

```text
Creation semantic brief
→ concrete Game-local opening assets
→ proactive opening beat
→ plausible unknown local NPC/Place materialization
→ stable identity / atomic commit
→ Narrative / Product projection
```

否则 AI RPG 仍然是静态世界 + 文本描写。

---

## 3. 当前 P1 blocker cluster｜G8-UAT-02

必须一次性关闭：

1. Creation Field Semantic Audit；
2. typed Creation Materialization Plan；
3. concrete opening NPC / Place / Item cardinality；
4. minimal Game-local canonical seed；
5. proactive opening beat；
6. bounded JIT local Character / Place materialization；
7. Inventory semantic fidelity；
8. Information Surface 只显示 information / knowledge，不显示 generic action ledger；
9. Player Profile 完整投影 identity/background/goals/past/personality/language style；
10. Dynamic Five 改为消费新的 concrete materialized world。

---

## 4. 继续保留的 G8-UAT-01 成果

不得回滚：

- Program Final Outcome authority；
- non-world-changing downgrade fail-closed；
- locationAuthority / interactableAuthority；
- No Phantom Interactable；
- Bounded != Starved；
- Ephemeral NPC Dialogue Freedom；
- Player Agency；
- server-side grounded exactly-five suggestions；
- controlled multi-action / Save / Restore / Recovery。

新世界内容必须先 materialize + commit，再 Narrative realization。

---

## 5. Product IA 修复

### Information

```text
Information Surface
!= Formal Event Journal
```

不得显示 generic：

- `行动已完成`
- `你前往XX`

Timeline / Journal 可以保留行动记录。

### Player Profile

Player Detail 至少显示：

- identity / calling
- public background
- goals / attachments
- important past
- personality
- language style

左侧摘要可以精简，但不能只剩一个名字和一句背景。

### Inventory

Narrative 中列出的具体随身物品必须与 authoritative inventory / Product Items Surface 一致。

---

## 6. 当前关键路径

```text
G8-UAT-01                 PASS / CLOSED
↓
Stage UAT rerun           FAIL
↓
G8-UAT-02                 ACTIVE / REQUIRED
↓
Independent Review
↓
Project Owner Stage UAT
↓
G8 PASS / CLOSED
↓
G9-01 Compatibility Audit
```

G9 external asset-spec / Creator / Adapter 仍未授权。

---

## 7. G9 DAG 传播

G8-UAT-02 会提前证明第 16 号裁定的最小 internal production vertical；G9-02 后续仍负责把该能力扩展到 Source Asset bind / external asset ecosystem，并与第 15 号 Context Orchestration 汇合。

```text
G9-01 Compatibility Audit
→ G9-02 Runtime Asset Binding
      + Context Orchestration
      + full Game-local Asset Layer
      + World Materialization Foundation
→ G9-03 asset-spec vNext
→ G9-04 Adapter / Compiler
→ G9-05 Creator
```

---

## 8. 当前 Next

> **G8-UAT-02 implementation。**

当前版本不再继续 Stage UAT；项目所有者无需重复测试同一个 placeholder 世界。
