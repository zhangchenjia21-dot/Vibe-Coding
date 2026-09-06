# D-002｜正式 Pivot 到 Personal State Core

状态：**APPROVED / CURRENT**  
日期：2026-09-06  
Owner：用户（最终 Product Owner）

## Decision

Owner 正式批准将 Personal Workbench 当前产品方向 Pivot 为：

> **长期驻留在电脑上的个人信息与行动桌面（Personal State / Information & Action Desktop）。**

当前 CORE：

```text
Today
Plan
Tracks
```

当前 V0：

> **Thin Tracks + Usable Plan + Derived Today**

同时批准：

- 桌面长期驻留 / 高频唤起 + Today 默认入口，作为 Product Experience Requirement；
- Milestones 保持 OPEN，不进入 V0 Required；
- GitHub Source / AI-assisted Track Update 不进入 V0 Required；
- 具体交互细节与技术实现进入后续 Architecture / UX / Reference Audit；
- 原 AI Collaboration V0 继续保持 `HOLD / HISTORICAL / FUTURE RE-DISCOVERY`。

## Context

原 AI Collaboration V0 高度依赖可靠保留和操控现有 ChatGPT Web 体验，包括 Selected Text 与 current conversation handoff。技术讨论表明这条路径存在足以威胁 Core Value 的可行性和长期维护风险，因此已退出 current。

新的 Product Pivot Discovery 发现 Owner 当前更明确的现实痛点是：

> 已有大量长期规划、个人资料、未来安排和日常事项，但这些信息分散，当前状态长期主要依赖记忆，缺少一个长期驻留、可以持续查看和直接维护的整体个人工作台。

`discussion/产品方向评审.md` 已对 Today / Plan / Tracks、V0 厚度、AI Position、成功/失败条件和 Pivot Options 做了讨论收敛。

Owner 在 2026-09-06 明确回复：**批准，进行下一步。**

## Why

选择该方向的主要理由：

1. 直接对应 Owner 已确认的真实生活 / 工作信息痛点；
2. Core Personal State 可以由 Workbench 自己拥有，不再把外部 SaaS UI 作为产品成立的单点依赖；
3. Today / Plan / Tracks 构成长期、未来、今天三个互补时间尺度；
4. `Thin Tracks + Usable Plan + Derived Today` 足以形成真实可用纵向，又避免立即构建完整 Personal OS；
5. 产品价值可以通过 Owner 的真实日常使用直接验证；
6. AI 可以未来作为辅助能力加入，而不是 V0 的 core dependency。

## Alternatives considered

### A｜继续 AI Collaboration

当前不采用。核心技术风险和外部依赖过高；保持历史 HOLD，未来若技术条件实质变化可重新 Discovery。

### B｜Personal State / Information & Action Desktop

**APPROVED**。

### C｜当前同时规划 Personal OS + AI Collaboration 双核心

当前不采用。会提前扩大 Scope 和 Architecture 负担；长期并不排除重新增加 AI Collaboration。

## Consequences

### Product

- 新建 `current/产品定义.md` 作为 current Product Definition；
- Today / Plan / Tracks 正式进入 CORE；
- Candidate V0 正式变为当前 V0 Scope；
- 原 Pivot Review 保留为 discussion evidence，不与 current 竞争 Authority。

### Stage Gate

00 在本决定后重新审核：

```text
G0.1 Product Baseline = PASS
G0.2 Scope = PASS
G0.3 Draft Task Axis = NEXT
```

旧 AI Collaboration V0 的 Gate 状态仍只属于历史路线。

### Architecture

本决定**不冻结**：

- Electron / Tauri / Native / Web；
- database；
- schema；
- state machine；
- recurrence implementation；
- persistence / sync；
- UI component architecture。

这些进入 03 / 02 的后续路线与证据流程。

### Implementation

**NOT AUTHORIZED**。

Route Freeze 之前不得因为本次 Pivot 直接开始正式功能实现。

## Affected owners

- `current/产品定义.md`
- `current/项目总纲.md`
- `current/项目状态.md`
- `current/README.md`
- `workbench/README.md`

## Historical relationship

- `D-001_暂停AI协作V0并重开产品方向.md` 继续有效，描述旧路线退出 current；
- 本 D-002 关闭了 D-001 打开的 Product Direction Re-evaluation，并建立新的 current 产品方向；
- `99_归档/workbench/AI协作路线_V0/` 继续只作历史 evidence。
