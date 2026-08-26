# 项目经验｜跨项目可复用知识

这里不保存任何单一项目的 current 状态，而保存从真实项目中提炼出的**可迁移方法、开发规范与复盘证据**。

## AI Start here

1. 先读 [`AGENTS.md`](AGENTS.md)。
2. 想知道“以后项目默认怎么做”，读 [`core/`](core/)。
3. 想知道“这个方法为什么形成、踩过什么坑”，再读 [`retrospectives/`](retrospectives/)。

## Repository map

| 路径 | 角色 | Authority |
|---|---|---|
| `core/` | 跨项目仍有效的 canonical 方法、流程与规范 | **方法 Authority** |
| `retrospectives/` | 按项目保存的第一版/第二版复盘、失败与演化证据 | 历史 Evidence |

旧版规范继续由仓库根 `99_归档/项目经验/` 保存。

## 核心原则

- 成熟方法进入 `core/` 后，默认原位更新 canonical；不要每次改进都在本目录根新增一个新版本。
- 项目复盘允许保留“第一版 / 第二版”等历史顺序，因为它们记录经验如何演化；但它们不和 `core/` 竞争 current Authority。
- 项目专属事实留在各项目目录；只有具备跨项目复用价值的结论才进入这里。
- 若复盘产生了新通用原则，应更新 `core/`，而不是只在复盘末尾留下结论副本。

Root is map; subfolders are depth.
