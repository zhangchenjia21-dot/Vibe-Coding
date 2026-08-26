# The World｜规划与治理资料

本目录只保存 **The World 的规划、正式裁定、执行任务包与延后事项**。项目代码、产品规格、架构与真实游戏状态由 `zhangchenjia21-dot/the-world` 仓库拥有。

## AI Start here

1. 读 [`AGENTS.md`](AGENTS.md) 确认边界。
2. 当前路由与 rolling backlog 读 [`current/`](current/)。
3. 需要理解正式决策时读 [`decisions/`](decisions/)。
4. 只有执行/审计某个工作线程时读 [`tasks/`](tasks/)。
5. 实现事实一律回到 `zhangchenjia21-dot/the-world`。

## Repository map

| 路径 | 角色 | Authority |
|---|---|---|
| `current/` | GPT 动态路由与当前延后事项 | 当前治理入口 |
| `decisions/` | 已生效的项目级正式裁定 | 决策 Authority |
| `tasks/` | 实施、审核、交接任务包 | 执行证据，不是产品事实 Owner |

历史版本继续进入仓库根 `99_归档/the-world/`。

## Boundary

```text
Vibe-Coding/the-world
= planning / decisions / task governance

zhangchenjia21-dot/the-world
= code / product spec / architecture / runtime evidence
```

同一事实不能在两个仓库手工维护两份。治理资料需要项目当前事实时，通过路由指向实现仓库，而不是复制 HEAD、游戏状态或实现细节。

Root is map; subfolders are depth.
