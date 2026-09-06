# Personal Workbench｜Owner Decisions

本目录保存 **已经由 Product Owner 明确批准、需要长期生效或影响多个工作流的项目级正式裁定**。

## Current decisions

- [`D-001_暂停AI协作V0并重开产品方向.md`](D-001_暂停AI协作V0并重开产品方向.md) — 旧 AI Collaboration V0 退出 current、整体归档并重新打开 Product Direction。
- [`D-002_正式Pivot到PersonalStateCore.md`](D-002_正式Pivot到PersonalStateCore.md) — 正式 Pivot 到 Personal State / Information & Action Desktop；Today / Plan / Tracks 成为 CORE。
- [`D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md`](D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md) — V0 Route Freeze PASS，并授权 TA-1A First Usable implementation。
- [`D-004_TA-1AOwnerUATCorrectionScope.md`](D-004_TA-1AOwnerUATCorrectionScope.md) — TA-1A 首轮 Owner UAT 未通过；收敛 Month View / Current Vector 必要 correction。
- [`D-005_TA-1AStageExit与TA-1BAuthorization.md`](D-005_TA-1AStageExit与TA-1BAuthorization.md) — TA-1A Review / Correction / focused UAT 全部通过，Stage Exit PASS；集成 implementation main 并授权 TA-1B Dispatch。
- [`D-006_TA-1BStageExit与TA-2Authorization.md`](D-006_TA-1BStageExit与TA-2Authorization.md) — TA-1B Independent Review + Complete V0 UAT PASS，Stage Exit；集成 Complete V0 并授权 TA-2 Reliability Hardening。
- [`D-007_TA-2StageExit与TA-3V0AcceptanceAuthorization.md`](D-007_TA-2StageExit与TA-3V0AcceptanceAuthorization.md) — TA-2 Reliability Hardening PASS_WITH_NOTES / Stage Exit；集成 accepted hardening baseline 并授权 TA-3 Final V0 Acceptance。

## 什么应该进入这里

- Scope / Non-scope 的重大边界；
- Route Freeze 或 Stage 级路线裁定；
- 高沉没成本架构选择；
- 改变 canonical owner / state model / public contract 的决定；
- 跨 01–05 聊天存在冲突后形成的 Owner 裁定；
- 会影响多个后续 Task、Review 或 UAT 的长期决定。

## 什么不应该进入这里

- 仍在讨论的候选方案；
- 单纯 Reference finding；
- 某次 Task 的普通实现细节；
- 某轮 Independent Review 的一次性证据；
- 可以直接更新现有 current Owner 而无需独立 Decision Record 的普通状态变化。

正式决定产生后必须执行 Decision Propagation；不得只新增 decision 文件却让 current Roadmap / Architecture / Status 或已发 Task 保持旧事实。
