# Vibe-Coding｜AI 协作与动态事实源规则

状态：current  
适用范围：本仓库以及以本仓库作为开发治理 / AI 协作来源的项目与聊天  
维护者：项目所有者

## 1. 文件定位

本文件是 `Vibe-Coding` 仓库的仓库级 AI 治理入口。

它不替代 `项目经验/AI驱动项目全生命周期开发流程规范_*.md` 等详细方法论文件，而是规定 AI 在开始项目工作、调用 Skill、读取核心开发文件时，**应从哪里取得当前最新版事实**。

若本文件与用户当前明确指令冲突，以用户当前明确指令为最高优先级。

## 2. Skill 动态最新版规则

当任务满足以下任一条件时：

- 用户明确要求使用某个 Skill；
- AI 判断该任务需要调用某个 Skill；
- 任务流程引用了某个已经存在的 Skill 名称或能力；

AI **必须先访问并搜索**：

`https://github.com/zhangchenjia21-dot/Skill`

执行规则：

1. 在 `main` 分支搜索与任务相关的 Skill；
2. 如果仓库中存在对应 Skill，以 GitHub `main` 上当前最新版为执行依据；
3. 不得因为聊天中已有旧副本、附件旧版、本地缓存、历史摘要或模型记忆而跳过 GitHub 版本检查；
4. 若同名 Skill 存在多个版本，优先采用当前正式 / 最新版本，并遵守其适用范围、前置条件和边界；
5. 若 GitHub 中不存在对应 Skill，再使用当前环境中可用的其他正式 Skill / 内置能力；
6. 若 GitHub 无法访问，必须明确说明“未完成最新版核验”，然后才可基于当前可得的最新副本继续，不得假称已验证 GitHub 最新版。

> 原则：**Skill 是动态上游，不是聊天附件的静态快照。**

## 3. Vibe-Coding 核心文件动态最新版规则

当任务依赖以下任一类资料时：

- AI 驱动项目全生命周期开发流程；
- 项目治理 / Agent 协作规则；
- Harness；
- 开发任务模板；
- Review / UAT / Git / 安全规范；
- 项目经验与复盘形成的通用开发规则；
- 本仓库维护的其他核心开发文件；

AI **必须先访问并搜索**：

`https://github.com/zhangchenjia21-dot/Vibe-Coding`

执行规则：

1. 以 `main` 分支中当前最新、正式、适用的文件为准；
2. 若 GitHub 版本比聊天附件、项目上传文件、旧摘要、本地副本或模型上下文更新，则以 GitHub 最新版覆盖旧副本；
3. 文件名中的“最新版”“最终版”等字样本身不构成权威，必须结合仓库中的实际版本、更新时间、状态和覆盖关系判断；
4. 不得把历史文档、Legacy Reference、归档版本与 current 版本并列为同等事实源；
5. 若 GitHub 无法访问，必须显式标记版本核验失败，再使用当前可得资料继续。

> 原则：**核心开发文件的 GitHub `main` 是动态事实源；聊天和附件只是在特定时间取得的副本。**

## 4. 项目专属事实与通用规范的优先级

GitHub 最新版优先不意味着通用规范可以覆盖项目自身已经冻结的正式产品 / 架构裁定。

发生冲突时，默认按以下顺序处理：

1. 用户当前明确指令；
2. 当前项目最新、已批准的正式项目裁定 / 产品规格 / 架构 / 合同；
3. 当前项目仓库中可验证的真实实现与测试事实；
4. `Vibe-Coding` 中当前最新的跨项目治理 / Lifecycle / Harness 规范；
5. 历史文档、旧附件、旧聊天摘要、Legacy Reference；
6. Agent 的一般经验与模型记忆。

若两个“现行”来源仍然实质冲突：

- 不得静默选择更方便实施的一份；
- 不得自行把两套规则拼接成第三套；
- 应先依据版本、状态、覆盖声明和项目权威顺序解决；
- 无法安全解决时，明确列出冲突、影响与推荐裁定，再交由项目所有者决定。

## 5. 每次任务的版本核验时机

以下情况必须重新检查 GitHub 最新源，而不是依赖“本聊天之前已经查过”：

- 新的正式任务开始；
- 用户明确点名某个 Skill；
- 进入新的生命周期阶段；
- 生成正式 Codex / Agent 指令；
- 进行独立 Review；
- 更新项目核心文件；
- 用户要求“按最新规则 / 最新 Skill / 最新核心文件”执行；
- 当前任务距离上次核验期间相关仓库可能已经发生更新。

不要求在同一原子任务的每个小步骤重复查询；一次任务开始时完成版本核验即可，除非执行过程中发现上游刚发生变化或用户要求重新核验。

### 5.1 Pre-push Freshness Revalidation

正式写回 authoritative `main` 前，再检查一次当前 HEAD。

Agent 不需要知道其它 Agent 是否正在运行，只需要判断：

```text
Task Base HEAD
!= Current HEAD ?
```

若 HEAD 未变，正常写回；若 HEAD 已变，先审计 `Base → Current` 增量：

- 与当前任务无关：吸收 / rebase 后回归验证；
- 改变当前依赖、Architecture、Roadmap、Skill、Schema、Owner、Stage 或目标文件：重新执行 Decision Propagation 后再写；
- 不允许旧 Base 静默覆盖新 current。

> 原则：**不调度其它 Agent 的实时行为；治理共享事实源的读写一致性。**

## 6. 仓库职责边界

### `zhangchenjia21-dot/Skill`

负责：

- 可复用 Skill；
- Skill 的最新说明、执行协议、模板和边界；
- Skill 版本演进。

### `zhangchenjia21-dot/Vibe-Coding`

负责：

- AI 驱动项目开发方法论；
- Lifecycle / Harness；
- 项目治理与 Agent 协作规则；
- 开发复盘与跨项目经验；
- 各项目需要长期保留的开发核心资料。

不得把 Skill 正式源长期复制到 `Vibe-Coding`，也不得把 `Vibe-Coding` 的开发治理规则误当成 Skill 仓库的替代品。

## 7. Current-only / Archive 治理

active 目录用于当前事实入口，不用于堆积版本历史。

### 7.1 同文档族只保留一个 current

同一个 Roadmap / Plan / Decision / Master Spec / Maintenance 文档族：

```text
new current created
→ old current superseded
→ old version moves to 99_归档/
```

不得让多个 superseded 版本继续平铺在 active 目录，让 Agent 靠文件名猜最新版。

### 7.2 不误归档独立核心

独立、仍有效的编号核心裁定不因为日期较早而归档。只有明确被同一核心的新版本 supersede 时才移动旧版。

### 7.3 Rolling current 优先固定路径

高频滚动解释层，例如项目综合总纲、current index、current state summary，优先使用：

`*_CURRENT.md`

后续直接更新固定路径；Git history + `99_归档/` 承担历史。

### 7.4 阶段过程资料

已关闭的 Task Spec、Exit checklist、阶段更新 Ledger、阶段复盘默认进入 `99_归档/` 对应分类，不与 current 架构文件平铺。

## 8. 人类治理文档版本号

本仓库的人类可读治理 / 规划 / 裁定文档**不采用 SemVer 多位 minor 习惯**，使用一位小版本序列：

```text
v1.0 → ... → v1.8 → v1.9 → v2.0 → v2.1
```

强制规则：

- `vM.N` 中 `N` 只允许 `0–9`；
- `vM.9` 的下一版本为 `v(M+1).0`；
- 禁止新建 `v1.10 / v1.11 / v1.12` 等文档版本；
- 版本号只表达演进顺序，不表达 SemVer breaking-change；重大变化使用 `status / supersedes / change_class / ADR` 显式说明；
- 已存在的 `v1.10 / v1.11 / v1.12` 作为历史原名进入归档，不追溯改写历史正文；当前继承版本从 `v2.0` 继续。

## 9. 执行摘要

任何读取本文件的 AI 默认执行：

```text
任务开始
→ 查 GitHub current / Skill current
→ 使用 active current，不把 99_归档 当现行事实
→ 实施 / 审核
→ pre-push HEAD revalidation
→ new current + old current archive in same delivery
```

## 10. 生效结论

> **聊天上下文不再被视为 Skill 或开发核心文件的最高版本来源。**
>
> **active 目录只保留 current；历史集中归档。**
>
> **文档版本从 `.9` 进入下一主版本 `.0`，不再生成 `.10/.11/.12`。**
