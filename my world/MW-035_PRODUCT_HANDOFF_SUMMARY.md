# MW-035 Product Handoff Summary

## 本次任务要实现什么

让 Public d20 的“是否需要检定 / 难度与修正”判断在长局里优先依据**当前角色、当前世界、当前背包、当前公开机制**，而不是反复主要读取 New Game 时的整套角色/NPC/世界背景。

## 完成后实际产品会有什么变化

玩家不会看到新的页面或按钮。

实际体验变化是：长时间游玩后，即使某项能力变化、世界变化已经发生在很多回合以前，d20 control 仍能从当前持久状态得到这些信息；同时大型开局背景不会无条件挤占 mechanics control 的上下文。

例如：主角经过几十回合已经学会某项技能，相关原始对话早已滚出旧窗口时，后续检定仍应看到“当前角色已经具备这项能力”。

## 这次明确不做什么

- 不增加属性点/技能数值系统；
- 不改变 d20 规则、随机数、CHECK/NO_CHECK schema；
- 不改变第二次 malformed 后降级为普通 Narrative 的既有策略；
- 不做通用 retry / Structured Output 平台；
- 不做 Embedding / Vector DB / semantic retrieval；
- 不改 UI；
- 不进入 Package 9。

## 之后如何验收

本轮由 Codex 完成 deterministic Engineering proof，GPT Independent Review。

Owner 本轮不需要单独测试。MW-035 与 MW-032/MW-033/MW-034 一起进入后续集中 G6+G7 Product test，重点观察长局时 mechanics 判断是否真正跟得上当前角色与当前世界。
