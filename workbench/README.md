# Personal Workbench｜项目治理工作区

本目录是 **Personal Workbench 的 Product / Architecture / Route / Research / Decision / Current Status 事实源**。

Implementation code、tests、runtime、CI、repository-native executable Task 与 Review evidence 由 `zhangchenjia21-dot/Workbench` 拥有。

## Current Stage

当前：

> **Stage 1｜TA-1A Owner UAT Correction — ACTIVE**

Stage 0 Route Freeze 已 PASS。当前 Product / Architecture / Route 不重开；TA-1A 首轮 Owner real-data UAT 暴露 First Usable 可用性问题，先完成 correction，再决定 TA-1A Stage Exit。

```text
G0.1 Product Baseline = PASS
G0.2 Scope = PASS
G0.3 Draft Task Axis = PASS
G0.4 Reference Audit = PASS
G0.5 Revised Route = PASS
G0.6 Route Freeze = PASS
TA-1A Owner UAT = REWORK REQUIRED
TA-1A Stage Exit = BLOCKED
TA-1B = NOT AUTHORIZED
```

当前 UAT correction 决策：

`decisions/D-004_TA-1AOwnerUATCorrectionScope.md`

## AI Start here

1. 读仓库根 `AGENTS.md`；
2. 读 `workbench/AGENTS.md`；
3. 读 `workbench/current/README.md`；
4. 读 `workbench/current/项目状态.md`；
5. 再按任务读取相关 Product / Route / Architecture / Decision / Research；
6. 实现问题进入 `zhangchenjia21-dot/Workbench`。

动态 Stage / Gate 事实以 `current/项目状态.md` 为唯一 current owner；若其它导航文字滞后，以该文件为准并由 00 做 Decision Propagation。

## Current Product

```text
Personal State / Information & Action Desktop

CORE
├─ Today
├─ Plan
└─ Tracks

V0
= Thin Tracks + Usable Plan + Derived Today

Desktop Host = Electron
Canonical Persistence = SQLite
```

原 AI Collaboration V0 保持 `HOLD / HISTORICAL / FUTURE RE-DISCOVERY`。

## Repository map

| 路径 | 角色 | Authority |
|---|---|---|
| `current/` | Product / frozen Route / Current Stage / Gate / Next Action | **当前项目入口** |
| `discussion/` | Owner 明确要求保存的 pre-approval material | 非 Authority |
| `research/` | Reference Audit / feasibility / Spike evidence | Evidence |
| `architecture/` | Owner-approved / frozen architecture contracts | Architecture Authority |
| `decisions/` | Owner-approved project decisions | Decision Authority |
| `../99_归档/workbench/` | historical / superseded / HOLD | 历史证据 |

## Current flow

```text
TA-1A Owner UAT REWORK
→ 00 scope convergence
→ 04 short PWB-001 UAT Correction Addendum
→ Codex correction
→ 05 Independent Re-review
→ Owner focused re-UAT
→ 00 TA-1A Stage Exit
→ only then consider TA-1B
```

## Discussion / Promotion rule

重大 Product / Architecture / Roadmap / Gate 仍遵循：

```text
Working Draft
→ Owner Discussion
→ Owner explicit approval
→ GitHub Promotion
→ 00 Gate Review（如适用）
```

普通 implementation correction 不需要重新走 Product / Architecture Gate；但若 UAT 证明 frozen contract 本身错误，应停止补丁并回 00 判断是否流转 01 / 03。

> GitHub 保存项目事实，Prompt 只传递本轮增量。
