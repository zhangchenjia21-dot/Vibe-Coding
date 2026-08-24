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
- **Revisit trigger**：Gate B RPG UI 工作时一并处理；或游玩体验明确恶化时提前。
- **Non-regression constraint**：不 fork / patch DSH 内部；若急需临时缓解，可用浏览器级 CSS（方案 A，自用、随 DSH 升级需维护），但不作为正式交付。
- **环境备注**：玩家走 DSH Web 界面（非 CLI）。

## 待办

- [ ] TW-01 后台维护两层拆分：实施（见同目录裁定文档 v1.0），完成后在 luan-shi-sanguo 实测验证。
- [ ] sillytavern-assets：STA-ADAPT-02（剩余 18 资产适配）已派发给 Kimi Code，完成后 Independent Review。
- [ ] sillytavern-assets：资产族治理文档（索引 / 蓝图 / 版本锁）同步时机待裁定——适配全量收口后统一处理，还是随 v2 合并处理。
- [ ] Vibe-Coding/the-world/ 目录骨架已建立（2026-08-24）。
