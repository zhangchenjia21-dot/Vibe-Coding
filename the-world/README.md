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
| `The_World_GPT项目源_GitHub动态路由_v1.2.md` | 项目导航入口 Router（canonical；ChatGPT 项目源为复制件） |
| `TW-01_后台维护两层拆分裁定_v1.0_2026-08-24.md` | 回合后维护耗时问题的正式裁定（已实施，待实测收口） |
| `任务包_TW-ASSET-2026-08-24-01_判定与检定过渡包提交与生效_v1.0.md` | d20 过渡判定包的提交交接（执行线程） |
| `任务包_TW-DOC-2026-08-24-02_WC-08设计文档回迁与克隆清理_v1.0.md` | WC-08 设计文档从 DSH 废弃克隆回迁 main + 清理克隆（已完成，`6f497f7`） |
| `任务包_TW-IMPL-2026-08-24-03_the-world-panel游戏面板实施_v1.0.md` | Gate B 首插件 the-world-panel 实施（待派发） |
| `GateB_首个RPG体验插件与游戏面板裁定_v1.1_2026-08-24.md` | Gate B 十项裁定：the-world-panel、better-sidebar 宿主（零修改）、projection only、构建链进仓库、Web-only（已收口，待派任务包） |
| `延后裁定与待办_CURRENT.md` | 滚动 backlog：正式延后项、触发条件与待办 |

## 治理规则

- 遵循本仓库根 `AGENTS.md`：current-only、归档入 `99_归档/`、版本号 `vM.N`（N 限 0–9）；
- 本目录文件与 `the-world` 仓库正式裁定冲突时，以 `the-world` 的 `docs/PRODUCT_SPEC_CURRENT.md` / `docs/ARCHITECTURE_CURRENT.md` 为准；
- 重大结论落盘后，需要时应回写 `the-world` 对应 Owner（实施时执行，不在本目录堆积副本）。
- **唯一工作树（2026-08-24 裁定）**：the-world 的开发与游戏唯一工作树为 `D:\AI\Projects\the world`；DSH 会话一律指向它，不再使用 `D:\AI\deepseekharness\user-repos\` 下的克隆（起因：WC-08 设计文档曾在废弃克隆中游离一天，险些丢失）。
- **规划线程写回授权（2026-08-24 项目所有者授予）**：规划线程对本仓库的文档、裁定、经验、任务包类改动可自行 commit + push；`the-world` 项目仓库的写操作不在此授权内，仍走任务包或所有者明示指令。
