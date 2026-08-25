---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 1.1
created: 2026-08-25
updated: 2026-08-25
current_phase: G1
next_task: G1-01
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
local_project_dir: D:\AI\Projects\my-world
engine: Godot v4.7.2
engine_local_dir: D:\AI\Engine
---

# my world｜总体规划路线图 CURRENT

## 0. 路线图定位

这份文件是 `my world` 的唯一 current 总体开发路线图。

它回答：

- 当前项目处于哪个阶段；
- G1–G9 每个阶段要证明什么；
- 哪些能力必须先做，哪些必须延后；
- 每一阶段怎样编号任务；
- 什么证据允许进入下一阶段。

产品定义仍以 `MY_WORLD_项目启动总纲_CURRENT.md` 为最高项目产品事实源；本文件负责把该产品定义转换成阶段路线与 Gate。

正式原则：

> **迁移经验，不迁移宿主债务。**

> **Commodity Foundation, Owned Game Semantics.**

> **Engine-native, not engine-semantic-coupled.**

> **先跑通真实核心循环，再扩展外围能力。**

---

## 1. 当前已冻结的项目约束

### 1.1 产品形态

- 2D 对话式 AI RPG / 互动小说；
- 主阅读流是 AI GM 文本 + 玩家自然语言输入；
- 后续加入角色立绘、场景图、地图、RPG 状态 UI；
- 不以自由移动 3D 世界为目标。

### 1.2 运行形态

- 本地优先；
- 单人优先，预计长期保持单人；
- 第一阶段不依赖服务器；
- 第一阶段先接远程 Model Provider API；
- Local Model Hosting 延后。

### 1.3 内容形态

- World Pack / Mod 是一级能力；
- Source 负责开局前的可复用世界定义；
- 游戏开始后 `game-local reality > source default`；
- Source 更新不能静默改写已经发生的本局历史。

### 1.4 Foundation

当前第一研究 Host：

> **Godot v4.7.2**

本地 Godot 路径：`D:\AI\Engine`

本地项目路径：`D:\AI\Projects\my-world`

当前实现仓库：`https://github.com/zhangchenjia21-dot/my-world`

实现仓库已完成 G1-01 最小 bootstrap，并建立：

```text
README.md
AGENTS.md
.gitignore
project.godot
src/main.tscn
```

已验证 Windows 本地工具链：

```text
Godot 4.7.2.stable.official.ed1daf0bf
Standard / non-.NET Windows x64
Git 2.54.0.windows.1
OS Architecture X64
```

当前 G1-01 runtime verification 被本地文件写权限 blocker 阻塞：Git 无法写 `.git/FETCH_HEAD`，Godot 无法创建 `user://logs` / `user://vulkan` / shader cache。该 blocker 解决前不进入 G1-02。

Godot 是当前 Foundation 候选，不是游戏语义 Owner。`Game / World / Timeline / Save / NPC / Knowledge / Agent Context / World Pack` 等核心概念由 `my world` 自己定义。

---

# 2. Task 编码规则

阶段编号固定为：

```text
G1 → G2 → G3 → G4 → G5 → G6 → G7 → G8 → G9
```

阶段内任务统一使用：

```text
G<阶段>-<两位序号>
```

例如：

```text
G1-01  初始化实现仓库
G1-02  Godot Foundation 最小工程
G3-04  Restore / Timeline Context 重建
```

任务类型不塞进编号，单独记录：

```text
Type: exploration | implementation | review | UAT-support | migration | docs
```

阶段 Gate 使用：

```text
G1-GATE
G2-GATE
...
G9-GATE
```

若出现返修，不新造并行阶段编号；继续使用该阶段下一个顺序号，例如 `G3-07`。

规则：

1. 一个 Task ID 默认只有一个主要 Outcome；
2. 不允许把多个阶段的大任务混成一个 Task；
3. 下游阶段大规模实现默认必须等待上游 Gate PASS；
4. 允许低成本 exploration 提前验证，但必须标记 `NOT canonical architecture / NOT production commitment`；
5. 产品面 Gate 必须区分 Engineering Acceptance 与 Product Owner / 真人体验验收。

---

# 3. 总体关键路径

```text
G1  Foundation & Project Bootstrap
↓
G2  AI Conversation Spine
↓
G3  Persistent Game & Timeline
↓
G4  World Pack & Local Content Foundation
↓
G5  World Semantics & GM Runtime
↓
G6  RPG Experience & 2D Presentation
↓
G7  Long-session Context & Performance
↓
G8  Mod / Authoring Ecosystem
↓
G9  Standalone Alpha / Release Validation
```

整个项目的第一条真正脊柱是：

```text
启动游戏
→ 进入 World Pack
→ 和 AI GM 对话
→ 世界产生 durable change
→ 退出
→ 重新进入同一世界
→ Save
→ 继续产生未来
→ Restore
→ AI 不知道被回滚的未来
```

在这条链稳定前，不以功能数量作为项目进度。

---

# G1｜Foundation & Project Bootstrap

## 目标

证明 Godot v4.7.2 能作为 `my world` 的成熟游戏 Host，并冻结第一阶段最小工具链与 Runtime 边界。

这是 Foundation Spike 阶段，不追求产品完成度。

## 主要任务

### G1-01｜实现仓库初始化

建立最小：

- `README.md`；
- `AGENTS.md`；
- Godot 工程；
- 必要源码 / 测试目录；
- `.gitignore`；
- 最小运行说明。

不创建大量空架构目录。

当前实现状态：GitHub-side bootstrap 已完成；Windows-local runtime verification 仍因文件写权限 blocker 未 PASS。

### G1-02｜Godot 4.7.2 工具链与语言确认

确认：

- 安装的是 Standard 还是 .NET 版；
- 可执行文件与 CLI；
- 第一阶段语言候选；
- Windows Export 能力；
- 是否需要外部 local runtime process。

### G1-03｜2D 长文本 / 输入 Foundation Spike

证明：

- 中文字体；
- 长文本滚动；
- 文本持续追加；
- 输入框；
- 复制 / 选择；
- UI 不因长文本明显失控。

### G1-04｜真实 Provider 流式调用 Spike

证明：

- 真正联网调用一个实际 Provider；
- 流式 token/文本进入 UI；
- cancel 可用；
- 网络失败有明确错误；
- 请求期间 UI 不冻结。

Provider abstraction 保持极薄：`send / stream / cancel`。

### G1-05｜本地 IO / 图片 / Windows Export Spike

证明：

- 本地读写；
- 动态加载立绘 / 场景 / 地图类图片；
- 打包后的 Windows 程序仍可工作；
- 不依赖编辑器才能运行核心路径。

### G1-06｜Foundation Architecture Decision

根据 Spike 决定：

- Godot 是否正式成为第一代 Host；
- Standard / .NET；
- GDScript / C# / 混合边界；
- Runtime 与 Godot 同进程，还是 Godot Client + Local Runtime Process；
- 第一阶段本地持久化技术的候选范围。

## G1-GATE｜Foundation Gate

PASS 必须满足：

- Godot 4.7.2 可稳定运行最小 Windows 工程；
- 中文长文本主路径可用；
- 真实模型流式输出与 cancel 可用；
- 后台请求不冻结 UI；
- 本地 IO 与图片加载可用；
- Windows 导出后核心 Spike 仍工作；
- Runtime boundary 与第一阶段语言已有明确裁定；
- 没有发现需要放弃 Godot 的 blocker。

失败时允许重新评估 Unity / 2D Desktop Foundation，不因为已写代码而强行继续 Godot。

---

# G2｜AI Conversation Spine

## 目标

建立第一个真正可玩的、但暂时不依赖完整持久世界的 AI RPG 对话主循环。

用户应该可以：

```text
打开游戏
→ 输入自然语言
→ 看 AI GM 流式叙事
→ 连续玩若干回合
→ cancel / retry / 继续
```

## 主要任务

### G2-01｜Application / Game Shell

建立应用启动、主界面、游戏运行状态与退出生命周期。

### G2-02｜Provider Adapter v0.1

实现单一实际 Provider 的最薄适配与可替换接口。

不提前建设多 Provider 路由平台。

### G2-03｜Narrative Conversation View

完成：

- 玩家消息；
- GM 消息；
- streaming；
- cancel；
- regenerate / retry 的最小语义；
- 错误态；
- 基础阅读体验。

### G2-04｜Turn / Conversation Domain v0.1

明确：

- Player Turn；
- GM Turn；
- Conversation Entry；
- Generation State；
- 哪些属于 UI 状态，哪些属于 Game Domain。

### G2-05｜Context Assembly v0.1

先只处理最小：

- system / GM instructions；
- 当前对话窗口；
- 当前游戏最小上下文。

不做复杂 retrieval / memory engine。

### G2-06｜第一轮 Owner Playtest

用真实模型玩至少一段连续 session，比较 The World / DSH 简单基线：

- 文本质量不能明显更差；
- 输入与输出不能因新壳产生明显操作税；
- UI 响应应明显比长局 DSH 更可控。

## G2-GATE｜Core Conversation Gate

Engineering PASS：

- 连续多回合不崩；
- stream/cancel/retry 状态正确；
- provider failure 可恢复；
- UI 主线程保持响应。

Product Value PASS 由 Product Owner 裁定：

> **作为“AI RPG 对话壳”，是否已经值得继续玩，而不是工程 demo。**

---

# G3｜Persistent Game & Timeline

## 目标

建立独立版最关键的长期世界脊柱：**Game / World State / Timeline / Save Point / Agent Context / Conversation 不再混为一个概念。**

本阶段直接解决 DSH 最重要的宿主债务。

## 主要任务

### G3-01｜Persistence Domain Architecture

先冻结语义，再选技术：

- Game；
- Timeline；
- Save Point；
- World State；
- World Event / Durable Mutation；
- Conversation；
- Agent Context。

优先评估 SQLite 等成熟本地存储，但不在调查前把数据库实现当产品语义。

### G3-02｜Authoritative World Commit Path

目标：

```text
模型 /规则产生候选变化
→ Domain 校验
→ 一次可靠 commit
→ authoritative truth 更新
→ UI / Agent Context 读取同一真相
```

不继承“若干回合后模型批量 consolidation”。

### G3-03｜Game Reopen / Resume

关闭程序后重新打开，同一个游戏应恢复：

- 世界状态；
- 当前 timeline；
- 必要 context；
- 玩家 UI projection。

### G3-04｜Save / Restore / Timeline Context Rebuild

Restore 必须同时做到：

- 世界回到目标时间点；
- Agent Context 与目标时间点一致；
- 被回档掉的未来不能从 Conversation / Context 泄漏回来。

### G3-05｜Crash / Interrupted Write Recovery

使用成熟事务 / 原子写原则解决：

- 中断写；
- 崩溃恢复；
- 不完整 save；
- 损坏检测。

不提前建设企业级分布式恢复体系。

### G3-06｜Persistence Reality Test

真实完成：

```text
游玩
→ durable change
→ 退出
→ 重开
→ 继续
→ save
→ 推进未来
→ restore
→ 继续
```

## G3-GATE｜Persistence & Timeline Gate

PASS 必须满足：

- 退出 / 重开不丢关键世界事实；
- durable mutation 不依赖周期性模型归并才生效；
- Save / Restore 正确；
- Restore 后无未来 context 泄漏；
- Conversation 不再等于 Timeline；
- UI projection 不建立第二真相源。

---

# G4｜World Pack & Local Content Foundation

## 目标

让 `my world` 不硬编码成一个世界，并从早期就证明 World Pack / Mod 是一级能力。

## 主要任务

### G4-01｜World Pack v0.1 最小语义

只定义第一版真正需要的内容，例如：

- metadata / manifest；
- world instructions；
- source lore；
- initial characters；
- authored map；
- portraits / scene assets；
- optional mechanic declarations。

### G4-02｜Source → Game-local Instance

建立：

```text
Reusable World Pack Source
→ new game snapshot / binding
→ Game-local Canonical Reality
```

旧游戏不能因 Source 更新被静默改写。

### G4-03｜Pack Discovery / Install / Load

本地发现、选择、安装 / 载入 World Pack。

第一版不做在线商店。

### G4-04｜Asset Resolution

让人物立绘、场景、地图等内容通过 World Pack 正常解析，而不是写死到核心工程。

### G4-05｜Second Pack Fixture

至少用第二个极小世界包证明核心不是对首个世界硬编码。

## G4-GATE｜Content Portability Gate

PASS 必须满足：

- 至少两个 World Pack 能建立独立新游戏；
- Source 与 game-local reality 分离；
- 旧局不被 Source 更新污染；
- 内容资产不需要改核心代码才能换世界。

---

# G5｜World Semantics & GM Runtime

## 目标

把 The World 已经通过真实试玩验证的 RPG 产品语义，重新落到独立 Runtime，而不是复制 Markdown Prompt 方案。

## 主要任务

### G5-01｜World Turn / GM Orchestration

明确一次玩家行动如何经过：

```text
Player Intent
→ relevant context
→ GM / rule resolution
→ candidate consequences
→ domain commit
→ narrative realization
```

### G5-02｜Knowledge Provenance

正式继承：

> **GM / Source / System knows X != NPC knows X.**

知识边界进入 context assembly / domain semantics，不建理论化万能 ACL。

### G5-03｜NPC Agency

重要 NPC 具备自己的：

- Current Agenda；
- Fear / Cost；
- Red Line；
- Obligation；
- Independent Next Move。

NPC 可以离屏行动，不只是对玩家作答。

### G5-04｜Player Agency / Control Mode

实现：

- Full Control；
- Light Delegation；
- 后续再决定 Narrative Delegation 是否首发需要。

核心是授权边界，而不是每一步都让玩家点菜单。

### G5-05｜Meaningful Choice / Checks

正式继承：

> **有意义的选择不仅叙事方向不同，风险结构也应不同。**

支持：

- 可行 / 不可行 / 无需掷骰；
- DC；
- advantage / disadvantage；
- 不同 failure stakes；
- deterministic RNG boundary。

同时：

> **Dice decides uncertainty. Dice does not erase character.**

### G5-06｜Pacing / Time / World-Led Events

保留：

- World Loop；
- Life Loop；
- Compress dead time；
- stop at meaningful choice。

不建设全世界逐 tick 模拟器。

### G5-07｜NPC / GM Owner Playtest

重点不是测接口，而是人工判断：

- 不同人物是否真的需要不同相处方式；
- NPC 是否会主动行动；
- 失败是否改变局面而非关闭游戏；
- 不同行动方案是否真的有不同风险。

## G5-GATE｜World Semantics Gate

Engineering PASS：关键语义可以持久化并跨重开继续。

Product Value PASS：真实试玩中，GM 自由度、人物质感、选择风险至少不明显差于 The World / DSH 基线。

---

# G6｜RPG Experience & 2D Presentation

## 目标

把已经成立的 AI RPG 核心循环变成真正的 2D 游戏产品，而不是聊天客户端。

## 主要任务

### G6-01｜Narrative UX Polish

阅读层、输入层、streaming、历史、状态提示、响应布局。

### G6-02｜Character Portrait

World Pack 驱动的角色立绘与当前发言 / 场景 projection。

### G6-03｜Scene Art

场景背景图 / 插图，并保持“美术是 presentation，不是第二世界状态源”。

### G6-04｜Authored Map

第一版只做：

- 世界包手工地图；
- 缩放 / 拖动；
- canonical current location marker；
- 无法定位时 fail-safe。

暂不做自动生成地图、GIS、路径规划。

### G6-05｜RPG State Panels

逐步覆盖真正需要的：

- Character；
- Relationship；
- Threads / Quest；
- Inventory / Economy；
- Faction / Reputation；
- Save / Restore；
- Settings。

按真实需求增量实现，不要求一次全部完成。

### G6-06｜Experience UAT

真人连续游玩，判断：

> **它是否已经明显像“游戏”，而不是换皮聊天工具。**

## G6-GATE｜RPG Experience Gate

PASS 关键不是页面数量，而是：

- 美术 / 地图 / 状态 UI materially improve 体验；
- 主叙事流仍然顺畅；
- UI 不建立第二事实源；
- 玩家不承担工程 Workspace / 文件管理心智。

---

# G7｜Long-session Context & Performance

## 目标

正面解决促使我们离开 DSH 的核心问题：**长局不能越玩越卡到无法继续。**

## 主要任务

### G7-01｜Context Budget & Assembly Architecture

区分：

- authoritative truth；
- current relevant state；
- conversation history；
- summaries / retrieval；
- model context working set。

### G7-02｜Incremental Memory / Retrieval

只把当前真正相关的信息放进模型工作集。

不把整个游戏数据库等同于 prompt。

### G7-03｜Conversation Compression / Historical Recall

支持长局历史压缩与按需回忆，但压缩结果不是 live truth。

### G7-04｜Background Work Scheduling

世界维护、summary、索引等后台工作不能冻结主 UI，也不能让玩家等后台 edit 才看到叙事。

### G7-05｜Performance Instrumentation

建立可观察指标：

- context size；
- generation start latency；
- stream responsiveness；
- local persistence time；
- load / restore time；
- background job cost。

先测再优化。

### G7-06｜Long-session Evolution Test

使用真实游戏跑长局测试，而不是只跑合成 benchmark。

目标是验证：几十到上百回合后，延迟、状态一致性和 GM 质量仍在可接受范围。

## G7-GATE｜Long-session Gate

PASS 必须满足：

- 长局没有 DSH 式持续恶化到不可玩的趋势；
- 世界状态即时一致，不靠大规模周期性文本 edit 收敛；
- prompt/context 有明确预算；
- UI 始终可响应；
- 长期世界恢复仍正确；
- GM 输出质量没有因过度压缩明显下降。

---

# G8｜Mod / Authoring Ecosystem

## 目标

把 World Pack 从“开发者 fixture”提升成真正可持续创作和安装的内容能力。

## 主要任务

### G8-01｜World Pack Author Contract

明确作者真正需要创建什么、哪些是 optional、错误如何反馈。

### G8-02｜Pack Validation

提供窄而实用的验证：

- manifest；
- refs；
- required assets；
- version；
- duplicate identity；
- obvious incompatibility。

不建设理论化万能 validator。

### G8-03｜Local Mod Manager

本地安装 / 启用 / 禁用 / 查看 World Pack 与内容 Mod。

### G8-04｜Source Upgrade Semantics

明确新版本 Source 对：

- 新游戏；
- 已有游戏；
- assets；
- optional fixes

分别意味着什么。

### G8-05｜Authoring Support

根据真实作者需求决定是否增加：

- template；
- preview；
- map anchor helper；
- asset checker；
- lightweight editor。

### G8-06｜Third Pack / External-style Test

用一个不依赖核心开发者特殊知识的第三内容包验证作者路径。

## G8-GATE｜Modability Gate

PASS 必须满足：

- World Pack 可以在不改核心代码的情况下安装并启动；
- 作者错误有可理解反馈；
- 旧游戏不被 Source 升级静默污染；
- 至少多个内容包验证不是单一世界特例。

复杂脚本沙箱仍然可以继续 Deferred。

---

# G9｜Standalone Alpha / Release Validation

## 目标

形成第一版真正可以长期玩的 standalone alpha，而不是“技术栈完成”。

## 主要任务

### G9-01｜Reference World Alpha

选择一个完整但受控的参考世界完成端到端产品路径。

### G9-02｜Onboarding / New Game / Continue

玩家无需开发者知识即可：

- 启动；
- 配置 Provider；
- 选择 World Pack；
- 新建游戏；
- 继续游戏；
- Save / Restore。

### G9-03｜Windows Packaging / Update Baseline

形成稳定 Windows 构建与发布包。

第一阶段不要求自动更新平台完整化。

### G9-04｜Recovery / Corruption / Error UX

模型失败、网络失败、读档失败、内容包错误、局部损坏均有明确恢复路径。

### G9-05｜Regression / Migration

建立首个真正的版本升级与存档兼容基线。

兼容只针对已经真实发布的契约，不为理论未来预造多年兼容层。

### G9-06｜Long-play Product UAT

由 Product Owner 用真实模型、真实 World Pack 进行长局试玩。

与基线比较：

```text
The World / DSH + 同一模型
vs
my world standalone + 同一模型
```

关键不是所有维度都赢，而是核心价值不能明显退步，并且 standalone 应实质解决：

- 长局性能；
- 原生 Timeline / Save；
- 2D 游戏体验；
- World Pack / Mod；
- 本地独立运行。

## G9-GATE｜Standalone Alpha Gate

Engineering Acceptance：

- Windows standalone 可安装 / 启动；
- 核心循环、持久化、World Pack、Save/Restore、长局可靠性通过；
- 没有已知高概率世界损坏路径。

Product Value Acceptance 由 Product Owner 裁定：

1. **Want to Continue**：是否真的愿意继续长期玩；
2. **GM Quality Preserved**：独立 Runtime 是否没有把模型变笨；
3. **Long-session Advantage**：是否实质优于 DSH 长局卡顿；
4. **Native Game Value**：2D UI / 美术 / Map / 状态是否真正增加游戏感；
5. **Persistence Confidence**：关闭、重开、Save、Restore 是否让人放心；
6. **World Pack Value**：换世界不等于改源码；
7. **Player Plays, Runtime Maintains**：玩家不承担后台工程维护。

G9 PASS 后再讨论 beta、公开发行、商业化、Steam、云服务、Local LLM、TTS/STT 等下一代路线。

---

# 4. 跨阶段不变量

## INV-01｜核心体验优先

工程完整性不能以明显损害 GM 输出质量、玩家自由度与沉浸为代价。

## INV-02｜成熟基底优先

能由可靠成熟 Foundation 解决的通用问题，默认不从零重写。

## INV-03｜游戏语义自有

Godot / Provider / SQLite / 任何第三方库都不能自动成为 `World / Timeline / NPC / Save` 的产品定义者。

## INV-04｜本地单人优先

服务器、多人与云能力不能反向复杂化 G1–G9 的第一代核心架构。

## INV-05｜Source ≠ Game Reality

World Pack 是可复用 Source；开局后的 game-local reality 是本局真相。

## INV-06｜Model authors, Domain commits

模型可创造候选内容与自然语言结果；正式世界变化必须经过明确 Domain commit path。

## INV-07｜UI 是 Projection

UI 可以聚合、重排、可视化世界事实，但不维护第二套长期真相。

## INV-08｜Timeline ≠ Conversation

Save / Restore / Branch 与聊天历史必须从第一代架构就分离。

## INV-09｜产品 Gate 需要真人

用户体验、游戏性、GM 质量不能只由自动测试宣布 PASS。

---

# 5. 当前阶段

当前状态：

```text
Product Definition Gate      PASS
Standalone Strategy          PASS
Godot Toolchain              v4.7.2 Standard x64 VERIFIED
Implementation Repo          INITIALIZED / MINIMAL G1-01 BOOTSTRAP
G1-01 Runtime Verification   BLOCKED: LOCAL WRITE PERMISSIONS
Current Phase                G1
Next Task                    G1-01
```

因此下一步不是实现 G2/G3 的完整 Runtime，也不是提前进入 G1-02，而是：

> **继续 G1-01：解决 Windows 本地写权限 blocker，完成最小 Godot runtime proof。**

---

# 6. 当前 Non-scope

在相应阶段到来前，不提前建设：

- Multiplayer；
- Server backend；
- Cloud account / cloud save；
- 3D 自由移动；
- Automatic map generation；
- 全世界逐 tick simulation；
- Universal ECS；
- 巨型通用 schema/protocol；
- 复杂脚本沙箱；
- Steam Workshop；
- Local LLM hosting；
- TTS / STT；
- 程序化角色 / 场景美术；
- 自动生成 World Pack；
- 大规模兼容层；
- 与当前 Gate 无关的“未来可能需要”基础设施。

---

# 7. Roadmap 变更规则

本文件是 rolling current。

发生以下任一情况时必须更新：

- Product Spec 改变核心用途 / 产品承诺；
- 真实 Spike 推翻当前 Foundation；
- Gate 失败导致阶段重排；
- 新证据改变关键依赖；
- G 阶段 PASS 并进入下一阶段；
- 下游能力被真实证明必须提前或延后。

不得因为“已经写了很多代码”拒绝重排路线。

历史由 Git history 承担；不在 active 目录并排维护多个 current Roadmap。
