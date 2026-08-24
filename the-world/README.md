---
title: The World｜项目规划与治理资料
status: current
created: 2026-08-24
scope: the-world-project
---

# The World｜规划与治理资料目录

本目录存放 **The World 项目**（`zhangchenjia21-dot/the-world`）的长期规划、裁定记录、延后事项与复盘资料。

## 仓库边界

- 项目代码、产品规格、架构、游戏工作区事实 → `zhangchenjia21-dot/the-world`（**保持纯净**，只放项目本体）；
- 可复用资产与适配工作 → `zhangchenjia21-dot/sillytavern-assets`（private）；
- 可复用执行 Skill → `zhangchenjia21-dot/Skill`（Kimi 专用 Skill 在 `skill/kimi/`，需要时新建）；
- 本目录 → The World 的**规划、裁定、延后记录、复盘**等治理类资料。

## 动态事实源原则

本目录不保存项目动态事实（当前 HEAD、game state、插件实现细节等）。项目事实以 `the-world/main` 的 current 文件为准：

```text
the-world/README.md
→ AGENTS.md
→ docs/PRODUCT_SPEC_CURRENT.md
→ docs/ARCHITECTURE_CURRENT.md
→ docs/TW-01_WORLD_CORE_PLAN.md
```

## 当前文件

| 文件 | 内容 |
|---|---|
| `TW-01_后台维护两层拆分裁定_v1.0_2026-08-24.md` | 回合后维护耗时问题的正式裁定（已采纳，待实施） |
| `延后裁定与待办_CURRENT.md` | 滚动 backlog：正式延后项、触发条件与待办 |

## 治理规则

- 遵循本仓库根 `AGENTS.md`：current-only、归档入 `99_归档/`、版本号 `vM.N`（N 限 0–9）；
- 本目录文件与 `the-world` 仓库正式裁定冲突时，以 `the-world` 的 `docs/PRODUCT_SPEC_CURRENT.md` / `docs/ARCHITECTURE_CURRENT.md` 为准；
- 重大结论落盘后，需要时应回写 `the-world` 对应 Owner（实施时执行，不在本目录堆积副本）。
