# AGENTS.md｜项目经验 AI 读写协议

本文件适用于 `Vibe-Coding/项目经验/`。

## 1. 读取顺序

```text
README.md
→ core/README.md
→ 相关 canonical 方法
→ 只有需要证据 / 演化原因时读取 retrospectives
```

## 2. Canonical Owner

- 跨项目开发流程、Agent 协作、Review/Gate/Recovery 等成熟方法：`core/`
- 某个具体项目的复盘、失败案例、经验形成过程：`retrospectives/<project>/`
- 已 supersede 的旧规范：仓库根 `99_归档/项目经验/`

## 3. 提炼链

```text
真实项目事实
→ 项目复盘 Evidence
→ 判断是否可跨项目复用
→ 更新 core canonical
→ 在项目目录只保留路由 / 应用，不复制方法正文
```

## 4. 写入规则

- `core/` 是方法 Owner，不是版本展览馆；默认更新现有 canonical。
- `retrospectives/` 可以按时间保留多代经验，因为其价值在演化与证据。
- 不把某个项目的临时 workaround 直接升级成通用规则。
- 新增通用规则前先检查是否应合并进现有 lifecycle / Recovery / governance Owner。
- 根层不新增方法正文或复盘文件。
