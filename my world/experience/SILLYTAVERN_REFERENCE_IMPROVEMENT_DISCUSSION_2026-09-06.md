---
title: my world｜SillyTavern 参考研究改进讨论通过清单
status: working-approved-candidate-list
version: 1.4
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
- 本文件不是 CURRENT Roadmap、Task DAG 或 Architecture Decision；
- 本轮改进讨论完成前，不据此创建新的 MW Work Item；
- 讨论结束后，由 GPT 基于全部通过提案统一做重叠、依赖、冲突、阶段与 UAT 审计，再提交 Revised Task Axis 给 Owner；
- 只有路线重构经 Owner 批准后，才更新正式 Product / Architecture / Roadmap / Status 并进入 Task Packet。

---

## 1. 已通过提案

### P-01｜玩家安全的生成状态 / 诊断｜原提案 1

让关键 AI 后台链路具备有限、玩家安全、按需展开的可观察性，区分 processing / success / no-update / malformed / timeout / cancelled / stale-currentness / presentation failure 等真实状态。

普通玩家只看安全、可理解的阶段状态；Owner/developer 高级诊断可进一步查看实际 Provider/model、请求是否发出、结构终态和 accepted-prefix/currentness。不得泄露 NPC-private、未公开人物、GM-private context、credential；诊断不是模型思维解释，也不是第二事实源。

### P-02｜叙事偏好 / Narrative Preference｜原提案 3

允许通过少量高层偏好与自然语言补充调节节奏、镜头、细节密度和关注重点，而不是复制 Prompt 工程面板。

> **Narrative Preference 只影响表达与镜头，不成为 World Truth、NPC 意图、结果保证或剧情状态机。**

后续需冻结长期偏好与单回合临时叙事要求的 owner / 注入边界。

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

例如玩家把 People 卡中的“我信任李亭”改成“我只是暂时合作”，表达的是对玩家侧派生表述的纠正，不会改写李亭真实态度；删除一条 Important Experience 也不意味着 accepted history 中该事件没有发生。

后续需冻结 correction 的 durable owner、对 Curator 的优先级、Restore/Regenerate currentness，以及它与普通 Player Note 的边界。Program 不得把纠正实现成 personality/relationship keyword machine。

### P-11｜玩家私人笔记 / Player Notes｜原提案 17

提供纯 Player-owned 笔记本，用于记录怀疑、计划、线索和个人想法。

Player Notes 不是 World Truth、NPC Knowledge、Character Sheet、Important Experience、Quest 或 Curator 自动产物。第一版默认**不自动喂给 GM**，避免“玩家猜测”被模型误当成世界事实；以后若需要，可另行讨论由玩家显式标记“允许 GM 参考”。

未来可选择在冒险纪事导出中附带私人笔记，但默认隐私与导出范围需清楚。

### P-12｜作品套装 / 推荐 Composition｜原提案 20

允许作者把多个 Source 组织成一个可理解的作品推荐组合，例如：

```text
《张琛：汉末求生》
World            三国乱世
Entry            189 年冬 · 洛阳以东
Player Character 张琛
Recommended      公共 d20 / 汉末经济系统
Optional NPC     …
```

玩家可以“一键按推荐组合开始”，也可以进入高级设置调整可选内容。

作品套装只是 creation-time Composition Preset，不是新的 Runtime Truth；Final Create 后 Game 仍冻结各组件 exact generation。不扩张成在线商店、远程代码包或通用依赖管理器。

路线重构时优先判断是否与 P-04 Source Library 作品化合并成同一产品阶段，而不是机械拆成独立 Work Item。

### P-13｜角色性格 ↔ 玩家行动 ↔ 推荐行动动态反馈闭环｜原提案 21

**Owner verdict：通过；该提案由 Owner 主动提出。**

未来推荐行动应参考玩家主角**当前、player-safe 的 Character / 性格投影**；玩家最终真正提交并进入 accepted history 的自由行动，又由现有 Model-driven Character Curation 判断是否构成持久人格/自我方向变化，从而反过来影响未来推荐。

建议闭环：

```text
当前 Character / 性格
↓
Action Recommender 把它作为行动倾向参考
↓
生成更符合“此刻这个人”的 5 个建议，但不限制自由输入
↓
玩家点击后编辑 / 或完全自由输入
↓
只有最终提交并 accepted 的玩家行动成为性格演化证据
↓
Character Curator 判断长期行为是否真的改变“现在的我是谁”
↓
Character 当前性格自然演化
↓
未来推荐读取新的 current Character projection
```

关键边界：

- 性格是 recommendation tendency，不是 legal-action whitelist；
- 推荐本身不改变性格，点击未发送也不产生 Character effect；
- 编辑后的最终提交文本才是玩家选择；
- 单次反常行动不机械触发人格突变；
- Program 不做“勇敢 +1 / 谨慎 -1”、关键词人格分类或固定人格数值条；
- 推荐必须允许合理偏离、尝试和成长，避免旧性格 → 同类推荐 → 同类选择的自我锁定；
- Recommender 只能消费 player-safe current Character projection，不读取 GM-private / omniscient World material；
- Restore / Regenerate 后 Character currentness 与 recommendation currentness 必须一起回到对应历史点；
- P-10 玩家纠正 Character 派生信息后，推荐应遵守纠正后的 current player-safe Character projection。

该能力不是 MW-019 自动 Revision 范围；路线重构时需判断它是 Character Curation × Recommendation 的新 outcome，还是后续 G6 vertical。

### P-14｜对话式 Creator / AI-assisted Source Authoring｜原提案 23

未来 Creator 允许用户通过自然语言与 AI 讨论 World / Entry / Character / Expansion Draft，并把每次 AI 建议收敛成**可见、可验证、可拒绝的 Draft ChangeSet**。

候选流程：

```text
用户与 Creator AI 讨论
↓
AI 提议 typed Draft changes
↓
界面明确展示“本次拟修改什么 / 没修改什么”
↓
Validator 检查
↓
用户接受 / 拒绝 / 继续讨论
↓
只修改 Draft
↓
显式 Save / Publish 后才进入正式 Source
```

核心边界：Conversation 不直接成为 Source Truth；AI 不能静默发布、覆盖已发布 Source 或修改正式 Game。Creator AI 的作用是帮助形成 Draft，不绕过 Source contract、validation、版本与 publish gate。

该提案与 P-07 Creator Preview Sandbox 天然组成“对话创作 → 查看 ChangeSet → Preview → 继续改 → Publish”的 G8 Creator 主循环，路线重构时优先整体设计而非重复建设。

### P-15｜事务 / 线索 / Open Threads Surface｜原提案 24

增加一个模型维护的 player-safe `事务` Surface，回答：

> **“我现在还有哪些正在处理、尚未解决、值得持续记住的事情？”**

可以包括：

- 当前问题；
- 尚未核实的线索；
- 已作出的承诺；
- 玩家明确表达的计划；
- 当前未解决的风险或 open thread。

它不是传统预写 RPG 的 Quest List，不使用“主线/支线/完成 3/5”来反向驱动世界，也不要求所有事情都变成任务。

关键边界：

- 是否形成/更新/关闭事务由模型根据 accepted player-visible history 判断；
- Program 不做“调查/寻找/承诺”等关键词任务识别；
- NPC-private / Agency / hidden Evolution 不能提前泄露到事务；
- `事务` 是玩家当前关注事项的派生整理，不是 GM 剧情脚本或世界目标队列；
- Restore / Regenerate 后必须跟随 accepted-history currentness；
- 玩家自由放弃或改变计划时，模型可以更新/移除，不把旧任务永久钉死。

该提案会填充既有 Mother Taxonomy 中的 `事务`，并提供一个真实的新 Information Curator consumer；路线重构时应评估它与 Character / Experiences / People 的统一 Curator 扩展方式，而不是新增独立语义规则系统。

---

## 2. 当前讨论状态

当前已通过 15 项：

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
P-10 玩家纠正 AI 派生信息                 ← 原提案 16
P-11 玩家私人笔记                         ← 原提案 17
P-12 作品套装 / 推荐 Composition           ← 原提案 20
P-13 性格 × 玩家行动 × 推荐动态闭环        ← 原提案 21
P-14 对话式 Creator                       ← 原提案 23
P-15 事务 / 线索 / Open Threads Surface    ← 原提案 24
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
- Player-known Map（原提案 25）。

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