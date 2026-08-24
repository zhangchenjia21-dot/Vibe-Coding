---
title: The World｜延后裁定与待办
status: current
version: rolling
created: 2026-08-24
last_updated: 2026-08-24
scope: the-world-project
---

# The World｜延后裁定与待办 CURRENT

> 滚动 current 文件：延后项在此正式登记（延后必须是正式裁定，不是遗忘）；完成或失效时移出并记入相关复盘 / 归档。

## DEF-01｜DSH Chat 工作噪音隐藏（quiet mode）

- **能力**：隐藏 DSH Web Chat 正文前后的 read / think / edit 单行记录，玩家主阅读流只见叙事文本。
- **选定方案**：方案 B——向 `deepseek-ai/deepseek-harness` 上游提 Chat 显示偏好开关（隐藏 tool rows / thinking rows）。已核验 DSH current：Web 与 CLI 两侧均无现成开关；trace 已有独立 Trajectory 视图与详情面板，隐藏不丢调试能力，符合"隐藏工作噪音，不限制 Agent 能力"（DEC-P11）。
- **Why deferred**：不阻塞 TW-01；体验影响可控；上游 PR 周期不受项目控制。
- **Underlying semantics already proven by**：DSH UI 插件化 slots 机制存在；ToolRow 单行折叠已是现状，隐藏属纯展示层。
- **Revisit trigger**：~~Gate B RPG UI 工作时一并处理；或游玩体验明确恶化时提前。~~ **已触发（2026-08-24，DEC-B6）**：Gate B 面板插件工作期间一并处理，待随实施任务包派发。
- **Non-regression constraint**：不 fork / patch DSH 内部；若急需临时缓解，可用浏览器级 CSS（方案 A，自用、随 DSH 升级需维护），但不作为正式交付。
- **环境备注**：玩家走 DSH Web 界面（非 CLI）。

## 待办

- [ ] WC-08 设计文档回迁：`D:\AI\deepseekharness\user-repos\the-world`（DSH 克隆）藏有唯一一份 WC-08 Game Composition 设计批次（PLAN +47 行、6 文件配套、`library/worlds/三国/` 草稿），功能已实施但 main 文档缺失。执行线程按任务包 TW-DOC-2026-08-24-02（见同目录 v1.0）回迁 → push → 删克隆。删除前禁止清空该目录。
- [ ] Gate B 面板插件（the-world-panel）：十项裁定已收口（见 `GateB_首个RPG体验插件与游戏面板裁定_v1.1_2026-08-24.md`），待生成正式实施任务包并派发执行线程；quiet mode 上游 PR 可并行另派。环境前置：启用 web profile 中被禁用的 better-sidebar；建议卸载 `@linxin666/dsh-web-ui-all` 全家桶（要求 DSH>=0.1.1-rc.1，与 rc.8 不匹配）。
- [ ] TW-ASSET-2026-08-24-01：判定与检定过渡包（d20）已由规划线程在 `D:\AI\Projects\the world` 本地落地（5 个文件，未提交），执行线程按任务包验证并提交推送（见同目录任务包 v1.0）。
- [ ] TW-01 后台维护两层拆分：已在 the-world/main 实施（commit `eab96a4`，2026-08-24），待 luan-shi-sanguo 实测验证后正式收口。
- [ ] Gate A 正式收口宣告：试玩基本成立，待项目所有者择机正式宣布。
- [ ] sillytavern-assets：STA-ADAPT-02 资产已入库 the-world/main（commit `dc46271`，v1+v2 收口）；Independent Review 是否已做待确认。
- [ ] sillytavern-assets：资产族治理文档（索引 / 蓝图 / 版本锁）同步时机待裁定——适配全量收口后统一处理，还是随 v2 合并处理。
