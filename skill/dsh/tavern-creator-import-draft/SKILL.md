---
name: tavern-creator-import-draft
description: 将 World Pack、Character Card、Expansion Pack、资产族蓝图或其他语义资产稿，转换为普通用户可阅读、AI 易生成、未来 tavern-creator 可在不调用 AI 的情况下确定性解析的逐行字段式 Creator Import Draft。支持创建、转换、修订、审查与合并草稿；严格区分人类可读导入草稿、Creator 内部 Draft Model、asset-spec 最终机器合同和 Game Runtime State。
whenToUse: 用户提供酒馆游戏世界包、角色卡、拓展包、资产族资料、Obsidian Markdown 或其他语义稿，并要求整理成 Creator 可导入草稿；需要制定或校验“名字：、性别：、Region：、Place：、Scene：……”这类逐行字段格式；需要把旧语义资产迁移为未来 Creator 的无 AI 导入源文件；需要审查草稿是否可被确定性 Parser 读取。
version: 0.1.0
status: candidate
format_contract: tavern-creator-import-draft/0.1
---

# Tavern Creator Import Draft

> 本 Skill 负责把**资产语义素材**转换为**人类可读、逐行字段、可被确定性 Parser 读取的 Creator 导入草稿**。
>
> 它解决的是：
>
> ```text
> 人 / AI 创作的语义资产稿
> → Human-readable Creator Import Draft
> → Creator deterministic parser（不调用 AI）
> → Creator Internal Draft Model
> → 未来 asset-spec vNext
> → Game
> ```
>
> 本 Skill 不冻结最终 `asset-spec vNext`，也不假装当前 Game Host 已支持所有机制。它冻结的是一份**长期创作源格式候选**，目标是避免今天完成的资产未来需要人工重新创作。

---

# 0. 强制执行协议

读取本 Skill 后，不得只概述格式，必须按以下顺序行动。

## 0.1 识别工作模式

将请求归入一种或多种模式：

1. 从语义稿创建新 Draft；
2. 把已有 Markdown / 文档转换为 Draft；
3. 修订现有 Draft；
4. 审核 Draft 是否符合导入格式；
5. 合并多个资产为资产族清单；
6. 从旧 Draft 迁移到当前版本；
7. 从 Draft 反向生成 Creator 编辑任务或字段缺口清单。

不得把“资产语义创作”“Draft 格式转换”“asset-spec 设计”“Creator UI 实现”混成一个动作。

## 0.2 先读取真实素材

必须先读取用户提供的资产原文，确认：

- 资产类型、名称、版本、状态、资产族；
- 一句话定位与核心价值；
- 明确负责与明确不负责；
- 稳定语义、Owner、依赖、跨资产引用；
- State / Rule / Action / Resolution / Event / UI / Host Requirement；
- 来源、争议、假设、未决事项；
- 哪些是资产内容，哪些只是项目过程、审核、Gate 或平台需求。

不得根据文件名或一般知识补齐原文没有提供的字段。

## 0.3 区分四层对象

任何输出都必须保持：

```text
Creator Import Draft
≠ Creator Internal Draft Model
≠ asset-spec Final Asset
≠ Game Runtime State
```

### Creator Import Draft

- 给人和 AI 写；
- 一行一个字段；
- 普通用户可读；
- 无 AI 可确定解析；
- 允许 `待确认`、`未提供`、`未映射内容`；
- 保存创作者意图与来源。

### Creator Internal Draft Model

- Creator 解析后使用的类型化编辑状态；
- 普通用户不必直接阅读；
- 未来由 Creator 实现决定。

### asset-spec Final Asset

- 严格机器合同；
- 负责安装、验证、依赖、冲突和 Runtime 交换；
- 当前不由本 Skill 冻结。

### Game Runtime State

- 当前某局真正发生的一切；
- 不能写入 World Pack / Character Card / Expansion Definition 草稿；
- 不能从 Draft 直接当成正式世界提交。

## 0.4 只转换，不擅自重写

- **事实**：按原文转换；
- **推断**：必须标注 `推断`；
- **缺失**：写 `未提供`；
- **确实尚未决定**：写 `待确认`；
- **原文明确没有**：写 `无`；
- **与该类型无关**：写 `不适用`；
- **无法安全映射但需要保留**：进入 `未映射内容` 条目。

不得为了让 Draft 看起来完整而发明性别、年龄、ID、依赖、Host 能力、公式、历史事实或机制结果。

## 0.5 输出一资产一文件

默认输出：

```text
<asset-id>.tavern-draft.md
```

多资产输入时：

- 每个资产一份 Draft；
- 另生成一份 `family.<slug>.tavern-draft.md` 资产族清单；
- 不把多个正式资产塞进同一 Draft 文件。

## 0.6 完成确定性校验

输出前必须验证：

- Draft 头完整；
- 资产类型合法；
- 所有 Section / Record 均闭合；
- 字段名使用当前规范名称；
- 必填字段不为空；
- ID 唯一且格式合法；
- 引用可解析或明确 `待确认`；
- 没有任意 JS、SQL、React、Prompt、eval 或执行代码；
- 没有把 Game State 写回 Definition；
- 没有把隐藏信息错误归入玩家公开字段；
- 没有丢失无法映射的重要原文。

---

# 1. 格式定位与设计依据

本格式从四类真实资产需求抽象：

1. **World Pack**：世界身份、历史/时间锚点、Region → Place → Scene、社会制度、公开/后台事实、开放未来、来源层级、资产接口；
2. **Character Card**：身份锚点、人格核心、能力语义、决策逻辑、关系风格、语言表现、T0 前后边界、Historical Resolver 与 Capability Bootstrap；
3. **Expansion Pack**：Owner、State、Rule、Action、Resolution、Event、Information Boundary、UI Contribution、Host Requirement、Dependency、Test Scenario、Creator Authorability；
4. **Asset Family**：成员、必需/可选关系、推荐组合、缺失资产、未来一键导入语义。

关键原则：

> **Draft 长得像普通文档，但语法必须像一个小型 DSL。**

---

# 2. Canonical Draft 语法

## 2.1 文件规则

- 编码：UTF-8；
- 换行：LF 推荐，Creator 应兼容 CRLF；
- 扩展名：`.tavern-draft.md`；
- 每个文件只允许一个资产；
- 第一节必须是 `【草稿头】`；
- 空行可以存在，Parser 忽略；
- 除多行正文块外，不允许无字段名的自由文本。

## 2.2 Section

```text
【Section 名称】
```

Section 名称必须来自当前模板。未知 Section 只允许使用：

```text
【扩展字段】
```

## 2.3 单值字段

```text
字段名：字段值
```

Parser 只按该行第一个全角冒号 `：` 分隔字段名与值。

## 2.4 简单列表

```text
字段名：
- 第一项
- 第二项
```

列表项必须单行。复杂对象必须使用 Record。

## 2.5 多行正文

```text
字段名：|
  第一行
  第二行
  
  第四行
```

规则：

- 正文行必须以两个空格开头；
- 空行也必须保留两个空格；
- 遇到下一个非缩进行、Section 或 Record 标记时结束；
- 多行正文内部可以包含普通冒号与 Markdown 列表，但必须保留两个空格缩进。

## 2.6 重复 Record

```text
《条目开始：记录类型》
记录ID：example.record
名称：示例
说明：|
  多行说明。
《条目结束：记录类型》
```

规则：

- 开始与结束的记录类型必须一致；
- v0.1 不允许 Record 内嵌 Record；
- Record 内可使用单值、列表和多行正文；
- 同一 Section 中的 `记录ID` 必须唯一。

## 2.7 保留缺失值

四个保留字语义不同：

| 值 | 含义 |
|---|---|
| `未提供` | 原始素材没有提供，不能推断 |
| `待确认` | 已识别为重要问题，但尚未正式决定 |
| `无` | 原始素材明确表示没有 |
| `不适用` | 对当前资产类型不适用 |

不得混用。

## 2.8 ID

推荐格式：

```text
world.han-late-three-kingdoms
character.cao-cao
expansion.character-capability
family.han-late-three-kingdoms
```

规则：

- 仅使用小写 ASCII、数字、点和连字符；
- 不用中文显示名称作为引用键；
- 正则建议：`^[a-z0-9]+(?:[.-][a-z0-9]+)*$`；
- 显示名称单独保存；
- ID 由创作者或转换任务提出；原文没有时必须在报告中说明“由本次 Draft 转换新建”。

## 2.9 引用

所有跨资产引用使用 `资产引用` Record：

```text
《条目开始：资产引用》
记录ID：ref.capability
关系类型：需要
目标资产ID：expansion.character-capability
目标名称：汉末三国：人物能力与技艺
必需性：推荐
缺失时行为：保守降级
说明：|
  政治判定只读取能力贡献，不复制第二套人物能力。
《条目结束：资产引用》
```

不得以 Obsidian Wikilink、文件名或显示名称冒充稳定机器 Ref。

## 2.10 扩展字段

当前格式无法表达但必须保留的结构化信息：

```text
【扩展字段】
命名空间.字段名：字段值
```

命名空间建议使用资产 ID。未知扩展字段由 Creator 原样保存，不自动执行。

## 2.11 未映射内容

原文重要但当前模板无法安全归类时：

```text
《条目开始：未映射内容》
记录ID：unmapped.001
来源章节：原文标题
建议归属：待确认
正文：|
  原文内容。
《条目结束：未映射内容》
```

这是防止信息丢失的最后兜底，不应用来逃避正常映射。

---

# 3. `【草稿头】` 通用字段

所有资产必须以以下字段开始，顺序固定：

```text
【草稿头】
草稿协议：tavern-creator-import-draft
协议版本：0.1
资产类型：世界包 / 角色卡 / 拓展包 / 资产族清单
资产ID：
资产名称：
资产版本：
草稿状态：草稿 / 候选 / 已审核 / 冻结候选
语言：zh-CN
资产族：
来源文件：
来源版本：
转换说明：|
  本 Draft 从什么素材转换，哪些 ID 为本次新建。
```

### 必填字段

- 草稿协议；
- 协议版本；
- 资产类型；
- 资产ID；
- 资产名称；
- 资产版本；
- 草稿状态；
- 语言。

`资产族` 对独立通用 Expansion 可以写 `无`。

---

# 4. World Pack 转换规范

## 4.1 固定 Section 顺序

```text
【草稿头】
【世界身份】
【职责边界】
【核心原则】
【来源与创作】
【时间与历史】
【空间结构】
【社会与身份】
【世界背景】
【信息边界】
【人物与玩家】
【开局设计】
【世界事实】
【趋势与矛盾】
【语言与叙事】
【资产接口】
【平台需求】
【创作备注】
【扩展字段】（可选）
```

## 4.2 世界包最小字段

### `【世界身份】`

- 一句话定位；
- 题材标签；
- 世界尺度；
- 默认体验基调；
- 玩家可见摘要；
- 时代范围；
- 历史模式；
- 默认超自然。

### `【职责边界】`

- 必须负责；
- 明确不负责；
- 越界处理。

### `【核心原则】`

重复 `世界原则` Record：

```text
《条目开始：世界原则》
记录ID：principle.open-history
名称：历史基线不是剧情脚本
规则：|
  ...
违反后果：|
  ...
《条目结束：世界原则》
```

### `【来源与创作】`

重复 `来源规则` Record：

- 来源级别；
- 名称；
- 适用范围；
- 处理规则；
- 可信度；
- 说明。

### `【时间与历史】`

可包含：

- 总体窗口；
- 历法说明；
- 精确日期规则；
- `历史阶段` Record；
- `时间锚点` Record。

时间锚点推荐字段：

- 年份 / 时间表达；
- 名称；
- 定位；
- 已成立事实；
- 尚未锁定未来；
- 适合身份；
- 适用人物；
- 初始化备注。

### `【空间结构】`

- 空间模型；
- Region 设计原则；
- `Region` Record；
- `Place` Record；
- `Scene 示例` Record；
- 地名时间敏感规则。

### `【社会与身份】`

重复 `身份空间` Record：

- 名称；
- 说明；
- 机会；
- 约束；
- 适用开局。

### `【世界背景】`

重复 `背景主题` Record：

- 领域：政治制度 / 经济物质 / 军事社会 / 技术 / 交通 / 通信 / 文化 / 宗教 / 超自然 / 其他；
- 名称；
- 世界事实；
- 时代边界；
- 不负责的动态机制。

### `【信息边界】`

必须区分：

- 世界真实；
- 玩家知识；
- NPC 知识；
- 玩家可见叙事；
- 公开事实；
- 后台锁定事实；
- 禁止预写未来。

### `【人物与玩家】`

- 历史人物引用规则；
- 一人一卡原则；
- World Pack 可保存的人物信息；
- World Pack 不保存的人物信息；
- `玩家身份` Record；
- 历史人物接管规则。

### `【开局设计】`

- 开局组合原则；
- `开局入口` Record；
- 默认不拥有；
- 自定义时间规则。

### `【世界事实】`

- 玩家可见公开事实；
- 后台锁定事实；
- 禁止后台预写内容。

### `【趋势与矛盾】`

重复 `世界趋势` Record。

### `【资产接口】`

重复 `资产引用` Record。

### `【平台需求】`

重复：

- `Runtime要求`；
- `UI Host要求`；
- `asset-spec要求`；
- `Creator要求`。

### `【创作备注】`

- 已核查；
- 未核查；
- 已冻结裁定；
- 待确认；
- 质量 Gate；
- 修订说明；
- 未映射内容。

## 4.3 World Pack 禁止项

不得把以下内容写成 World Pack Definition：

- 某局当前位置、官职、关系、兵力、粮草、伤势、任务或计划；
- T0 后必然发生的剧情；
- Expansion 的正式动态公式；
- Creator UI 字段实现；
- asset-spec 最终 Schema；
- Program RNG / Dice / Commit。

---

# 5. Character Card 转换规范

## 5.1 固定 Section 顺序

```text
【草稿头】
【人物身份】
【公开资料】
【私密资料】
【人格核心】
【核心矛盾】
【能力与局限】
【决策逻辑】
【合作与拒绝】
【失败与改变】
【关系与自主性】
【语言与表现】
【玩家接管】
【历史基线】
【能力初始化】
【来源与争议】
【资产接口】
【创作备注】
【扩展字段】（可选）
```

## 5.2 人物身份字段

建议保留普通用户熟悉的逐行字段：

```text
【人物身份】
姓名：
别名：
字：
性别：
出生年份：
出生说明：
籍贯：
历史人物：是 / 否
核心复杂度：
允许玩家接管：是 / 否
一句话辨识：|
  ...
身份消歧：|
  ...
```

原文未提供 `性别` 时必须写 `未提供`，不能仅凭常识补齐。

## 5.3 人格与行为

### `人格特征` Record

- 名称；
- 描述；
- 适用条件；
- 不是；
- 来源说明。

### `核心矛盾` Record

- 两端；
- 冲突说明；
- 触发条件；
- 可能表现。

### `决策问题` Record

用于保存角色面对重大问题时通常会问什么。

### `判断改变因素` Record

用于保存什么证据能真正改变其判断。

## 5.4 能力与局限

Character Card 只保存稳定的**能力语义轮廓**，不擅自生成 Runtime 数值。

- 主要长处；
- 重要局限；
- 能力证据；
- 不等同于；
- Capability Expansion Handoff。

## 5.5 关系与自主性

必须保留：

- 信任来源；
- 边界；
- 对挑战的反应；
- 玩家不在场时的自主行为；
- 不因玩家身份自动发生的事情。

## 5.6 玩家接管

严格分成：

```text
T0 前锁定
≠
T0 后交给玩家
```

不得把现实历史后续写成玩家接管后的强制路线。

## 5.7 历史基线

重复 `历史恢复项` Record：

- 项目；
- 由谁恢复；
- 截止时间；
- 需要的证据；
- 争议处理；
- 禁止事项。

## 5.8 来源与争议

- 主要史料；
- 争议；
- 文学传统；
- 合理推断；
- 原创补全；
- 不能自动升级为事实的标签。

---

# 6. Expansion Pack 转换规范

## 6.1 固定 Section 顺序

```text
【草稿头】
【机制身份】
【职责边界】
【所有权】
【机制目标】
【核心原则】
【玩法循环】
【作用域与对象】
【状态定义】
【资源与预设】
【规则定义】
【动作定义】
【判定定义】
【时间与过程】
【事件定义】
【信息边界】
【初始化】
【UI贡献】
【Host要求】
【依赖与兼容】
【测试场景】
【非目标】
【Creator创作性】
【平台需求】
【交接台账】
【创作备注】
【扩展字段】（可选）
```

## 6.2 机制身份

字段：

- 一句话定位；
- 是否 Runtime Asset；
- 可复用性；
- 安装后玩家价值；
- 不安装时降级；
- 默认配置；
- 核心体验原则。

## 6.3 所有权

重复 `所有权` Record：

```text
《条目开始：所有权》
记录ID：owner.current-condition
概念：当前身体状态
唯一Owner：本拓展包
本包使用方式：正式职责
禁止复制：|
  Capability、War、Economy 不得维护第二套当前身体状态。
《条目结束：所有权》
```

## 6.4 机制原语 Record

### State

```text
《条目开始：状态定义》
记录ID：state.example
名称：
作用域：Character / Organization / Region / Place / Scene / Item / Game / 其他
含义：|
  ...
包含：
- ...
不包含：
- ...
不变量：
- ...
信息可见性：
持久化要求：
《条目结束：状态定义》
```

### Resource

```text
《条目开始：资源定义》
记录ID：resource.example
名称：
作用域：
语义：|
  ...
来源：
- ...
消耗方式：
- ...
是否独立权威：是 / 否
缺失时行为：
《条目结束：资源定义》
```

### Preset / Profile

用于 Survival Intensity、Relationship Accessibility、System Power 等：

```text
《条目开始：预设定义》
记录ID：preset.standard
名称：标准
默认：是
目标体验：|
  ...
启用内容：
- ...
不改变：
- ...
《条目结束：预设定义》
```

### Module

用于穿越者系统、关系辅助等可独立启停能力：

```text
《条目开始：模块定义》
记录ID：module.teleport
名称：传送
模块类型：
默认启用：否
强度：
权限范围：
- ...
输入：
- ...
输出：
- ...
成本与限制：
- ...
目标Owner接口：
- ...
不拥有：
- ...
《条目结束：模块定义》
```

### Derived Projection

用于五维、关系摘要、健康摘要等非权威投影：

```text
《条目开始：派生投影》
记录ID：projection.example
名称：
权威输入：
- ...
输出用途：
是否第二事实源：否
公式状态：未冻结 / 有限声明式 / 固定Host规则
信息边界：|
  ...
《条目结束：派生投影》
```

## 6.5 Rule

```text
《条目开始：规则定义》
记录ID：rule.example
名称：
规则：|
  ...
适用范围：
例外：
违反后果：|
  ...
《条目结束：规则定义》
```

## 6.6 Action

```text
《条目开始：动作定义》
记录ID：action.example
名称：
发起者：
目标：
尝试语义：|
  ...
前置条件：
- ...
玩家授权要求：
- ...
能力与权限检查：
- ...
输入：
- ...
可能结果：
- ...
正式效果：
- ...
不保证：
- ...
缺失依赖时：
《条目结束：动作定义》
```

Action Definition 不是自由输入白名单。

## 6.7 Resolution

```text
《条目开始：判定定义》
记录ID：resolution.example
名称：
输入：
- ...
确定性检查：
- ...
需要Program判定的条件：
- ...
不需要Dice的条件：
- ...
可能结果：
- ...
正式Owner：Game Runtime
原子提交内容：
- ...
《条目结束：判定定义》
```

## 6.8 Timed Process

用于成长、恢复、病程、后台关系发展等：

```text
《条目开始：时间过程》
记录ID：process.example
名称：
时间来源：权威世界时间
开始条件：
- ...
推进条件：
- ...
暂停条件：
- ...
完成条件：
- ...
可能输出：
- ...
《条目结束：时间过程》
```

## 6.9 Event

简单 Event 可列表保存；复杂 Event 使用 `事件定义` Record：

- 事件ID；
- 名称；
- 发生条件；
- 来源；
- 参与者；
- 正式载荷；
- 玩家可见性；
- 是否改变当前状态。

## 6.10 UI Contribution

```text
《条目开始：UI贡献》
记录ID：ui.example
名称：
Surface：人物 / 世界 / 地图 / 右栏 / 设置 / 系统面板 / 其他
显示内容：
- ...
隐藏内容：
- ...
交互：
- ...
交互输出：Action Intent
无专用UI降级：
信息边界：|
  ...
《条目结束：UI贡献》
```

不得声明任意 React、JS 或 DOM 注入。

## 6.11 Host Requirement

```text
《条目开始：Host要求》
记录ID：host.example
要求ID：HR-EXAMPLE-01
能力：
必需性：必需 / 推荐 / 可选
责任方：
玩家价值：
缺失行为：
安全边界：|
  ...
《条目结束：Host要求》
```

## 6.12 Dependency / Compatibility

使用 `资产引用` Record，并明确：

- 需要 / 推荐 / 可选 / 冲突 / 集成 / 消费；
- 缺失时拒绝、降级还是关闭某子能力；
- 不得复制其他资产的 Owner 状态。

## 6.13 Test Scenario

```text
《条目开始：测试场景》
记录ID：test.example
测试ID：T-EXAMPLE-01
名称：
场景：|
  ...
输入：|
  ...
期望：
- ...
禁止结果：
- ...
覆盖边界：
- ...
《条目结束：测试场景》
```

## 6.14 Creator Authorability

保存：

- Creator 所需通用 Primitive；
- asset-spec vNext Requirement；
- Runtime / UI Host Requirement；
- Unresolved Declarative Gap；
- Authorability Gate；
- 不应写死进 Creator Core 的题材内容。

Creator 不执行 Runtime 逻辑。

---

# 7. Asset Family Draft 转换规范

资产族清单用于 Creator 建立 Workspace 和未来一键导入候选，不等于第四种 Runtime 机制。

## 7.1 固定 Section

```text
【草稿头】
【资产族身份】
【成员清单】
【推荐组合】
【依赖顺序】
【一键导入】
【缺失与待补】
【共享创作规则】
【创作备注】
```

## 7.2 成员 Record

```text
《条目开始：资产成员》
记录ID：member.world
资产ID：world.han-late-three-kingdoms
资产名称：汉末三国：天下未定
资产类型：世界包
资产版本：0.2.1
必需性：必需 / 推荐 / 可选
默认选中：是 / 否
安装顺序：10
族内职责：|
  历史舞台与世界事实 Owner。
《条目结束：资产成员》
```

## 7.3 推荐组合

```text
《条目开始：推荐组合》
记录ID：preset.standard
名称：标准历史体验
成员：
- world.han-late-three-kingdoms
- expansion.character-capability
- expansion.politics
说明：|
  ...
《条目结束：推荐组合》
```

## 7.4 一键导入语义

至少记录：

- 是否要求原子安装；
- 必需成员失败时行为；
- 可选成员失败时行为；
- 冲突检查；
- 重复导入策略；
- 版本锁定；
- 媒体资源策略。

当前只保存产品语义，不冻结 Game 安装 API。

---

# 8. 从语义 Markdown 映射到 Draft 的算法

## 8.1 Frontmatter

映射：

- `title` → 资产名称；
- `type / asset_type` → 资产类型；
- `version` → 资产版本；
- `status` → 草稿状态；
- `asset_family` → 资产族；
- `language` → 语言；
- `aliases` → 别名；
- 其他项目管理字段按需进入 `创作备注`，不得直接变成 Runtime 字段。

## 8.2 定位与 Scope

- “一句话定位 / 资产定位 / 文档定位” → 身份与目标；
- “必须负责 / 必须完成” → 职责边界；
- “明确不负责 / 非目标” → 明确不负责 / 非目标；
- “越界台账” → 交接台账。

## 8.3 Ownership Map

表格每一行转换为一个 `所有权` Record。

不得把同一概念复制到多个 Owner。

## 8.4 EP / 机制章节

根据语义映射，而不是只按标题编号：

- State → 状态定义；
- Resource → 资源定义；
- Profile / Tier / Intensity → 预设定义；
- Module → 模块定义；
- Rule → 规则定义；
- Action → 动作定义；
- Resolution → 判定定义；
- Event → 事件定义；
- Information Boundary → 信息边界；
- UI Contribution → UI贡献；
- Host Requirement → Host要求；
- Initialization → 初始化；
- Test → 测试场景。

## 8.5 Creator / asset-spec / Runtime Requirements

这些是重要的设计输入，但不应假装已经成为正式机器字段。

分别进入：

- Creator创作性；
- 平台需求；
- Host要求；
- 待确认。

## 8.6 Quality Gate 与修订说明

进入 `【创作备注】`，不作为 Runtime 资产内容。

## 8.7 Obsidian Wikilink

例如：

```text
[[汉末三国：政争与势力]]
```

转换时：

- 保留显示名称；
- 尝试从同批资产建立稳定 ID；
- 无法确认 ID 时写 `目标资产ID：待确认`；
- 不把 Wikilink 原样当机器 Ref。

## 8.8 长篇原文压缩

默认输出应保留**全部稳定语义**，但不必逐字复制重复解释。

可以：

- 合并重复表达；
- 把同类条目拆为 Records；
- 保留代表性示例；
- 将审查过程移入创作备注。

不得：

- 删除 Scope 边界；
- 删除 Owner；
- 删除信息边界；
- 删除失败/降级语义；
- 删除 Host Requirement；
- 删除关键 Test Scenario；
- 把多个概念压成单一模糊字段。

---

# 9. ID 生成规则

原文无稳定 ID 时，本 Skill可以提出候选 ID，但必须在转换说明中记录。

## 9.1 前缀

- `world.`
- `character.`
- `expansion.`
- `family.`

## 9.2 生成原则

- 优先使用已有英文名、规范拼音或稳定短语；
- 同一资产族内保持一致；
- 不使用版本号进入 ID；
- 名称改变时 ID 默认不变；
- 不根据文件路径作为 ID；
- 无法确定稳定英文/拼音时，写候选并标记 `待确认`，不要使用随机 UUID。

---

# 10. 信息边界与安全

## 10.1 World / Character

必须区分：

```text
公开资料
≠ 私密资料
≠ 世界真实
≠ 角色知识
≠ 玩家知识
≠ 叙事披露
```

## 10.2 Expansion

必须区分：

```text
资产声明可以提供什么
≠ Runtime 当前允许什么
≠ 玩家当前是否授权执行
≠ Program 最终提交了什么
```

## 10.3 禁止可执行内容

Draft 不能包含可执行：

- JavaScript；
- TypeScript；
- React；
- SQL；
- shell；
- eval；
- 任意 Prompt 注入；
- 文件系统路径执行；
- GM 权限请求。

代码示例只能作为说明文本进入 `创作备注`，并标记 `不可执行示例`。

---

# 11. Draft Quality Gates

## G0｜Source Fidelity

- 所有稳定语义均来自用户素材；
- 推断已标记；
- 缺失未伪造。

## G1｜Syntax

- Header、Section、Record、缩进合法；
- 无自由悬空正文；
- Record 成对闭合。

## G2｜Identity

- 资产类型正确；
- ID 合法且唯一；
- 名称与版本明确。

## G3｜Scope / Owner

- 必须负责与明确不负责完整；
- Owner 不漂移；
- Character / World / Expansion / Runtime 边界正确。

## G4｜Definition / Instance

- 没有把某局当前状态写入资产 Definition；
- T0 / Bootstrap /初始化语义与运行状态分离。

## G5｜Cross-Asset

- 依赖、推荐、冲突和降级明确；
- 跨资产 Ref 使用稳定 ID 或明确待确认；
- 不复制其他资产的权威事实。

## G6｜Information Boundary

- 隐藏信息不进入公开字段；
- Knowledge 与 Truth 分离；
- UI 只描述玩家安全投影。

## G7｜Creator Determinism

- Creator 无需 AI 即可根据字段名、Section 和 Record 解析；
- 未知字段能明确报错或进入 namespaced extension；
- 不依赖模糊标题猜测。

## G8｜No Executable Asset

- 无任意代码；
- UI 只声明 Contribution；
- Action 只产生 Intent；
- Runtime 保留验证与 Commit 权。

## G9｜Lossless Handoff

- 无法映射的重要内容进入 `未映射内容`；
- 项目需求、Host Gap 和测试语料未丢失；
- 转换报告列出所有待确认项。

---

# 12. 常见错误

## 12.1 把 Markdown 标题当 Parser 语义

错误：Creator 根据“看起来像身份介绍”猜字段。

正确：所有正式内容必须进入固定 Section / Field / Record。

## 12.2 把 JSON 当普通用户草稿

错误：要求创作者直接维护深层 JSON。

正确：用户写逐行字段；Creator 内部再转类型化数据和最终 JSON。

## 12.3 用显示名称做 Ref

错误：`目标：曹操`。

正确：`目标资产ID：character.cao-cao` + `目标名称：曹操`。

## 12.4 把 Action 列表当行为白名单

错误：未定义 Action 就拒绝玩家自由输入。

正确：Action 只是高频结构化正式效果路径。

## 12.5 为了格式完整发明未来 Host

错误：把“需要某能力”写成“Host 已支持”。

正确：进入 Host Requirement，并写明当前支持状态 `待验证`。

## 12.6 把 Creator 要求写进资产运行正文

Creator Editor、Preview、Validator、错误定位等进入 Creator 创作性 / 平台需求，不冒充世界规则。

## 12.7 丢弃长篇语义稿

格式转换不是摘要任务。若无法安全归类，必须保留到 `未映射内容`。

---

# 13. 输出合同

完成转换后，返回：

1. 生成了哪些 Draft；
2. 每份 Draft 的资产 ID、类型与版本；
3. 新建了哪些候选 ID；
4. 哪些字段来自事实；
5. 哪些字段标记待确认；
6. 哪些内容进入未映射内容；
7. 跨资产引用状态；
8. 通过了哪些 Quality Gate；
9. 当前格式是否可以被无 AI Parser 确定读取；
10. 未来 asset-spec / Creator 仍需决定什么。

如果用户要求文件，应生成：

```text
SKILL.md
FORMAT_REFERENCE.md
SOURCE_MAPPING.md
templates/
examples/
```

---

# 14. 配套文件

本 Skill 包含：

- `FORMAT_REFERENCE.md`：精确语法、字段类型和错误码；
- `SOURCE_MAPPING.md`：如何从 World / Character / Expansion 语义稿映射；
- `templates/`：四类空白模板；
- `examples/`：基于“汉末三国：天下未定”、曹操与五个 Expansion 的代表性 Draft 片段。

使用时以 `SKILL.md` 为执行规范，以 `FORMAT_REFERENCE.md` 为 Parser 合同候选。

---

# 15. 版本与迁移

## v0.1.0

- 建立人类可读、逐行字段式 Creator Import Draft；
- 支持 World Pack、Character Card、Expansion Pack、Asset Family；
- 建立 Section / Field / List / Multiline / Record 语法；
- 建立稳定 ID 与跨资产 Ref；
- 建立未映射内容兜底；
- 从历史世界、复杂人物、能力、政治、生存、关系、穿越系统等真实资产中抽象通用模板；
- 明确 Draft 不是 asset-spec，也不是 Runtime State；
- 明确未来 Creator Import 不调用 AI，必须确定性解析；
- 建立自动迁移原则：Creator 对已正式发布的 Draft 版本提供显式 Migration，不要求创作者人工重写资产。

---

## DSH 执行适配

在 DeepSeek Harness 下：

- 读取资产素材：用 `read`/`glob`/`grep` 读取源资产稿与既有草稿。
- 生成 Creator 导入草稿：用 `write` 落盘到工作区，字段遵循本 SKILL 契约；不调用 AI 的确定性 Parser 由 tavern-creator 负责解析，DSH 只负责产出人类可读草稿。
- 字段语义不确定时：先 `ask_user_question` 确认，不臆造字段值。
- 配套文件缺失：本部署未随附 FORMAT_REFERENCE.md / SOURCE_MAPPING.md / templates / examples，执行以本 SKILL.md 正文为准，不伪造配套文件。