# AGENTS.md｜SillyTavern 治理目录读写协议

本文件只约束 `Vibe-Coding/sillytavern/`。

## 1. 最小读取顺序

```text
README.md
→ current/README.md
→ 与任务直接相关的 current Owner
→ 必要的 architecture 裁定
→ 需要跨项目执行方法时读 Vibe-Coding/skill/ 下相关 Skill
→ 只有追溯/审计时才读 stages
→ 实现问题再去 zhangchenjia21-dot/sillytavern
```

禁止用“把所有阶段文档都读一遍”替代 Owner 定位。

## 2. Owner 规则

- 当前项目总纲 / 当前产品方向 / 当前路线：`current/`
- 跨阶段仍有效的产品与架构原则：`architecture/`
- Agent、分支、Worktree、任务交付协议：`governance/`
- 单一阶段的规格、Review、Correction、Gate 证据：`stages/<stage>/`
- Draft / optional / 尚未批准内容：`discussion/`
- 被新版本 supersede 且只需追溯的内容：仓库根 `99_归档/sillytavern/`
- 跨项目可复用执行方法：仓库根 `skill/`

## 3. 写入流程

```text
Freshness
→ 判断是 current / architecture / governance / stage / discussion
→ 更新唯一 Owner
→ 必要时更新 current 路由
→ 检查是否有 superseded 文件应归档
→ 检查实现仓库是否需要同步任务或事实
→ 若产生跨项目可复用方法，再按 skill/AGENTS.md 判断是否上提 Skill
→ 集中汇报
```

## 4. Current-only

不要新增 `CURRENT_2`、`FINAL_v2`、`最新版` 作为更新策略。当前文档原位更新；需要保存演变时依靠 Git 历史或进入 `99_归档/`。

阶段中的 `IndependentReview`、`Correction`、`阻塞发现` 是 evidence，不自动成为当前状态 Owner。

## 5. 边界

- `Vibe-Coding/sillytavern` = SillyTavern 规划、产品、架构与治理事实源。
- `Vibe-Coding/skill` = 跨项目可复用执行方法，不保存 SillyTavern 专属 current 状态。
- `Vibe-Coding/项目经验` = 跨项目经验与复盘，不保存 SillyTavern 专属 current 状态。
- `zhangchenjia21-dot/sillytavern` = 代码、测试、构建与运行事实源。
- `zhangchenjia21-dot/sillytavern-assets` = 资产正文事实源。

如果同一个事实跨仓库或跨子树出现，只能有一个 canonical Owner，其他地方只做路由。通用 Skill 不得覆盖项目已经批准的 current 产品 / 架构裁定。