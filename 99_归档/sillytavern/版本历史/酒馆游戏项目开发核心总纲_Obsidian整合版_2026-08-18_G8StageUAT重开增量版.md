---
title: 酒馆游戏项目开发核心总纲
status: superseded
updated: 2026-08-18
scope: G8-Stage-UAT-reopen
superseded_by: 酒馆游戏项目开发核心总纲_Obsidian整合版_2026-08-18_第二版经验复盘增量版.md
---

# 酒馆游戏项目开发核心总纲｜2026-08-18 G8 Stage UAT 重开增量版

> [!warning] 已被覆盖
> 本文件保留为 G8 Stage UAT 首次重开时的历史解释层。
>
> 当前请使用：
>
> `酒馆游戏项目开发核心总纲_Obsidian整合版_2026-08-18_第二版经验复盘增量版.md`
>
> 新文件继续保留本文件的 G8 重开事实，并增加 G8-UAT-01 v1.1、动态五推荐、第二版开发经验、Game-local Canonical Assets 与 G9-02 新成功标准。

---

> 历史解释：Engineering Exit Gate 曾在 `sillytavern@3ad5b419...` PASS，但项目所有者真实 Stage UAT 随后发现 P0/P1 blocker，因此 G8 正式 REOPENED，G9 未授权。

## 0. 当时状态

```text
G1–G7              PASS / CLOSED
G8                  ACTIVE / UAT FIX
WEB-04 Host         PASS / CLOSED
WEB-05 Migration    PASS / CLOSED
WEB-08 Multi-action PASS / CLOSED
Engineering Gate    historical PASS
Stage UAT           FAIL / BLOCKED
Current Next        G8-UAT-01
G9                  NOT AUTHORIZED
```

## 1. 当时新增正式来源

- `G8_StageUAT_交互响应性与空壳世界阻塞发现_v1.1_2026-08-18.md`
- `G8-UAT-01_PlayableRuntimeSeed与NarrativeAuthority收口规格_v1.0_2026-08-18.md`
- `G8阶段收口与G9前置裁定_v1.6_2026-08-18.md`
- `G8网页产品化启动规划_v1.7_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v1.11.md`

## 2. 当时冻结事实

### 2.1 Narrative Authority

```text
Program Final Outcome
=
唯一正式世界变化权威
```

Narrative 不得写出未提交的 Location、Entity、Item、Time、Relationship、Knowledge、Commitment 或 Formal Event。

### 2.2 Minimum Playable T0

AI-assisted Creation 不得只生成：

```text
1 Region + 1 Place + 1 Scene + Player
```

G8 exit minimum：current Scene + visible non-player Character + public reachable destination / Connection + optional Item。

### 2.3 No Phantom Interactable

具体可交互人物、物品、地点必须有 authoritative Runtime ref。

### 2.4 Bounded != Starved

当前任务需要的 player-safe 玩家身份、背景、目标、经历、语言风格、NPC public description 等必须进入 bounded working context。

### 2.5 Narrative Freedom Envelope

允许 ephemeral NPC dialogue / tone / refusal / banter；不自动形成 durable Relationship / Commitment / Knowledge / state change。Player Agency 继续严格。

## 3. 当时关键路径

```text
G8-UAT-01
↓
Focused Engineering + Real Provider Gate
↓
Project Owner Stage UAT Re-run
↓
G8 PASS/CLOSED
↓
G9-01 Compatibility Audit
```

## 4. 延期

WEB-06 / WEB-07 继续 DEFERRED。

## 5. 当时 G9 路线

```text
G9-01 Compatibility Audit
→ G9-02 Runtime Asset Binding + Context Orchestration Foundation
→ G9-03 asset-spec vNext
→ G9-04 Game Asset Adapter / Compiler
→ G9-05 Creator rebuild
```

第 15 号 Runtime Context Orchestration v1.1 继续有效；后续由当前替代文件补充 Game-local Canonical Assets 与 Runtime World Materialization。
