---
title: The World｜Gate B 首个 RPG 体验插件与游戏面板裁定
status: current
version: 1.2
created: 2026-08-24
last_updated: 2026-08-24
scope: the-world-project / Gate B
decision_type: product-architecture
supersedes: GateB_首个RPG体验插件与游戏面板裁定_v1.1_2026-08-24.md
---

# Gate B 首个 RPG 体验插件与游戏面板裁定 v1.2

> v1.2 变更：修订 DEC-B3——面板初版落地后，所有者提出真实痛点「被模型误判为任务的事项会一直卡在任务栏」。裁定面板开放**唯一窄写口**：「关闭线程」归档操作（把 THREADS.md 线程块移入 story/LEDGER.md，遵循工作区既有归档约定）。除该写口外，面板仍然零编辑、零写回。v1.1 已归档（`99_归档/the-world/`）。
>
> v1.1 变更：新增 DEC-B10（宿主方案定案）——经实证核查 `dsh-better-sidebar` v0.15.2 服务化 API 后，确定以其为宿主、不 fork、不自建外壳。v1.0 已归档（`99_归档/the-world/`）。

## 0. 背景

2026-08-24 规划线程与项目所有者的插件讨论收口。前置状态：Gate A 经真实试玩基本成立（正式收口宣告由所有者择机发出）；DSH current 已核验，Web 端存在正式 client module 机制与声明式 slot 系统；宿主候选 `dsh-better-sidebar` v0.15.2 已实测核查（本地安装于 web profile，peer 下限 `^0.1.0-rc.8` 与本机 DSH rc.8 兼容）。

本文件冻结十项裁定（DEC-B1 ~ DEC-B10）。实施时回写 `the-world` 正式文档（`docs/ARCHITECTURE_CURRENT.md` 等），本文件不替代项目仓库 truth。

## 1. 裁定清单

### DEC-B1｜插件开关与作用域模式（确认现状即正式模式）

- The World 全部 RPG 插件挂同一个独立 "The World" preset（以 standard 为底）；选用 standard preset 时 The World 插件不加载，正常 agent 功能零影响；
- 每个插件内部保留 cwd / game 语义门（`resolveGame` 模式）；
- 已知维护债：preset 以 standard 为底复制，DSH 升级时需人工跟进同步。

### DEC-B2｜首个插件：只读游戏面板 `the-world-panel`

- 包名 `the-world-panel`（与 `the-world-core` 并列于 `plugins/`）；一个插件内部分页，不拆多个插件；
- 数据全部来自 Workspace Architecture v0.2 已稳定 Owner：

| 面板内容 | 数据 Owner |
|---|---|
| 角色 / 状态 | `state/PLAYER.md` |
| 在场人物 | `state/characters/` |
| 物品 / 货币 / 系统 | `mechanics/<mechanic-id>/STATE.md` |
| 任务 / 悬而未决 | `state/THREADS.md` + 系统任务合同 |

### DEC-B3｜UI 硬边界：projection only + 唯一窄写口（v1.2 修订）

- 面板**永远只读**，不提供任何编辑 / 写回入口——**唯一例外**：「关闭线程」归档操作（见下）；
- 唯一窄写口的语义：玩家在任务页可将某条 `### T-xx` 线程关闭——面板把该线程块从 `state/THREADS.md` 移入 `story/LEDGER.md`（遵循 THREADS.md 自己的约定：「closed 线程归档入 story/LEDGER.md（历史）」）。这是归档不是删除：内容可追溯、git 可见、世界内语义正确；
- 写口实现约束：由 Node 半单一 POST 端点承载；必须通过 preset 门与 game 语义门；threadId 必须匹配 `^[A-Za-z]+-\d+$`；除该端点外插件不存在任何写路径；
- 遵守既有原则：UI 是 game truth projection，不是第二事实源；Chat 展示机制事件，UI 承载机制当前状态；
- 面板显示"当前活档案"（state/、mechanics/ 当下事实），不显示 saves/ 历史快照；玩家手动回档后面板跟随显示恢复后的时点。

### DEC-B4｜数据通路

- 浏览器半（前台网页）受浏览器安全约束不能直接读本地文件；数据由插件 Node 半（后台）经 DSH webserver 前缀路由提供 HTTP 端点；
- 刷新节奏：每回合结束后刷新一次，玩家无感；不做实时轮询重活。

### DEC-B5｜构建链进仓库（方案 A）

- the-world 仓库引入前端构建链（对齐 DSH 使用的 tsdown），仓库存人可读源码，构建产物不提交（或按实施时裁定的最小例外处理）；
- 一次搭建，后续所有 UI 插件（Map、战斗面板等）复用同一构建链；
- 接受仓库性质变化：the-world 从"纯 Markdown + 简单脚本"变为带前端工程链的仓库。

### DEC-B6｜DEF-01（quiet mode）搭车

- Gate B UI 工作期间一并处理 Chat 工作噪音隐藏；
- 仍走既定方案 B：向 `deepseek-ai/deepseek-harness` 上游提显示偏好开关 PR，不 fork / patch DSH 内部。

### DEC-B7｜Gate B 验收

- 以项目所有者授意为准，所有者认为可收口时亲自宣告；
- 参考口径（非机械门槛）：同段游戏流程有/无面板对照，沉浸感与信息可读性主观提升；面板内容与 state 文件事实一致（projection 不失真）；GM 质量不因插件引入而下降。

### DEC-B8｜Web-only

- 第一阶段 UI 插件只做 DSH Web 端，CLI 不投入（玩家环境为 Web）。

### DEC-B9｜多 game 识别

- 面板经 `resolveGame(cwd)` 同一逻辑识别当前 session 对应 game；
- `游戏定位.js` 抽为 the-world-core 与面板插件的共同依赖，不复制实现。

### DEC-B10｜宿主：dsh-better-sidebar，零修改接入（v1.1 新增）

- `the-world-panel` 以 `dsh-better-sidebar` 为宿主，经其公开服务 `ctx.betterSidebar` 接入：`registerTab` 注册「世界」面板 tab，`openTab` 在游戏 session 启动时自动打开并顶入视野（布局按会话持久化，玩家体验 = 默认显示）；
- **不 fork、不改名、不修改 better-sidebar 任何文件**；其内置功能（文件管理器 / 终端 / Git / 浏览器等）原样保留并随上游更新；
- **薄适配层**：全插件只有单个适配模块接触 `ctx.betterSidebar` API；面板组件与数据端点保持外壳无关，宿主不可用时可将适配层改接 DSH 官方 ui-slots，主体不废；
- **版本钉住**：better-sidebar 不自动升级，升级前审 changelog，升级后冒烟验证面板；
- **故障隔离**：面板失效不影响聊天、游戏本体与 the-world-core；
- **环境前置（2026-08-24 实测备注）**：宿主单独安装（peer 下限 `^0.1.0-rc.8`，匹配本机 DSH rc.8），不经 `@linxin666/dsh-web-ui-all` 全家桶引入（全家桶要求 DSH `>=0.1.1-rc.1` 且内嵌一份钉版 better-sidebar，有双份与升级滞后风险）；实施前确认 web profile `cordis.patch.yml` 中 better-sidebar 未处于 disabled；
- 第一版只作主面板 tab；`registerFileViewer` 定制渲染（如 PLAYER.md 显示为角色卡）列为后续候选，不进本版。

## 2. 风险与缓解

| 风险 | 缓解 |
|---|---|
| DSH 快速演进，client module / webserver 契约变化 | 只用文档化 seam；实施前核验 DSH current；DSH 升级时跟随检查 |
| better-sidebar 第三方 API 漂移 | DEC-B10：版本钉住 + 薄适配层 + 故障隔离 |
| 前端构建链维护成本 | 对齐 DSH 自身工具链（tsdown），不自选异构工具 |
| 面板变成"第二事实源"诱惑 | DEC-B3 硬边界（v1.2：唯一窄写口）+ Review 时专项检查 |
| 窄写口被误用 / 扩大 | 端点只接受 `T-xx` id 的归档语义，不暴露通用编辑；归档内容原块保留可恢复 |
| 为 UI 倒逼游戏数据结构化 | 禁止；数据结构由游戏真实需求驱动（Freedom Before Prevention），面板迁就文件现状 |

## 3. 后续动作

1. 生成正式实施任务包（执行线程，按 agent-task-packet 协议；实施前重做 DSH seam freshness）；
2. 实施完成 → Independent Review（含 DEC-B3 与 DEC-B10 薄适配层专项检查）；
3. 真实试玩对照 → 所有者宣告 Gate B 是否收口；
4. quiet mode 上游 PR 另派任务（可并行）。

## 4. 明确不做

- 不 fork / 改名 / 修改 dsh-better-sidebar；
- 不做 Map / 战斗面板（后续插件候选，本次不启动）；
- 不做面板编辑功能（唯一例外：DEC-B3 v1.2 的线程归档关闭）；不做 fileViewer 定制渲染（后续候选）；
- 不做 CLI 端 UI；
- 不为面板新增任何游戏数据文件 / 结构（线程归档只移动既有内容到既有 LEDGER.md，不产生新文件）。
