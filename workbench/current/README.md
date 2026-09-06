# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与治理规则 | current v0.7 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — frozen product baseline** |
| [`开发路线.md`](开发路线.md) | Frozen Task Axis / First Usable / Complete V0 / Hardening / Acceptance | **v1.2 — ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Gate / Active Work / Next Action | **current — TA-2 AUTHORIZED FOR DISPATCH** |

## 当前 Architecture

- [`../architecture/架构方案.md`](../architecture/架构方案.md) — **v1.1 / ROUTE-FROZEN V0 ARCHITECTURE**
- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — G0.3 supporting input

关键 Decision：

- [`../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md`](../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md)
- [`../decisions/D-005_TA-1AStageExit与TA-1BAuthorization.md`](../decisions/D-005_TA-1AStageExit与TA-1BAuthorization.md)
- [`../decisions/D-006_TA-1BStageExit与TA-2Authorization.md`](../decisions/D-006_TA-1BStageExit与TA-2Authorization.md)

## 当前产品与路线

```text
Personal State / Information & Action Desktop
CORE = Today / Plan / Tracks
V0 = Thin Tracks + Usable Plan + Derived Today
Desktop Host = Electron
Canonical Persistence = SQLite
```

路线：

```text
TA-1A First Usable        = PASS / INTEGRATED
TA-1B Complete V0         = PASS / INTEGRATED
→ TA-2 Hardening          = CURRENT / AUTHORIZED FOR DISPATCH
→ TA-3 V0 Acceptance      = NOT AUTHORIZED
```

Implementation canonical base：

`zhangchenjia21-dot/Workbench main@d990ef0f1807a2a0cacd91ba9447ebef5be692b8`

## TA-2 当前授权边界

TA-2 只做可靠性 / 失败模式硬化：

- adjacent state / failure audit；
- restart / migration / restore regression；
- recurrence identity / exception / acknowledgement cleanup audit；
- Month / Week / Today canonical consistency regression；
- tray / window lifecycle regression；
- repeated-use / long-session appropriate checks；
- known limitations；
- 只修真实 finding，不新增产品能力。

05 留下的 stale recurrence acknowledgement 非阻塞 note 进入 TA-2 检查，但不是强制新功能。

继续 Deferred：Milestones / AI / Sync / Custom / Plugin / SDK / Knowledge / Habit / Finance / Health / AI Collaboration / cloud sync 等。

## 当前流转

```text
TA-1B Review + Owner UAT PASS
→ 00 Stage Exit + main integration
→ 04 TA-2 Hardening Task Packet
→ Codex audit / focused fix
→ 05 Independent Review
→ focused Owner retest only where behavior changed
→ 00 TA-2 Stage Exit / TA-3 decision
```

## 强制规则

- TA-2 不是新功能阶段；
- 04 只把 frozen TA-2 hardening 目标转成 executable work；
- Codex 完成后直接去 05，不经过 04 中转；
- 如果没有需要改代码的问题，允许以 hardening evidence 结束；
- Product / Architecture / Route blocker 才回 00 决定是否流转 01 / 03；
- implementation code / tests / runtime / executable Task 由 `zhangchenjia21-dot/Workbench` 拥有。
