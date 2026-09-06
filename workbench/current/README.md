# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与治理规则 | current v0.8 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — frozen product baseline** |
| [`开发路线.md`](开发路线.md) | Frozen Task Axis / First Usable / Complete V0 / Hardening / Acceptance | **v1.2 — ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Gate / Active Work / Next Action | **current — TA-3 V0 ACCEPTANCE ACTIVE** |

## 当前 Architecture

- [`../architecture/架构方案.md`](../architecture/架构方案.md) — **v1.1 / ROUTE-FROZEN V0 ARCHITECTURE**
- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — G0.3 supporting input

关键 Decision：

- [`../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md`](../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md)
- [`../decisions/D-005_TA-1AStageExit与TA-1BAuthorization.md`](../decisions/D-005_TA-1AStageExit与TA-1BAuthorization.md)
- [`../decisions/D-006_TA-1BStageExit与TA-2Authorization.md`](../decisions/D-006_TA-1BStageExit与TA-2Authorization.md)
- [`../decisions/D-007_TA-2StageExit与TA-3V0AcceptanceAuthorization.md`](../decisions/D-007_TA-2StageExit与TA-3V0AcceptanceAuthorization.md)

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
TA-2 Hardening            = PASS / INTEGRATED
→ TA-3 V0 Acceptance      = ACTIVE
```

Implementation canonical base：

`zhangchenjia21-dot/Workbench main@5fba3cb0ad3016f5c2f259e668f97699f7c7c80a`

## 当前流转

```text
TA-2 Independent Review PASS_WITH_NOTES
→ 00 Stage Exit + main integration
→ TA-3 Final V0 Product Acceptance
→ 若 PASS：V0 Stage Close + next Product Discovery
```

TA-3 不需要默认经过 04 / Codex。它是最终产品判断，不是新功能阶段。

## 当前验收重点

Owner + 00 只需要判断：

- Today / Plan / Tracks 是否形成自然的真实使用路径；
- 当前版本是否比依赖记忆和分散工具更省心；
- 维护成本是否值得；
- 是否还存在必须在 V0 关闭前解决的 blocker；
- TA-2 accepted notes 是否可以作为 V0 已知限制接受。

继续 Deferred：Milestones / AI / Sync / Custom / Plugin / SDK / Knowledge / Habit / Finance / Health / AI Collaboration / cloud sync 等。

## 强制规则

- TA-3 不自动开启新功能；
- 最终验收发现普通实现 bug 时，00 可回 05/Codex 做 focused correction；
- Product / Architecture blocker 才回 01 / 03；
- 若 V0 Acceptance PASS，下一阶段必须重新 Product Discovery，而不是直接开始 Future 功能开发。
