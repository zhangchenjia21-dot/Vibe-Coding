---
title: my world｜SillyTavern 参考研究改进讨论通过清单
status: working-approved-candidate-list
version: 1.1
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
- 讨论结束后，由 GPT 基于全部通过提案统一做依赖、顺序、范围与 Stage Gate 重构，再提交 Owner 审核；
- 只有经过路线重构与 Owner 批准后，才进入正式 Product/Architecture Freeze 和 Task Packet。

未明确通过的提案不进入本清单；它们既不视为已批准，也不自动视为永久否决。

---

## 1. 已通过提案

### P-01｜玩家安全的生成状态 / 诊断

**Owner verdict：通过。**

目标不是复制 SillyTavern 的完整 Prompt Itemization，而是让 `my-world` 对关键 AI 后台链路具备有限、玩家安全、按需展开的可观察性。

拟解决的问题：

> 当人物卡、推荐行动、信息整理或其它 AI 能力“没有发生”时，玩家/Owner 应能区分它到底是尚未完成、模型没有产生有效结果、结构校验失败、currentness 使结果失效，还是保存/展示链路有问题，而不是只能猜测。

候选产品形态：

```text
本回合处理状态

世界理解            ✓ 已完成
角色 / 人物整理     ✓ 已完成 / 无更新 / 失败
推荐行动            ✓ 已生成 / 输出格式无效 / 超时
其它后台阶段        按真实 owner 投影
```

普通层只显示安全、可理解的阶段状态；高级诊断可以在需要时提供：

- 实际使用的 Provider / model；
- 请求是否发出；
- success / malformed / timeout / cancelled 等结构状态；
- 当前 accepted Turn / currentness 是否匹配；
- 结构验证为何拒绝结果。

硬边界：

- 不向普通玩家暴露 NPC 私密资料、未公开角色、幕后计划、GM-private context、credential；
- 不把模型输入明细伪装成模型真实“思维过程”；
- 不为诊断方便破坏 `World Truth != actor Knowledge != human-player disclosure`；
- 诊断是 observability，不是第二套世界事实源；
- 不允许 UI 自己拿 omniscient state 后本地过滤。

后续路线重构时需决定：

- 作为近期 G6 产品能力还是 G7 observability/context 能力；
- 普通玩家状态与 Owner/developer 高级诊断是否分层；
- 哪些现有 lane 需要稳定的只读 terminal/status seam，避免另造通用 event bus。

### P-02｜叙事偏好 / Narrative Preference

**Owner verdict：通过。**

目标是吸收 Author's Note / Prompt Manager 背后的真实用户诉求，但不复制 Prompt 工程师式配置面板。

拟解决的问题：

> 玩家可能认可同一个世界、人物和事实发展，但希望 GM 在镜头、节奏、细节密度和关注重点上更符合自己的阅读/游玩偏好。

候选产品形态可以包含少量高层偏好与一个自然语言补充，例如：

```text
叙事节奏
紧凑 / 均衡 / 细致

镜头倾向
事件推进 / 人物互动 / 环境探索 / 均衡

补充偏好
“重要人物对话可以写细一些，普通赶路快速略过。”
```

核心产品边界：

> **Narrative Preference 只影响表达与镜头，不成为 World Truth、NPC 意图、结果保证或剧情状态机。**

允许：

- 多写人物互动；
- 放慢重要场景；
- 普通过渡更紧凑；
- 更多环境细节；
- 少解释、多动作与对话呈现。

不允许把它解释为：

- 所有战斗都必须赢；
- 某 NPC 必须爱上玩家；
- 强制某个未来剧情；
- 固定每回合必须达到某字数；
- 把风格示例当作已经发生的历史。

后续路线重构时需决定：

- 长期偏好与单回合临时叙事要求如何分开；
- Preference 由 Game-local settings、Player profile 还是其它 owner 持有；
- 如何进入 GM request 而不覆盖 Source/World truth；
- 是否先用真实 Owner UAT 提炼最小维度，再冻结设置项。

### P-03｜结构化模型输出可靠性

**Owner verdict：通过（原讨论提案 6）。**

目标是在本来就要求机器结构输出的后台 AI lane 中，优先利用 Provider 原生 Structured Output / JSON Schema 等能力，提高“模型语义基本正确但因为包装格式错误而整项失效”的成功率。

典型问题来自 MW-019 真实验证：模型给出了正确的五条行动和正确 JSON 内容，但在外层添加 Markdown code fence，严格契约因此按设计拒绝整个结果。

候选方向：

```text
具有明确机器契约的 AI 调用
→ Provider 原生结构化输出（若该 Provider / model 明确支持）
→ 仍做 Program structural validation
→ 不支持时继续严格 JSON + fail-soft
```

适用候选包括：

- Action Recommender；
- Information Curator；
- World semantic lane；
- 其它未来本来就有明确 machine schema 的后台调用。

硬边界：

- 不用 Regex / fence stripping / 字段猜测构建语义修补森林；
- 不自动补造缺失人物、行动或事实；
- 不因格式失败无限重试；
- 不静默切换 Provider；
- Structured Output 只改善传输/结构可靠性，不改变“模型决定开放语义、程序验证机器边界”的责任划分；
- 若 Provider 不支持或能力不可靠，则保留现有 fail-soft 行为。

后续路线重构时需决定：

- 是否作为跨 AI lane 的可靠性基础任务；
- Provider capability detection / compatibility owner 放在哪里；
- 如何先在一个真实 lane 验证，再推广，避免提前建立通用平台。

### P-04｜Source Library 作品化 / 内容发现体验

**Owner verdict：通过（原讨论提案 7）。**

目标是把当前“选择可加载资产/数据包”的建局体验逐步升级为“选择我想玩的作品和角色”。

候选玩家体验：

```text
三国乱世
189 年末 · 写实历史倾向
以东汉末年的社会与政治结构为基线，允许历史自由演化。
标签：历史 / 政治 / 战争 / 长期经营

张琛
现代穿越者 · 24 岁
退伍军人、历史爱好者，缺乏本地身份与社会关系。

公共 d20 行动判定
为有明显成败不确定性的行动提供公开骰点；普通叙事行动仍保持自由。
```

未来可逐步加入：

- 封面 / 缩略图；
- 公开简介；
- 标签；
- 作者；
- 版本说明；
- Expansion 功能说明；
- player-safe Compatibility / dependency 提示；
- 建局前预览。

硬边界：

- 展示公开产品介绍，不直接暴露 Source 中的 GM-private material；
- 介绍/封面/标签是 discovery metadata，不是 Runtime World Truth；
- 已创建 Game 继续绑定 exact frozen Source generation，后续资料库展示信息更新不能改写旧 Game；
- 不为了作品化提前建设在线商店、账号、云服务或万能 Package Manager。

后续路线重构时需决定：

- 哪些 discovery metadata 应进入 Source contract，哪些只是 Library-local presentation metadata；
- 与 G8 Creator、Bundle/Distribution 的依赖关系；
- 是否先以当前真实 World/Character/Expansion 做第一版 preview consumer。

### P-05｜玩家可读的冒险纪事导出

**Owner verdict：通过（原讨论提案 9）。**

目标是让一条长期 AI RPG 时间线最终不仅“玩过”，还能够留下属于这局世界与玩家选择的可阅读作品记录。

第一版候选结果不是重新生成小说，而是优先从已接受、玩家可见的正式材料确定性整理：

```text
《乱世行纪》

第一章 / 时间段
→ 玩家行动
→ 已接受 GM Narrative

重要人物
→ 当前/阶段性玩家已知信息（按最终产品范围决定）

重要经历
→ 已有 Important Experiences

完整冒险记录
→ 可选附录
```

候选输出格式可优先考虑 Markdown / HTML；之后是否提供 AI 文学化整理稿另行讨论。

必须明确区分：

```text
可读冒险纪事
!=
可恢复 Game Backup / Migration Package
```

硬边界：

- 默认只包含玩家可见/已接受信息，不导出 GM-private truth、NPC private Knowledge、幕后 Agency/Evolution、credential；
- 派生整理稿不反向成为游戏事实；
- 若未来有 AI 文学化版本，必须标识为 derived edition，可回到原始 accepted history；
- 导出失败不能损坏当前 Game；
- 不用“导出了一份聊天文本”冒充完整可恢复游戏包。

后续路线重构时需决定：

- G6 作为轻量阅读/分享能力，还是 G9 与发布/迁移一起做；
- chapter / section 是否只按确定性结构组织，避免过早引入模型重新编史；
- People / Experiences 在历史时点还是导出时 current snapshot 的语义，需要先冻结。

---

## 2. 当前讨论状态

已通过：

```text
P-01 玩家安全的生成状态 / 诊断
P-02 叙事偏好 / Narrative Preference
P-03 结构化模型输出可靠性                  ← 原提案 6
P-04 Source Library 作品化 / 内容发现体验    ← 原提案 7
P-05 玩家可读的冒险纪事导出                ← 原提案 9
```

当前未进入通过清单（不等于永久否决）：

- 回局回顾 / 长局回忆；
- Timeline 分支产品化；
- 常用行动收藏；
- SillyTavern Character / Lorebook 导入转换器（原提案 8）；
- 按需 TTS（原提案 10）。

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
决定哪些是新 Work Item、哪些应合并到已有阶段、哪些只保留长期候选
↓
重新排列 Reality Gate / Owner UAT
↓
提交 Revised Task Axis 给 Owner
↓
Owner 批准后再写 CURRENT Roadmap / Architecture / Status
↓
才允许派发实现
```

在此之前，本文件只承担“已通过改进意向不丢失”的作用。
