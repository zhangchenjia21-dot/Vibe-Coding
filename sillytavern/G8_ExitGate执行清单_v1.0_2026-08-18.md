# G8｜Exit Gate 执行清单 v1.0

状态：`CURRENT EXECUTION CHECKLIST`  
日期：2026-08-18  
前置：WEB-04 / WEB-05 / WEB-08 均 PASS / CLOSED。

## 1. 目标

本 Gate 只判断 G8 是否可以正式 CLOSED，不新增功能。

判定分级：

- P0：authority / data / safety / atomicity 破坏 → BLOCK；
- P1：G8 关键产品路径不可用 → BLOCK；
- P2：重要但不改变 G9 上游事实 → backlog；
- P3：polish → backlog。

## 2. Freshness

Gate 开始前重新检查：

- `Vibe-Coding/main` current route / decisions；
- `Skill/main` relevant lifecycle skill；
- `sillytavern/main` latest HEAD。

若出现新 current / superseding decision，先执行 Decision Propagation。

## 3. Engineering Gate

### 3.1 Current Main

- branch / remote current；
- no unreviewed newer critical commit；
- protected local dirty/untracked untouched。

### 3.2 Core Regression

至少：

```text
npm run g5:test
npm run g6:test
npm run g7:test
npm run g8:test
npm run g8:ui-host:test
npm run g8:creation-project:test
npm run g8:product-e2e
npm test
```

### 3.3 Build / Safety

```text
npm run typecheck
npm run lint
npm run product:build
npm run launcher:smoke
npm run g2:disclosure
git diff --check
```

### 3.4 Required Verticals

- Creation Project → Game Instance → Product Session；
- Host typed definition → real server → player-safe materialized DTO → Product UI consumer；
- single action → Formal Turn；
- controlled multi-action → one atomic Formal Turn；
- multi-action unsupported/resolution-required → no partial commit / zero RNG；
- crash recovery exactly-once；
- no-key manual Creation；
- configured-key AI-assisted path。

## 4. Product Critical-path Sanity

### Desktop

- Main Menu；
- New Game；
- Session；
- composer；
- Core five surfaces；
- extension surface host path；
- settings / API path。

### Narrow

只要求关键路径可用，无 P0/P1 blocker；不要求完整视觉 polish。

### Keyboard / Focus

- 主菜单可达；
-关键按钮可操作；
- composer 可聚焦/提交；
-明显 focus trap / invisible focus = blocker。

## 5. Stage UAT

项目所有者只做产品体验 UAT，不做内部竞态。

最低建议：

1. 用正式 launcher 进入产品；
2. 创建一局新游戏并进入 Session；
3. 执行一个普通动作；
4. 执行“回到酒馆，然后等到第二天早晨”类两步动作；
5. 确认仍表现为一次输入 / 一个正式回合；
6. 切换几个 Core Surface，确认 Host 表现无明显 blocker；
7. 若 API 已配置，确认 AI-assisted 路径仍可使用；若未配置，manual/no-key 路径仍可完成创建。

本 Gate 不要求 WEB-06 Save Center 或 WEB-07 Undo/Re-input。

## 6. Final Review

必须 GitHub-first 审核：

- exact final SHA；
- required gates；
- current route consistency；
- no unexpected G9 implementation；
- no authority regression；
- deferred backlog 仍明确存在。

## 7. PASS 条件

同时满足：

- Engineering Gate PASS；
- no P0/P1；
- Project Owner Stage UAT PASS；
- route / current source 已同步；

则：

> **G8 PASS / CLOSED**

并进入：

> **G9-01 tavern-asset-spec + tavern-creator Compatibility Audit**

## 8. 禁止扩张

Exit Gate 不得顺手开始：

- G9 Router / Context Orchestration；
- asset-spec vNext；
- Creator；
- Game Asset Adapter；
- Save Center；
- Undo；
- resolved multi-action；
- deep WCAG / visual polish。
