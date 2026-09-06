# Personal Workbench｜Architecture

本目录只保存 **已经经过 Owner 讨论并明确提升的跨阶段长期架构约束、contract、ownership/state model 与关键设计裁定**。

当前项目处于：

> **Stage 0｜Pre-Implementation Alignment**

当前仍未 Route Freeze，因此本目录中的已批准内容只代表 Owner 已认可的架构约束与问题边界，不代表 Electron / Tauri / Native / database / schema 等技术路线已经冻结。

## 当前 Architecture Owner

- [`架构问题登记表.md`](架构问题登记表.md) — **v1.0 / OWNER-REVIEWED / G0.3 SUBMISSION / NOT ROUTE-FROZEN**
  - 收敛 Today / Plan / Tracks 的 ownership；
  - Track / Plan / recurrence / Current Vector / Today acknowledgement 开放问题；
  - persistence / backup / migration；
  - Desktop host / system tray；
  - UI / IA；
  - Future extension boundary；
  - Route Freeze 前高沉没成本问题。

原 AI Collaboration V0 的 Architecture Questions Register 已随旧路线归档至：

`Vibe-Coding/99_归档/workbench/AI协作路线_V0/架构问题登记表_未审核草案_v0.1.md`

该文件只作历史证据，不是当前 Architecture Authority。

## 当前 Owner-approved Architecture Constraints

当前已明确：

- Workbench 自己拥有 Core Personal State；
- Today 主要是 derived view，不复制 Plan / Tracks 的 live truth；
- Today 的“已完成”只作为当天视觉确认，不推进 Plan / Track；
- Current Vector 日期区间禁止重叠；
- recurrence 使用 series definition + dynamic occurrence + exception；
- recurrence edit / delete V0 只支持“仅这一次 / 整个循环”；
- 核心数据本地可靠保存；V0 需要 backup / restore 与 migration；
- system tray 属于 V0 Required，开机启动 Deferred；
- Plan 不采用万能事项表单强行统一不同用户语义；
- Sources / Update Mode 不在 V0 建同步 / AI 平台；
- Future Milestones / AI / Sync / Custom / Plugin / SDK 等继续 Deferred。

仍然开放、需要 G0.4 Reference Audit / Spike 的高风险技术决策包括：

- Electron / Tauri / Native / Web-local host；
- local store / database；
- exact schema；
- occurrence stable identity；
- timezone / date-only semantics；
- backup format；
- migration mechanism；
- packaging / update mechanism。

## 写入条件

新的架构内容进入本目录前必须：

1. 有 Owner-approved Product Baseline；
2. 已在专业聊天中向 Owner 展示关键问题 / 选项 / 风险；
3. Owner 明确批准对应内容进入 architecture；
4. 文档状态与实际批准范围一致；
5. 未裁定部分继续标 `OPEN / HYPOTHESIS / PROPOSED / DEFERRED`，不得伪装成冻结决定。

如果只是 Working Draft / Proposal：

- 默认留在聊天；
- 只有 Owner 明确要求保存草案时，才进入 `../discussion/`。

## Canonical rule

- Product / Roadmap 当前状态由 `../current/` 拥有；
- 未审核提案由聊天或 `../discussion/` 承载；
- Reference evidence 由 `../research/` 拥有；
- Owner 正式项目级裁定由 `../decisions/` 拥有；
- 代码与 runtime implementation fact 由 `zhangchenjia21-dot/Workbench` 拥有；
- `99_归档/workbench/` 只提供历史证据，不参与 current authority。

> 当前 `架构问题登记表.md` 是 G0.3 submission input，不自行宣布 G0.3 PASS；Route Freeze 前 Implementation 仍未授权。
