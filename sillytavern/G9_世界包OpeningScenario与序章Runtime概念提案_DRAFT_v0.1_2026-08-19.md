# G9｜World Pack Opening Scenario / Prologue Runtime 概念提案 DRAFT v0.1

状态：`DISCUSSION DRAFT / NOT FROZEN / NOT IMPLEMENTATION AUTHORITY`
日期：2026-08-19
性质：Project Owner gameplay reflection / product-architecture exploration

> 本文件只记录讨论方向，不构成 current architecture decision，不修改正在执行的 G9-02A，不授权 Codex/Grok 实现。只有后续多轮讨论形成正式裁定后，才传播到 G9-02B/C、G9-03 与资产线。

## 1. 问题来源

Project Owner 在真实游戏过程中发现：AI-assisted Creation 后直接进入开放世界时，玩家可能对新世界缺少认知、情绪抓手与继续探索动力。

因此提出：World Pack 可否提供作者精心设计的一个或多个专属【Opening Scenario / Prologue】，用于：

1. 快速建立世界体验与情绪；
2. 给玩家一个明确但不等同于长期 Objective / Mainline 的短期开场抓手；
3. 可选地承担部分教程职责；
4. 在完成后自然移交给开放式 Model-driven Runtime。

## 2. 当前事实

当前 G8 Runtime 只支持：

```text
Creation semantic materialization
→ Structured Opening Beat
→ authority-safe Opening Narrative
→ persisted Turn-0 prose
→ normal free Runtime
```

当前没有：
- multi-beat Scenario state machine；
- script/prologue authority；
- authored beat progression；
- scenario-specific deviation / abort policy；
- tutorial capability composition；
- explicit scripted-mode → sandbox-mode transition。

因此如果采用该产品方向，需要额外 Runtime isolation，而不能只给 Narrative Model 一段“必须照着演”的 Prompt。

## 3. 与现有 World Pack 语义的兼容

汉末三国 World Pack 当前已经冻结：

```text
Opening = time anchor + player identity + reasonable Region/Place
```

并明确禁止所有玩家被强迫进入同一个“名场面”。

因此未来若加入 Opening Scenario，必须：
- 多 Scenario；
- 有 eligibility / compatibility；
- 与玩家身份、时间、地点合理匹配；
- 不把未来历史主线事件当必经剧本；
- 不因 Scenario 强迫历史回到固定轨迹。

## 4. 初步推荐模型

### 4.1 Source Ownership

概念上由 World Pack 拥有或引用一个 `Opening Scenario Library`。

暂不冻结它是：
- World Pack 内嵌 section；
- World Pack child asset；
- 独立 scenario asset type。

该选择必须等 Runtime vertical 证明后再进入 G9-03 schema。

### 4.2 Scenario 固定什么

建议固定：
- scenario identity；
- opening situation；
- key beats / beat graph；
- mandatory world facts；
- authored hook；
- tutorial intents（如有）；
- allowed persistent consequence scope；
- completion / abort / sandbox transition rules。

不一定固定：
- 每一句 Narrative prose；
- NPC 所有对白；
- 玩家必须输入什么；
- 所有局部装饰性细节。

### 4.3 AI / Program 分工

```text
Author-authored Scenario Definition
↓ bind to Game-local Scenario Instance
Program owns current beat / transition / constraints
↓
AI interprets player input + realizes current beat
↓
Program validates / commits allowed consequence
↓
next beat OR abort/exit to sandbox
```

`Scenario Authority != Player Action Whitelist`。

玩家仍可自由尝试；如果行为破坏 Scenario 前提，应有明确 alternate / abort-to-sandbox，而不是强迫玩家回轨。

## 5. Player Identity Compatibility

不要求“一个 Scenario 适用于任何身份”。

更可靠的是：每个 Scenario 声明 hard/soft eligibility，例如：
- time window / anchor；
- region / place archetype；
- social-role family；
- historical-character compatibility；
- required/forbidden world facts；
- player-status constraints。

Program 先过滤 hard compatibility；若多个都可用，可由玩家选择或由 AI 做 semantic fit selection。

如果无 Scenario 合法匹配，必须能够回退到当前 dynamic Opening Beat，而不是强套错误剧本。

## 6. Mainline / Trend Neutrality

Opening Scenario 的默认设计原则建议为：

> `local hook / onboarding ≠ mainline trend mutation`

Scenario 不应以赤壁、官渡、政权覆灭、重大历史人物命运等高影响事件作为默认必经开场。

建议优先：
- local NPC；
- ordinary social/world conflict；
- small mystery；
- local journey / request；
- identity-compatible public incident；
- low-impact exploration hook。

Scenario 内可产生真实 Game-local NPC / Item / Knowledge / Relationship 等持久事实，但默认不主动修改宏观 Politics / War / History trend。

如果玩家主动做出足以破坏该边界的极端行为，应由正常 Runtime authority 接管，必要时提前终止 Scenario，而不是忽略真实后果。

## 7. Tutorial Composition 是最大未决问题之一

不能使用：

```text
World Pack Scenario
→ 默认演示所有可能 Expansion
```

因为：

```text
Package Included
!= Feature Enabled
!= Module Enabled
```

初步候选方向：

```text
Base Opening Scenario
+ optional Tutorial Inserts / Hooks
```

只有对应 Feature / Module enabled 且 Host/Runtime capability 可用时，才组合对应教学节点。

可能模型包括：
1. World Pack 自己 author optional beats，并声明 capability requirement；
2. Expansion 提供 generic tutorial contribution，World Pack 提供 scenario slot/binding；
3. 两层混合：World Pack owns scenario，Expansion contributes capability-specific tutorial unit。

该问题需要后续专门讨论，当前不冻结。

## 8. Scripted Prologue 与 Normal Runtime 隔离

当前本体没有该模式。

若实现，建议至少区分：

```text
PROLOGUE_SCENARIO_MODE
↓ explicit transition
NORMAL_SANDBOX_MODE
```

Prologue Mode 至少需要：
- scenario instance identity；
- current beat / branch；
- authored invariant refs；
- allowed/forbidden mutation scopes；
- player open-attempt handling；
- save / restore / recovery；
- tutorial completion evidence；
- deterministic transition to sandbox。

Model 不得自行宣布 Scenario beat 已完成或任意新增主线；Program/Scenario Host 拥有 progression commit。

## 9. 与 G9 的潜在关系

### G9-02A

`NO CHANGE / DO NOT INTERRUPT`。

当前 Source Binding + Game-local Revision 反而是未来把 Source Opening Scenario bind 成 Game-local Scenario Instance 的基础。

### G9-02B

如果后续正式采用该方向，必须评估是否需要：
- built-in Scenario Runtime Owner / Domain Module；
- Game-local Scenario record；
- typed scenario beat / transition / event seam；
- expansion tutorial contribution seam。

因此建议：**在 G9-02B 正式 Task Freeze 前完成本议题核心裁定。**

### G9-02C

需要保证：Scenario context bounded；只加载 current beat / required entities / enabled tutorial modules，不加载全 Scenario Library。

### G9-03

只有 Runtime vertical 证明后才决定 Opening Scenario 的 external machine representation。

### G9-05 Creator

未来 Creator 可让 World Pack 作者：
- 创建多个 Opening Scenario；
- 定义 eligibility；
- 编辑 beat graph；
- 绑定 optional tutorial capabilities；
- preview/test scenario；
- 验证 identity / expansion compatibility。

## 10. 后续建议讨论顺序

1. Product semantics：Opening Scenario 是不是每个 World Pack 必须/可选？谁选择？
2. Player agency：Scenario 多“固定”？偏离时继续、分支还是退出？
3. Identity compatibility：如何让国王/平民/历史人物都不违和？
4. Tutorial composition：Expansion optional 时怎么模块化演示？
5. Persistence / consequences：哪些变化允许带入正式沙盒？
6. Runtime isolation：Scenario Host / state machine 最小能力。
7. Asset ownership：World Pack embedded vs child asset vs new asset type。
8. 等 Runtime proof 后再进入 G9-03 external schema。

## 11. 当前非裁定

本 Draft 不冻结：
- Opening Scenario 必须存在；
- 固定 beat 数量；
- 是否默认启用；
- Scenario selection UI；
- tutorial taxonomy；
- exact runtime state machine；
- asset-spec fields；
- Creator UI；
- 三国具体开场内容。
