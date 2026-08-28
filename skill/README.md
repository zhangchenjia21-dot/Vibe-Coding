# Vibe-Coding / skill

跨项目可复用 Skill 的 current 源。

## 入口

先读 [`AGENTS.md`](./AGENTS.md)。每个 Skill 的正式路径：

```text
skill/<runtime>/<skill-name>/SKILL.md
```

## 当前结构

```text
skill/
├─ AGENTS.md
├─ README.md
├─ MIGRATION_PROVENANCE.md
├─ gpt/
│  ├─ agent-task-packet/
│  ├─ grill-me/
│  ├─ lifecycle-dev-process/
│  ├─ lifecycle-templates/
│  ├─ tavern-asset/
│  ├─ tavern-creator-import-draft/
│  └─ 长上下文交接/
└─ dsh/
   ├─ lifecycle-dev-process/
   ├─ lifecycle-templates/
   ├─ project-thread-handoff/
   ├─ tavern-asset/
   ├─ tavern-creator-import-draft/
   └─ user-profile/
```

## Authority

Skill 是**跨项目默认方法**，不是项目事实源。

```text
项目 current decision
> 通用 Skill default
```

具体项目的 Product / Architecture / Roadmap / Current Status 在仓库对应项目目录；代码和运行事实仍在各自 implementation repository。

## 常用 Skill

`gpt/lifecycle-dev-process`：项目启动、开发前路线审计、Stage/Task DAG、架构与生命周期。

`gpt/agent-task-packet`：把 current spec 转换成有界、可验收、可交接的正式 Agent 指令。

`gpt/lifecycle-templates`：Product Definition、Ownership、Migration、UAT 等治理模板。

`gpt/长上下文交接`：长线程、新线程与角色切换的项目现场交接。

`dsh/`：DeepSeek Harness 运行环境适配版本。

## 迁移

本目录从原独立 `zhangchenjia21-dot/Skill` 仓库迁入。迁移来源、HEAD 和内容校验见 [`MIGRATION_PROVENANCE.md`](./MIGRATION_PROVENANCE.md)。迁移完成后，本目录是唯一 current authority；旧仓库仅作为历史来源，直至 Owner 删除或归档。
