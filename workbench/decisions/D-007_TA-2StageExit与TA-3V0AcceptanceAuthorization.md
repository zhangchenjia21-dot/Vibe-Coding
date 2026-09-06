# D-007｜TA-2 Stage Exit 与 TA-3 V0 Acceptance Authorization

状态：**APPROVED / CURRENT**  
日期：2026-09-06  
Owner：用户（最终 Product Owner）  
Gate Owner：00｜项目总控

## Decision

00 基于 TA-2 Independent Review 与 implementation evidence，正式记录：

1. `TA-2 Reliability Hardening = PASS / STAGE EXIT`；
2. 接受 05 的 `PASS_WITH_NOTES`，当前 notes 均为已知限制，不构成 Stage Exit blocker；
3. A-01 stale acknowledgement 已确认并完成最小 root fix；
4. 05 明确判断无需 Owner focused retest；
5. 将 `task/PWB-003-ta2-reliability-hardening@5fba3cb0ad3016f5c2f259e668f97699f7c7c80a` fast-forward 集成到 `Workbench/main`；
6. `TA-3 V0 Acceptance = AUTHORIZED`；
7. TA-3 是最终产品验收阶段，不是新的功能开发阶段；除非最终验收发现真实 blocker，否则不生成新的功能实现任务。

## TA-2 Accepted Result

TA-2 只发现一个真实 reliability finding：

- 单次删除 recurring occurrence 后 stale acknowledgement 可能在后续 re-edit 同一 original occurrence key 时重新可达。

修复保持极小边界：

- 只修改现有 `saveException` 流程；
- tombstone / tombstone re-edit 时在同一 transaction 内精确清理对应 occurrence acknowledgement；
- 不改变 schema、public API、UI、identity、backup/restore/migration 或 frozen architecture。

05 已独立确认完整 regression、packaged Windows 与 repeated-use evidence 通过。

## Accepted Notes

以下作为 V0 已知限制接受，不阻塞 TA-3：

1. 老备份中可能存在从未再次触碰过的 tombstoned occurrence acknowledgement；它们不可见、不参与 projection，后续触碰对应 tombstone 时会精确清理；V0 不增加 startup/restore 全表 sweep。
2. Reliability evidence 是 bounded repeated-use，而不是多日 soak / memory benchmark / 断电硬件测试。
3. Tray 物理 notification-area 鼠标路径沿用 TA-1A Owner 已验证结果；TA-2 未改变 tray production code。

## TA-3 Boundary

TA-3 只回答：

> 当前 V0 是否已经值得 Owner 作为真实个人工作台持续使用？

最终验收重点：

- Today / Plan / Tracks 是否形成自然使用路径；
- 维护成本是否低于它节省的记忆、查找与重新规划成本；
- Complete V0 在真实数据下是否存在阻塞性可靠性或体验问题；
- 已知限制是否可接受；
- V0 是否应正式关闭，进入下一阶段 Product Discovery。

TA-3 不自动授权 Milestones、AI、Sync、Custom、Plugin / SDK 等 Future scope。

## Consequences

- `Workbench/main` 进入 TA-2 accepted baseline；
- `current/项目状态.md` 切换到 TA-3 V0 Acceptance；
- 04 / Codex 暂无默认实现任务；
- 最终 V0 Product Acceptance 由 Owner + 00 完成；
- 若验收发现具体 blocker，再由 00 判断回 05/Codex、03 或 01。