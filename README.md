# Vibe-Coding

AI 驱动开发项目的治理、方法论、项目核心资料与复盘仓库。

## AI 入口

任何 GPT、Codex、Grok 或其它 Agent 开始工作前，先读 [`AGENTS.md`](./AGENTS.md)。

```text
AGENTS.md
→ 动态事实源、权威顺序、Freshness、Agent 指令与仓库治理

项目经验/
→ 跨项目 Lifecycle、Harness、复盘与通用开发规范

sillytavern/
→ 酒馆游戏当前路线、裁定、阶段状态与项目核心资料

99_归档/
→ superseded 版本与已关闭过程资料，不构成 current authority
```

## 仓库边界

- 当前代码、测试与运行事实：`zhangchenjia21-dot/sillytavern`
- 世界包、人物卡、拓展包与资产族：`zhangchenjia21-dot/sillytavern-assets`
- 可复用执行 Skill：`zhangchenjia21-dot/Skill`
- 项目治理与正式项目裁定：本仓库

正式 Agent 指令由 `Skill/skill/gpt/agent-task-packet/SKILL.md` 约束；不要把完整项目文档重复复制进每条 Prompt。
