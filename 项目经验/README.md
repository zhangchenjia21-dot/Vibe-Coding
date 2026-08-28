# 项目经验｜跨项目可复用知识

这里不保存任何单一项目的 current 状态，而保存从真实项目中提炼出的**可迁移方法、开发规范与复盘证据**。

## AI Start here

1. 先读 [`AGENTS.md`](AGENTS.md)。
2. 想知道“为什么形成这套方法、应该怎样理解”，读 [`core/`](core/)。
3. 想知道“踩过什么坑、证据来自哪里”，再读 [`retrospectives/`](retrospectives/)。
4. 需要**可直接执行的 Skill** 时转到仓库根 [`skill/`](../skill/)；不要把经验文档当成 Skill 执行合同。

## Repository map

| 路径 | 角色 | Authority |
|---|---|---|
| `core/` | 跨项目仍有效的方法、流程、解释与规范 | 方法论 / 解释层 |
| `retrospectives/` | 按项目保存的复盘、失败与演化证据 | 历史 Evidence |
| `../skill/` | 有明确触发条件、执行步骤、边界与成功标准的可复用 Skill | 可执行方法层 |

旧版规范继续由仓库根 `99_归档/项目经验/` 保存。

## 知识成熟路径

```text
真实项目事实
→ 项目复盘 Evidence
→ 项目经验/core 提炼通用原则
→ 若形成明确可执行协议，再进入 skill/
```

项目 current、项目经验与 Skill 可以互相引用，但不能复制成多个同级事实源。

## 核心原则

- 成熟方法进入 `core/` 后，默认原位更新 canonical；不要每次改进都在本目录根新增一个新版本。
- 项目复盘允许保留历史顺序，因为其价值在演化与证据；但它们不和 `core/` 或 `skill/` 竞争 current Authority。
- 项目专属事实留在各项目目录；只有具备跨项目复用价值的结论才进入这里。
- 当经验已经形成稳定、可执行的工作协议时，更新 `skill/`；不要在 `core/` 与 `skill/` 长期维护两份同义全文。

> **项目产生事实，复盘保存证据，core 提炼原则，skill 固化可重复执行的方法。**

Root is map; subfolders are depth.