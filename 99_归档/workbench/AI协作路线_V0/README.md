# Personal Workbench｜AI Collaboration V0 归档

状态：**HISTORICAL / HOLD / NON-AUTHORITY**  
归档日期：2026-09-06  
Owner：用户（最终 Product Owner）

本目录保存 Personal Workbench 第一条产品路线——**AI Collaboration V0**——退出 current 后的历史资料。

## 1. 为什么归档

该路线的核心产品纵向是：

```text
ChatGPT Web
→ Owner Selected Text
→ Coding Agent / Codex
→ GitHub
→ 当前 ChatGPT Conversation Review
```

经过 Product / Architecture / Technical Discovery，Owner 认为其中最关键的一环——在 Workbench 中可靠保留现有 ChatGPT Web 体验，并完成 Selected Text 与 current conversation handoff——存在足以威胁 Core Value 的技术可行性与长期维护风险。

因此 Owner 决定：

> **将 AI Collaboration V0 从 active tree 撤出，状态设为 HOLD，并重新打开 Personal Workbench 的 Product Direction。**

这不是永久 `REJECTED`。未来如果 ChatGPT / Agent 官方能力、浏览器集成条件或其它技术路径发生实质变化，可以重新做 Product / Technical Discovery。

## 2. Authority

本目录内全部文件：

- 只作历史、失败经验、参考和 future revisit evidence；
- **不构成 current Product / Roadmap / Architecture / Gate**；
- 不应被新 Product Discovery 默认加载；
- 不能因为文件中曾出现 `PASS` / `DECIDED` 就恢复为 current；
- 未经 Owner 审核的 03 草案中任何 `DECIDED` 字样均视为未生效历史提案。

## 3. 归档内容

- `产品定义_v0.1.md` — 原 AI Collaboration V0 Product Definition；
- `项目总纲_v0.2.md` — 旧路线仍为 current 时的项目总纲快照；
- `项目状态_归档前.md` — 旧路线退出前的 current status 快照；
- `开发路线_未审核草案_v0.1.md` — 03 未经 Owner Discussion 生成的 Draft Task Axis；
- `架构问题登记表_未审核草案_v0.1.md` — 03 未经 Owner Discussion 生成的 Architecture Questions Register；
- `备选方案池_旧版.md` — 以 AI Collaboration V0 为 current 前提编写的旧 Idea Pool。

Git history 仍保留完整演变。

## 4. 旧 Gate 状态

曾记录：

```text
G0.1 Product Baseline = PASS
G0.2 Scope = PASS
```

这些 PASS **只适用于 AI Collaboration V0**。

随着该路线退出 current，它们也成为历史状态，不能迁移到新的 Personal OS 或其它候选方向。

## 5. Revisit Trigger

仅在出现实质新信号时考虑重新激活，例如：

- ChatGPT 提供稳定官方能力支持当前 conversation / Projects / content handoff；
- Selected Text → Agent 与 Review Handoff 能通过稳定、可维护、合规路径实现；
- 新技术方案不再要求用脆弱网页自动化维持核心闭环；
- Owner 重新确认该问题仍比其它 Workbench 方向更值得优先解决。

重新激活时不得直接恢复旧 Roadmap，应重新走：

```text
Product Discovery
→ Technical Evidence
→ Owner Approval
→ New Product Definition / Route
```
