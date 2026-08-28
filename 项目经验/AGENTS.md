# AGENTS.md｜项目经验 AI 读写协议

本文件适用于 `Vibe-Coding/项目经验/`。

## 1. 读取顺序

```text
README.md
→ core/README.md
→ 相关 canonical 方法 / 原则
→ 需要可执行协议时转到仓库根 skill/
→ 只有需要证据 / 演化原因时读取 retrospectives
```

## 2. Canonical Owner

- 跨项目开发流程、Agent 协作、Review / Gate / Recovery 等成熟**解释性方法与原则**：`core/`
- 具有明确触发条件、执行步骤、边界和成功标准的**可执行方法**：仓库根 `skill/`
- 某个具体项目的复盘、失败案例、经验形成过程：`retrospectives/<project>/`
- 已 supersede 的旧规范：仓库根 `99_归档/项目经验/`

`core/` 与 `skill/` 不维护两份同义全文：前者负责解释“为什么 / 应如何理解”，后者负责“何时触发 / 具体怎么执行 / 怎样算完成”。

## 3. 提炼链

```text
真实项目事实
→ 项目复盘 Evidence
→ 判断是否可跨项目复用
→ core 提炼原则 / 方法
→ 若形成稳定可执行协议，再更新 skill/
```

项目目录只保留项目 current 与必要路由，不复制跨项目方法正文。

## 4. 写入规则

- `core/` 是方法论与解释层，不是版本展览馆；默认更新现有 canonical。
- `retrospectives/` 可以按时间保留多代经验，因为其价值在演化与证据。
- 不把某个项目的临时 workaround 直接升级成通用规则或 Skill。
- 新增通用规则前先检查是否应合并进现有 lifecycle / Recovery / governance Owner。
- 新增可执行 Skill 前先按 `skill/AGENTS.md` 做 Reuse-before-Creation 与 Evidence-before-Abstraction 检查。
- 根层不新增方法正文或复盘文件。

> **项目产生事实，复盘保存证据，core 提炼原则，skill 固化可重复执行的方法。**