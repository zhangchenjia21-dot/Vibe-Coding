# SillyTavern｜项目治理与事实源

本目录保存 SillyTavern 项目的**产品治理、架构裁定、阶段证据与执行规则**。实现代码、测试和运行行为以 `zhangchenjia21-dot/sillytavern` 的 `main` 为实现事实源。

## AI Start here

1. 先读本目录 [`AGENTS.md`](AGENTS.md)。
2. 当前项目理解只从 [`current/`](current/) 开始。
3. 需要长期设计约束时读 [`architecture/`](architecture/)。
4. 需要查“为什么当时这么做 / 某 Gate 如何收口”时再进入 [`stages/`](stages/)。
5. Agent / Git / 任务交付规则看 [`governance/`](governance/)。

不要默认把全部 G8/G9 过程文件加载进上下文。

## Repository map

| 路径 | 角色 | Authority |
|---|---|---|
| `current/` | 当前总纲、当前重构总纲、当前路线与继承策略 | **当前入口** |
| `architecture/` | 已沉淀的长期产品/架构裁定与原则 | 长期约束 |
| `governance/` | Agent、Worktree、任务包交付治理 | 执行约束 |
| `stages/` | G8/G9/PLAY 的阶段规格、Review、阻塞与收口证据 | 历史/阶段证据 |
| `discussion/` | Draft、可选增量、尚未成为 Authority 的备忘 | 非 Authority |

历史旧版本继续由仓库根 `99_归档/sillytavern/` 保存。

## Canonical rule

- `current/` 中的现有 current Owner **原位更新**；不要再新增新的顶层 `*_CURRENT.md`。
- 新的长期架构裁定进入 `architecture/`，新阶段施工证据进入 `stages/<stage>/`。
- 同一结论只维护一个 Owner；阶段 Review 可以作为证据，但不复制 current 状态。
- 项目进入下一阶段时，更新 current Owner 与 README 路由，而不是把旧阶段文件改名成“最新版”。

Root is map; subfolders are depth.
