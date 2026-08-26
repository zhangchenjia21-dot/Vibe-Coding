# AGENTS.md｜The World 治理目录读写协议

本文件适用于 `Vibe-Coding/the-world/`。

## 1. 默认读取

```text
README.md
→ current/README.md
→ 必要的 decision
→ 仅在执行/审计时读取 task packet
→ 实现问题转到 zhangchenjia21-dot/the-world
```

## 2. Canonical Owner

- GPT 动态项目路由：`current/The_World_GPT项目源_GitHub动态路由_v1.2.md`
- 正式延后事项：`current/延后裁定与待办_CURRENT.md`
- 正式项目裁定：`decisions/`
- 一次性实施/审核/交接过程：`tasks/`
- 已 supersede 版本：仓库根 `99_归档/the-world/`

## 3. 写入规则

- 不在本目录记录实现仓库 HEAD、运行状态、代码细节的第二份真相。
- task packet 完成后保留为执行证据；其结论需要长期生效时，应回写 `decisions/` 或实现仓库对应 Owner。
- current Owner 原位更新，不持续制造新的 `*_CURRENT.md`。
- 新裁定放 `decisions/`，不直接堆在本目录根层。
- 新任务包放 `tasks/`；根层只保留 README / AGENTS 与稳定子目录。

## 4. 写入链

```text
Freshness
→ 判断 current / decision / task
→ 更新唯一 Owner
→ 如涉及实现，路由到 the-world repo
→ 完成后检查 task 结论是否需要传播
→ 归档 superseded 材料
```
