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

- [ ] Gate B 面板插件（the-world-panel）：十项裁定已收口（见 `GateB_首个RPG体验插件与游戏面板裁定_v1.1_2026-08-24.md`），实施任务包见同目录 `任务包_TW-IMPL-2026-08-24-03_the-world-panel游戏面板实施_v1.0.md`，待派发执行线程；quiet mode 上游 PR 可并行另派。环境前置：启用 web profile 中被禁用的 better-sidebar；卸载 `@linxin666/dsh-web-ui-all` 全家桶（项目所有者已确认将卸载）。
- [ ] TW-01 后台维护两层拆分：已在 the-world/main 实施（commit `eab96a4`，2026-08-24），待 luan-shi-sanguo 实测验证后正式收口。
- [ ] Gate A 正式收口宣告：试玩基本成立，待项目所有者择机正式宣布。
- [ ] sillytavern-assets：STA-ADAPT-02 资产已入库 the-world/main（commit `dc46271`，v1+v2 收口）；Independent Review 是否已做待确认。
- [ ] sillytavern-assets：资产族治理文档（索引 / 蓝图 / 版本锁）同步时机待裁定——适配全量收口后统一处理，还是随 v2 合并处理。

## 已完成（移出登记）

- TW-DOC-2026-08-24-02（WC-08 回迁+删克隆）：2026-08-24 完成，commit `6f497f7`，六项 AC 全 PASS，规划线程已对照 GitHub 核验。遗留 P3：世界包内容 v0.2.4 与文件名 v0.2.3 脱钩（为保护约 20 张人物卡引用，Revision Notes 已注明）；下一份世界包资产文档更新时评估是否统一更名。
- TW-ASSET-2026-08-24-01（d20 判定包提交）：2026-08-24 由项目所有者直接 push 完成（HEAD `3381595`），已实用于 luan-shi-sanguo。
