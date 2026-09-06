# D-004｜TA-1A Owner UAT Correction Scope

状态：**APPROVED / CURRENT**  
日期：2026-09-06  
Owner：用户（最终 Product Owner）  
Gate Owner：00｜项目总控

## Decision

TA-1A 首轮 Owner real-data UAT **未通过 Stage Exit**。当前不进入 TA-1B；先在同一 `PWB-001` / TA-1A 内完成 UAT Correction，再由 05 复审并进行 focused Owner re-UAT。

本次反馈不要求重开 Product Definition、Frozen Architecture 或 V0 Route：它们属于已冻结 TA-1A “Usable Month View / Derived Today / Low Maintenance”目标下的真实使用纠偏。

## TA-1A 必要修正

### A｜Plan Month View / 日程维护

1. 月历日期格默认不再用开始/结束时间占据主要展示空间；格内优先显示**事件标题**。时间仍保留在当天详情与编辑界面。
2. 左键点击某一天的日期格，应打开一个放大的**当天详情 GUI**，能查看该日全部 TA-1A 单次日程，并在这里直接新建、编辑、删除日程。
3. 右键日期格提供快捷菜单：
   - `新建日程`
   - `编辑日程`：进入该日详情/编辑入口，不在多事件情况下猜测某一条；
   - `清空日程`：只清空该日期的 TA-1A 单次日程，执行前必须明确确认。
4. 当天详情支持**多选日程并删除所选**，用于降低维护成本。
5. 本轮多选能力只限定在当天详情中的单次日程，不扩展成跨日期批量管理器，不引入 recurrence / Reminder / Memo / Unscheduled / Week View。

### B｜Today Current Vector

1. Today 中的 Current Vector **不得出现“今日确认 / 已完成”控制**；Current Vector 没有 Completed / acknowledgement 语义。
2. Today 中的 Current Vector **不提供直接编辑按钮**；Vector 的维护入口留在 Plan。
3. Current Vector 必须使用比“今日日程”更醒目、独立的视觉层级，不能与普通日程卡片呈现为同一种信息对象。
4. Current Vector 仍由 Plan authoritative state 派生，以上修改不得产生 Today 第二份 truth。

## 后续观察，不阻塞 TA-1A

### Tracks 手工维护摩擦

Owner 当前感觉 Track 的手动创建/维护“有点怪”，但尚不能明确指出具体字段、流程或信息结构问题。

处理：**OBSERVATION / NOT TA-1A BLOCKER**。

- 当前不因为模糊摩擦提前接入 GPT / Codex / AI-assisted Track Update；
- 相关 AI / Source maintenance 继续保持 Deferred；
- 在 TA-1A correction 后继续真实使用，待出现可描述的重复摩擦再进入 Product Discovery / Future scope。

## 已通过项继续有效

首轮 UAT 已确认以下方向正常，后续只要求 regression 不被本轮修改破坏：

- Windows tray 实际使用；
- Core state persistence / restart；
- 今日日程 acknowledgement 持久化；
- backup / restore。

## Product / Architecture Impact

**Product Definition：不重开。**

理由：
- `PE-03 Low Maintenance` 已要求维护成本足够低；
- Plan 已要求 genuinely usable Month View；
- Current Vector 在 current Product Definition 中本来就“无 Completed 语义”。

**Frozen Architecture：不重开。**

本轮不改变 canonical ownership、identity、SQLite、Electron、date semantics 或 state model。Current Vector 仍归 Plan；Today 仍是 derived projection。

**Frozen Route：不重开。**

TA-1A 仍是当前 Stage，只进入 UAT Correction loop；TA-1B 继续 BLOCKED。

## Task / Dispatch Impact

- 保持原 Task：`PWB-001｜TA-1A First Usable Core Vertical`；
- 保持同一 Task Branch 与 Primary Builder（Codex）；
- `04｜Agent 任务调度` 只需形成一个**简短 UAT Correction Addendum / Acceptance Amendment**，不得重新拆 Stage、扩大到 TA-1B 或创建大规模新 Task；
- Addendum 应把本 Decision 的 A/B 修正转成可验证 Acceptance，并要求已通过项 regression；
- Codex 修复并 Push 后**直接进入 05 复审**，不再回 04 中转；
- 05 PASS 后只对本轮纠偏点做 focused Owner re-UAT，同时确认已通过的 tray / persistence / acknowledgement / backup-restore 未回归。

## Stage Consequence

```text
TA-1A Independent Review
→ Owner UAT = REWORK REQUIRED
→ 04: short correction addendum
→ Codex: correction implementation
→ 05: independent re-review
→ Owner focused re-UAT
→ 00: TA-1A Stage Exit decision
```

在上述链路完成前：

**TA-1A = NOT EXITED**  
**TA-1B = NOT AUTHORIZED**
