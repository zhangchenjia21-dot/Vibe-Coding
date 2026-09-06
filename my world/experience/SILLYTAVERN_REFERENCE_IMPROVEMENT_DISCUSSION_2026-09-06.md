---
title: my world｜SillyTavern 参考研究改进讨论通过清单
status: working-approved-candidate-list
version: 1.2
created: 2026-09-06
updated: 2026-09-06
source_reference: ./SILLYTAVERN_UPSTREAM_FUNCTIONAL_REFERENCE_AUDIT_2026-09-06.md
implementation_authorization: none
roadmap_authorization: none
---

# my world｜SillyTavern 参考研究改进讨论通过清单

## 0. 用途

本文件只记录 Owner 在基于原版 SillyTavern 功能参考研究进行的产品改进讨论中，**明确表示通过的提案**。

重要边界：

- `通过提案` != `立即实施`；
- 本文件不是 CURRENT Roadmap，不是 Task DAG，不是 Architecture Decision；
- 在本轮改进讨论完成前，不据此创建新的 MW Work Item；
- 讨论结束后，由 GPT 基于全部通过提案统一做重叠、依赖、冲突、阶段与 UAT 审计，再提交 Revised Task Axis 给 Owner；
- 只有路线重构经 Owner 批准后，才进入正式 Product / Architecture Freeze 与 Task Packet。

未明确通过的提案不进入本清单；它们既不视为已批准，也不自动视为永久否决。

---

## 1. 已通过提案

### P-01｜玩家安全的生成状态 / 诊断

**Owner verdict：通过（原提案 1）。**

让关键 AI 后台链路具备有限、玩家安全、按需展开的可观察性，使“人物卡没出现、推荐没生成、整理没更新”等情况能区分：仍在处理、模型无有效输出、结构校验失败、currentness 失效、超时/取消、保存/展示问题等。

候选产品层：普通玩家只看安全状态；Owner / developer 高级诊断可进一步看实际 Provider/model、请求是否发出、结构终态、accepted prefix/currentness 等。

硬边界：不泄露 NPC 私密资料、未公开角色、幕后计划、GM-private context、credential；不把 Prompt 明细冒充模型思维；诊断不是第二事实源，叶 UI 不接 omniscient state 自行过滤。

### P-02｜叙事偏好 / Narrative Preference

**Owner verdict：通过（原提案 3）。**

用少量高层偏好与自然语言补充控制叙事的节奏、镜头、细节密度和关注重点，而不是复制 Author's Note / Prompt Manager 的提示词工程面板。

核心边界：

> **Narrative Preference 只影响表达与镜头，不成为 World Truth、NPC 意图、结果保证或剧情状态机。**

允许“重要对话写细、普通赶路快速略过、少解释多动作”；不允许“所有战斗必须赢、某 NPC 必须爱上玩家、强制未来、固定每回合字数”等结果控制。

后续需冻结长期偏好 vs 单回合临时要求的 ownership 与 GM-request 注入边界。

### P-03｜结构化模型输出可靠性

**Owner verdict：通过（原提案 6）。**

对本来就要求机器结构输出的后台 AI lane，优先使用 Provider 明确支持的 Structured Output / JSON Schema 等原生能力，降低“语义正确但包装格式错误”造成的整项失效。

典型证据：MW-019 真实验证中模型返回正确的五条行动与 JSON 内容，但外包 Markdown fence，被当前严格契约正确拒绝。

硬边界：不靠 Regex/fence stripping/字段猜测建立语义修补森林；不自动补造缺失内容；不无限重试；不静默切换 Provider；Program 仍做结构验证；Provider 不支持时继续严格 JSON + fail-soft。

采用时应先选一个真实 lane 做 vertical，再决定是否推广，避免提前抽象通用平台。

### P-04｜Source Library 作品化 / 内容发现体验

**Owner verdict：通过（原提案 7）。**

把“选择机器资产/数据包”的建局体验逐步升级为“选择我要玩的世界、角色和玩法内容”。

候选展示包括封面/缩略图、公开简介、标签、作者、版本说明、Expansion 功能说明、player-safe compatibility/dependency 提示与建局前预览。

硬边界：公开介绍 != GM-private Source material；discovery metadata != Runtime World Truth；既有 Game 继续绑定 exact frozen generation；不因此提前建设在线商店、账号、云或万能 Package Manager。

### P-05｜玩家可读的冒险纪事导出

**Owner verdict：通过（原提案 9）。**

让长期时间线最终可导出为属于这局世界与玩家选择的可阅读作品记录。第一版优先确定性整理 accepted Player action + GM Narrative、Important Experiences、允许范围内的 People 等玩家可见材料，输出 Markdown / HTML 等阅读格式。

必须区分：

> **可读冒险纪事 != 可恢复 Game Backup / Migration Package。**

默认不导出 GM-private truth、NPC private Knowledge、Agency/Evolution、credential。未来若提供 AI 文学化版本，必须标识为派生版且可回溯 accepted history，不反向成为游戏真相。

### P-06｜Reference Library / 大型世界参考资料层

**Owner verdict：通过（原提案 11）。**

为历史、制度、地理、文化、技术等大型参考资料建立可复用、按需取用的 Reference Library，使大型世界不必把所有研究材料永久塞入 World Pack / 每轮上下文。

典型内容：官制、物价、地图与地理、交通、军制、服饰礼仪、历史人物研究等。

最关键语义：

> **Reference says X != 当前 Game 中 X 仍然成立。**

Reference retrieval 只负责提供“可能相关的背景材料”，不能裁定当前世界事实、人物身份、玩家知情或未来必然事件。历史基线与已经演化的 Game-local Reality 必须严格分层；Restore/branch 也不得检索未来分支材料。

后续重点放在 G7/G8：Reference binding、player/GM disclosure、检索 currentness、Source vs Game frozen semantics，以及何时真正需要向量/RAG。

### P-07｜Creator Preview Sandbox

**Owner verdict：通过（原提案 12）。**

未来 G8 Creator 中，为 World / Character / Expansion Draft 提供发布前的临时预览/试运行能力，例如“测试开场”“测试人物反应”“测试某个机制描述实际会产生什么体验”。

核心边界：

> **Preview Sandbox 永远不是正式 Game。**

它不写正式 Game、不进入 Timeline、不创建正式 People/Knowledge、不修改已发布 Source、不把 Preview Narrative 认定为发生过的历史。满意后仍需用户显式 Save / Publish Source。

优先复用正式 Provider/validation contract，但要给 Preview 使用 task-owned / sandbox state，避免污染 Owner 真实数据。

### P-08｜叙事模型 / 后台辅助模型分离配置

**Owner verdict：通过（原提案 13）。**

未来允许将高价值 GM Narrative 与后台辅助任务使用的模型配置分开，以改善等待时间、额度与成本；但默认仍使用同一模型，普通玩家不必理解多模型调度。

候选形态：

```text
叙事模型：Kimi K3
后台辅助模型：与叙事模型相同 ▼
```

高级模式才允许显式选择另一套已验证配置。

硬边界：

- 默认同模型；
- 不静默 fallback；
- 不是所有后台 lane 都能降级到便宜模型；
- World semantic / identity / curation 等是否允许分离必须由真实质量验证决定；
- 每次任务实际使用的 Provider/model 应能被 P-01 诊断观测。

进入条件应包括真实延迟/成本/额度问题或明确性能收益，不为“架构漂亮”提前建设复杂 router。

### P-09｜玩家收藏关键剧情节点 / Bookmark

**Owner verdict：通过（原提案 14）。**

允许玩家在任意重要 Narrative 节点手动“收藏这一刻”，以后从 Save/Timeline/Chronicle 等入口快速跳回阅读。

语义必须区分：

```text
Important Experience = 模型判断“塑造了我什么”
Save / Recovery       = 可恢复世界状态
Bookmark              = 玩家判断“这一刻我以后想再看”
```

Bookmark 是 Player-owned annotation，不提高该段历史在模型中的事实权重、不自动进入长期记忆，也不等于创建 Save。

未来可与冒险纪事导出联动，把收藏节点作为章节候选；是否支持“从该收藏创建分支/存档”需另行冻结完整 Timeline semantics。

---

## 2. 当前讨论状态

当前已通过 9 项：

```text
P-01 生成状态 / 诊断                       ← 原提案 1
P-02 叙事偏好                             ← 原提案 3
P-03 结构化模型输出可靠性                 ← 原提案 6
P-04 Source Library 作品化                 ← 原提案 7
P-05 玩家可读冒险纪事导出                 ← 原提案 9
P-06 Reference Library                    ← 原提案 11
P-07 Creator Preview Sandbox              ← 原提案 12
P-08 叙事模型 / 后台辅助模型分离配置      ← 原提案 13
P-09 玩家收藏关键剧情节点                 ← 原提案 14
```

当前未进入通过清单（不等于永久否决）：

- 回局回顾 / 长局回忆（原提案 2）；
- Timeline 分支产品化（原提案 4）；
- 常用行动收藏（原提案 5）；
- SillyTavern Character / Lorebook 导入转换器（原提案 8）；
- 按需 TTS（原提案 10）；
- 候选式 Regenerate / Swipe（原提案 15）。

除非 Owner 后续明确批准，上述内容不进入最终路线重构输入。

---

## 3. 本轮讨论结束后的统一处理

等 Owner 明确表示本轮改进讨论完成后：

```text
全部通过提案
↓
GPT 做重叠 / 依赖 / 冲突审计
↓
与当前 G6–G9 Roadmap 对照
↓
决定哪些是新 Work Item、哪些并入已有阶段、哪些只保留长期候选
↓
重新排列 Reality Gate / Owner UAT
↓
形成 Revised Task Axis
↓
Owner 审核批准
↓
更新 CURRENT Product / Architecture / Roadmap / Status
↓
才允许派发实现
```

在此之前，本文件只承担“已通过改进意向不丢失”的作用。
