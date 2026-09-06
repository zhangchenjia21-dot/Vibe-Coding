# D-005｜TA-1A Stage Exit 与 TA-1B Authorization

状态：**APPROVED / CURRENT**  
日期：2026-09-06  
Owner：用户（最终 Product Owner）  
Stage Owner：00｜项目总控

## Decision

00 基于 05 Independent Review、Owner real-data UAT、UAT Correction、05 focused re-review 与 Owner focused re-UAT，正式裁定：

1. `TA-1A First Usable Core Vertical = PASS`；
2. `PWB-001` 已完成其 Stage 目标，无已知 Stage Exit blocker；
3. 已接受实现以 fast-forward 方式集成到 `zhangchenjia21-dot/Workbench main@50bebd2b07619e4e06852ad8a20c110be0553315`；
4. `TA-1B Complete V0 = AUTHORIZED FOR TASK SHAPING / DISPATCH`；
5. TA-1B 必须从当前 integrated `main` 起步，并保持 frozen Product / Architecture / Route；
6. TA-1B 完成后仍必须经过 05 Independent Review + Owner Complete V0 UAT，不能自动进入 TA-2。

## Evidence

- 05 Independent Review：PASS；
- 首轮 Owner UAT：发现 Month View / Current Vector usability issues；
- D-004：收敛 TA-1A UAT Correction Scope；
- correction implementation + 05 independent re-review：PASS；
- Owner focused re-UAT：PASS；
- Task Branch 与原 implementation main 无并行冲突，可 fast-forward；
- 05 Stage Exit Handoff：当前无已知 blocker。

## Product / Architecture impact

不重开 Product Definition、Frozen Architecture 或 Frozen Route。

TA-1A UAT correction 证明了 First Usable 验证边界有效：产品方向成立，但实际交互必须通过 Owner 使用纠偏。

保留观察项：Tracks 手工维护略显生硬，但问题尚未具体化；AI / GPT / Codex assisted Track maintenance 继续 Deferred。

## TA-1B boundary

TA-1B 只补齐 frozen V0 中被 TA-1A 有意延后的：recurrence、Reminder、Unscheduled、Memo、Week View 与相关 Today projection / boundary regression。

不得引入 recurrence `这一次及以后`、Milestones、AI/sync、Custom、Knowledge/Habit/Finance/Health、Plugin/SDK、cloud sync 等 Future scope。

## Consequences

- `current/项目状态.md` 切换到 TA-1B Authorized for Dispatch；
- `current/README.md` / `项目总纲.md` 更新 Stage；
- 下一专业聊天：`04｜Agent 任务调度`；
- 04 为 TA-1B 形成新的 executable Task Packet；
- Codex 完成后直接交 05，不通过 04 中转。
