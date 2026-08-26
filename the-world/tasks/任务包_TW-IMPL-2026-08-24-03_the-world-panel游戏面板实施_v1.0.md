---
title: 任务包｜the-world-panel 游戏面板实施（Gate B 首插件）
status: current
version: 1.0
created: 2026-08-24
task_id: TW-IMPL-2026-08-24-03
task_type: implementation
scope: the-world-project / Gate B
---

# 任务包｜the-world-panel 游戏面板实施

## 1. 任务标识

- Task ID：`TW-IMPL-2026-08-24-03`
- 类型：implementation（新插件 + 构建链 + 共享模块抽取 + 部署接入）
- Owner：执行线程（由项目所有者指派）；Independent Review 另派（建议 DSH/K3）
- Repo / branch：`zhangchenjia21-dot/the-world` / `main`
- Task Base HEAD：`6f497f7`（2026-08-24，WC-08 回迁后 main HEAD）
- 工作树：`D:\AI\Projects\the world`（唯一工作树）
- DSH 部署：`D:\AI\deepseekharness`（npm 安装版，`@deepseek-ai/* @ 0.1.0-rc.8`）

## 2. Outcome 与 Why Now

实现 Gate B 首个 RPG 体验插件 `the-world-panel`：DSH Web 侧边栏中的只读游戏面板（角色/人物/物品系统/任务四分页），以 `dsh-better-sidebar` 为宿主（`ctx.betterSidebar` 的 `registerTab`/`openTab`），数据经插件 Node 半从 game workspace 投影。

产品价值（Reality Gate B 口径）：验证"DSH 插件体系能让 The World 在不重造 Agent Runtime 的情况下明显更像一款 RPG"——信息可读性与游戏感提升，且 GM 质量不下降。

十项裁定全文见 Canonical Spec（§3-2），本包不重复；与之冲突时以裁定为准并停止回报。

## 3. Authority / Source Manifest

1. 用户当前明确指令；
2. **Canonical Spec**：`Vibe-Coding/the-world/GateB_首个RPG体验插件与游戏面板裁定_v1.1_2026-08-24.md`（GitHub `zhangchenjia21-dot/Vibe-Coding` main；DEC-B1~B10 必须通读）；
3. `the-world/docs/GAME_WORKSPACE_ARCHITECTURE_v0.2.md` — 数据 Owner 真相；
4. DSH current 文档（GitHub `deepseek-ai/deepseek-harness`）：`docs/subsystems/client-modules.zh.md`、`docs/subsystems/web-server.zh.md`、`docs/cookbook/extension-cookbook.zh.md`；
5. 宿主 API 事实源（本地安装实例）：`C:\Users\MRVHREVO\.dsh\profiles\web\node_modules\dsh-better-sidebar\lib\types\client\service.d.ts`（v0.15.2，`BetterSidebarService` / `TabDescriptor` / `OpenTabSeed`）；
6. 现有实现参照：`plugins/the-world-core/`（cordis 插件形态、preset 登记、游戏定位逻辑）；
7. 本任务包。

不权威：历史聊天、旧摘要、模型记忆、对 DSH API 的旧印象（一律以 4/5 的当前文本为准）。

## 4. Read First（最小充分工作集）

1. `D:\AI\Projects\the world\AGENTS.md`
2. 本任务包 + 裁定 v1.1（§3-2 路径）
3. `plugins/the-world-core/lib/index.js` 与 `lib/游戏定位.js`（复用/抽取对象）
4. `plugins/the-world-core/preset/agent.cordis.yml`（preset 登记方式）
5. `docs/GAME_WORKSPACE_ARCHITECTURE_v0.2.md`
6. 宿主 `service.d.ts`（§3-5）
7. DSH `client-modules.zh.md` 与 `web-server.zh.md`（§3-4）

## 5. DEC / INV / AC / NON

### DEC（裁定摘要，全文以 v1.1 为准）

- DEC-B2：面板四分页数据 Owner：`state/PLAYER.md`、`state/characters/`、`mechanics/<id>/STATE.md`、`state/THREADS.md`；
- DEC-B3：**只读 projection**，无任何写回路径；显示活档案，不显示 saves/ 快照；
- DEC-B4：数据经 Node 半 HTTP 端点提供；每回合结束后刷新一次，不做高频轮询；
- DEC-B5：构建链（tsdown）进仓库，源码入库；
- DEC-B8：Web-only；
- DEC-B9：`resolveGame(cwd)` 逻辑抽为 the-world-core 与本插件共同依赖，不复制实现；
- DEC-B10：宿主零修改；**仅单个适配模块**接触 `ctx.betterSidebar`；宿主缺失时优雅降级不崩；游戏 session 启动时 `openTab` 自动顶入视野。

### INV（不变量）

- INV-1：`git add` 只用显式路径，禁止 `git add -A`；
- INV-2：不触碰 `games/luan-shi-sanguo/` 与 `games/_template/` 任何文件（游戏现场与模板）；
- INV-3：不修改 `dsh-better-sidebar`、不修改 DSH 内部（`node_modules/@deepseek-ai/*`）任何文件；
- INV-4：the-world-core 既有行为不回归（其 `test/` 与 `scripts/check-preset.mjs` 须通过）；
- INV-5：面板到游戏文件之间不存在任何写调用（代码层无 write API 暴露）；
- INV-6：public 仓库，不提交凭证/私密内容；构建产物（`dist/`、`lib/client.js` 等）不提交，入 `.gitignore`；
- INV-7：push 前重新核验远程 HEAD。

### AC（验收）

- AC-1：`plugins/the-world-panel/` 构建通过，产出 client bundle，`package.json` 含正确 `dsh.client` 声明与 `exports["./client"]`；
- AC-2：DSH Web 启动（The World preset、游戏 cwd）后，better-sidebar 出现「世界」tab 并自动打开顶入视野；
- AC-3：四分页内容与 `games/luan-shi-sanguo` 的 PLAYER/characters/mechanics/THREADS 文件事实一致（抽查 ≥3 处，projection 不失真）；
- AC-4：代码审查确认面板链路到游戏文件只读；UI 无任何编辑/写回入口；
- AC-5：完成一个游戏回合后，面板无需手动操作即更新；实现中不存在 >1 次/5s 的定时轮询；
- AC-6：非游戏 cwd 下 tab 不自动打开（或显示空闲态）；standard preset 下插件不加载；
- AC-7：better-sidebar 未启用/不可用时，插件加载不报错、不崩 DSH，面板功能静默降级；
- AC-8：the-world-core 全部既有测试与 check-preset 通过；
- AC-9：commit 仅含 §7 范围文件；push 后远程可见。

### NON（禁止范围）

- 不做 `registerFileViewer` 定制渲染（后续候选）；
- 不做 Map / 战斗面板 / 编辑功能 / CLI 端；
- 不为面板新增游戏数据文件或改变任何 Owner 文件格式；
- 不做 quiet mode（另任务）；
- 不修改 `library/`、`docs/`（实施发现文档漂移时停止回报，不顺手改）。

## 6. Allowed / Prohibited Scope

Allowed：读取一切相关文件；在 §7 范围内新建/编辑；本地构建与冒烟测试；显式路径 add/commit/push。

Prohibited：`git add -A`；force-push；改游戏现场；改宿主与 DSH 内部；把面板做成第二事实源的任何写路径。

## 7. Deliverables（范围指导，精确文件树由实现定）

```text
plugins/the-world-panel/           # 新插件（Node 半 + client 半 + package.json + tsdown 配置 + README）
plugins/shared/ 或等价位置          # DEC-B9：resolveGame 等游戏定位共享模块（抽取自 the-world-core/lib/游戏定位.js）
plugins/the-world-core/            # 仅改动：改为引用共享模块（行为不变）
plugins/the-world-core/preset/agent.cordis.yml  # 登记 the-world-panel
plugins/README.md                  # 补 the-world-panel 一行
部署脚本/文档                       # 把插件（含构建产物）接入 D:\AI\deepseekharness 的可重复流程（脚本或 README 步骤）
.gitignore                         # 构建产物排除
```

架构脊柱（指导性，非强制文件名）：

```text
Node 半：cordis apply() → webserver 前缀路由（如 /the-world/panel/state）
        → 读取 cwd 对应 game 的四个 Owner → JSON projection（含 game id / 更新时间）
浏览器半：宿主适配模块（唯一接触 ctx.betterSidebar）
        → registerTab('the-world:panel')；启动时探测端点，是游戏则 openTab
        → 面板组件：四分页渲染 projection；刷新机制满足 AC-5（推荐 fs.watch + SSE 或回合事件驱动，禁止高频轮询）
```

## 8. Acceptance Gates

§5 AC-1 ~ AC-9 全部通过；随后移交 Independent Review（专项：DEC-B3 只读边界、DEC-B10 薄适配层、INV-2/3 现场纪律）。

## 9. Validation Commands

```bash
cd "D:/AI/Projects/the world"
git status -sb                       # 现场检查：游戏现场文件与本任务文件可分

# 构建（具体命令以最终工具链为准）
cd plugins/the-world-panel && tsdown   # 或 package.json scripts.build

# the-world-core 回归
cd ../the-world-core && node scripts/check-preset.mjs && node --test test/  # 按其实际脚本

# 部署后冒烟（人工 + 浏览器）：DSH Web → The World preset → games/luan-shi-sanguo cwd
# ① 「世界」tab 出现并自动打开；② 四分页内容对照文件；③ 玩一个回合看自动刷新；
# ④ 禁用 better-sidebar 重启 → 无报错；⑤ standard preset → 插件不加载。

git add <显式路径>
git commit -m "feat(panel): the-world-panel 只读游戏面板（better-sidebar 宿主，Gate B 首插件）"
git push origin main
```

## 10. Git / Integration 责任

- 开始前 `git fetch` 核验远程 HEAD 与 Base `6f497f7` 的关系；有新增量先审计是否触及 `plugins/`；
- 提交仅显式路径；push 后记录远程新 HEAD SHA。

## 11. Stop / Return Conditions

- client module 扫描机制要求把插件发布到 npm 或 patch DSH 内部才能加载（部署链路走不通时停止回报，不准绕道 fork/patch）；
- 宿主 `service.d.ts` 实际 API 与本包描述（v0.15.2）不符；
- 环境前置未满足（better-sidebar 被禁用 / 全家桶残留导致双份宿主）——先停止并要求项目所有者处理；
- 远程 HEAD 增量触及 `plugins/` 或本任务文件；
- 为让面板"更好看"而需要改动游戏文件格式（触发 DEC-B3/Freedom Before Prevention 边界）。

出现以上任一情况：停止，保留现场，回报规划线程。

## 12. Final Report 格式

```text
Task: TW-IMPL-2026-08-24-03
Base HEAD / Final remote HEAD: <sha> / <sha>
Validation: AC-1..AC-9 PASS/FAIL（各一行证据）
Commit: <sha> <message>
Files: <新增/修改清单>
部署方式: <脚本/步骤摘要>
宿主接入: <registerTab/openTab 实际用法与版本>
Deviations: 无 / 描述
```
