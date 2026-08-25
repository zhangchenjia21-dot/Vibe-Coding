---
title: my world｜项目启动总纲
status: current-canonical-product-spec
version: 1.0
created: 2026-08-25
updated: 2026-08-25
stage: Stage 0 Complete / Standalone Preflight
product_definition_gate: PASS
next: Foundation Spike
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
local_project_dir: D:\AI\Projects\my world
engine_candidate: Godot
engine_local_dir: D:\AI\Engine
---

# my world｜项目启动总纲 CURRENT

## 0. 项目起点

`my world` 是在 The World / DeepSeek Harness 长期真实游玩已经验证核心 AI RPG 体验之后启动的独立游戏项目。

它不是把 `the-world` 从 DSH 搬到 Godot，也不是重写一份 DSH。

正式原则：

> **迁移经验，不迁移宿主债务。**

The World / DSH 继续作为：

- 产品实验场；
- 真实试玩证据库；
- 失败方案与宿主债务记录；
- `my world` 的前代参考实现。

`my world` 从独立游戏 Host 的语义重新设计世界状态、时间线、Save、Agent Context、UI 与 World Pack。

---

## 1. Primary Purpose / Job To Be Done

> **让单个玩家通过自然语言，与优秀 AI GM 在一个长期持续、可保存、可恢复、会自主演化的 2D RPG 世界中长期游玩。**

玩家主要不是操作传统地图角色移动，也不是阅读预写分支剧情，而是在一个类似高质量互动小说 / 对话式 RPG 的界面里：

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

玩家选择 `my world`，而不是直接打开一个通用聊天模型说“陪我玩 RPG”，应得到：

- 长期持续的世界，而不是一次聊天里的临时设定；
- 高自由度自然语言行动，而不是封闭选项树；
- AI GM 的高质量叙事与临场创造力；
- 自主 NPC 与势力，而不是等待玩家点击的功能角色；
- 可信的知识边界、失败后果与历史分歧；
- 原生 Save / Restore / Timeline；
- 角色立绘、场景图、地图、人物与机制 UI 等真正游戏化体验；
- 可安装和创作的 World Pack / Mod；
- 本地优先、长期单人、无需服务器才能开始玩的产品形态。

核心价值概括：

> **长期持续 AI 世界 + 优秀自由 AI GM + 原生 RPG 游戏体验。**

---

## 3. 产品形态

### 3.1 视觉形态

当前已定：

> **2D 对话式 RPG / 互动小说体验。**

主阅读流仍然是：

- GM 生成文本；
- 玩家自然语言输入。

逐步加入的美术与游戏 Surface：

- 角色立绘；
- 场景图；
- 世界 / 区域地图；
- 角色、关系、任务、系统、势力、物品等 RPG UI；
- 适度动画、音效与转场。

当前不以自由移动 3D 世界为产品目标。

### 3.2 运行形态

当前已定：

- **本地优先**；
- **单人优先，且预计长期保持单人**；
- 第一阶段不建设服务器依赖；
- 第一阶段不建设多人同步；
- 模型可先通过本地游戏连接远程 Provider API；
- Local Model 属于未来可扩展能力，不是第一阶段 blocker。

### 3.3 内容形态

World Pack / Mod 是一级产品能力，而不是后期附加项。

未来不同世界可以拥有各自的：

- 世界规则与背景；
- Source Lore；
- 人物；
- 地图；
- 角色立绘；
- 场景图；
- 可选机制；
- 世界专属表现。

但统一遵循：

> **Source 定义开始前的参考世界；游戏开始后，game-local reality 优先。**

---

## 4. Core Experience / Core Loop

```text
选择 / 安装 World Pack
↓
创建游戏与角色
↓
进入世界
↓
AI GM 恢复当前世界与上下文
↓
叙事 / 玩家自然语言行动
↓
世界裁定、NPC 行动、机制结果
↓
durable world mutation
↓
玩家 UI 即时投影当前世界真相
↓
Save / 时间推进 / 离开
↓
以后重新进入同一个世界继续
```

长期循环中同时保留：

```text
World Loop
局势 → 事件 → 后果 → 时间推进

Life Loop
自由活动 → 日常 → 人物互动 → 关系 / 人格积累
```

正式节奏原则继续继承：

> **Compress dead time; stop at meaningful choice.**
>
> **压缩无意义时间，停在有意义选择。**

---

## 5. 继承自 The World 的已验证产品原则

### 5.1 Freedom Before Prevention

> **Freedom Before Prevention.**
>
> **Prefer recovery over prevention.**

玩家可以尝试任何行动。世界负责后果，GM 负责让后果仍然可玩。

### 5.2 Player / World / GM 权责

```text
Player owns Attempt
World owns Consequence
GM owns Playability of the Consequence
```

### 5.3 Knowledge Boundary

> **GM / Source / System knows X != NPC knows X.**

人物知识必须来自世界内合法来源。

### 5.4 NPC Agency

> **NPC 不是等待玩家交互的回应面，而是即使玩家不理他也会继续行动的人。**

重要 NPC 应具有自己的：

- Current Agenda；
- Fear / Cost；
- Red Line；
- Obligation；
- Independent Next Move。

### 5.5 Meaningful Choice Risk Structure

> **有意义的选择不仅结果方向不同，风险结构也应不同。**

不同方案可以在以下维度真实不同：

- 可行性；
- 固有难度；
- 优势 / 劣势；
- 失败代价；
- 暴露、资源、关系与长期后果。

同时：

> **Dice decides uncertainty. Dice does not erase character.**

### 5.6 World Truth / UI Projection

> **UI is a projection of game truth, not a second truth source.**

但独立版不再要求把 game truth 组织成 DSH Workspace Markdown Owner 树；实现方式重新设计。

---

## 6. Foundation Strategy

### 6.1 成熟基底优先

正式原则：

> **Commodity Foundation, Owned Game Semantics.**
>
> **通用基底尽量复用，游戏核心语义必须掌握在自己手里。**

优先复用经过长期大众验证的成熟能力，例如：

- Window / Rendering；
- 2D；
- UI；
- Input；
- Audio；
- Font / Text Rendering；
- Animation；
- Asset Pipeline；
- Platform Packaging；
- Profiler；
- 其它不构成产品差异的成熟基础设施。

除非现成方案实质不适配，否则不从零重写这些能力。

### 6.2 游戏核心语义自有

`my world` 自己拥有并定义：

- Game；
- World；
- Timeline；
- Save Point；
- Agent Context；
- Conversation；
- NPC；
- Knowledge；
- Relationship；
- Faction；
- Thread / Quest；
- Mechanic State；
- World Event；
- Player Turn / AI Turn；
- World Pack / Mod。

这些概念不能仅因为使用某个引擎就被等同为该引擎的 Scene / Node / Resource 等内部对象。

正式原则：

> **Engine-native, not engine-semantic-coupled.**

---

## 7. Engine / Foundation 当前裁定

当前不是最终技术选型，而是研究顺序：

1. **Godot：第一研究候选**；
2. **Unity：主要比较候选**；
3. **成熟 2D Desktop App Foundation：对照组**；
4. **Unreal：除非产品明显转向重 3D，否则低优先级**。

由于当前产品明确是：

- 2D；
- 长文本；
- RPG UI；
- 立绘 / 场景图 / 地图；
- 本地优先；
- 单人；
- World Pack / Mod；

Godot 当前拥有最高的第一轮验证优先级。

但最终选择必须经过真实 Foundation Spike，而不是凭偏好冻结。

当前本地 Godot 路径：

`D:\AI\Engine`

当前 Godot 版本尚未在项目事实中登记，Preflight 时补录。

---

## 8. 初步系统分层

目标架构不是“所有东西都塞进 Godot Scene Tree”，而是：

```text
my world
│
├─ RPG Experience Layer
│  ├─ Narrative
│  ├─ Character Portrait
│  ├─ Scene Art
│  ├─ Map
│  └─ RPG UI
│
├─ The World Runtime
│  ├─ World State
│  ├─ Timeline / Save
│  ├─ NPC Agency
│  ├─ Knowledge
│  ├─ Relationships
│  ├─ Mechanics
│  ├─ Context Assembly
│  ├─ Agent Orchestration
│  └─ World Pack / Mod
│
├─ Engine Adapter
│
└─ Mature Game Foundation
   └─ Godot / other selected host
```

是否让 Runtime 与 Godot 同进程，或拆成 Godot Client + Local Runtime Process，当前**未冻结**，交由 Foundation Spike 决定。

---

## 9. Persistence / Timeline 原则

DSH 已经证明两件事必须在独立版从第一天成为一等概念：

### 9.1 World State 不再依赖周期性 consolidation 才收敛

目标：

```text
世界发生 durable mutation
↓
一次可靠提交
↓
权威世界状态即时更新
↓
UI / Agent Context 读取同一真相
```

独立版不继承“每若干回合让模型批量 edit Markdown Owner”的运行方式。

具体存储技术当前未冻结。

### 9.2 Timeline 与 Conversation 分离

必须明确区分：

- Game；
- Timeline；
- Save Point；
- World State；
- Agent Context；
- Conversation History。

Restore 必须恢复世界，并重建与该时间点一致的 Agent Context。

不能再次出现：

> 世界已经回档，但聊天历史仍记得未来。

---

## 10. Model / Provider 原则

第一阶段 Provider 抽象保持极薄。

概念能力只需要覆盖：

```text
send
stream
cancel
```

第一阶段先把一个实际使用的优秀模型接通并跑出 RPG 体验，不预造：

- 十几个 Provider；
- 自动模型路由；
- 大型 fallback mesh；
- 通用 AI 平台。

AI Runtime 必须支持流式文本，且后台模型 / 世界维护不能冻结玩家 UI。

---

## 11. 第一阶段明确 Non-scope

在首个真实 Vertical Slice 通过前，默认不做：

- Multiplayer；
- Server backend；
- Cloud account；
- Cloud save；
- 3D 自由移动世界；
- Automatic map generation；
- 全世界逐 tick 模拟；
- Universal ECS；
- 大型通用 Schema / Protocol；
- 复杂脚本沙箱；
- Steam Workshop；
- Local LLM Hosting；
- TTS / STT；
- 程序化角色美术；
- 为未来理论需求提前建设的大型基础设施。

---

## 12. Simple Baseline / Reference Baseline

当前最重要基线不是别的商业游戏，而是：

> **The World on DSH + 同一优秀模型。**

独立版第一阶段不要求所有工程能力都超过 DSH，但在核心玩家体验上不能明显退步：

- GM 文本质量；
- 自由度；
- 长期世界感；
- NPC 质感；
- 玩家自然语言交互；
- 沉浸感。

如果独立版架构更“正确”，但实际 RPG 明显更不好玩，则不能判成功。

---

## 13. Success / Acceptance

### Product Definition Gate

当前裁定：**PASS**。

理由：

- Primary Purpose 明确；
- 核心用户与核心体验明确；
- 2D / 本地 / 单人 / Mod 方向明确；
- Must-have 与 Non-scope 已可区分；
- 核心游戏语义从 DSH 实玩中已有大量证据；
- 剩余问题主要属于技术 Foundation / Architecture Spike，不再改变产品定义。

### 下一 Gate

> **Foundation Spike Gate**

必须证明候选 Host 能自然支撑：

- 中文长文本；
- 玩家输入；
- 流式模型输出；
- 后台任务不冻结 UI；
- 本地 persistence；
- 图片 / 立绘 / 场景加载；
- Windows 打包；
- 与独立 Runtime 边界的可行集成。

通过后再进入首个真实 Vertical Slice。

---

## 14. Open Questions

以下问题当前明确为**架构 / 技术问题，不是产品定义 blocker**：

- Godot 具体版本；
- GDScript、C# 或混合策略；
- The World Runtime 是否独立进程；
- 第一阶段 persistence 技术；
- Agent Context 的具体数据模型；
- World Pack manifest 的最小格式；
- Mod 是否允许可执行脚本，以及何时引入沙箱；
- 第一批 Provider 接入方式；
- 第一阶段测试 / packaging harness。

这些问题以 Spike 和真实 Vertical 证据裁定，不凭空提前冻结。

---

## 15. Decision Ledger

- **MW-DEC-01** 独立项目名 = `my world`。
- **MW-DEC-02** 本地项目目录 = `D:\AI\Projects\my world`。
- **MW-DEC-03** 实现仓库 = `zhangchenjia21-dot/my-world`。
- **MW-DEC-04** 产品核心形态 = 2D 对话式 RPG / 互动小说，而非 3D 自由移动 RPG。
- **MW-DEC-05** 角色立绘、场景图、地图与 RPG UI 是未来正式游戏体验的一部分。
- **MW-DEC-06** Local-first；服务器不是第一阶段前置条件。
- **MW-DEC-07** Single-player first，且长期预计保持单人。
- **MW-DEC-08** World Pack / Mod 是一级产品能力。
- **MW-DEC-09** 成熟 Foundation 优先，不从零制造通用游戏外壳。
- **MW-DEC-10** Commodity Foundation, Owned Game Semantics。
- **MW-DEC-11** Engine-native, not engine-semantic-coupled。
- **MW-DEC-12** Godot 是 Foundation Spike 的第一研究候选，但不是未经验证的最终锁定技术栈。
- **MW-DEC-13** The World / DSH 是产品与经验参考实现，不是代码迁移模板。
- **MW-DEC-14** 独立版必须重新设计 Persistence / Timeline / Agent Context，不继承 DSH consolidation 与 Session workaround。
- **MW-DEC-15** 第一阶段 Provider 抽象保持薄，只优先跑通一个真实优秀模型。
- **MW-DEC-16** 核心玩家体验优先于工程完整度；复杂系统必须证明自己没有让 RPG 比简单基线更差。
- **MW-DEC-17** 正式大规模实现前先通过 Foundation Spike Gate。

---

## 16. Current Decision

当前状态：

```text
Product Definition Gate       PASS
DSH Experience Extraction     READY
Implementation Repository     EMPTY / NOT INITIALIZED
Godot Foundation Candidate    READY FOR SPIKE
First Vertical Slice          NOT STARTED
```

**当前下一步不是直接开发完整游戏功能，而是执行 Standalone Preflight，并完成 Godot Foundation Spike。**

对应执行计划：

`MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`
