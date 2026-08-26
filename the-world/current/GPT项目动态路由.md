---
title: The World GPT 项目源｜GitHub 动态路由
aliases:
  - The World 项目源引导器
  - The World GitHub Project Source Router
status: current
version: 1.2
created: 2026-08-23
updated: 2026-08-24
scope: the-world-project
supersedes: The_World_GPT项目源_GitHub动态路由_v1.1.md（仅存于 ChatGPT 项目附件，无仓库副本）
---

# The World GPT 项目源｜GitHub 动态路由 v1.2

> [!abstract] 文件定位
> 本文件是 The World 项目的**静态导航入口**，唯一职责：
>
> **告诉项目中的 GPT / Agent：当任务需要项目事实、产品定义、架构、插件、资产、游戏状态、方法论或 Skill 时，必须前往哪个 GitHub current source 恢复。**
>
> v1.2 起，本文件**只保留稳定的导航与门禁规则**，删除一切有时效性的内容（阶段状态、当前决策、Gate 进度、模板细节、插件清单）——那些事实住在各自 GitHub Owner 里，以仓库 current 为准，本文件不复制、不摘要、不转述。
>
> 聊天历史、模型记忆、旧附件、旧项目源副本不能替代 GitHub current truth。
>
> 本文件 canonical 副本存放于 `Vibe-Coding/the-world/`（GitHub），ChatGPT 项目源为复制件。

---

# 1. 固定入口

## 1.1 项目主仓库（主要动态事实源）

```text
https://github.com/zhangchenjia21-dot/the-world     （main，public）
```

默认读取顺序：

```text
README.md
→ AGENTS.md
→ docs/PRODUCT_SPEC_CURRENT.md
→ docs/ARCHITECTURE_CURRENT.md
→ 当前任务 Owner（plugins/ | library/ | games/<game-id>/ | tools/）
```

不是每个任务都要全部读取——**读取完成当前任务所需的最小充分工作集**。

## 1.2 本地唯一工作树（2026-08-24 裁定）

```text
D:\AI\Projects\the world
```

- the-world 的开发与游戏**唯一**工作树；DSH 会话一律指向它；
- 禁止在 `D:\AI\deepseekharness\user-repos\` 等地另起克隆使用（起因：设计文档曾在废弃克隆中游离、险些丢失）；
- 本地未 push 的修改不是跨聊天共享事实；**"本地已改完"不等于"项目已同步"**；
- 无法访问本机磁盘的环境（如 ChatGPT 网页端）不得假称已读取本地目录，必须经 GitHub 或用户显式提供的内容恢复事实。

## 1.3 Reference Host

```text
DeepSeek Harness  = 通用 Agent Host / Plugin Runtime
https://github.com/deepseek-ai/deepseek-harness
```

产品边界（稳定）：**DSH-native, not DSH-internal-coupled**——优先使用 DSH 文档化 plugin / capability seams，不 fork / patch 通用 Agent Runtime。具体 event / package / API 版本**不在本文件冻结**，涉及实现时必须读取 DSH current documentation / code（DSH 处于快速演进期，不得依赖旧聊天中的 API 记忆）。

## 1.4 配套仓库

```text
https://github.com/zhangchenjia21-dot/Vibe-Coding
→ 跨项目方法论、项目治理；the-world/ 目录 = 本项目规划、裁定、任务包、延后记录

https://github.com/zhangchenjia21-dot/Skill
→ 可复用执行 Skill 最新事实源（skill/gpt/、skill/dsh/、skill/kimi/）

https://github.com/zhangchenjia21-dot/sillytavern-assets
→ 可复用资产（private）
```

# 2. 事实类型 → Owner 速查

| 事实类型 | Owner |
|---|---|
| 项目导航 / 高层状态 | `the-world/README.md` |
| 仓库治理 / 协作规则 / 原则 | `the-world/AGENTS.md` |
| 产品应该是什么 / 产品决策 / Gate 口径 | `docs/PRODUCT_SPEC_CURRENT.md` |
| 当前工作架构 / Host 边界 / Owner 划分 | `docs/ARCHITECTURE_CURRENT.md` |
| 当前阶段计划 | `docs/` 下 current-stage-plan 标记的文件 |
| 游戏工作区结构语义 | `docs/GAME_WORKSPACE_ARCHITECTURE_*.md`（current 版） |
| RPG 插件实现 | `plugins/` |
| 可复用 Source 资产（世界/角色/机制/lore） | `library/` |
| 当前游戏真实状态 | `games/<game-id>/state/` |
| 重要历史 / hooks | `games/<game-id>/story/` |
| 上下文压缩 / 待归并变化 | `games/<game-id>/memory/` |
| 恢复点 | `games/<game-id>/saves/` |
| 本局启用组合（玩家确认） | `games/<game-id>/COMPOSITION.md` |
| 确定性支持工具 | `tools/` |
| 规划 / 裁定 / 任务包 / 延后项 | `Vibe-Coding/the-world/` |
| 方法论 | `Vibe-Coding`（项目经验/、AGENTS.md） |
| Skill 执行方法 | `Skill/main` |
| DSH 接口与扩展缝 | `deepseek-ai/deepseek-harness` current |

规则：**Source（library/）是可复用起点，不是某一局 live truth**；单局演化不得反向污染 library。**一个事实只有一个 Owner**。

# 3. 稳定门禁（不随版本失效）

## 3.1 Cross-Chat Freshness Gate

本项目允许多聊天 / 多 Agent 并行。聊天框彼此不是可靠同步通道：

```text
Chat / Thread  = 并行工作台
Local Working Tree = 当前执行现场
GitHub main  = 跨聊天共享动态事实源
本 Router    = 启动级导航入口
```

以下工作开始前必须核验相关 GitHub current 状态：新聊天接管、用户问"现在/当前/最新/接下来"、正式产品或架构判断、Gate 判断、创建/恢复/修改游戏、插件开发、生成正式任务、Independent Review、用户表示另一线程刚 push、上次核验后可能有并行工作。

默认假设：**相关仓库可能已经发生更新**。增量检查优先（有 last-seen SHA 查增量，无则枚举最近提交），Delta first，必要时才扩大读取。涉及 DSH 集成的任务，必须同时核验 `deepseek-ai/deepseek-harness` current。

## 3.2 Decision Propagation Gate

Freshness 只回答"我是否读到最新事实"，不保证"最新事实已传播到所有受影响 Owner"。发现新的/修改的重大事实后，至少回答：是否改变 Product / Architecture / 当前游戏 / 当前任务 / Owner 归属 / 恢复语义 / Source 隔离 / 插件边界 / DSH 边界 / Gate 判断 / 下一步建议。

影响分级：

- **P0 立即阻断**：会覆盖新 canonical state、造成第二事实源、污染 library、替玩家做不可逆选择、提交敏感信息、推向重造 Runtime 时，立即停止旧方向先处理冲突；
- **P1 任务路线改变**：下一个正式任务前更新计划；
- **P2 后续约束**：写入正确 current source / backlog；
- **P3 说明 / Polish**：记录即可。

## 3.3 Failure-driven Tooling Gate

防错类能力（校验、权限、Schema enforcement、事务、typed mutation、复杂恢复基础设施）默认必须由**真实重复失败**拉动：

```text
真实失败 → 分类根因 → 最窄修复 → 再试玩 → 仍失败才升级
```

RPG 体验类能力（UI、地图、战斗、经济、角色成长、世界机制）可由**产品价值**直接驱动，不需要先证明模型犯错。

## 3.4 Public Repository Safety

the-world 是 public 仓库。不得提交 API Key、Token、Cookie、密码、私钥、私密聊天原文、不愿公开的个人信息、无权公开的版权/保密材料。

# 4. 稳定产品边界（仅以下几条）

以下原则长期稳定，细节以 `docs/PRODUCT_SPEC_CURRENT.md` 为准：

- **Persistent World + Player Spotlight**：世界独立存在并持续演化，叙事聚光灯照向玩家；
- **Player owns Attempt / World owns Consequence / GM owns Playability**；Player Agency ≠ Player Entitlement；
- **Model Freedom / Freedom Before Prevention**：不为防低成本模型错误预建重型限制体系；
- **Recovery First**：Prefer recovery over prevention；
- **Player Plays, Agent Maintains**：游戏 Workspace 是 Agent 工作区，不是玩家操作负担；
- **plugins/ = 游戏价值与体验；tools/ = 窄确定性支持**，两者不混；
- **UI 是 game truth projection，不是第二事实源**。

当前处于哪个阶段、Gate 状态如何、有哪些插件与工具——**一律以 GitHub current 为准**，本文件不列。

# 5. 恢复算法

任何读取本 Router 的 GPT / Agent，任务需要项目事实时：

```text
0. 判断环境：能否访问本地唯一工作树 / GitHub；是否可能有本地未 push 修改
1. Freshness Preflight：the-world/main（有 last-seen SHA 做增量，无则枚举最近提交）
2. 按 §2 速查表定位事实 Owner，只读任务所需的最小充分工作集
3. 涉及 DSH 集成 → 再核验 deepseek-ai/deepseek-harness current
4. 需要方法论 / Skill → Vibe-Coding/main、Skill/main
5. 生成正式 Agent 指令 → 先读 Skill/main/skill/gpt/agent-task-packet/SKILL.md
   （涉及 Stage / 架构 / 迁移 / Gate 再读 lifecycle-dev-process）
6. 发现重大 current change → 执行 §3.2 Decision Propagation
7. 再回答、修改、Review、规划或执行
```

# 6. 权威优先级与冲突处理

发生冲突时默认：

1. 用户当前明确指令；
2. `docs/PRODUCT_SPEC_CURRENT.md`；
3. `docs/ARCHITECTURE_CURRENT.md`；
4. 当前 game `state/` canonical facts；
5. 当前 game `story/` / `memory/`；
6. `README.md` / `AGENTS.md` 派生导航与治理；
7. Vibe-Coding current 方法论；
8. Skill current 执行方法；
9. 历史聊天、旧摘要、旧附件、模型记忆。

不同 Owner 不机械互相覆盖；按事实类型归位判断。两个来源冲突时：不静默选方便的一份；检查 current / version / Owner；能按 Owner 关系解决就直接解决；无法安全解决时列出冲突双方、Owner、影响与推荐裁定，交项目所有者决定；**禁止未经批准自行融合为第三套长期规则**。

# 7. 本地与 GitHub 冲突

- 本地有未提交修改：`git status / diff` 判断是否属于本任务，不覆盖未知用户改动；
- 本地 commit 未 push：只是本地事实，正式跨聊天工作前应 push 或明确说明；
- GitHub 有本地没有的新 commit：继续正式修改前 fetch / pull、检查影响、必要时重新 Decision Propagation；
- 正式写回 main 前再核一次 HEAD（Pre-push Revalidation）：HEAD 已变则先审计增量，相关则重新传播后再写，不允许旧 Base 静默覆盖新 current。

# 8. 生效结论

> **本 Router 只做导航。一切有时效的项目事实——阶段、Gate、决策、插件、工具、游戏状态——以 GitHub current 为唯一真相。**
>
> **聊天上下文不是事实源；附件副本不是最高版本。**
