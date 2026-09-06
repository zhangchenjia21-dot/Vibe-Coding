# D-003｜V0 Route Freeze 与 TA-1A Implementation Authorization

状态：**APPROVED / CURRENT**  
日期：2026-09-06  
Owner：用户（最终 Product Owner）  
Gate Owner：00｜项目总控

## Decision

Owner 正式批准：

1. `workbench/current/开发路线.md` v1.1 的 Revised Route；
2. `workbench/architecture/架构方案.md` v1.0 的 V0 Architecture Candidate；
3. 将二者提升为当前 V0 的 frozen route / architecture；
4. `G0.6 Owner Route Freeze = PASS`；
5. `Implementation = AUTHORIZED`，当前首先授权进入 `TA-1A First Usable Core Vertical` 的正式 Task Shaping / Dispatch；
6. Owner 工作站的 Electron tray focused check 不再阻塞 Route Freeze，但必须在 **TA-1A Exit 前**完成。

## Frozen Core

```text
Product
= Personal State / Information & Action Desktop

CORE
= Today / Plan / Tracks

V0
= Thin Tracks + Usable Plan + Derived Today

Desktop Host
= Electron

Canonical Persistence
= SQLite

First Usable
= TA-1A First Usable Core Vertical
→ Owner real-data UAT
→ TA-1B Complete V0
```

冻结的关键合同还包括：

- Tracks / Plan authoritative，Today 主要 derived；
- Today acknowledgement 只保存当日视觉确认；
- immutable internal IDs；
- occurrence identity = `SeriesStableID + OriginalScheduledOccurrenceKey`；
- `date-only != timestamp`，recurrence 使用 local wall-time semantics；
- Current Vector `[startDate, endDate]` inclusive 且禁止 overlap；
- recurrence = series + dynamic occurrence + exception；
- whole-series edit 清除既有 exceptions 前必须提示并确认；
- consistent SQLite backup；
- restore validation + pre-restore safety snapshot + staged replacement；
- schema version + ordered transactional migrations + upgrade safety backup；
- system tray 为 V0 Required；close → tray；tray restore / real exit；开机启动 Deferred。

## Implementation Authorization Boundary

本决定授权的是：

> **进入 04｜Agent 任务调度，为 TA-1A 生成正式 executable Task Packet，并按已冻结路线开始实现。**

不表示：

- TA-1B 可以绕过 TA-1A UAT 自动开工；
- implementation Agent 可以自行修改 frozen Product / Architecture / Route；
- Deferred 功能自动进入 Scope；
- 具体 SQLite binding / ORM、UI component library 等未冻结 implementation detail 自动升级为 Architecture Decision。

TA-1B 仍依赖 TA-1A Engineering / Independent Review / Owner real-data UAT 的 Stage Exit。

## Accepted Residual

Owner 明确接受将以下 focused check 后移到 TA-1A Exit 前：

```text
Owner Windows machine
→ notification-area icon 实际可见
→ 鼠标点击恢复 / focus
→ close → hide to tray
→ tray real exit
```

G0.4 packaged Windows evidence 已证明对应 technical seam 可行，因此该 residual 不再阻塞 Route Freeze；若 Owner-machine check 失败，则 TA-1A 不得 Exit，并应进入 05 / 03 做根因与必要路线回流。

## Deferred Remain Deferred

- 开机启动；
- Milestones；
- GitHub / AI Track Update；
- Google Calendar sync；
- Launch Target；
- Custom Functions；
- Knowledge / Habit / Finance / Health；
- AI Collaboration；
- Plugin / SDK / Page Builder；
- completion analytics / streak / scoring；
- recurrence “这一次及以后”；
- multi-device / cloud sync。

## Consequences

- Stage 0 Pre-Implementation Alignment 正式关闭；
- 下一主线切换到 `04｜Agent 任务调度`；
- 04 应为 TA-1A 形成正式 Task Packet；
- 05 在 TA-1A implementation 后承担 Independent Review / Reality Gate / Owner UAT 设计；
- frozen route 如需重大修改，必须重新进入 Owner Discussion / Architecture / Gate 流程，不得在实现中静默漂移。
