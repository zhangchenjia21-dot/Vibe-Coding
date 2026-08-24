---
title: 任务包｜the-world-panel 独立审核（Gate B）
status: current
version: 1.0
created: 2026-08-24
task_id: TW-REV-2026-08-24-04
task_type: independent-review
scope: the-world-project / Gate B
---

# 任务包｜the-world-panel 独立审核

## 1. 任务标识

- Task ID：`TW-REV-2026-08-24-04`
- 类型：independent-review（只审不改）
- Owner：审核线程（**不得是实施 TW-IMPL-03 的同一线程**；建议 DSH/K3"住户验房"或独立 Kimi Code 会话）
- Repo / branch：`zhangchenjia21-dot/the-world` / `main`
- Review Base HEAD：`f1b61d7`（2026-08-24 12:41 UTC，面板实施最新提交）
- 工作树：`D:\AI\Projects\the world`
- 被审对象：任务包 TW-IMPL-03 的全部交付（`Vibe-Coding/the-world/任务包_TW-IMPL-2026-08-24-03_the-world-panel游戏面板实施_v1.0.md`）

## 2. Outcome 与 Why Now

the-world-panel 已上线并被玩家真实使用。按项目规矩 Implementation 与 Independent Review 分离：本任务独立核对实现是否守住裁定边界与任务包验收，产出三类审核结论（**推进前必须修复 / 阶段末清理 / Future**）。Gate B 宣告（所有者授意）以本审核通过为前提之一。

## 3. Authority / Source Manifest

1. 裁定 Canonical：`Vibe-Coding/the-world/GateB_首个RPG体验插件与游戏面板裁定_v1.2_2026-08-24.md`（GitHub main；**注意 DEC-B3 已是 v1.2 修订版：唯一窄写口=线程归档**）；
2. 实施任务包：`Vibe-Coding/the-world/任务包_TW-IMPL-2026-08-24-03_the-world-panel游戏面板实施_v1.0.md`（AC-1~9 为验收基线）；
3. 被审代码：`the-world/main @ f1b61d7` 的 `plugins/the-world-panel/`、`plugins/shared/`、`plugins/the-world-core/` 相关改动、`plugins/README.md`；
4. 数据语义：`docs/GAME_WORKSPACE_ARCHITECTURE_v0.2.md`；
5. `the-world/AGENTS.md`。

不权威：实施线程的自述、聊天摘要、模型记忆。

## 4. Read First（最小充分工作集）

1. `D:\AI\Projects\the world\AGENTS.md`
2. 本任务包 + 裁定 v1.2 + TW-IMPL-03 任务包（§3-1/3-2）
3. `plugins/the-world-panel/`（`lib/index.js`、`lib/线程归档.js`、`src/client/index.jsx`、`package.json`、`tsdown.config.mjs`、`scripts/`、`test/`、`cordis.patch.yml`）
4. `plugins/shared/游戏定位.js` 与 `plugins/the-world-core/lib/index.js`（共享抽取核对）
5. `git log --oneline 6f497f7..f1b61d7` 与对应 diff

## 5. 审核清单

### R1｜DEC-B3 v1.2 窄写口（专项重点）

- [ ] 全插件**唯一写路径**是线程归档 POST 端点（搜全部 write/POST/PUT/fs 写调用）；
- [ ] threadId 校验 `^[A-Za-z]+-\d+$`；端点经 preset 门与 game 语义门；
- [ ] 归档语义正确：线程块**完整**从 `state/THREADS.md` 移入 `story/LEDGER.md`，内容不丢不改写（归档≠删除）；
- [ ] 失败显形：归档失败在 UI 明确呈现（不静默）；
- [ ] CRLF 兼容修复（`f1b61d7`）正确且不无回归；
- [ ] `test/线程归档测试.js` 覆盖上述语义并实际通过。

### R2｜DEC-B10 薄适配层

- [ ] 仅单个适配模块接触 `ctx.betterSidebar`；面板组件与数据端点外壳无关；
- [ ] 宿主缺失/未启用时插件不崩、不阻塞 DSH（优雅降级）。

### R3｜projection 保真与只读

- [ ] 除 R1 窄写口外，面板到游戏文件无任何写路径；
- [ ] 面板渲染内容与 `games/luan-shi-sanguo` 的 PLAYER / characters / mechanics STATE / THREADS 抽查 ≥3 处一致（不失真、不显示 saves/ 快照）。

### R4｜任务包 AC 对照

- [ ] TW-IMPL-03 §5 AC-1~9 逐条核对（构建、dsh.client 声明、自动 openTab、刷新机制无高频轮询、非游戏 cwd 行为、standard preset 不加载、the-world-core 回归）。

### R5｜纪律

- [ ] `git log 6f497f7..f1b61d7` 全部提交未触碰 `games/` 现场、`library/`、`docs/`、宿主与 DSH 内部文件；
- [ ] 构建产物未入库（.gitignore 正确）；
- [ ] public 安全：无凭证/私密内容。

### R6｜the-world-core 回归

- [ ] `plugins/the-world-core` 全部测试与 `scripts/check-preset.mjs` 通过；共享抽取未改变其行为。

## 6. Allowed / Prohibited Scope

Allowed：读取一切；运行构建、测试、冒烟脚本；`git log/diff/show`；本地运行 DSH 冒烟（如需）。

Prohibited：**修改任何文件**（发现问题的输出是报告，不是补丁）；commit / push；评判裁定本身（裁定问题单列"裁定层面观察"一节，不混入实现审核）。

## 7. 审核方法

1. 静态：Read First + §5 逐条核；
2. 动态：跑 `plugins/the-world-panel/test/`、`scripts/smoke-render.mjs`、`the-world-core` 测试；
3. 实证：对照 `games/luan-shi-sanguo` 文件抽查渲染保真；
4. 每条结论附证据（文件:行 / 命令输出摘要）。

## 8. Stop / Return Conditions

- 发现 P0 级问题（第二事实源、写路径外溢、覆盖 canonical state、私密信息入库）：立即报告，不继续审；
- Review Base 之后远程出现新的 panel 相关提交：报告并说明审核覆盖到哪个 HEAD。

## 9. Final Report 格式

```text
Task: TW-REV-2026-08-24-04
Review Base HEAD: f1b61d7
R1..R6: PASS / FAIL / PARTIAL（各附证据）
推进前必须修复: <列表，或"无">
阶段末清理: <列表，或"无">
Future: <列表，或"无">
裁定层面观察: <列表，或"无">
总体结论: 通过 / 有条件通过 / 不通过
```
