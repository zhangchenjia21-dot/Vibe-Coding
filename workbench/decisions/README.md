# Personal Workbench｜Owner Decisions

本目录保存 **已经由 Product Owner 明确批准、需要长期生效或影响多个工作流的项目级正式裁定**。

## Current decisions

- [`D-001_暂停AI协作V0并重开产品方向.md`](D-001_暂停AI协作V0并重开产品方向.md) — 旧 AI Collaboration V0 退出 current、整体归档并重新打开 Product Direction；Personal OS 仍只是候选，不自动获批。

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
- 某次 Task 的实现细节；
- 某轮 Independent Review 的一次性证据；
- 可以直接更新现有 current Owner 而无需独立 Decision Record 的普通状态变化。

## Decision record 最小字段

建议至少包含：

```text
Status
Date
Decision
Context
Why
Alternatives considered
Consequences
Affected current owners / tasks
Supersedes / Superseded by（如适用）
```

正式决定产生后必须执行 Decision Propagation；不得只新增 decision 文件却让 `current/项目状态.md`、Roadmap、Architecture 或已发 Task 继续保持旧事实。
