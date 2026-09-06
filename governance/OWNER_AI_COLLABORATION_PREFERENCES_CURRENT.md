---
title: Owner AI Collaboration Preferences
status: current-cross-project-owner-preference
version: 1.0
created: 2026-09-06
updated: 2026-09-06
owner: Owner
scope: all AI-assisted development projects governed by Vibe-Coding
---

# Owner AI Collaboration Preferences｜CURRENT

本文件记录 Owner 对 AI 驱动开发协作方式的长期偏好。它约束 GPT/Planner 如何向 Owner 解释任务、派发任务和汇报开发目标；不替代各项目 Product / Architecture / Roadmap / Task Packet。

Authority：Owner 当前明确指令 > 本文件 > 通用协作默认习惯。

## 1. 派工时必须同时给 Owner 一份“产品语言翻译”

每当 GPT / Planner 准备向 Codex、KimiCode、Grok、Claude Code 或其它执行 Agent 派发正式任务时，除 repository-native Task Packet / Agent 指令外，**必须在同一次对 Owner 的回复中，用产品呈现级别、直白、非工程术语优先的语言说明本次任务到底要实现什么。**

不能只告诉 Owner：

```text
Task ID
branch
packet path
schema / API / DTO / migration / refactor
```

而必须让 Owner 能直接理解：

```text
这次做完以后，产品会发生什么变化？
我实际会看到什么 / 能做什么 / 感受到什么？
为什么现在要做？
这次明确不做什么？
之后我应如何验收？
```

## 2. 最低必含信息

正式派工时，Owner-facing summary 至少覆盖以下四点；可以很短，但不能省略产品含义：

1. **本次任务要实现什么**：用 1–3 句话说明目标，不先从内部模块名讲起；
2. **完成后 Owner / 用户实际得到什么**：界面、行为、流程、可靠性或体验上的可观察结果；
3. **这次不做什么**：若有容易误解的边界，明确说明，防止 Owner 以为范围更大；
4. **如何判断做成了**：说明 Owner 最终需要看到或验证的结果；若当前任务不需要 Owner UAT，也说明 Engineering PASS 后下一步是什么。

推荐默认表达：

```text
本次任务要实现：<产品语言目标>。
完成后你会看到 / 能做到：<可观察结果>。
这次不会做：<关键非范围>。
验收时主要看：<产品结果>。
```

不要求机械使用固定标题；重点是信息必须存在且 Owner 一眼能懂。

## 3. 纯后台 / 基础设施任务也必须翻译成产品后果

即使某个任务没有立即可见的 UI，也不能只用工程语言描述。

例如不要只说：

```text
新增 Save migration compatibility layer。
```

应同时说明：

```text
这次不会直接改变界面；目标是让旧存档升级到新版本后仍能正常打开，并确保失败时不会破坏原存档。
```

如果任务只是为后续能力铺路，也必须明确：

```text
本次用户暂时看不到新功能；它完成后只是让下一项 <具体产品能力> 可以安全实现。
```

禁止把“基础设施完成”包装成“产品功能已经完成”。

## 4. Engineering language 与 Product language 必须分层

正式协作保持两套表达同时存在：

```text
Repository Task Packet
→ 给执行 Agent
→ 精确、工程化、可验证

Owner-facing Product Summary
→ 给 Owner
→ 直白说明产品目标、可观察结果、边界和验收
```

二者不能互相替代。

工程实现再复杂，也不能迫使 Owner 从 diff / schema / code path 中自行推断“这次到底会变成什么样”。

产品说明也不能代替 Task Packet 的技术约束、Acceptance、Git、Evidence 和 Stop Conditions。

## 5. Product-facing task 的额外要求

若任务直接影响 UI、主用户路径、生成质量、游戏性、内容呈现、交互或其它 Product Promise：

- 派工前必须说明最终真实体验目标；
- Engineering Acceptance 与 Owner Product UAT 明确分开；
- Agent 不能用测试通过代替产品成功；
- GPT 在 Owner UAT 前应重新用产品语言告诉 Owner“这轮具体看什么”。

## 6. 派工前的 Owner comprehension check

GPT / Planner 在发送正式 Agent handoff 前必须自检：

> **如果 Owner 完全不读 Task Packet，只读我这段聊天说明，他是否仍然清楚这次任务完成后产品应变成什么样？**

如果答案是否，则派工说明不合格，应先补齐产品语言解释。

## 7. 默认保持简洁

Owner-facing Product Summary 的目的不是复制 Task Packet。

默认：

- 2–6 句话通常足够；
- 复杂产品改动可使用少量分点；
- 不重复整个架构历史；
- 不为了“专业”堆叠内部术语；
- 如果用户继续追问，再展开工程细节。

原则：

> **Agent 需要精确的工程任务；Owner 需要清楚知道产品会变成什么样。两者必须同时得到。**
