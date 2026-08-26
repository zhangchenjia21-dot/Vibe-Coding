---
title: my world｜项目启动总纲
status: current-canonical-product-spec
version: 1.8
created: 2026-08-25
updated: 2026-08-26
product_definition_gate: PASS
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
local_project_dir: D:\AI\Projects\my-world
---

# my world｜项目启动总纲 CURRENT

## 0. 文档职责

本文件只拥有 `my world` 的**产品定义**：为什么做、给谁做、核心价值、核心体验、范围与成功标准。

它不再重复维护：

- 当前 Task / PASS / UAT：看 `MY_WORLD_CURRENT_STATUS.md`；
- 系统架构与专题设计：看 `MY_WORLD_架构_CURRENT.md`；
- 跨阶段设计哲学：看 `MY_WORLD_核心设计原则_CURRENT.md`；
- 阶段与 Task DAG：看 `MY_WORLD_总体规划路线图_CURRENT.md`。

正式起点：

> **迁移 SillyTavern / The World / DSH 已验证的产品经验，不迁移宿主债务。**

---

## 1. Primary Purpose / Job To Be Done

> **让单个玩家通过自然语言，与优秀 AI GM 在一个长期持续、可保存、可恢复、会自主演化的 2D RPG 世界中长期游玩。**

玩家主要不是操作传统地图角色移动，也不是阅读预写分支剧情，而是：

```text
阅读 GM 叙事
↓
用自然语言决定行动
↓
世界与人物按自身因果回应
↓
状态、关系、地点、历史与后果长期保留
↓
继续形成属于本局的历史
```

---

## 2. Core Value

相比“直接打开通用模型让它陪我玩 RPG”，`my world` 必须提供：

- 长期持续的世界，而不是一次聊天的临时设定；
- 高自由度自然语言行动，而不是封闭选项树；
- 高质量 AI GM Narrative 与临场创造力；
- 自主 NPC / Faction，而不是只等待玩家触发的角色；
- 世界事实、知识、关系与后果可以长期存在；
- 原生可靠的 Save / Restore / Recovery；
- 角色立绘、场景图、地图、人物与机制 UI 等真正 RPG 产品体验；
- 可安装、可创作的 World Pack / Mod；
- local-first、single-player-first，不依赖服务器才能开始玩。

核心价值：

> **长期持续 AI 世界 + 优秀自由 AI GM + 原生 RPG 游戏体验。**

---

## 3. Product Form

### 3.1 视觉 / 交互

第一代是：

> **2D 对话式 RPG / 互动小说。**

主体验：

```text
AI GM Narrative
+
玩家自然语言输入
```

逐步加入：角色立绘、场景图、世界/区域地图、角色/关系/任务/势力/物品/机制等 RPG Surface、适度动画/音效/转场。

不以自由移动 3D 世界为目标。

### 3.2 运行形态

- 本地优先；
- 单人优先，预计长期保持单人；
- 第一代不建设服务器依赖、账户系统和多人同步；
- 模型先通过本地游戏调用远程 Provider API；
- Local Model 是未来扩展，不是第一代 blocker。

### 3.3 内容形态

World Pack / Mod 是一级产品能力。

不同世界可以拥有自己的：

- 世界规则 / Source Lore；
- 人物；
- 地图；
- 立绘 / 场景；
- 机制组合；
- 世界专属表现。

统一原则：

> **Source 定义开始前的参考世界；游戏开始后，game-local reality 优先。**

---

## 4. Core Experience / Core Loop

```text
选择 / 安装 World Pack
↓
创建 Game / Player Character
↓
进入世界
↓
Runtime 恢复当前世界与必要 Context
↓
AI GM Narrative / 玩家自然语言行动
↓
世界、NPC、Faction 与机制产生发展
↓
需要长期存在的变化 durable
↓
UI 投影当前游戏真相
↓
Save / 离开 / 以后继续同一世界
```

长期同时存在：

```text
World Loop
局势 → 事件 → 后果 → 时间推进

Life Loop
自由活动 → 日常 → 人物互动 → 关系 / 人格积累
```

节奏原则：

> **Compress dead time; stop at meaningful choice.**

---

## 5. Non-negotiable Product Principles

### Model freedom

> **Model freedom first. Reversibility over prevention.**

开放式 AI RPG 不以消灭所有模型错误为目标。普通可逆错误优先通过 Context、Cancel、Regenerate/Retry、明确 Save/Restore 等恢复，而不是层层增加 Narrative whitelist、Regex、Confirmation 与 Validator。

### Player / World / GM

```text
Player owns Attempt
World owns Consequence
GM owns Playability of the Consequence
```

玩家可以尝试任何行动，但世界不保证成功。

### World agency

> **Source provides inertia; actors create history.**
>
> **Off-screen != Inactive.**

玩家不是唯一历史创造者；重要 NPC / Faction 有自己的目标、风险、义务和 next move。

### Knowledge

```text
World Truth != NPC Knowledge != Player Knowledge
```

知识边界是世界可信度目标，但不能无限扩张成 Narrative 审查系统。

### UI truth

> **UI is a projection of game truth, not a second truth source.**

### Reversibility

> **Player owns the timeline.**
>
> **Reversibility != frictionless arbitrary rewind.**
>
> **Save Point != Timeline Node.**

局部生成错误应低成本纠正；重大历史恢复要表达明确意图。内部 Timeline 不自动成为每个历史 Turn 的公开回档按钮。

---

## 6. Foundation Strategy

> **Commodity Foundation, Owned Game Semantics.**
>
> **Engine-native, not engine-semantic-coupled.**

成熟 Engine/Foundation 负责窗口、渲染、2D、UI、输入、字体、图片、音频、动画、资源与平台打包等通用能力。

`my world` 自己拥有：

- Game；
- World；
- Timeline；
- Save Point；
- Conversation / Turn；
- Agent Context；
- NPC / Knowledge / Relationship / Faction；
- Thread / Quest / Mechanic State / World Event；
- World Pack / Mod。

当前第一代技术结论与详细导航统一见：

`MY_WORLD_架构_CURRENT.md`

---

## 7. Product UI Direction

长期桌面主界面：

```text
Player Host | Narrative Host | World Surface Host
左主角信息  | 中央叙事与输入 | 右世界信息
```

Narrative 永远是视觉与交互重心。

宽窗口三栏；窄窗口保持中央可读，侧栏折叠/隐藏/overlay。

声明式 UI Host 是长期重要能力，但顺序必须是：

```text
真实固定 UI
→ stable Host Slots
→ real Domain projections
→ Internal Declarative Host
→ external World Pack / Mod UI contract
```

不能为了 Mod 理论未来提前建立通用 UI 平台。

---

## 8. Persistence / Long-term World Product Requirement

必须原生区分：

```text
Game
World State
Timeline
Save Point
Conversation
Agent Context
UI Preference
```

关键产品要求：

- durable world mutation 不依赖周期模型 consolidation 才收敛；
- 关闭 / 重开仍是同一世界；
- 玩家可以明确 Save / Load；
- Restore 同时恢复世界和相符 Context；
- 被回滚未来不能泄漏给 AI；
- Load 旧 Save 不应轻易不可逆销毁刚才的 current future；
- arbitrary per-turn rewind 当前不是默认第一代功能。

详细设计由 `MY_WORLD_架构_CURRENT.md` 导航。

---

## 9. Context Product Requirement

世界和历史可以长期增长，但 ordinary Turn 的 model working set 必须保持有界：

```text
System Total State
!= Runtime Relevant Set
!= Model Visible Working Set
```

Transcript 不是 World DB；不能通过不断扩大 Prompt 来维持长期世界。

---

## 10. Explicit Non-scope Before Core Vertical Is Proven

默认不做：

- Multiplayer / server backend / cloud account / cloud save；
- 3D 自由移动；
- 自动地图生成；
- 全世界逐 NPC tick 模拟；
- Universal ECS / 大型 Event Bus；
- 通用巨大 Schema / Protocol；
- 复杂脚本沙箱 / Steam Workshop；
- Local LLM Hosting；
- TTS / STT；
- 自动 Provider routing / fallback mesh / marketplace；
- 为未来理论需求提前建设大量扩展点。

---

## 11. Simple Baseline

当前最重要比较基线：

> **The World / DSH + 同类优秀模型。**

独立版可以在工程能力上逐步建设，但核心体验不能因为“架构更正确”而明显退化：

- GM Narrative 质量；
- 自然语言自由度；
- 长期世界感；
- NPC 质感；
- 沉浸感；
- 操作税。

如果复杂系统在核心价值上明显差于简单基线，必须重开产品/架构分析，不能用测试数量或文档完整度宣布成功。

---

## 12. Product Success / Acceptance

产品最终成功不是“所有模块都实现”，而是玩家真实感受到：

1. AI GM 值得长期互动；
2. 世界会自己活着，不只围绕主角刷新；
3. 自然语言行为自由；
4. 世界变化长期可靠；
5. Save / Restore 可信；
6. UI 让世界更可理解，而不是变成工程工具；
7. 长局以后仍然保持可玩、可恢复、可理解；
8. World Pack / Mod 可以扩展内容而不污染核心 Runtime。

Product-facing Task 的 Engineering PASS 不能替代 Owner UAT。

---

## 13. Open Questions

当前继续由后续真实阶段证据回答：

- G3 authoritative persistence 的正式 binding / Schema / migration / recovery；
- Conversation / Turn 与 Agent Context 的具体数据模型；
- World Pack manifest 与 Source → game-local materialization；
- G5 bounded event/priority-driven autonomous world evolution；
- G6 Internal Declarative UI Host vocabulary；
- G7 bounded context 与长局性能阈值；
- G8 外部 Mod UI / authoring protocol；
- 何种真实需求足以增加第二个正式 product-facing Provider。

这些问题不得在当前阶段无证据预建。
