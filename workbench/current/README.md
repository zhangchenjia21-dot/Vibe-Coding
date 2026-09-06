# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与 Stage 0 规则 | current v0.4 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — G0.1 / G0.2 PASS** |
| [`开发路线.md`](开发路线.md) | Revised Task Axis / First Usable / Complete V0 / UAT sequencing | **v1.1 — G0.5 PASS / NOT YET ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Next Action | **current — G0.6 READY** |

## 当前 Architecture

- [`../architecture/架构方案.md`](../architecture/架构方案.md) — **v1.0 / OWNER-APPROVED G0.5 CANDIDATE / G0.5 PASS / NOT ROUTE-FROZEN**
- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — G0.3 question register / supporting input

## 当前 Stage 0 Gate

```text
Product Direction        = APPROVED
G0.1 Product Baseline    = PASS
G0.2 Scope               = PASS
G0.3 Draft Task Axis     = PASS
G0.4 Reference Audit     = PASS
G0.5 Revised Route       = PASS
G0.6 Route Freeze        = READY / OWNER DECISION REQUIRED
Implementation           = NOT AUTHORIZED
```

## G0.5 已收敛候选

- Electron = V0 Desktop Host；
- SQLite = V0 local canonical persistent store；
- Today = derived projection + 极窄当日 acknowledgement；
- immutable internal identities；
- occurrence identity = `SeriesStableID + OriginalScheduledOccurrenceKey`；
- date-only / local wall-time / instant 明确分离；
- Current Vector `[startDate, endDate]` inclusive + no-overlap；
- recurrence = series + dynamic occurrence + exception；
- whole-series edit 清除已有 exceptions 前显式警告 / 确认；
- backup / restore / migration safety contracts；
- system tray V0 lifecycle；
- `TA-1A First Usable → Owner UAT → TA-1B Complete V0`。

## Route Freeze Residual

G0.4 已在真实 Windows packaged runner 中证明 Electron tray lifecycle seam 可行，但 Owner 工作站仍保留一个极小 focused check：通知区图标实际可见、真实鼠标恢复 / focus、close→tray、tray real exit。

该 residual 可在 G0.6 前完成，或由 00 + Owner 明确接受为 `TA-1A Exit 前必须完成` 的 residual。

## 当前流转

```text
G0.5 PASS
→ 00 + Owner: G0.6 Route Freeze
→ 若 PASS：Implementation AUTHORIZED
→ 04: TA-1A executable Task Packet
→ TA-1A First Usable
→ Owner Real-data UAT
→ TA-1B Complete V0
→ Complete V0 UAT
→ Correction / Hardening
→ V0 Acceptance
```

## 强制规则

- 未经 00 + Owner 记录 G0.6 Route Freeze PASS，04 不得启动正式 Coding；
- implementation code / tests / runtime / executable Task 进入 `zhangchenjia21-dot/Workbench`；
- Future Milestones / AI / Sync / Custom / Plugin / SDK 等继续 Deferred；
- Route Freeze 不代表字段级 schema、ORM、UI framework、installer/updater 等实现细节全部冻结；这些只在不违反 frozen contracts 的范围内由 implementation task 选择。