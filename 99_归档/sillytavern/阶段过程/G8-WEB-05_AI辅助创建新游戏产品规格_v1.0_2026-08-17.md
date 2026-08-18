# G8-WEB-05｜AI 辅助创建新游戏产品规格 v1.0

状态：`FROZEN IMPLEMENTATION BASELINE`
日期：2026-08-17
适用阶段：`G8-WEB-05｜My Games / New Game / Settings Product Flow`
性质：**G8 阶段工作规格 / Implementation Baseline；不是新增核心项目源，不增加核心项目源计数。**
上游依据：G5–G7 已关闭 Runtime 权威边界、G8 Runtime-extensible UI Host 架构、当前 Product New Game 主流程、2026-08-17 创建流程产品讨论。

---

# 0. 本规格解决什么

本规格冻结 `AI 辅助创建新游戏` 的产品结构、玩家填写语义、AI 权限、Program 权限、Creation Draft 持久化、Expansion Creation Contribution 边界、Semantic Review Protocol、人格与快捷输入衍生要求，以及最终 Create Game 的最小 Hard Gate。

本规格不设计 G9 最终 `asset-spec vNext`；不允许为了完成 WEB-05 提前发明半套 Creator / Asset Compiler / 通用 Compatibility Resolver。

本规格的最高目标不是“让创建流程验证得尽可能严格”，而是：

> **让玩家能够低摩擦创建一局自洽、可恢复、可继续编辑的游戏；只有 Program 可以百分之百证明继续会导致无法实例化、数据损坏、安全越权或明确机器合同冲突时，才允许 Hard Gate。**

---

# 1. 顶层产品结构冻结

旧结构：

```text
One Creation Draft
└─ 5 个顶层 Section
```

正式替换为：

```text
Creation Project
│
├─ Step 1｜世界
│   ├─ 我想玩什么
│   └─ 这个世界从哪开始
│
├─ Step 2｜玩法
│   └─ 选择已有玩法 / Expansion
│
└─ Step 3｜角色与开场
    ├─ 我是谁
    └─ 怎样开场

↓
最终确认（不是第四个导航 Step）
↓
Create Game Instance
```

顶部导航：

```text
● 1 世界
○ 2 玩法
○ 3 角色与开场
```

完成态只是 UI 进度提示：

```text
✓ 1 世界
✓ 2 玩法
● 3 角色与开场
```

**三个 Step 不是 Wizard 监狱。** 玩家可以自由返回、提前浏览下游、再回来补充上游。除最终创建外，不建立“Step 未 PASS 就禁止进入下一步”的硬状态机。

---

# 2. Creation Project 与 Durable Draft

三个页面必须属于同一个持久化 Creation Project，而不是三个临时 React Form。

逻辑上至少拥有：

```text
Creation Project
├─ World Draft
├─ Expansion Composition Draft
├─ Character Definition Draft
└─ Opening Parameters Draft
```

每个 Draft 可以有内部：

- revision；
- autosave 状态；
- AI generation 状态；
- preview/recommendation freshness；
- validation/review result。

但这些内部状态**不得轻易升级为玩家流程 Gate**。

## 2.1 Durable Resume

必须支持：

```text
编辑
→ 自动保存
→ 关闭页面
→ 关闭酒馆
→ 重新启动
→ New Game
→ 继续未完成 Creation Project
```

`保存并继续` 的主要语义是“进入下一阶段”，不是唯一保存动作。

## 2.2 Autosave 去交易化

Creation Draft 不是 Formal Turn，不照搬 G7 exactly-once / strict stale revision 的产品体验。

推荐：

```text
前端即时更新
→ debounce / blur autosave
→ 保存失败时保留用户本地编辑
→ UI 提示“尚未保存，正在重试”
```

可以保留 revision 作为内部控制，但正常单用户编辑不得频繁向玩家暴露：

- `STALE_DRAFT_REVISION`；
- `409 Conflict`；
- 因一次 autosave 竞争而整页失效。

---

# 3. 修改上游时的保留规则

修改 Step 1 / Step 2 不删除下游真实用户输入。

```text
World Draft 修改
→ 保留 Expansion 选择
→ 保留 Character / Opening
→ 旧 AI 推荐 / Preview 可标记“可能已过期”
```

不采用：

```text
改一个上游字段
→ 下游全部 needs_review
→ 禁止继续
```

AI 派生内容可以提示：

> 世界已修改，之前的玩法推荐可能不再完全适用。[重新分析]

玩家可以选择不重新分析。

## 3.1 Dormant

已选 Expansion 贡献的字段因为玩法关闭而暂时失效时：

```text
Active Contribution
→ Dormant
```

- 不删除原值；
- 重新启用时恢复；
- Dormant 数据不得进入最终 Game Setup；
- Dormant ≠ blank；
- Dormant ≠ deferred。

---

# 4. Step 1｜世界

页面名称：`创建世界`

产品原则：

> **玩家决定世界的方向与边界；AI 在这些方向内帮助扩展成 game-local 的公开 World Definition。**

这里语义上对应 World Pack，但不会向资产库自动创建正式可复用 World Pack。正式 World Pack Authoring 属于 Creator。

## 4.1 默认 8 个核心字段

| 字段 | 默认展示 | AI Fill | 可暂不设定 | 最终 Hard Gate |
|---|---:|---:|---:|---:|
| 世界概念 | 是 | 是 | 否 | **是** |
| 世界名称 | 是 | 是 | 否 | 否；可使用确定性默认名 |
| 整体基调 | 是 | 是 | 是 | 否 |
| 重点体验 | 是 | 是 | 是 | 否 |
| 不想体验 | 是 | **否** | 是 | 否 |
| 时间 / 时代锚点 | 是 | 是 | 是 | 否 |
| 主要开局区域 | 是 | 是 | 是 | 否 |
| 世界特殊规则 | 是 | 是 | 是 | 否 |

其中唯一真正需要玩家最终提供最小语义的是：`世界概念`。

### 世界概念

示例：

- 一个开放历史的汉末三国世界；
- 一个成熟高魔文明，魔法像现代技术一样进入日常生活；
- 一个没有客观超自然的架空中世纪世界。

### 时间 / 时代锚点

保持自由文本 / 世界纪年语义，不做现实世界 Date Picker 专用设计。

合法示例：

- 公元 208 年赤壁前夕；
- 断界历 1287 年；
- 王朝末年；
- 第三次星际战争结束后二十年。

**Runtime 需要内部权威时间初值，不等于玩家必须填写时间锚点。** 未填写时由 Program 使用确定性最小 T0 初始化。

### 不想体验

这是特殊 player-owned 字段：

- AI 不代填；
- AI 不覆盖；
- AI 不删除；
- AI 不弱化语义；
- AI 可以把它作为生成与 Review 约束读取。

## 4.2 更多世界设定（默认折叠）

允许深度玩家进一步指定：

- 题材标签；
- 世界尺度；
- 社会与制度背景；
- 科技水平；
- 魔法 / 超自然水平；
- 世界地理概况；
- 当前公开世界局势；
- 其他必须存在的世界元素。

不继续拆成“经济制度 / 法律 / 军制 / 教育 / 交通 / 医疗 / 货币 / 宗教……”的世界百科问卷。玩家给方向，AI 扩展内容。

## 4.3 世界特殊规则

用于表达世界与常识相比必须成立的根本差异，例如：

- 没有客观超自然；
- 魔法是真实自然规律；
- 诸神客观存在但并非全知；
- 历史没有修正力；
- 人工智能具有法律人格；
- 死亡通常不可逆。

该字段不用于设计 Spell Runtime / Combat Runtime 等具体玩法机制。

## 4.4 世界预览是输出，不是输入

旧字段 `玩家开局已知的世界概述` 从必填输入中删除。

```text
玩家世界字段
↓
AI
↓
World Preview
```

可包括：

- 一句话世界介绍；
- 时代与社会；
- 力量 / 科技边界；
- 当前公开局势；
- 可选身份空间；
- 可考虑的开局方向。

Preview 只能使用玩家可见内容，不能泄露 Hidden World Truth、NPC 私密计划或后台秘密。

Preview 失败：

> 显示“暂时无法生成预览，可继续编辑”。

**Preview 永远不是 Step Gate。**

## 4.5 Step 1 不生成完整 Hidden World

这里只冻结：

```text
Public World Definition
World Constraints
T0 World Context（玩家已指定部分）
Open Conflicts / Open Directions
```

不在 Step 1 一次生成完整 Hidden World Truth。

---

# 5. Step 2｜玩法

页面名称：`选择玩法`

产品原则：

> **玩家决定这一局想深入支持哪些玩法；系统处理已知资产的呈现与确定性基础关系；AI 负责推荐与语义组合建议。**

普通玩家 UI 默认使用“玩法”语言，不要求理解 Expansion Pack、Owner、Namespace、Schema、Surface ID 等工程术语。

## 5.1 New Game 不允许创建 Expansion

正式冻结：

> **AI Assisted Game Creation 不包含 Expansion Pack Authoring。**

原因：Expansion 可能声明 State、Rule、Resolution、Permission、Dependency、UI Contribution、Extension Surface 等正式运行能力。

玩家要制作新玩法资产：进入 Creator（G9+）。

## 5.2 世界不强制要求玩法

删除：

```text
World Requirement
→ 某 Expansion 必须启用
```

即使世界明确是高魔文明，也只允许：

> `魔法｜强烈推荐`

玩家仍可不启用深度魔法玩法。

合法示例：

- 三国战争世界，只想经营医馆，不选战争深度机制；
- 高魔世界，但玩家不想操作正式施法系统。

## 5.3 AI 推荐等级

AI 可以输出：

- 强烈推荐；
- 推荐；
- 可选；
- 不推荐。

这些全部是建议，不拥有自动启用权。

AI 推荐 ≠ 玩家选择。

只有真正 machine-declared 的技术 Dependency 才可以由 Program 自动带入必要基础资产。

## 5.4 页面区域

建议只有：

1. `推荐给你的玩法`；
2. `更多玩法`；
3. `当前玩法组合 / AI 组合建议 / 基础检查`。

玩法卡默认只显示：

- 玩家语言名称；
- 一句话体验说明；
- AI 推荐原因（若有）；
- 启用/关闭；
- 有 Creator-declared config 时才出现“设置”。

版本、Asset ID、Owner、Hard Dependency 等默认不暴露，可放技术详情。

---

# 6. Step 2｜AI 与 Program 权限

## 6.1 AI 是语义推荐 / 组合分析的主要负责人

AI 输入：

```text
World Draft
+
Experience Focus
+
Program 提供的实际可用 Expansion Catalog
+
当前玩家选择
+
各资产允许用于 Review 的 Scope / Ownership / Integration / Handoff 说明
```

AI 可以判断：

- 哪些玩法更适合玩家当前世界与体验；
- 多个玩法之间是否互补；
- 一个玩法是否可能架空另一个玩法；
- 是否存在值得玩家知道的语义重叠；
- 哪些组合可能改变预期体验；
- 哪些玩法可能冗余。

AI 不可以：

- 发明 Catalog 中不存在的 Expansion；
- 自动启用/关闭可选玩法；
- 修改 Draft；
- 把“我不推荐”升级为 Hard Gate。

## 6.2 Program 只做确定性 Mechanical Validation

Program 不推断：

> “这个包看起来和那个包有点冲突。”

只处理机器可以 100% 证明的事实。

在 G8 当前阶段尤其禁止为了 WEB-05 提前建设 G9 的通用 Asset Validator。

G9+ 正式 asset-spec 存在后，可以机械校验例如：

- 资产是否存在 / 可读取；
- Creator 显式声明的 Hard Dependency 是否存在；
- Creator 显式声明的 Hard Conflict；
- 两个结构化声明精确 owns 同一个 canonical unique Surface ID；
- Creator-declared config 类型 / schema 是否有效。

语义兼容：交给 AI 软建议。

## 6.3 AI 组合建议永不成为 Hard Gate

UI 建议使用：

- 没有明显问题；
- 值得注意；
- 高风险组合。

不使用 `PASS / FAIL` 作为玩家语义。

即使 AI 认为组合很奇怪，只要 Program 没有证明硬错误，玩家必须可以：

> `[仍然这样玩]`

## 6.4 AI 组合检查不在每次 checkbox 变化后自动强制调用

优先：

> `[AI 帮我检查这套玩法]`

世界修改后可显示：

> 推荐基于旧世界设定。[重新分析]

Provider 不可用/超时：不影响玩家继续。

---

# 7. Expansion Creation Contribution｜正式职责边界

本体只提供声明式 Contribution Host / Interface。

> **具体贡献什么字段、字段含义、何时显示、是否必填，都由 Expansion 创作者在资产中显式声明。**

本体不得根据“这是魔法包 / 政治包 / 穿越包”自行推断需要哪些字段。

AI 也不得发明 Creation Schema。

职责：

```text
Creator
→ 定义字段存在与语义

Asset
→ 保存声明

Host
→ 校验 / 渲染 / 保存 / dormant / validation

AI
→ 只在声明允许时辅助填字段值

Player
→ 最终选择与确认
```

## 7.1 合法目标位置

G8 Host 产品语义至少应支持未来：

```text
game_creation.expansion_settings
game_creation.character
game_creation.opening
```

实际 G9 machine ID 可重新设计；G8 不冻结最终 asset-spec 字段名。

## 7.2 Creator 可声明的字段行为（G9 需求，不在 G8 发明最终 Schema）

未来至少需要表达：

- field identity；
- label；
- help/description；
- target placement；
- 标准 component type；
- required / optional；
- AI Fill allowed；
- deferred allowed；
- default value；
- 简单 visibility condition；
- validation constraint。

## 7.3 禁止任意前端执行

Contribution 不允许：

- 自定义 JS；
- 自定义 React；
- 任意 DOM；
- eval；
- 任意 CSS 注入；
- 直接写 Game State。

Creator 声明语义，Host 掌握表现与安全权。

## 7.4 条件语言要极度克制

不要建设通用 Rules Engine。

未来优先只支持非常简单的声明条件，例如：

- own_field == value；
- own_feature enabled。

跨 Expansion 的复杂语义不由表单 Condition Language 解决。

## 7.5 Contribution 错误隔离

错误的一个 Expansion 不应拖垮整个 Creation Project。

原则：

- 可选 Contribution Host 不支持 → 隐藏该项 + Creator/开发警告；
- 可选字段无效 → 忽略该字段，不拖垮整个 Expansion；
- Runtime 必需 Contribution 无法呈现 → 该 Expansion 标记不可用，玩家可取消后继续；
- 整个资产无法读取 → 只让该资产不可选；
- AI 不会填写 → 玩家可手填或暂不设定（除非 Creator 明确声明 Runtime 必需且没有默认/Deferred 路径）。

---

# 8. Step 3｜角色与开场

页面名称：`角色与开场`

内部两个 Section：

```text
我是谁
怎样开场
```

但数据域必须明确：

```text
Player Character Definition
≠
T0 Opening Parameters
```

UI 同页不等于 Ownership 相同。

正式 Character Card Authoring 仍属于 Creator；这里生成的是当前 Game 的 Player Character Definition。

---

# 9. Step 3｜我是谁：7 个核心字段

| 字段 | 默认展示 | AI Fill | 可暂不设定 | 最终 Hard Gate |
|---|---:|---:|---:|---:|
| 姓名 / 称呼 | 是 | 是 | 是 | 否；可使用确定性默认称呼 |
| 身份定位 | 是 | 是 | 否 | **是** |
| 年龄 / 人生阶段 | 是 | 是 | 是 | 否 |
| 出身与公开背景 | 是 | 是 | 是 | 否 |
| 能力与局限 | 是 | 是 | 是 | 否 |
| 初始性格与行为风格 | 是 | 是 | 是 | 否 |
| 当前目标与重要牵挂 | 是 | 是 | 是 | 否 |

Core 不固定写死：

- 种族；
- 国家；
- 职业；
- 阶级；
- 宗教；
- 学院；
- 政治身份；
- 魔法适性；
- 穿越方式；
- 系统类型。

这些要么自然包含在“身份定位/背景”里，要么由 Creator-declared Expansion Contribution 提供。

## 9.1 身份定位

自由文本示例：

- 汉末江陵人，二十六岁的游方医者，出身地方小吏之家；
- 塞勒汀学院联邦的人类正式法师，专攻基础术式理论。

系统不因为自由文本存在就自动创造题材字段 Schema。

## 9.2 能力与局限

玩家填写语义，不填 RPG 数值表。

例如：

> 擅长医术与辨认药材，观察力强，但身体普通，不擅长正面冲突。

正式 Capability Bootstrap 由对应 Owner / Runtime 解释；Core Character Form 不绑定固定属性模型。

## 9.3 更多角色设定（默认折叠）

可包括：

- 外观与明显特征；
- 性别 / 自我认同；
- 文化 / 族群 / 出身地；
- 价值观 / 信条；
- 重要过去经历；
- 重要人物关系；
- 语言与表达风格；
- 其他角色要求。

---

# 10. 玩家角色的初始性格、人格演化与 Roleplay Loop

`初始性格与行为风格` 不是让 AI 替玩家扮演主角的命令。

它定义：

> **角色 T0 的初始人格画像。**

玩家角色真正说什么、想什么、做什么，仍由玩家最终输入决定。

## 10.1 创建页必须提前提醒玩家

建议产品文案：

> **这是角色开局时的人格画像。游戏过程中，它会根据你长期实际做出的选择逐渐变化，并影响输入框上方的快捷行动建议。你可以随心行动，让角色逐渐变得更像你；也可以有意识地按照角色设定进行扮演。一次不同寻常的选择不会改变性格，只有持续、稳定的行为变化才会形成新的人格特征。**

这段提醒属于正式体验要求，不能只放帮助文档。

## 10.2 初始人格与当前人格分离

```text
Initial Personality
→ immutable Creation Definition

Current Personality Profile
→ evolving player-character state / profile
```

不得覆盖原始 Creation Definition。

## 10.3 人格演化证据

只来自玩家真正提交的 Player Input。

不能来自：

- AI Narrative；
- NPC 评价；
- Formal Outcome；
- 未选择的快捷输入；
- AI 猜测玩家内心；
- 世界事件本身。

一次非常规行动不等于人格改变。

人格变化需要：

> 跨场景、长期、重复、稳定的行为模式。

## 10.4 避免快捷输入自证偏差

人格分析应知道输入来源。

概念权重：

- 玩家完全自行输入 → 强行为证据；
- 玩家明显修改快捷建议 → 正常行为证据；
- 玩家原样发送快捷建议 → 弱行为证据。

本规格不冻结具体数值权重。

## 10.5 人格变化提示

人格演化是低频、非阻塞增强能力。

只有形成明显稳定变化时，提示：

> **你的性格似乎发生了一些变化。**

变化以自然语言表达，不做简单 `谨慎 -2 / 勇敢 +3`。

不要求玩家“接受变化”；长期真实行为已经发生。但必须允许：

> `这不像我的角色`

用于纠正 AI 对行为意义的错误解释，而不是删除真实历史行为。

模型调用失败 / 置信度不足 / 没有明显变化：什么都不做，不能影响正式 Turn。

---

# 11. 五条快捷输入建议

正式 Session 输入框上方产品目标：动态提供 5 条 `Suggested Player Inputs`。

输入来源：

```text
Current Personality Profile
+
Current Scene
+
Player-safe Knowledge
+
Current observable environment / characters
↓
AI
↓
5 条候选 Player Input
```

## 11.1 玩家代理权

建议只是候选，不是 Action Command。

推荐交互：

```text
点击建议
→ 填入输入框
→ 玩家可编辑
→ 玩家点击发送
→ 才成为正式 Player Input
```

不允许点击候选即自动提交，除非未来另有明确产品裁定。

## 11.2 快捷输入硬边界

候选必须：

- 只使用 player-safe Knowledge；
- 不读取 Hidden Truth 生成泄密动作；
- 不提前写 Formal Outcome；
- 不替玩家声明未成立的感情、承诺或动机；
- 五条尽量提供不同方向，而不是五个同义改写。

合法：

> 尝试向守卫解释自己的身份，请他允许进入。

非法：

> 成功说服守卫放我进去。

## 11.3 无人格字段时仍可工作

如果 Initial/Current Personality 未设定，仍可根据当前 Scene 生成中性建议。

## 11.4 绝不成为 Turn Gate

- 只生成 4 条 → Turn 仍成功；
- Provider timeout → 输入框照常使用；
- 暂时没有建议 → 玩家照常输入。

尽量避免为每回合增加一个强制独立 Provider 单点失败；可作为 sidecar / 非阻塞增强实现。

---

# 12. Step 3｜怎样开场：5 个核心字段

| 字段 | 默认展示 | AI Fill | 可暂不设定 | 最终 Hard Gate |
|---|---:|---:|---:|---:|
| 开局地点 | 是 | 是 | 是 | 否 |
| 当前处境 | 是 | 是 | 是 | 否 |
| 眼前的问题 / 契机 | 是 | 是 | 是 | 否 |
| 重要开局人物 | 是 | 是 | 是 | 否 |
| 初始资源与随身物品 | 是 | 是 | 是 | 否 |

此处不再重复填写 Step 1 的当前世界局势。

```text
World T0
+
Character T0
→ Opening
```

## 12.1 当前处境，不叫“开场剧情”

只定义 T0 当前状态，不预写未来主线。

合法：

> 你是江陵的一名年轻医者。大量伤兵刚刚进入城内，你被临时征召到伤营帮忙。

不表示玩家必须治疗谁、加入哪方或触发固定剧情。

## 12.2 重要开局人物不是 NPC Creator

玩家只需写最小关系：

- 姐姐：与我共同生活；
- 导师：最近失联。

不要求在创建 PC 时顺便制作完整 Character Card。

## 12.3 初始资源写语义

例如：

> 一匹普通马、一套医具、大约半个月生活费。

不要求玩家输入数据库结构。

---

# 13. 角色预览 / 开局预览

Step 3 可以提供：

- Character Preview；
- Opening Preview。

它们用于确认 AI 是否理解正确。

Preview 绝不能：

- 创建 Formal Turn；
- 推进 worldTime；
- 写 Event；
- 写 Game State；
- 创建 Durable Execution。

Preview 失败不影响最终创建。

---

# 14. AI Fill / Adjust 的正式容错语义

AI 只能填已有 Schema 的值。

> **AI Fill values, never invent schema.**

保护：

- player-owned 非空值默认不被 bulk AI 覆盖；
- deferred 不被 bulk AI 自动激活；
- system-defined 不被 AI 修改；
- `不想体验` AI 永不拥有；
- Expansion Contribution 只有 Creator 声明 `AI Fill allowed` 才能让 AI 填。

## 14.1 AI 输出采用字段级安全容错，不采用全有或全无

例如 AI 返回：

- A 合法；
- B 合法；
- C fieldId 不存在；
- D 类型错误。

Program 应：

- 应用 A；
- 应用 B；
- 忽略 C；
- 忽略 D；
- UI 提示“已补全 2 项，其余内容未修改”。

**一个坏字段不能让整次 Draft Generation 全部失败。**

这与 Formal Outcome 的 fail-closed 不同；Draft 是可逆编辑材料。

---

# 15. Semantic Review Protocol｜必须固定架构，不允许一句“帮我检查”自由发挥

AI Semantic Review 是 Reviewer，不是 Creator，也不是 Program Judge。

必须实现为：

> **版本化 Review Kernel + 固定 Profile + 固定 Checklist + 结构化输出。**

不要做一个万能 Prompt，也不要让模型自行决定“今天想检查哪些维度”。

## 15.1 权限

Reviewer 可以：

- 找语义矛盾；
- 找明显遗漏；
- 找前后不一致；
- 分析 Expansion 组合影响；
- 指出体验风险；
- 给修改建议。

Reviewer 不可以：

- 修改 Draft；
- 自动启用/关闭 Expansion；
- 创建 Expansion；
- 创建字段；
- 替玩家补成正式事实；
- 把未知自行升级成确定事实；
- 因审美判断错误；
- 把建议升级成 Hard Gate。

## 15.2 共同 Kernel 固定检查原则

至少明确：

1. blank / deferred / unknown 不是错误；
2. 不要为了显得审核全面而制造 Finding；
3. 完全没有 Finding 是合法结果；
4. Finding 必须引用输入中的具体事实 / 具体资产 Scope 依据；
5. 找不到证据就不要报冲突；
6. 可以输出“信息不足，无法判断”；
7. 玩家有权创建奇怪但自洽的世界 / 角色；
8. Review 结果永不拥有 Hard Gate 权。

建议在 Kernel 中明确：

> **Do not manufacture findings to make the review appear thorough. A completely valid result may contain zero findings.**

## 15.3 Profile A｜Expansion Composition Review

固定检查：

1. World / Experience 与玩法的匹配程度；
2. Expansion 之间的互补；
3. Expansion 是否可能互相削弱体验；
4. Scope / Ownership 的明显语义重叠；
5. 配置组合带来的体验影响；
6. 哪些玩法可能冗余；
7. 哪些玩法值得推荐。

不检查 Character / Opening。

## 15.4 Profile B｜Final Game Setup Review

固定检查至少包括：

1. World Internal Coherence；
2. World ↔ Gameplay Fit；
3. Expansion ↔ Expansion Interaction；
4. Character ↔ World Fit；
5. Character Internal Coherence；
6. Character ↔ Expansion Fit；
7. Opening ↔ World / Character Fit；
8. Player Agency；
9. Knowledge & Disclosure Boundary；
10. T0 是否偷偷预写未来结果。

### Player Agency 固定检查

专门检查：

- 是否替玩家决定感情；
- 是否替玩家决定长期目标；
- 是否写“你必然会做某事”；
- 是否预写 Player Character 的未来行为；
- Opening 是否把尚未选择的行动写成已发生；
- 是否用人格字段限制玩家行动。

### Knowledge Boundary 固定检查

固定检查：

```text
World Truth
≠ Character Knowledge
≠ Narrative Disclosure
```

避免 Preview / Opening / Suggestion 泄露 Hidden Truth。

## 15.5 结构化输出

建议固定：

```text
overall:
  no_major_issue
  attention_recommended
  significant_issue_found

findings[]:
  category
  severity
  evidence
  explanation
  suggestion
  confidence
```

severity 只允许：

- `note`；
- `attention`；
- `significant`。

**不得有 `blocking`。**

## 15.6 Review 版本化

至少记录：

- Kernel Version；
- Profile Version；
- Output Schema Version。

例如：

```text
Creation Semantic Review Kernel v1.0
Expansion Review Profile v1.0
Final Setup Review Profile v1.0
```

Prompt 行为变化必须升版本，不在 service 里静默改一大串字符串。

## 15.7 Review Corpus

实现时应建立正例 + 负例测试。

应发现：

- 世界明确无魔 + 角色明确天生法师且没有任何解释；
- Opening 偷偷声明玩家已经决定效忠；
- Player Character 开局知道明确 Hidden Truth；
- 普通身份开局直接拥有世界最高权力且没有来源；
- 两 Expansion Scope 明显重复且值得提醒。

不应乱报：

- 时间锚点暂不设定；
- 高魔世界不启用 Magic Expansion；
- 三国世界启用穿越；
- 生存 + Ultimate System；
- 角色很怪但自洽；
- 背景没有写完整；
- 开局没有主线任务；
- 传奇人物已经给出合理背景。

测试目标不仅是“能不能发现问题”，也包括“会不会乱报”。

---

# 16. Final Review / AI Check 永远可选

最终确认页可以提供：

> `[AI 帮我检查一下这套设定]`

AI 可以提醒明显问题，但：

- 没配置 API Key；
- Provider unavailable；
- timeout；
- AI 认为不合理；
- Review 返回 significant；

都不能禁用 `创建游戏`。

**最终 Create Game 必须零强制 AI Call。**

---

# 17. 最终确认不是第四个导航 Step

Step 3 完成后打开 Review Page / Overlay。

展示：

- 世界；
- 玩法；
- 角色；
- 开局；
- 游戏名称。

`游戏名称` 不要求 Step 1 提前填写。

优先可由 AI 根据 World + Character + Opening 建议；AI 不可用时使用确定性默认名称，绝不因此阻止创建。

---

# 18. Hard Gate 白名单｜Creation 默认开放

任何未来新增 Validator 必须先证明自己属于 Hard Mechanical Failure，才有资格禁用 Continue / Create。

## 18.1 当前 G8 可以 Hard Gate 的最小集合

1. **世界概念最终为空，无法形成最小 World Definition。**
2. **玩家角色身份定位最终为空，无法知道玩家扮演谁。**
3. **Game Setup / Creation Project 无法安全持久化或最终提交失败。**
4. **存在明确安全越权输入 / Host 无法安全接受的执行性内容。**

其他 Creation 字段尽量通过：

- deterministic default；
- optional；
- deferred；
- warning；

解决，而不是 Hard Gate。

## 18.2 G9+ 有正式机器合同后才允许增加的机械 Gate

例如：

- 已选择资产不存在 / 无法读取；
- Creator 显式 Hard Dependency 缺失；
- Creator 显式 Hard Conflict；
- 两个结构化声明精确 owns 同一 canonical unique Surface ID；
- Creator 明确声明 Runtime 初始化不可缺少的必填 Contribution 没有值、默认值或 Deferred 路径。

不得把这一白名单扩张成“所有 Ownership / Rule / Resource / Namespace 都做通用窄 Gate”。只有反复出现、能够精确机器证明、确有现实风险的规则才升级合同。

---

# 19. 检查分级

| 类型 | 例子 | 是否可以挡住玩家 |
|---|---|---|
| Hard Mechanical Failure | 持久化失败、明确安全越权、G9 后精确机器合同错误 | **可以** |
| AI Semantic Advice | 世界/玩法不协调、角色背景奇怪、玩法相互削弱 | **不可以** |
| Optional Product Enhancement Failure | Preview、AI 推荐、人格分析、5 条快捷建议、Final AI Check 失败 | **不可以** |

最高原则：

> **凡是需要 AI 理解、语义判断、质量判断、推荐、Preview 或概率推断的事情，都没有资格阻止玩家创建游戏。**

---

# 20. API 未配置时的 Creation 语义

API Key 是 AI Provider 能力前置，不是产品启动前置，也不是 Creation Project 浏览/手填/保存前置。

没有 API Key 时必须能够：

- 启动产品；
- 进入 New Game；
- 创建/编辑/保存三个 Workspace；
- 手动选择玩法；
- 手动填写 Character / Opening；
- 打开最终确认；
- 在不需要新模型结果的前提下提交已接受的 Creation Definition。

AI Fill / Preview / Recommendation / Semantic Review 不可用时：

- 返回清晰提示；
- 不改 Draft；
- 不创建 Formal Turn；
- 不推进 worldTime；
- 不写 Event；
- 不创建 Durable Execution；
- 不把整个 Creation Project 标记 invalid。

---

# 21. Final Create Game｜零强制 AI Call

正式创建必须基于玩家已经接受的 Draft：

```text
World Draft
+
Expansion Composition
+
Character Definition Draft
+
Opening Parameters Draft
↓
Deterministic Compile
↓
Hard Gate 白名单
↓
immutable Game Setup / Creation Definition
↓
最小 T0 Runtime Definition / Game State
↓
Game Instance
```

Create 按钮本身不再发起强制 Semantic Review 或完整 Hidden World Bootstrap 模型调用。

需要的 Hidden World 内容采用最小初始化 + Runtime 按需生成/实例化，不让玩家在最后一步因为 Provider timeout 丢掉半小时创建工作。

---

# 22. Creation Definition 永久保留

创建成功后，原 Creation Project 形成不可变：

> **Game Setup / Creation Definition**

至少保存：

- 最初 World Definition；
- 最初玩法组合与创建期配置；
- Player Character Initial Definition；
- Initial Personality；
- T0 Opening Parameters；
- 创建时间与必要版本身份。

它不是 Live Game State，不应被后续游戏过程覆盖。

Current Personality、Current Relationship、Current Position、Current Goal 等属于后续演化状态，不回写初始 Creation Definition。

---

# 23. G8 / G9 边界

WEB-05 当前应完成：

- 三 Step Product IA；
- Durable Creation Project；
- 固定 Core Fields；
- AI Fill / Preview / Recommendation / Semantic Review Host Contract；
- 当前可用玩法 Catalog 的产品接口；
- Creator-declared Contribution 的 Host 接口/占位与受控渲染能力（不发明最终 G9 Schema）；
- Dormant / Deferred / player-owned 语义；
- Final Create 的 deterministic compile；
- 无 API Key 产品路径。

G9 再正式设计：

- `tavern-asset-spec vNext`；
- Creator；
- Asset Catalog compilation；
- Expansion Creation Contribution 最终 machine schema；
- Hard Dependency / Hard Conflict 的正式机器合同；
- 更完整 Asset Compatibility；
- 资产导入/发布/版本治理。

禁止 WEB-05 为了“先跑起来”扫描当前 Markdown 资产并发明临时协议。

---

# 24. 已废止设计

以下不再允许 Grok / Codex 重新实现：

1. 一个页面放 5 个顶层 Section；
2. `玩家开局已知世界概述` 由玩家必填；
3. 单一 `runtime_mechanism` explanatory field；
4. New Game 中由 AI 创建 Expansion；
5. 世界设定可以强制玩家启用某玩法；
6. Program 负责广义语义 Compatibility；
7. AI 推荐即可自动启用可选玩法；
8. Character Core 固定包含魔法 / 政治 / 穿越等题材字段；
9. 系统根据 Expansion 类型自行猜 Creation Contribution；
10. AI 自行发明/删除/重排 Creation Schema；
11. `初始性格与行为风格` 驱动 AI 自动扮演 Player Character；
12. Player Character 人格永远静态；
13. 修改上游直接删除下游填写；
14. 修改上游导致下游全部 `needs_review` 并强制重审；
15. 第四个“确认”导航 Step；
16. Step 1 就生成完整 Hidden World Truth；
17. Final Create 前强制 AI Semantic Review；
18. Final Create 时强制 Hidden AI Bootstrap；
19. Creation Draft 正常编辑照搬 Formal Turn 的严格 stale revision 产品 Gate；
20. AI Fill 一个字段坏掉就整次生成全部失败；
21. 每个 checkbox 变化都强制 AI Compatibility Call；
22. 每回合 5 条快捷输入生成失败就阻止 Turn；
23. 每回合都必须执行 Personality Judge；
24. 为 G8-WEB-05 提前建设完整 G9 asset-spec / Creator / Asset Validator。

---

# 25. 实施责任边界

## Grok Build｜UI / Interaction Owner

负责：

- 三 Step Creation IA；
- 页面布局、视觉层级、折叠详细设定；
- autosave 状态表现；
- AI Fill / Preview / Recommendation / Review 的玩家交互；
- Step 2 玩法卡、推荐解释、软警告；
- Step 3 Core / Contribution 分区；
- Final Review 页面/Overlay；
- 人格演化提示文案与 5 条 Suggested Input 的 Session UI（若本轮纳入实现）；
- 所有 no-key / provider-failure 的非阻塞产品表现。

Grok 不拥有：

- Runtime authority；
- Game State；
- Asset Dependency Judge；
- Semantic Review 结论权；
- 任意动态 React / JS Expansion 执行。

## Codex｜Contract / Runtime / Data / Git Integration Owner

负责：

- Durable Creation Project；
- Draft storage / resume / autosave contract；
- fixed Core field contract；
- AI Fill partial-safe apply；
- Deferred / Dormant / player-owned semantics；
- Step 2 Catalog contract；
- Creator-declared Contribution Host contract（G8 capability，不提前冻结 G9 schema）；
- Semantic Review Kernel / Profile / Output Schema / tests；
- Final Create deterministic compile；
- immutable Game Setup；
- minimal T0 bootstrap；
- no-key / provider unavailable fail-safe；
- tests / lint / typecheck / build / Product E2E；
- 按项目 Git standing policy 精确 commit / push。

---

# 26. 建议实施顺序

不要 UI 与底层长期各自猜合同。

推荐：

```text
1. Codex 冻结 Creation Project / API / DTO / persistence contract
2. Grok 按本规格与固定 Contract 完成三 Step Product UI
3. Codex 接回真实 API / durable storage / compile / tests
4. Grok 做视觉与交互收口
5. Codex 完成 Product E2E / regression / Git Integration
6. ChatGPT GitHub-first Independent Review
7. 真人 UAT 一次覆盖：no-key startup → API Settings / Creation → 三步保存恢复 → Create Game
8. G8-WEB-05 PASS / CLOSED
```

如果现有代码耦合度使第 1/2 步必须交错，可允许实现交叉，但 Contract Owner 仍归 Codex，Grok 不自行发明后端语义。

---

# 27. WEB-05 最终 UAT 核心场景

至少自然覆盖：

1. 无 `.env.local` / 无 Key 仍能启动并进入 New Game；
2. Step 1 只写世界概念，其余大量留空/Deferred，也能保存与浏览下游；
3. AI Fill 失败不修改 Draft；
4. 关闭页面/重启后恢复 Creation Project；
5. Step 2 AI 推荐可用时显示，但玩家可以忽略强烈推荐；
6. 世界高魔但玩家不选 Magic，不被硬挡；
7. AI Compatibility Review 失败/超时仍可继续；
8. 启用/关闭 Expansion 后 Creator-declared Contribution 正确 active/dormant，不丢值；
9. Step 3 玩家只填最小身份定位也能完成最小角色；
10. 创建页明确说明人格会随长期实际选择演化；
11. 返回 Step 1 修改内容不会清空 Step 2/3；
12. Final AI Check 可跳过；
13. Create Game 不发起新的强制 AI Call；
14. 创建成功后 Game Setup 不丢失；
15. 正式 Game State / Formal Turn / Event 只在正确 Runtime 边界产生。

---

# 28. 冻结结论

本规格经产品讨论与“去 Gate 化”总审核后冻结为 G8-WEB-05 AI 辅助创建新游戏 Implementation Baseline。

核心关系：

```text
玩家
→ 决定世界方向、玩法选择、角色、开场和最终行动

AI
→ 补全、扩写、推荐、解释、语义审核、人格演化理解、快捷输入建议

Creator / Asset
→ 显式声明正式玩法能力与 Creation Contribution

Host / Program
→ 保存、受控渲染、机械边界、Runtime 权威、Formal Outcome、Atomic Commit
```

最终产品原则：

> **Creation 默认开放；Hard Gate 严格白名单。AI 可以严格 Review，但永远不是阻止玩家创建游戏的 Judge。Creator 决定 Expansion 能贡献什么，Host 不猜，AI 不造 Schema。玩家可以随心而动，也可以主动扮演；角色的人格由长期真实选择逐渐塑形，但任何正式 Player Action 始终来自玩家最终提交的输入。**
