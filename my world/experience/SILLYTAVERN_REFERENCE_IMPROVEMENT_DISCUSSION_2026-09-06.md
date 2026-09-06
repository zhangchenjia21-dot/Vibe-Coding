---
title: my world｜SillyTavern 参考研究改进讨论通过清单
status: working-approved-candidate-list
version: 1.6
created: 2026-09-06
updated: 2026-09-06
source_reference: ./SILLYTAVERN_UPSTREAM_FUNCTIONAL_REFERENCE_AUDIT_2026-09-06.md
implementation_authorization: none
roadmap_authorization: none
---

# my world｜SillyTavern 参考研究改进讨论通过清单

## 0. 用途

本文件记录 Owner 在基于原版 SillyTavern 功能参考研究进行的产品改进讨论中，**明确通过的提案**。

重要边界：

- `通过提案` != `立即实施`；
- 本文件不是 CURRENT Roadmap、Task DAG 或 Architecture Decision；
- 本轮讨论结束前，不据此创建新的 MW Work Item；
- 讨论结束后，由 GPT 对全部通过项做重叠、依赖、冲突、阶段和 UAT 审计，形成 Revised Task Axis；
- 只有 Revised Task Axis 经 Owner 批准后，才更新正式 Product / Architecture / Roadmap / Status 并进入 Task Packet。

---

## 1. 已通过提案

### P-01｜玩家安全的生成状态 / 诊断｜原提案 1

让关键 AI 后台链路具备有限、玩家安全、按需展开的可观察性，区分 processing / success / no-update / malformed / timeout / cancelled / stale-currentness / presentation failure 等真实状态。

普通玩家只看安全、可理解的阶段状态；Owner/developer 高级诊断可进一步查看实际 Provider/model、请求是否发出、结构终态和 accepted-prefix/currentness。不得泄露 NPC-private、未公开人物、GM-private context、credential；诊断不是模型思维解释，也不是第二事实源。

### P-02｜叙事偏好 / Narrative Preference｜原提案 3

允许通过少量高层偏好与自然语言补充调节节奏、镜头、细节密度和关注重点，而不是复制 Prompt 工程面板。

> **Narrative Preference 只影响表达与镜头，不成为 World Truth、NPC 意图、结果保证或剧情状态机。**

后续需冻结长期偏好与单回合临时叙事要求的 ownership / 注入边界。

### P-03｜结构化模型输出可靠性｜原提案 6

对本来就要求 machine schema 的后台 AI lane，优先使用 Provider 明确支持的 Structured Output / JSON Schema 等能力，Program 仍做严格结构验证。

不得靠 Regex、fence stripping、字段猜测、补造缺失内容、无限重试或静默 Provider fallback 建立修补森林。先选一个真实 lane 验证，再决定是否推广。

### P-04｜Source Library 作品化 / 内容发现体验｜原提案 7

把“选择机器资产/数据包”逐步升级成“选择我要玩的世界、角色和玩法内容”。候选包括封面、公开简介、标签、作者、版本说明、Expansion 功能说明、player-safe compatibility 与建局前预览。

Discovery metadata != Runtime truth；既有 Game 继续绑定 exact frozen generation；不因此提前建设在线商店、账号、云或万能包管理器。

### P-05｜玩家可读的冒险纪事导出｜原提案 9

让长期时间线可导出为属于该局世界与玩家选择的阅读作品。第一版优先确定性整理 accepted Player action + GM Narrative、Important Experiences 和允许范围内的 People 等玩家可见材料，候选格式 Markdown / HTML。

> **可读冒险纪事 != 可恢复 Game Backup / Migration Package。**

不导出 GM-private truth、NPC-private Knowledge、Agency/Evolution、credential；未来 AI 文学化版本只能是可追溯的 derived edition。

### P-06｜Reference Library / 大型世界参考资料层｜原提案 11

为制度、地理、文化、技术、历史研究等大型材料建立可复用、按需取用的 Reference Library。

> **Reference says X != 当前 Game 中 X 仍然成立。**

检索只提供候选背景材料，不裁定当前世界事实、人物身份、玩家知情或未来事件；必须遵守 Game-local 演化、Timeline/Restore currentness 和 Source/Game 分层。

### P-07｜Creator Preview Sandbox｜原提案 12

未来 G8 Creator 中允许测试 World / Character / Expansion Draft 的开场、人物反应或机制表现。

> **Preview Sandbox 永远不是正式 Game。**

Preview 不写正式 Game/Timeline/People/Knowledge，不修改已发布 Source，不成为历史；满意后仍需显式 Save / Publish。

### P-08｜叙事模型 / 后台辅助模型分离配置｜原提案 13

未来允许 GM Narrative 与后台辅助任务使用不同的显式模型配置，以改善延迟、成本和额度；默认仍使用同一个模型。

不静默 fallback；不是所有 semantic/identity/curation lane 都允许使用更便宜模型，必须由真实质量验证决定；实际 Provider/model 应可被 P-01 诊断观测。

### P-09｜玩家收藏关键剧情节点 / Bookmark｜原提案 14

允许玩家手动收藏某个 Narrative 节点，未来快速跳回阅读。

```text
Important Experience = 模型判断“塑造了我什么”
Save / Recovery       = 可恢复世界状态
Bookmark              = 玩家判断“这一刻我以后想再看”
```

Bookmark 是 Player-owned annotation，不提高 AI 事实权重、不自动进入长期记忆，也不等于 Save。未来可作为 Chronicle 章节候选。

### P-10｜玩家纠正 AI 派生信息｜原提案 16

Character / Important Experiences / People 等模型派生信息允许玩家低摩擦纠正、删除或重述，使系统不必试图用规则保证模型永远不犯错。

> **纠正派生信息 != 修改世界事实。**

纠正的是玩家侧派生表述，不直接修改 NPC 真实态度或 accepted history。后续需冻结 correction durable owner、Curator 优先级和 Timeline currentness。

### P-11｜玩家私人笔记 / Player Notes｜原提案 17

提供纯 Player-owned 笔记本，用于记录怀疑、计划、线索和个人想法。

Player Notes 不是 World Truth、NPC Knowledge、Character、Important Experience、Quest 或 Curator 自动产物。第一版默认**不自动喂给 GM**，避免玩家猜测被模型误当成事实；以后若需要，再讨论由玩家显式允许 GM 参考。

### P-12｜作品套装 / 推荐 Composition｜原提案 20

允许作者把多个 Source 组织成一个可理解的作品推荐组合，并让玩家“一键按推荐组合开始”或进入高级设置调整。

作品套装只是 creation-time Composition Preset，不是新的 Runtime Truth；Final Create 后 Game 仍冻结各组件 exact generation。不扩张成在线商店、远程代码包或通用依赖管理器。

路线重构时优先判断是否与 P-04 Source Library 作品化合并为同一产品阶段。

### P-13｜角色性格 ↔ 玩家行动 ↔ 推荐行动动态反馈闭环｜原提案 21

**该提案由 Owner 主动提出。**

未来推荐行动参考主角当前 player-safe Character / 性格投影；玩家最终真正提交并进入 accepted history 的自由行动，又由 Model-driven Character Curation 判断是否构成持久人格/自我方向变化，从而反过来影响未来推荐。

```text
当前 Character / 性格
↓
Action Recommender 作为“行动倾向”参考
↓
生成更符合此刻这个人的建议，但不限制自由输入
↓
只有最终提交并 accepted 的玩家行动成为性格演化证据
↓
Character Curator 判断是否发生持久变化
↓
未来推荐读取新的 current Character projection
```

性格只是 tendency，不是 legal-action whitelist；推荐本身、点击未发送、原始推荐草稿均不得改变 Character。Program 不做人格数值条、关键词人格分类或“勇敢 +1”之类规则。推荐应允许合理偏离、尝试和成长，避免自我锁定。

### P-14｜对话式 Creator / AI-assisted Source Authoring｜原提案 23

未来 Creator 允许用户通过自然语言与 AI 讨论 World / Entry / Character / Expansion Draft，并把每次 AI 建议收敛成**可见、可验证、可拒绝的 Draft ChangeSet**。

```text
Creator Conversation
→ typed Draft changes
→ 明确展示本次拟修改 / 未修改项
→ Validator
→ 用户接受 / 拒绝 / 继续讨论
→ 只修改 Draft
→ 显式 Save / Publish 才进入正式 Source
```

Conversation 不直接成为 Source Truth；AI 不能静默发布、覆盖 Source 或修改正式 Game。与 P-07 Preview Sandbox 共同组成未来 Creator 主循环。

### P-15｜事务 / 线索 / Open Threads Surface｜原提案 24

增加模型维护的 player-safe `事务` Surface，回答：

> **“我现在还有哪些正在处理、尚未解决、值得持续记住的事情？”**

可包括当前问题、未核实线索、已作承诺、玩家计划与未解决风险。它不是传统 Quest List，也不是 GM 剧情脚本。

是否形成/更新/关闭事务由模型基于 accepted player-visible history 判断；Program 不做关键词任务识别；NPC-private / Agency / hidden Evolution 不得提前泄露；Restore/Regenerate 后必须跟随 accepted-history currentness。

### P-16｜长期上下文编排器 / Context Orchestrator｜原提案 26

未来 G7 建立正式 Context Orchestrator，解决长局中“这一回合 GM 真正应该看到什么、不能看到什么、哪些旧信息已经失效”的问题，而不是简单把所有记忆塞进 Prompt。

候选输入层包括：

```text
必须保留
→ 当前玩家行动 / 最近完整 accepted 对话 / 必要 Game state

长期当前信息
→ current Character / 相关 People / 事务 / 机制状态

按需找回
→ 相关旧事件 / Reference material / 人物历史

GM-private
→ 当前真正需要的 World / Knowledge / Agency material

↓ 权限 + currentness + budget
最终 GM Context
```

核心边界：`相关 != 当前有效 != 当前有权使用`。被 Restore 掉的未来、过期 Game 状态、仅作为历史基线的 Reference 均不能因为相似度高就重新成为当前事实。第一版优先冻结确定性优先级与权限/currentness，再通过真实长局验证是否需要更复杂 token 策略。

### P-17｜OOC / 给 GM 的场外说明｜原提案 27

提供与“角色行动”明确分离的 GM Guidance 通道，让玩家表达临时的创作与游玩意愿，例如“接下来普通赶路略过”“重要人物对话放慢”“不要替我的角色决定内心想法”。

它与 P-02 的区别：Narrative Preference 是长期偏好，OOC Guidance 是本次/近期临时指导。

OOC 不是角色行动、不是 World mutation API，也不能绕过 mechanics / d20 或保证结果。诸如“希望剧情有机会遇见某人”只能作为创作意愿参考，不能自动宣告该人已经出现在场景中。

### P-18｜AI 使用情况 / 性能与调用可视化｜原提案 28

在 P-01 单次诊断之外，提供 Session / Game 级的 AI 使用概览，用于回答“这一局跑了多少调用、哪个 lane 最慢、哪个模型在消耗时间/额度”。

候选指标：Narrative / semantic / curation / recommendations 等 lane 的请求次数、成功/失败、延迟；若 Provider 返回可靠 token usage，可展示 input/output tokens。

不得虚构 token 或价格；只有存在可靠 usage 与明确价格依据时，才可提供明确标为“估算”的成本。该能力应为 P-08 多模型分工提供实际数据，而不是为了监控而监控。

### P-19｜知识来源 / Provenance 可追溯｜原提案 30

对 People / Knowledge / 事务等重要长期玩家信息保留可追溯来源，使玩家在需要时能回答：

> **“我为什么会知道这件事？”**

候选交互：

```text
李亭可能认识负责通行凭证的人
来源：第 27 回合，你从李亭的谈话中了解到
[查看原文]
```

默认界面保持干净，仅按需展开来源。来源可帮助核对 AI 整理、支持 P-10 correction，并为“传闻 / 尚未确认 / 后来被纠正”的认识提供 epistemic basis。

Provenance 不能把 GM-private truth 暴露给玩家，也不能因“后台知道来源”而提升玩家知情。必须跟随 accepted-history / Restore / Regenerate currentness。

### P-20｜玩家认识状态 / Epistemic Status｜原提案 31

**Owner verdict：通过。**

在 P-19 Provenance 基础上，让长期玩家知识不仅记录“知道什么、从哪里知道”，还表达玩家当前如何看待这条信息，例如：

```text
听闻 / 尚未确认
个人怀疑
已有较强证据
已确认
存在矛盾
此前认识已被推翻
```

典型体验：同一句“曹操已经抵达许都”，从旅商转述得到时可显示为“听闻，尚未确认”；后来亲眼或从可靠证据证实后，更新为“此前传闻已被证实”。

核心边界：

- 不使用 83% 之类伪精确置信度；
- epistemic status 是 Player Knowledge 的状态，不等于 World Truth；
- 状态由模型基于 accepted player-visible evidence 判断，Program 不用关键词/来源类型硬编码可信度；
- 可以存在玩家误信、合理怀疑和互相矛盾的来源；
- Restore / Regenerate 后跟随当时玩家真正拥有的证据与认识；
- 与 P-19 Provenance 一起设计时，优先形成“内容 + 当前认识状态 + 来源依据”的统一 player-safe knowledge presentation。

### P-21｜人物共同经历 / Shared History｜原提案 32

**Owner verdict：通过。**

保持 People 主卡“当前最新认识快照”的冻结语义，同时为长期人物关系增加次级入口：**“与此人的经历”**。

```text
People 主卡
= 他现在在我眼里是谁

Shared History
= 我和这个人是怎样走到今天的
```

候选展示可链接：初次见面、重要合作、冲突、承诺、关系转折等真正发生过的 accepted history 节点。优先依赖 exact stable NPC identity、accepted history、Important Experiences / Provenance，而不是由模型脱离历史重新编一篇人物传记。

核心边界：

- Shared History 不取代 People current snapshot；
- 不包含 NPC-private / 玩家未获知事件；
- 不因为“与此人有关”就把每个普通回合全部塞入经历列表，仍需有界、可读；
- same-name 人物只能按 stable identity 区分，禁止 display-name 聚合；
- Restore 到认识该人物之前时，相关 Shared History 与 People 一起消失；Restore 到较早阶段时只能看到当时已经发生的共同历史；
- 未来若允许点击历史节点，应优先复用 Bookmark / Provenance 的“跳回原文”能力，而不是另造聊天副本。

---

## 2. 当前讨论状态

当前已通过 **21 项**：

```text
P-01  生成状态 / 诊断                         ← 原提案 1
P-02  叙事偏好                               ← 原提案 3
P-03  结构化模型输出可靠性                   ← 原提案 6
P-04  Source Library 作品化                   ← 原提案 7
P-05  玩家可读冒险纪事导出                   ← 原提案 9
P-06  Reference Library                      ← 原提案 11
P-07  Creator Preview Sandbox                ← 原提案 12
P-08  叙事模型 / 后台辅助模型分离配置        ← 原提案 13
P-09  玩家收藏关键剧情节点                   ← 原提案 14
P-10  玩家纠正 AI 派生信息                   ← 原提案 16
P-11  玩家私人笔记                           ← 原提案 17
P-12  作品套装 / 推荐 Composition             ← 原提案 20
P-13  性格 × 玩家行动 × 推荐动态闭环          ← 原提案 21
P-14  对话式 Creator                         ← 原提案 23
P-15  事务 / 线索 / Open Threads Surface      ← 原提案 24
P-16  长期上下文编排器                       ← 原提案 26
P-17  OOC / 给 GM 的场外说明                 ← 原提案 27
P-18  AI 使用情况 / 性能调用可视化           ← 原提案 28
P-19  知识来源 / Provenance 可追溯            ← 原提案 30
P-20  玩家认识状态 / Epistemic Status         ← 原提案 31
P-21  人物共同经历 / Shared History           ← 原提案 32
```

当前未进入通过清单（不等于永久否决）：

- 回局回顾 / 长局回忆（原提案 2）；
- Timeline 分支产品化（原提案 4）；
- 常用行动收藏（原提案 5）；
- SillyTavern Character / Lorebook 导入转换器（原提案 8）；
- 按需 TTS（原提案 10）；
- 候选式 Regenerate / Swipe（原提案 15）；
- 历史全文搜索（原提案 18）；
- 换一组推荐行动（原提案 19）；
- 完整可迁移 Game Package（原提案 22）；
- Player-known Map（原提案 25）；
- 多 Entry / 多开局（原提案 29）；
- Source 版本差异与影响预览（原提案 33）；
- 可分享 Source Package（原提案 34）；
- Player Note → Persistent GM Reminder（原提案 35）。

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
决定新 Work Item / 合并阶段 / 长期候选
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