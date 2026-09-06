---
title: my world｜原版 SillyTavern 功能参考审计与可选方向
status: non-canonical-reference
version: 1.0
created: 2026-09-06
updated: 2026-09-06
purpose: functional-reference-and-selective-adoption
upstream_repository: SillyTavern/SillyTavern
upstream_branch: release
upstream_commit: 8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8
implementation_baseline: b83865c6c4e7bdccf6c40341789e03502baa78a3
governance_baseline_before_write: 9b23507bbe4d45f55f42deab1534726f3d2540ef
implementation_authorization: none
---

# 原版 SillyTavern｜功能参考审计、可选方向与取舍

## 0. 结论与阅读边界

**学习成熟的 AI 交互体验，不继承其宿主代码，不把聊天前端的机制直接当成持续世界的规则。**

Owner 本轮要求：查看原版仓库代码，形成可参考经验、未来可选功能，以及应扬弃的功能机制；明确不学习其代码组织/架构。本文据此只使用源码来核实“功能做了什么、用户获得什么、有什么边界”。不提出复制 JavaScript、移植模块、沿用文件组织或复刻插件运行时的建议。

上游依据为 `release@8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8`，不是 Owner 的 SillyTavern fork。查阅了世界书、摘要、向量检索、快捷回复、聊天分支、群聊、角色资料、提示词管理/明细、正则、连接配置、TTS/表情、聊天备份等代表性源码，并交叉核对官方文档。代码入口与具体读取范围见第 8 节。

这是**功能链路抽查与产品分析**，不是全仓逐行审计、代码质量评级、安全认证或运行实测。未启动 SillyTavern，未做性能/生成质量基准；不把“屎山”传闻当成已证实结论。源码存在某能力也不等于其默认开启、用户一定启用或产品效果已经得到实测。

官方将 SillyTavern 定位为面向高级用户的 LLM 前端。它围绕角色卡、聊天、上下文与扩展形成高度可配置的体验；my-world 的正式目标则是持续 AI 世界、自由 GM 与原生 RPG。两者可共享体验经验，但不可直接等同。[S00][D01]

**本文不授权新任务，不改变 MW-019，不把 MW-018 标为 Product PASS。** 本轮读取的 current 状态仍是：MW-019 推荐行动实现；MW-018 人物卡 UAT 暂缓，等待组合 UAT。所有下文阶段建议都是候选适配，不是排期。

## 1. 三种取舍，不混为一谈

| 分类 | 含义 | 例子 |
|---|---|---|
| 值得继承的产品经验 | 用户收益与 my-world 方向一致，具体能力仍需按需实现 | 查看实际模型输入、可修改行动草稿、可追溯的回顾与导出 |
| 值得改造的机制 | 上游方案服务聊天前端，我们要保留收益、改变产品约束 | 摘要变成可追溯记忆；聊天分支变成世界和上下文一致的分支体验 |
| 不应照搬的功能取向 | 引入后会削弱当前产品原则，或复杂度收益不成立 | 用关键词决定世界存在、把群聊轮流说话当世界模拟、自动替玩家提交行动 |

低优先级不等于糟粕。TTS、角色立绘、主题、翻译、群聊本身都有合理用途。需要拒绝的是不适合本项目的用法、权限与默认行为，不是给所有高级功能贴负面标签。

## 2. 可继承的八条功能经验

### 2.1 模型跑偏时，用户需要看到“它实际收到了什么”

**上游事实：** `itemized-prompts.js` 按角色描述、人物性格、场景、Persona、World Info、摘要、Author's Note、向量材料等分类统计上下文，并保留模型/API/预设信息；明细模板提供原始 Prompt、复制与差异查看入口。官方也建议用实际 Prompt 排查生成行为。[S01][S02][D02]

**可学经验：** 可观察性本身就是 AI 产品功能。不能让用户面对失忆、跑偏、漏卡时，只能在“模型不行”和“是不是没接通”之间猜。

**适配方向：** 一个按需展开的“本次生成诊断”，先说明本轮用了多少近期对话、哪些记忆/参考材料，哪些后台阶段成功、失败或尚未完成。详细开发诊断再展示对应请求，而不是在正常游戏界面铺日志。

**硬边界：** Prompt 明细只能解释模型输入，不能声称解释模型真实内心过程。GM 私密材料不进入普通玩家页面；开发者/Owner 深度诊断应单独进入、标注可能剧透，并在导出前去掉凭据。不能因为调试方便破坏 People 与推荐行动的玩家可知边界。

**触发条件与验收：** UAT 反复出现“没更新/失忆但不知原因”时可提前；否则并入 G7。验收是一次失败能定位到有无材料、版本、调用结果，而不是只多一个 token 仪表。

### 2.2 持续记忆需要“近期原文 + 摘要 + 按需找回”，不是无限堆历史

**上游事实：** 内置 Summarize 能更新和编辑摘要，源码记录摘要关联消息，并在切换聊天、删除消息、编辑/重生成等事件中处理已存摘要。向量扩展分别提供聊天、文件、World Info 的检索与容量设置。官方明确提醒模型摘要可能遗漏或幻觉。[S03][S04][D03][D04]

**可学经验：** 长局不仅需要“AI 记住”，也需要玩家重新进入故事时迅速找回状态。摘要和检索应保留出处，允许知道其覆盖范围。

**适配方向：** G7 可组合近期原文、阶段摘要和有来源的旧事件检索；另有玩家可见的“上次发生了什么”回顾。玩家回顾只用玩家可见历史，GM 工作记忆仍服从其既有权限。两者不能混成一段通用摘要。

**硬边界：** 摘要是派生记忆，不替代世界事实、人物身份或已接受历史。Restore 后不得检索到被撤回分支；发现摘要错误应可回到来源核对，而不是用“摘要写了”证明事件发生。向量相似度只证明候选相关性，不证明人物身份、当前性或真实性。

**触发条件与验收：** 多日长局出现遗忘、回局困难或上下文预算压力后进入 G7；用跨章节人物重现、旧情报被纠正、Restore 到获知前等具体场景验证。

### 2.3 快捷输入的价值是降低起步阻力，而不是替玩家做决定

**上游事实：** Quick Reply 的 `executeWithOptions` 区分仅填入输入框、直接发送、运行斜杠命令；支持与当前草稿拼接等行为。它首先是预定义文本/脚本入口，不能据此声称原版已经实现“每轮恰好五个动态模型建议”。[S05][D05]

**可学经验：** 交互入口可以很轻：用户看到一种行动表达，点击后仍保留编辑权。

**适配方向：** 当前 MW-019 已采用“模型推荐 → 填入草稿 → 玩家编辑 → 正常发送”，无需因这次参考研究改动任务。未来真有重复输入需求，可讨论“我的常用行动”收藏，与每轮动态建议区分。

**硬边界：** 推荐文字不作为代码执行；不能因为以 `/` 开头就调用脚本；不能绕过正常行动判定。自动代写不等于自动提交。草稿替换的撤销便利性可在 UAT 后单独改进，但不在本轮追加需求。

### 2.4 回退与分支应该服务“尝试另一条路”，不是展示内部版本号

**上游事实：** `bookmarks.js` 从指定消息截取聊天快照，可选定目标消息的 swipe；用户可创建分支或命名检查点。该行为复制聊天前缀并记录源聊天关联。[S06][D06]

**可学经验：** 从一个有意义的叙事点保存、命名和尝试另一种选择，比让玩家理解内部节点更自然。

**适配方向：** 未来可做“在这次谈判前留一个分支”“回到离城前”，并能清楚识别当前分支。my-world 已有 Save/Restore，不应为学习上游重做一套。

**硬边界：** 聊天前缀不是完整游戏存档。新分支必须同时恢复世界、人物认知、机制状态与模型上下文；仅换屏幕上的对话会制造未来记忆泄漏。不能直接把原版 checkpoint 克隆方式当成我们的存档语义。

**触发条件与验收：** 现有 named Save/Recovery 不能满足真实试错需求时再进入 G6/G7。验收是玩家能试另一条路，且旧路线的知识、人物卡和结果不串过来。

### 2.5 叙事风格应成为可理解的产品选择，而非要求玩家会调 Prompt

**上游事实：** Author's Note 提供内容、频率、深度、位置与角色设置；角色资料包含开场和对话示例；Prompt Manager 允许组织与覆盖提示块。官方指出开场和示例有助于表达期望文风，但这不是跨模型质量保证。[S07][S08][S09][D07]

**可学经验：** 玩家确实可能想调整镜头、节奏、对话比例、描写密度；“换世界”与“换叙事风格”是两个问题。

**适配方向：** 少数清晰的叙事偏好，例如偏行动/偏人物、细节更多/节奏更快，或选择一个表达风格参考。保留正常自然语言表达，而非开放十几个注入框。

**硬边界：** 偏好不是固定字数要求，不承诺某个剧情结局，不修改 canonical World，也不把风格示例当已经发生的历史。临时节奏要求和长期偏好需可区分，避免旧要求永久驻留。

**触发条件与验收：** 当前 GM 基本体验稳定后，Owner 明确提出风格不匹配时进入 G6；用同一场景比较，而不是只验收设置能保存。

### 2.6 角色与作品应该便于复用，但资料包不是当前世界

**上游事实：** 原版角色数据包含描述、性格、场景、开场、对话示例、作者/版本/标签、备用开场、关联世界书及扩展字段；官方支持角色/Persona 管理。部分扩展字段还可携带正则或提示覆盖，因此“角色文件”不只有传记文字。[S08][S10][D07][D08]

**可学经验：** 可携带的内容单元、清楚的说明与试读入口，有利于反复开局和创作。

**适配方向：** 在已有 Source Library 上逐步增加作品封面、简介、标签、预览、作者/版本说明；G8 有需要时增加 SillyTavern 资料导入与转换预览，而非全功能兼容宿主。

**硬边界：** 上游 Character Card、玩家 Persona 与 my-world 局内 People Card 不是同一东西。导入只是来源材料，不能自动成为当前 NPC、玩家已知信息、既定未来或高优先级系统指令。保留来历、明确有损转换，不执行随卡脚本；已有 Game 的 exact generation 不被导入更新改写。

### 2.7 模型配置应整体保存，避免“改了模型，留下旧参数”

**上游事实：** Connection Profiles 组合 API、模型、服务地址、预设、模板、停止串等选择，便于切换配置；源码中存在明确的配置字段集合。[S11][D09]

**可学经验：** 用户需要的是一组可理解、可复现的配置，而非每次重新翻找多个面板。

**适配方向：** 本机模型配置预设、连接测试，以及本次生成使用哪个模型的清楚记录。若以后确有速度/语义质量需求，再讨论 GM 与后台整理分工，不默认要求多模型。

**硬边界：** 不隐式换 Provider；不同任务是否使用不同配置必须由明确规则决定。导出作品/游戏时不携带 API Key。后端兼容性验证不能被一个“预设名称”替代。

**触发条件与验收：** 用户开始频繁切换现有配置，或配置不一致引发真实失败时；切换后能说明实际生效的完整配置并安全回退。

### 2.8 音画可以提升沉浸，但应在文本之外按需增强

**上游事实：** 原版 TTS 支持单条消息朗读与人物音色映射；Expressions 根据消息分类或其它来源选择表情图，带有更新间隔；VN Mode 提供立绘式布局。[S12][S13][D10]

**可学经验：** 声音和立绘可以让同样的文本更易进入，但不能让生成音画变成继续游戏的前提。

**适配方向：** 优先候选是按需朗读已接受段落，其次是已有授权视觉资产下的背景/立绘。只有真实收益明确，才考虑自动情绪表情、动态插画或语音输入。

**硬边界：** 一句情绪分类不是 NPC 内心真相；表情不能提前暴露隐秘敌意。生成图中的装饰也不是世界事实。音画失败不能阻止阅读、保存或下一行动；不为每轮默认叠加多次辅助请求。

**触发条件与验收：** Owner 提供/批准真实素材或提出明确阅读/听读需求；G6 视觉重新进入仍须遵守 existing deferred trigger。

## 3. 有争议、应扬弃或限制的八种产品机制

### 3.1 不把世界书触发等同于世界存在或玩家获知

World Info 确实提供关键词、递归、预算、逻辑组合、sticky/cooldown 等控制，原版也有向量化补充，不能把它描述成只会简单匹配词。[S04][S14][D11]

可保留：有界的相关资料进入模型、作者能理解某条资料是否被引用。

不继承：提到一个名字就判定该 NPC 现身、某段情报被检索到就判定玩家知道、没触发就像角色不存在。即使触发机制对受控文本很方便，也不能承担身份、知识、关系和因果裁定。

**本项目取舍：模型判断开放语义；程序负责版本、身份、权限和资源边界。检索结果是材料，不是裁决。**

### 3.2 不把群聊的发言顺序当成独立 NPC 世界模拟

原版群聊有自然/列表/手动/池式顺序；自然顺序涉及名字提及、talkativeness 和随机选择。官方明确说明群聊历史在成员间共享，并提醒组合角色卡可能造成角色/性格混淆。[S15][D12]

可保留：多人场景的可读呈现，用户知道是谁在说话，必要时控制前台节奏。

不继承：每个 NPC 轮流说一句就算独立行动；所有 NPC 看同一历史就算拥有合理知识；没有人适合说话时仍必须随机选一个。

对于群聊工具，这些规则有实际用途；对于 my-world 的秘密、传闻、离屏行动，它们不能代替既有 Knowledge/Agency 边界。

### 3.3 不把正则修饰扩张成第二个语义裁判

原版 Regex 明确区分显示、Prompt、输入/输出等处理位置，还提供角色/预设脚本的许可判断，并非无条件执行所有导入正则。[S10]

可保留：纯显示整理、已声明结构的语法处理、可解释且可撤销的格式操作。

不继承：从散文关键词计算人物关系、删去“不合设定”的情节、抽取一句话就改变 HP/金钱，或让屏幕文字与世界事实靠隐藏替换长期分离。

这是一条**不建议采用的使用模式**，不是声称原版默认用正则管理 RPG 数值。禁止语义 heuristic 不等于禁止所有 Regex。

### 3.4 不要求普通玩家先成为 Prompt 工程师

Prompt Manager、Author's Note、角色覆盖、世界书位置、宏和扩展让高级用户有很大控制权。[S07][S09][D02][D13]

可保留：预设、作用域、诊断、专家入口。

不继承：为了开始玩，必须理解多层注入、优先级和相互覆盖；遇到跑偏只能继续堆规则。对本项目更合适的是简单游戏面 + 单独作者/诊断面，而不是从用户视线中完全抹掉高级能力。

### 3.5 不把自动重试次数当成语义质量

原版设置中存在 auto-swipe、最低长度与黑名单阈值，并且所检查默认配置的 auto-swipe 为关闭。[S16]

可保留：用户主动要求另一版、保留候选、可靠取消。

不继承：正文不够长就默认重写、出现某个词就反复生成、靠隐形多次调用筛出“合格故事”。这些结构/词法指标不能证明剧情好，且会让用量与等待不透明。短过渡也可能是正确的 Narrative。

### 3.6 不把快捷按钮、宏与自动执行赋予玩家决策权

Quick Reply 除了填入文字，还可直接发送或执行 STscript；官方说明存在脚本库和自动执行用法。[S05][D05]

可保留：清楚、可见、用户选择的便利入口。

不继承：默认自动替玩家行动；模型输出字符串被当脚本执行；在用户以为只是点建议时发生世界写入。高级自动化也不能没有成本、作用域与取消边界。

### 3.7 不把摘要、聊天副本、导出文本混成“游戏真相”

原版摘要、JSONL 聊天导出、纯文本导出和聊天 checkpoint 各有自己的用途；官方特别区分可重导入 JSONL 与失去关键元数据的纯文本，还指出附件不会都随聊天文件导出。[S03][S06][S17][D03][D06]

可保留：让玩家拥有记录、可阅读可分享、可迁移，并有备份。

不继承：一份文本就声称是完整世界存档；改了摘要就自动改事实；分支只切聊天，不切相关状态。

my-world 未来应分别提供“阅读版游记”和“可恢复游戏包”，并有清楚的范围说明。并非所有导出都能恢复游玩。

### 3.8 不把高级用户的无限可配置当成我们的产品完整度

主题、VN、多个音频/图片后端、宏、脚本、扩展都有需求，但增加一个可选模块会带来配置、失败处理和维护负担。[S11][S12][S13][D01][D10]

可保留：按需、可关闭、明确失败后果的增强。

不继承：一开局同时启用摘要、检索、表情分类、TTS、画图等所有后台工作；无穷设置和兼容模式作为“完整产品”的证明。

这是资源/复杂度取舍，不是声称原版默认同时运行全部扩展。先给用户一个无需组装就能玩的路径，再让确有需要的人扩展。

## 4. 未来可选功能池：产品结果、阶段与进入条件

下表条目仅是参考候选，不是新的 MW Work ID，也不因存在于本文而自动获得开发授权。已有同类候选见 [既有候选池](./备选开发方向候选池_2026-08-28.md)。

| 候选方向 | 玩家实际得到什么 | 建议匹配阶段（非排期） | 必要进入条件 | 最小验收与主要风险 |
|---|---|---|---|---|
| 按需生成诊断 | 知道漏卡/失忆发生在材料、模型输出还是保存展示阶段 | G6 真实排障触发，或 G7 | 连续出现不可解释的体验问题 | 一次问题能定位；默认不剧透、不泄露 Key |
| 回局回顾 | 隔几天回来，能迅速看懂上次经历和当前处境 | G6/G7 | 多日长局真实回局困难 | 摘要对得上玩家历史；Restore 后无未来 |
| 分层记忆与历史检索 | 旧人物/承诺在长局后仍可被适时引用 | G7 | 可复现遗忘与上下文预算证据 | 旧事实可追溯；相关性不被当成真相 |
| 叙事偏好 | 调节描写、镜头和节奏，不换世界 | G6 | Owner 给出具体风格不满意案例 | 实际文风变化且不强制剧情/固定长度 |
| 分支/检查点浏览 | 在关键抉择前留路，清楚自己在哪条路线 | G6/G7 | named Save/Recovery 已不足 | 世界与认知一起切换，不只换聊天 |
| 可读游记导出 | 带走或分享本局故事 | G6/G9 | 真实整理、阅读、分享需求 | 默认仅玩家可见内容；不冒充可恢复存档 |
| 可恢复整局包 | 换机/备份后继续玩同一局 | G9 | Standalone 发布与迁移需求 | 完整恢复验证；排除凭据 |
| 模型连接预设 | 一次切换一组已验证配置 | G6/G9 | 高频切换或错配问题 | 生效状态可见；不偷偷 fallback |
| 常用行动收藏 | 保存自己常写的行动表达，仍可编辑 | MW-019 UAT 后的 G6 候选 | 动态建议之外确有重复输入 | 只填草稿、不执行命令；不挤压正文 |
| 作品/资料包预览与标签 | 建局前理解要玩什么、角色是什么风格 | G8；轻量发现性可按 G6 实需 | Source 数量/作者交付带来查找成本 | 区分可见介绍与 GM 私密材料 |
| 多开场 Entry 候选 | 同一世界从不同已定义情境开始 | G8 或真实 Source 创作触发 | 一份真实作品确有多开场价值 | 不预定未来结果、不让角色卡接管世界 |
| SillyTavern 资料导入 | 把既有角色/世界资料带进 Source 创作 | G8 | Owner 指定真实待导入卡包 | 转换预览、来历保留、丢弃不支持权限，非原样执行 |
| 按需 TTS 听读 | 朗读一段已接受叙事 | G6 可选增强 | Owner 明确需要听读 | 可停止；失败不阻断；无隐私外发误解 |
| 背景/立绘/VN 展示 | 人物场景更有视觉氛围 | G6 Visual re-entry | 真实授权资产与明确受益场景 | 不伪造状态、不揭露隐藏人物/心理 |
| 背景资料库 | 按需参考制度、地理、文化等大型资料 | G7/G8 | 既有候选池 §5 的真实需求 | 历史参考不变成未来结局，已有 Game 不读 Source-current |

**建议的优先候选顺序：生成诊断 → 回局回顾/长局记忆 → 叙事偏好。** 这只是产品价值判断；若 UAT 没暴露这些需求，不为排名强行开发。音画、全面兼容和自动化暂不应挤占当前真实游玩闭环。

## 5. 对当前 MW-018 / MW-019 的直接启发（不追加范围）

MW-018 人物卡：Source 角色卡不等于玩家人物志，群聊成员也不等于玩家认识的人。已有 exact identity + latest-known + Timeline 方案应保持；漏关联字段是既有 UAT 风险，不通过程序猜姓名补齐。[P01]

MW-019 推荐行动：Quick Reply 的“仅填入”机制支持我们既定交互取舍。五项建议是模型生成的玩家草稿，不是脚本、固定技能按钮或合法行动全集。[P01]

组合 UAT 时可额外观察一个原有目标内的问题：用户能否分清“还在生成、正常无信息、生成失败”，以及选项是否抢占正文/草稿。发现问题再按各自 lineage 修正，不在本研究中增加测试门槛或修改 Task Packet。

## 6. 将来真正采纳某一候选时的最小过程

先说明玩家遇到了什么问题、拟改善的可见行为、当前简单功能为何不足；再指出上游功能证据与本项目不能继承的部分。讨论确认后，才进入现有产品/架构 Gate 和任务派发。

参考可以提供失败样本和验收灵感，但不能替代我们自己的 UAT。当前已实现的 Save/Restore、角色资料、People、推荐行动应优先做体验验证，不为“学过”而重复建设。

## 7. 不确定性、争议与待实测事项

1. 没有比较各种模型下世界书、摘要、推荐或群聊质量；不声明成功率、成本优势或延迟数字。
2. 源码中存在某个设置不代表默认启用。尤其 auto-swipe 在所查默认配置中关闭；TTS/检索/表情是否生效依赖用户配置与服务。
3. 官方在线文档不是版本固定快照，可能与所查 release 有差异；发生冲突时应回到固定源码，不把 staging/第三方插件混入原版能力清单。
4. 没有证明原版可以/不可以通过任何第三方扩展实现持续世界；本文仅指出所查内置聊天/群聊/世界书机制不能直接证明 my-world 的世界语义成立。
5. “设置太多”“默认自动化太重”等为面向本项目的产品风险判断，不是用户满意度调查结论。
6. 没有全仓复杂度、测试密度、缺陷率或可维护性评测，因此不以“屎山代码”作为功能取舍理由。

## 8. 证据索引

### 固定上游源码（全部为同一 release 提交）

- [S00 README / 产品定位](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/README.md)
- [S01 itemized-prompts.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/itemized-prompts.js#L1-L220)：读取保存提示明细、itemizedParams 分类计数等入口。
- [S02 itemizationChat.html](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/templates/itemizationChat.html)：搜索核验 Show Raw Prompt / Copy / Differences 控件，不宣称审读整个模板。
- [S03 memory/index.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/extensions/memory/index.js)：读取 1–260、390–575；摘要配置、onChatChanged/onChatEvent、summarizeChat 等入口。
- [S04 vectors/index.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/extensions/vectors/index.js#L1-L220)：聊天/文件/World Info 的容量与检索设置、向量化入口；未实测检索质量。
- [S05 QuickReplySet.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/extensions/quick-reply/src/QuickReplySet.js#L100-L260)：executeWithOptions 的填入/发送/命令分流。
- [S06 bookmarks.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/bookmarks.js#L1-L250)：getBranchChatSnapshot、createBranch。
- [S07 authors-note.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/authors-note.js#L1-L170)：内容、频率、深度、位置、角色设置。
- [S08 char-data.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/char-data.js)：完整读取数据定义；角色资料、备用开场、世界书、扩展字段。
- [S09 PromptManager.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/PromptManager.js#L1-L185)：Prompt 注入/覆盖字段，结合官方使用文档理解功能。
- [S10 regex/engine.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/extensions/regex/engine.js)：读取 1–250、275–520；作用域许可、getRegexedString、runRegexScript。
- [S11 connection-manager/index.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/extensions/connection-manager/index.js#L1-L200)：配置组合字段与 ConnectionProfile。
- [S12 tts/index.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/extensions/tts/index.js#L1-L210)：音色映射、onNarrateOneMessage/onNarrateText。
- [S13 expressions/index.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/extensions/expressions/index.js#L1-L190)：分类来源、更新间隔、VN sprites 入口。
- [S14 world-info.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/world-info.js#L1-L260)：扫描/递归/预算/逻辑与 timed-effect 定义；checkWorldInfo 定位搜索；复杂执行效果未运行实测。
- [S15 group-chats.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/group-chats.js#L1-L150)：四种 activation 策略、generation mode、automode；activateNaturalOrder 定位搜索，具体用户规则与官方文档交叉核对。
- [S16 power-user.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/public/scripts/power-user.js) 与 [默认 settings.json](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/default/content/settings.json)：定点搜索 auto_swipe 配置字段与关闭默认值，不宣称审计整个生成循环。
- [S17 src/endpoints/chats.js](https://github.com/SillyTavern/SillyTavern/blob/8172dcd0ee672d3cd9a5e5f7af134f91a45cd2b8/src/endpoints/chats.js#L1-L220)：backupChat、节流与部分导入路径；完整导出语义用官方文档补证。

### 官方功能文档（查阅于 2026-09-06）

- [D01 产品与功能定位](https://docs.sillytavern.app/)
- [D02 Prompts / Viewing the Prompt](https://docs.sillytavern.app/usage/prompts/)
- [D03 Summarize](https://docs.sillytavern.app/extensions/summarize/)
- [D04 Data Bank](https://docs.sillytavern.app/usage/core-concepts/data-bank/)
- [D05 STscript / Quick Replies](https://docs.sillytavern.app/usage/st-script/)
- [D06 Chat File Management / Checkpoints / Export](https://docs.sillytavern.app/usage/core-concepts/chatfilemanagement/)
- [D07 Character Design](https://docs.sillytavern.app/usage/core-concepts/characterdesign/)
- [D08 Personas](https://docs.sillytavern.app/usage/core-concepts/personas/)
- [D09 Connection Profiles](https://docs.sillytavern.app/usage/core-concepts/connection-profiles/)
- [D10 Visual Novel Mode](https://docs.sillytavern.app/usage/user-settings/visual-novel/)
- [D11 World Info](https://docs.sillytavern.app/usage/core-concepts/worldinfo/)
- [D12 Group Chats](https://docs.sillytavern.app/usage/core-concepts/groupchats/)
- [D13 Prompt Manager](https://docs.sillytavern.app/usage/prompts/prompt-manager/)

### 本项目事实源

- [P01 CURRENT STATUS](../MY_WORLD_CURRENT_STATUS.md)：读取时 v16.20；动态状态以后始终以该文件最新版本为准。
- [P02 核心设计原则](../MY_WORLD_核心设计原则_CURRENT.md)：读取时 v2.0，重点为模型自由、叙事优先、玩家时间线、Source/Game 分层与知识隔离。
- [P03 既有备选方向候选池](./备选开发方向候选池_2026-08-28.md)：本文补充上游功能证据，不复制或提升既有 deferred 项为已授权。

**最终定位：可检索的功能经验与候选池，不是架构模板、路线冻结或实现指令。**
