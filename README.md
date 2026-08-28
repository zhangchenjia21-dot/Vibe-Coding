# Vibe-Coding

AI 驱动开发项目的统一治理、方法论、可复用 Skill、项目核心资料与复盘仓库。

## AI 入口

任何 GPT、Codex、Grok、KimiCode 或其它 Agent 开始正式工作前，先读 [`AGENTS.md`](./AGENTS.md)。

```text
AGENTS.md
→ 总 Authority / Freshness / Decision Propagation / 文档治理

skill/
→ 跨项目可复用执行方法与模板

项目经验/
→ 跨项目 Lifecycle、复盘与通用开发经验

my world/
→ my world 项目 Product / Principles / Architecture / Roadmap / Current Status

sillytavern/
→ SillyTavern 项目治理与历史核心资料

the-world/
→ The World 项目治理、裁定与经验

99_归档/
→ superseded / closed process evidence，不构成 current authority
```

## Skill

原独立 `zhangchenjia21-dot/Skill` 仓库的 current Skill 已并入：

`skill/<runtime>/<skill-name>/SKILL.md`

常用入口：

- `skill/gpt/lifecycle-dev-process/SKILL.md` — 项目生命周期、阶段与路线审计；
- `skill/gpt/agent-task-packet/SKILL.md` — 正式 Agent Task Packet；
- `skill/gpt/lifecycle-templates/SKILL.md` — 生命周期/架构治理模板；
- `skill/gpt/长上下文交接/SKILL.md` — 长线程交接；
- `skill/dsh/` — DeepSeek Harness 适配版本。

Skill 子树规则见 [`skill/AGENTS.md`](./skill/AGENTS.md)，迁移 provenance 见 [`skill/MIGRATION_PROVENANCE.md`](./skill/MIGRATION_PROVENANCE.md)。

## 仓库边界

- 项目治理、跨项目经验、可复用 Skill：**本仓库**；
- `my world` 代码/测试/Godot 运行事实：`zhangchenjia21-dot/my-world`；
- SillyTavern 当前实现事实：`zhangchenjia21-dot/sillytavern`；
- The World 实现与游戏工作区：`zhangchenjia21-dot/the-world`；
- 世界包、人物卡、拓展包与资产内容：`zhangchenjia21-dot/sillytavern-assets`。

同仓库不等于同 Authority：项目 current 决策高于通用 Skill 默认规则，实现事实仍以对应 implementation repository 为准。

## 文档原则

> **Root is map; subfolders are depth.**

顶层保持少数稳定入口；项目深度进入各项目子目录；跨项目方法进入 `skill/`；经验进入 `项目经验/`；历史进入 `99_归档/`。
