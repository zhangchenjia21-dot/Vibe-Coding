---
title: 第一版 SillyTavern 项目构建经验复盘
aliases:
  - 酒馆游戏第一版工程复盘
  - SillyTavern Legacy Lessons Learned
  - 第一版酒馆游戏开发经验
created: 2026-08-13
status: evergreen
project: 酒馆游戏开发
type: 项目复盘
tags:
  - 酒馆游戏
  - SillyTavern
  - 项目复盘
  - LLM应用
  - 软件架构
  - Agent协作
  - 测试
  - UAT
  - 工程管理
related:
  - "[[01_World_OS_v1.0_FC2_现行规则整合版_2026-07-12]]"
  - "[[02_World_OS_FC2_补充裁定_Core与开发模式_2026-07-12]]"
  - "[[07_酒馆游戏三项目架构与资产职责_v1.0]]"
  - "[[08_酒馆游戏_H.0A_世界运行与信息架构_正式收口候选_v0.2]]"
  - "[[09_酒馆游戏项目开发与Agent协作规则_v1.0]]"
  - "[[10_酒馆游戏_补充裁定_代码层级与上帝权限命名空间_v1.0_2026-07-28]]"
  - "[[酒馆游戏新版主体重建总路线 v1.1]]"
---

# 第一版 SillyTavern 项目构建经验复盘

> [!summary] 一句话结论
> 第一版最重要的成果，不是留下了一套值得继续修补的游戏运行代码，而是通过大量真实失败，逼出了**更可靠的 World OS、玩家代理权、程序权威、存档语义、资产边界、Agent 协作方法和 LLM 工程纪律**。  
> 第一版真正的问题不是“Bug 太多”，而是**最核心的真实纵向链验证太晚，而外围架构、兼容层和恢复基础设施建设得太早**。

---

## 1. 如何看待第一版：不是“全部失败”，而是昂贵的架构发现阶段

第一版最终没有成为值得继续演进的游戏运行主体，但这并不意味着其中所有工作都应该丢弃。

更准确的定义是：

> **第一版是酒馆游戏的规格发现、边界发现和失败语料积累阶段。**

它留下了两类完全不同的资产：

### 应当保留的稳定资产

- World OS FC2 与后续补充裁定；
- 玩家代理权；
- 模型不是数据库、模型不能拥有正式世界状态；
- 程序负责验证、随机、事务提交和恢复；
- Region → Place → Scene；
- 隐藏世界事实与玩家知识分离；
- 权威世界时间；
- 物品所有权与 Placement 分离；
- 原子 Turn；
- Save / Restore / Branch / Recovery 的产品语义；
- 三项目架构；
- `tavern-asset-spec` 作为资产协议唯一事实源；
- Expansion Pack 不执行任意代码；
- L0–L3 代码层级与 GM0–GM3 权限命名空间分离；
- 已经积累的真实 DeepSeek 失败模式；
- 审核包、脱敏证据、批量 Gate 等 Agent 协作实践。

### 不应直接继承的实现资产

- 旧 Turn Orchestrator；
- 多代 `v1/v2/v3` 模型输出契约；
- 为兼容旧实现不断增加的 Adapter；
- pending / processing / failed Formal Turn 生命周期；
- 旧 Provider 多轨判断；
- 旧 Check Resolution 演化路径；
- 在真实 Turn 尚未稳定前建立的复杂 durable execution；
- 把模型输出做得过于接近正式 Delta 的旧接口；
- 把真人 UAT 当作最终集成测试的旧流程。

> [!important] 关键判断
> **规格可以继承，实现不等于规格。**  
> 第二版应该复用经过验证的“规则与边界”，而不是复用已经被这些规则反复证明不合适的运行代码。

---

# 2. 第一版做对了什么

## 2.1 最终建立了“程序拥有世界，模型只提出变化”的正确方向

第一版早期最危险的倾向，是让模型返回越来越接近正式世界状态的数据，例如：

- 内部 ID；
- revision；
- 时间；
- Event / Memory / Placement；
- authorization evidence；
- 各类状态 Patch。

随着真实 UAT 失败，这条边界逐渐被收紧，最终形成正确原则：

```text
模型：理解、推演、提出语义候选、写叙事
程序：身份、规则、权限、状态、时间、RNG、骰点、事务、提交、恢复
```

这个原则是整个酒馆游戏最值得继承的核心之一。

它对应 World OS 的长期方向：

> 模型不是数据库；模型只能提出状态变化；程序负责验证、随机、事务提交和恢复。

这条原则以后应该成为所有新功能的架构检查项。

---

## 2.2 玩家代理权最终被提升为不可绕过的 Core

第一版里大量非常具体的 Bug，其实都指向同一个问题：

> **程序和模型不能替玩家扩张原始表达。**

典型区别：

```text
“我去门口。”
≠ “我想去门口。”
≠ “如果安全，我就去门口。”
≠ “我能去门口吗？”
≠ “我不去门口。”
```

以及：

```text
“我把火把交给守卫。”
≠ “我把火把给守卫看看。”
```

第一版中，“给某人看”曾经被错误解释为物品转移。这个问题后来促成了更严格的：

- 原始玩家表达保真；
- 条件 / 愿望 / 问句 / 否定不能自动展开成行动；
- 玩家移动和物品转移必须逐变化精确授权；
- NPC 自主行为与玩家授权完全分开。

> [!lesson] 可复用经验
> 对玩家行为而言，**“语义合理”不等于“玩家已授权执行”**。  
> 任何主动玩家状态变化都必须有独立的程序权限链。

---

## 2.3 Save / Restore / Branch 的产品语义是成功资产

尽管旧运行实现最终被放弃，但存档语义本身已经被大量讨论和验证，值得继承：

- Save 只保存稳定 committed state；
- 恢复不能依赖模型“记得以前发生过什么”；
- Restore 必须恢复隐藏状态、知识边界、时间、空间、物品、计划等权威世界；
- Restore 本身不应立即制造新分支；
- Restore 后第一次成功的新 Turn 才形成新的未来；
- 原未来应保留，而不是静默覆盖；
- 玩家看见的是产品级存档体验，不是底层 timeline 垃圾。

这是一个典型案例：

> **旧代码可以扔，但经过讨论形成的产品语义应该保留。**

---

## 2.4 Region → Place → Scene 是一次成功的领域建模收口

早期混杂的 Location 很难同时表达：

- 大区域；
- 城镇 / 建筑；
- 同一地点内部不同空间；
- 角色是否真的“共同在场”。

后来收口为：

```text
Region → Place → Scene
```

这带来了非常重要的运行时不变量：

- Scene 属于 Place；
- Place 属于 Region；
- Region 可由 Place 推导；
- 共同在场默认要求同一 Scene；
- 只有 Place 信息时，不能假定角色近距离共处。

这是一个优秀的领域建模例子：

> **不要用一个模糊字段承载多个粒度不同的业务概念。**

---

## 2.5 三项目架构是正确的长期边界

第一版后期形成的三项目结构值得保留：

```text
游戏本体
↓ consumes
资产协议 tavern-asset-spec
↑ produces
Creator
```

核心经验：

- 一个职责只有一个事实源；
- Schema / Validator 不能在游戏与 Creator 中复制两套；
- 游戏实例与资产定义分离；
- 人物卡进入游戏后形成实例副本，不回写原人物卡；
- 原资产更新不能静默覆盖已有游戏实例；
- Expansion Pack 可以声明状态、资源、规则、判定、UI Contribution；
- Expansion Pack 不得执行任意代码。

这为未来的动态 UI、世界包、人物卡和拓展机制提供了正确的架构基础。

---

## 2.6 “四层架构 + 跨模块只走公开边界”是有效纪律

项目后来正式确认：

```text
L3 → L2 → L1 → L0
```

并规定：

- 依赖只能向下；
- 跨模块只能通过对方 L3 公开边界；
- 不能绕开公开契约直接依赖内部实现；
- 业务文件默认使用简体中文语义命名；
- 复杂公开契约应写清边界、不变量、失败方式和副作用。

同时，把旧的“上帝权限 L0–L3”改名为 `GM0–GM3`，避免两个完全不同的命名空间发生冲突。

这是一个很好的工程治理经验：

> **术语冲突本身就是架构风险，应该尽早消除。**

---

# 3. 第一版最根本的错误

## 3.1 最大错误：核心纵向链验证太晚，横向基础设施建设太早

第一版真正应该最先证明的是：

```text
玩家输入
→ 真实 DeepSeek
→ 程序理解
→ 程序裁定
→ 正式状态提交
→ 玩家看到正常回复
```

至少先成功一次，再逐步扩展。

实际却提前建设了很多复杂能力：

- Save / Restore；
- Timeline；
- Recovery；
- durable execution；
- Provider generation lease；
- crash / resume；
- Dice locking；
- 多 Provider；
- 大量状态类型。

结果是：

> **核心 Turn 合同还没有被真实模型证明稳定，周围已经建了一整圈生产级基础设施。**

于是任何真实模型问题都会沿着大量下游模块扩散。

### 新项目的规则

> 先证明纵向链活着，再允许横向扩展。

例如：

```text
真实模型兼容性
→ 最小世界 Turn
→ 移动
→ 物品转移
→ Dice
→ 长期连续性
→ Save/Restore
→ Crash/Recovery
```

而不是反过来。

---

## 3.2 在未发布项目里承担了过多“历史兼容”成本

第一版不断出现：

- v1；
- v2；
- v3；
- compatibility adapter；
- legacy reader；
- 旧字段兼容；
- 新旧路径并行。

这种方式适合已经发布、有大量用户数据的成熟系统。

但当时项目：

- 尚未正式发布；
- 真人 Turn 甚至没有稳定跑通；
- 却已经承担了生产系统级迁移复杂度。

最终造成典型问题：

> 新的 Program Resolution 已经有五档结果，旧的 Formal Check Resolution 仍只有四档，`partial_success` 被重新压成 `success`。

这不是偶然 Bug，而是**双轨事实源长期并存**的自然结果。

### 新项目规则

未发布阶段，如果早期设计错误：

> **优先删除 / 重写，不优先兼容。**

真正存在发布用户数据以后，才建立严格兼容策略。

---

## 3.3 模型边界最初离正式状态太近

第一版最早的思路接近：

```text
模型生成接近正式 Delta 的 JSON
→ 程序验证
→ 写入
```

这导致模型逐渐被要求理解：

- 内部字段；
- ID；
- revision；
- Event / Memory；
- 时间；
- Placement；
- authorization；
- 各种内部 enum。

问题在于：

> **LLM 擅长的是语义理解和推演，不擅长充当数据库事务客户端。**

后来才逐步收口成：

```text
模型提出语义
→ 程序编译领域命令
→ 程序裁定
→ 程序生成 Formal Delta
```

这个方向是对的，但出现得太晚。

### 可复用原则

> 模型接口越接近“人类语义语言”，越稳定；越接近“数据库内部协议”，越脆弱。

---

## 3.4 测试覆盖了“我们想象中的模型”，但没有覆盖“实际模型”

第一版有很多自动化测试，甚至经常出现：

- 几十；
- 几百；
- 上千测试；
- Playwright 全绿。

但真人 DeepSeek 一上来仍然失败。

原因不是自动化没用，而是测试真实性层级不足：

```text
deterministic fixture
fake SDK
mocked API
人工“真实风格 JSON”
```

这些证明的是：

> “如果模型按照我们预想的方式返回，程序能处理。”

但真正缺少的是：

> “真实 DeepSeek 实际会返回什么？”

第一版因此把真人 UAT 变成了最终集成测试，导致项目负责人亲自发现：

- Blueprint 参数问题；
- JSON mode 空正文；
- reasoning / content 组合问题；
- 字段缺失；
- 时间问题；
- Provider 配置问题；
- 语义扩张；
- Formal Dice 合同不一致。

### 正确测试阶梯

```text
Schema 单元测试
→ Program Judge 测试
→ deterministic fixtures
→ 历史真实模型 failure corpus
→ 真实 Provider Smoke
→ 真实 Provider Stability
→ 真人产品 UAT
```

真人 UAT 应该验证体验，而不是替工程团队发现基础协议错误。

---

## 3.5 Durable Execution / Recovery 做得太早

第一版后来建立了非常完整的：

- durable execution 状态机；
- semantic at-most-once；
- narrative recovery；
- Dice lock；
- response-loss；
- crash / resume；
- execution ownership。

这些概念本身并不差。

真正的问题是：

> **它们建立在一个仍然不断变化的 Turn Contract 之上。**

于是每次 Turn 语义变化，Recovery 也跟着重构。

### 新项目顺序

```text
正常 Turn 稳定
→ Dice 稳定
→ Save 稳定
→ 再做 durable execution / recovery
```

恢复系统应保护一个已经稳定的正常流程，而不是替尚未稳定的流程兜底。

---

## 3.6 多 Provider 抽象做得太早

第一版同时承担：

- DeepSeek；
- OpenAI；
- deterministic rehearsal；
- 多 Provider 配置分支。

这让 Provider-neutral abstraction 在真实主 Provider 尚未稳定前就变成正式负担。

### 更好的顺序

```text
DeepSeek 真正跑通
→ 冻结 Model Adapter 边界
→ 再加第二 Provider
→ 用第二 Provider 验证 abstraction
```

而不是：

> 为一个还没稳定的系统提前证明“我们能支持所有模型”。

---

## 3.7 UI 和产品形态的成熟度一度高于运行内核

第一版一度出现典型错位：

> 页面已经像产品，但核心 Turn 仍不可靠。

这会带来两个问题：

1. 已经投入的 UI 成本反过来约束内核修改；
2. 团队容易产生“项目已经快完成”的错觉。

第二版更合理的方式是：

- G1 无前端；
- G2 开始有最小 Playtest Shell；
- G2–G7 始终保持真人可玩路径；
- G8 才做正式产品化 UI。

即：

> **早期有“可玩前端”，但不要过早有“正式产品前端”。**

---

# 4. LLM / API 集成的具体经验

## 4.1 不要把“模型会严格遵守协议”当系统前提

可靠系统应该假设：

- 模型偶尔会误解；
- 模型偶尔会输出非法结构；
- Provider 的 JSON Schema 方言与标准 JSON Schema 不完全一致；
- 同一句话重复调用可能得到不同语义；
- Tool Call 可能出现多个调用；
- reasoning / content / tool_calls 的组合会随 Provider 特性变化。

因此正确目标不是：

> 让模型永不犯错。

而是：

> **模型犯错时，程序可以安全拒绝，而且不污染正式世界。**

---

## 4.2 结构约束、语义正确、安全正确必须分开统计

LLM 集成至少要分四层：

```text
Transport
Structure
Semantic
Program Safety
```

例如：

- HTTP 成功 ≠ Tool Call 正确；
- Tool Call 正确 ≠ JSON Schema 正确；
- Schema 正确 ≠ 语义理解正确；
- 语义理解错误 ≠ 世界一定不安全，只要 Program Judge 拦住。

这类分类非常重要，因为不同失败需要完全不同的处理方式。

---

## 4.3 Canonical Schema 只有一个事实源，但还需要 Provider Dialect Validator

一个很好的做法是：

```text
Canonical Schema Builder
→ Provider Tool Schema
→ Local AJV Validator
```

这样避免：

- Prompt 一套；
- Provider 一套；
- TypeScript 一套；
- Validator 又一套。

但第一版 / 新版实验又暴露了进一步经验：

> **本地 JSON Schema 库支持的关键字，不代表 Provider Strict 模式支持。**

例如字符串 `const` 在本地 AJV 合法，但 Provider 的 Strict 方言不一定承诺支持。

因此还需要：

```text
Canonical Schema
→ Provider Dialect Check
→ Provider
```

不要把“合法 JSON Schema”与“目标 Provider 支持的 JSON Schema 子集”混为一谈。

---

## 4.4 协议版本属于程序，不应该让模型复述

类似：

```json
{
  "version": "player-intent-candidate.v0"
}
```

这种字段如果只是协议元数据，不应该由模型返回。

更合理的是：

```text
模型返回 candidate
→ 程序验证
→ 程序附加 protocolVersion
```

通用原则：

> **程序已经知道的事实，不要要求模型重复。**

模型只应该表达它真正负责的语义。

---

## 4.5 动态 Ref Enum 比 Prompt 里的“不要乱编 ID”更可靠

如果当前场景只允许：

```text
character:guard-a
item:torch-a
place:doorway-a
```

那 Tool Schema 就应该把这些值直接收窄成 enum。

不要只在 Prompt 里说：

> 请不要生成不存在的 ID。

通用经验：

> **能在结构层禁止的错误，不要只靠自然语言提醒。**

---

## 4.6 “Capability”与“玩家本次授权”不是一个概念

这是第一版玩家代理权问题后来被进一步明确的重要经验。

Capability 回答：

> 玩家**能不能**做这件事？

Player Agency Authorization 回答：

> 玩家**这句话有没有授权现在执行**这件事？

例如：

```text
“我想去门口看看。”
```

世界状态可能允许玩家移动到门口，即 Capability = YES。

但这句话仍然不一定是正式执行授权。

因此主动玩家变化最终至少需要：

```text
Semantic validity
AND
Agency authorization
AND
Capability
```

不能因为 Capability 存在，就把模型误判的愿望执行掉。

---

## 4.7 真实模型兼容性实验时，不要偷偷修模型答案

实验阶段禁止：

- JSON repair；
- fuzzy matching；
- 自动补字段；
- 把 `mov` 猜成 `move`；
- 把中文名称自动映射成某个 Ref；
- 删除 unknown field 后继续；
- 第二次 LLM repair；
- 为了提高通过率偷偷改 Gold。

因为兼容性实验真正要测的是：

> **模型原生能不能使用这个语言。**

如果实验装置帮它“擦屁股”，数据就失去意义。

---

# 5. Turn 与正式世界的架构经验

## 5.1 Formal Turn 只在成功提交以后存在

一个非常重要的收口方向是：

### Execution

- 临时运行过程；
- 可以失败；
- 可以恢复；
- 不是世界历史。

### Formal Turn

- 只有 Atomic Commit 成功以后才出现；
- 是正式世界历史。

因此应避免重新建立：

```text
pending Formal Turn
processing Formal Turn
failed Formal Turn
```

作为长期核心模型。

失败执行不应该污染正式时间线。

---

## 5.2 Formal Turn 必须原子

一个失败的 Turn 不应该留下：

- 半个状态变化；
- 已推进时间；
- 无主 Dice；
- Receipt；
- 部分 Event；
- 部分 Memory；
- 自动分支。

核心不变量：

> **要么整个 Turn 成功成为历史，要么正式世界保持原样。**

---

## 5.3 Dice 是程序证据，不是第二个 Outcome 权威

第一版后期的四档 / 五档冲突说明：

> 不应该让 DiceRecord、Formal Check Resolution、Program Outcome 各自拥有一套“结果事实”。

正确方向是：

```text
Program Resolution Outcome = 唯一结果事实源
DiceRecord = 锁定证据 / 输入
Narrative = 对最终结果的表达
```

任何 reader / Session / Save 都不应从“骰点 + DC”重新推导一个可能不同的结果。

---

## 5.4 Narrative 必须发生在正式结果确定之后

模型叙事不能创造权威状态。

理想顺序：

```text
Semantic Proposal
→ Program Judge
→ Resolution
→ Final Delta
→ Narrative Realization
```

如果叙事失败：

- 不应该让世界状态重新裁定；
- 不应该要求模型重新决定 Outcome；
- 恢复应只恢复叙事阶段，而不是重做正式决策。

---

# 6. 测试、UAT 与验证流程的经验

## 6.1 Preflight 应该是正式 Gate，而且不能触发真实模型

好的 Preflight 应只检查：

- Provider 配置是否存在；
- 模式是否合法；
- 数据库 / Migration 是否就绪；
- 当前世界是否处于稳定状态；
- 是否存在 unresolved execution；
- 是否有 unowned Dice；
- route 是否指向正式路径；
- Schema / Corpus 是否有效。

它**不应该**通过“顺便发一个模型请求”来确认 Provider。

否则 Preflight 本身就会产生副作用。

---

## 6.2 真实 Provider 测试应分 Smoke 与 Stability

推荐流程：

### Smoke

少量、固定、代表性的场景。

目标：

- 协议基本可用吗？
- Schema 是否匹配 Provider？
- 有没有 Hard Safety 失败？

### Stability

Smoke 通过以后，再：

- 全 Corpus；
- 多次重复；
- 统计一致性。

不要一开始就烧几百次真实 API 调用。

---

## 6.3 阈值必须在实验前冻结

例如：

```text
Hard Safety violation = 0
Strict structure = 100%
明确场景 semantic >= 95%
repeat agreement >= 95%
```

这些数字应在实验前写进 Gate。

实验后如果结果不好，不能偷偷改成：

> “90% 其实也行。”

如果确实认为阈值不合理，应写正式 review 解释为什么，而不是事后改分数。

---

## 6.4 失败必须分类，而不是全部叫“Turn failed”

至少区分：

- Provider config；
- transport；
- structure；
- semantic；
- authorization；
- provenance；
- adjudication；
- resolution；
- narrative；
- commit；
- response loss；
- client state desync。

只有分类正确，后续修复才不会“哪里红就补哪里”。

---

## 6.5 历史真实失败必须变成 Regression Corpus

第一版那些令人痛苦的失败其实是高价值资产，例如：

- JSON mode 空 content；
- reasoning / content 组合；
- 缺字段；
- 多余字段；
- 内部 ID；
- 错误时间；
- 展示 vs 转移；
- 条件 / 愿望 / 问句；
- unknown ref；
- provider 配置空字符串；
- Dice Outcome 版本错位。

它们应该被脱敏，变成：

> **真实模型失败语料库。**

以后每次模型接口修改都先跑这些，而不是重新靠真人发现一遍。

---

# 7. Agent 协作与任务管理经验

## 7.1 最有效的任务粒度：中等规模“完整任务包”

两种极端都不好：

### 太大

一个任务同时改：

- 时间；
- 空间；
- 物品；
- Memory；
- Save；
- UI；
- Provider。

风险太高，Review 无法定位。

### 太小

一个字段一个任务、一个失败一个 Review，会导致：

- 项目负责人不断复制指令；
- 上下文切换巨大；
- 每轮只能看到局部；
- 很容易形成“补丁循环”。

最好的方式是：

> **一个清晰目标 + 内部多个步骤 + 一次集中验证 + 一次集中审核。**

后来形成的工作法：

```text
批量实现
→ 一次审核
→ 一次 consolidated rework
→ Gate
```

比早期“发现一个修一个”有效得多。

---

## 7.2 正式任务 ID 与 Review ID 分离

一个任务的身份不应因为 Review 变化：

```text
G2-CORE-03
```

审核只是：

```text
G2-CORE-03-review-01
G2-CORE-03-review-02
```

不要重新创造：

```text
G2-CORE-03A
G2-CORE-03B.1
G2-CORE-03-final2
```

这样任务图才不会失控。

---

## 7.3 项目负责人不应该成为人工调试路由器

错误工作流：

```text
Agent 写一点
→ 用户点一下
→ 报错
→ ChatGPT 再写一条小修
→ 用户再点一下
```

正确工作流：

```text
Agent 完成完整任务包
→ 自动化
→ 审核
→ 批量修正
→ 集成 Gate
→ 用户在产品节点做真人验收
```

用户应该负责：

- 产品方向；
- 关键架构裁决；
- 体验判断；
- 里程碑 UAT。

而不是成为 QA Bot。

---

## 7.4 Codex 与前端 Agent 应按职责分工，而不是同时乱改

新版形成的合理倾向：

### Codex

负责：

- Runtime；
- World OS 实现；
- Model Adapter；
- Program Judge；
- Capability；
- DB；
- Save / Restore；
- Dice；
- Provider；
- Asset Adapter；
- API Contract；
- DTO；
- 安全；
- 测试；
- Release。

### Grok Build

负责：

- 页面；
- 组件；
- 交互；
- 响应式；
- UI 状态；
- Runtime UI Host 表现层；
- 动态声明式 UI 渲染。

### 协作顺序

```text
Codex 冻结 Contract / DTO
→ Grok Build 实现页面
→ Codex 接真实运行并做安全 / E2E 审核
```

前端不能为了方便自行发明正式后端字段。

---

## 7.5 新项目单主干比“任务分支森林”更适合当前规模

旧项目大量分支让任务对应关系越来越难理解。

对单负责人 + Agent 开发的当前规模，更简单的做法是：

```text
main
↓
任务 commit
↓
review fix commit
↓
下一个任务 commit
```

Review ID 不对应 Git branch。

只有真正需要并行隔离的大型实验，才临时创建分支，而且完成后 merge + 删除。

---

## 7.6 审核包必须有固定输出目录

一个有效约定：

```text
D:\AI\AgentScratch\codex\<task-id>\review-xx\
```

而不是把：

- ZIP；
- diff；
- report；
- evidence；

全部散落在 `AgentScratch\codex` 根目录。

审核包应包含：

- consolidated report；
- sanitized diff；
- test summary；
- 关键矩阵 / evidence。

不能包含：

- API Key；
- raw provider response；
- reasoning；
- Authorization header；
- 私人存档；
- `.env.local`。

---

# 8. Git 与安全方面值得保留的做法

## 好做法

- 本地提交，不默认 push；
- `git diff --check` 作为固定 Gate；
- `git status --short` 明确工作树状态；
- 不覆盖用户已有脏项；
- 不随便 stash / reset / clean；
- 重要 UAT 数据先备份、Hash、quick_check；
- 密钥只报告 PRESENT / ABSENT；
- 真实模型 raw response 默认不持久化；
- reasoning 不进入审核包；
- Review 包与仓库分离。

## 明确应禁用的危险默认操作

```text
git add .
git add -A
git clean
git reset --hard
git stash
rebase
amend
push
```

除非任务明确授权。

---

# 9. 优秀的 Codex 任务指令结构

第一版后期形成了一套非常值得复用的任务格式。

一个高质量 Codex 指令应至少包含：

```markdown
# TASK-ID｜任务标题

任务性质：
工作区：
当前 HEAD：

## 一、背景
为什么做这个任务。

## 二、目标
必须完成什么。

## 三、非目标
明确哪些东西这次绝对不碰。

## 四、架构裁定
关键不变量与事实源。

## 五、允许修改范围
目录 / 模块 / 文件类型。

## 六、禁止事项
数据库、Provider、旧仓库、危险 Git 等。

## 七、实施要求
内部步骤与接口要求。

## 八、测试
单测、集成、Corpus、E2E、lint、typecheck、diff check。

## 九、安全
API Key、Prompt、raw response、私人数据等。

## 十、Git
main / commit / no push。

## 十一、审核包
固定输出目录与文件清单。

## 十二、统一返回格式
让 Agent 输出结构化结果，便于一次审核。
```

> [!tip] 为什么这种指令有效
> 它把“目标、边界、证据、停止条件”一次性讲清，降低 Agent 在实现过程中自行猜测产品语义的概率。

---

# 10. 优秀的 Review / Correction 指令模式

当发现问题时，不要立即说“把这个字段改一下”。

更好的 Review 指令应包含：

```text
1. 真实证据是什么
2. 根因属于哪一层
3. 哪些冻结事实不能修改
4. 允许修哪一层
5. 禁止通过改 Gold / 降 Gate / 特判绕过
6. 必须新增什么永久 regression
7. 旧证据怎样保留
8. 修完后重新跑哪个 Gate
```

例如 Provider Schema 方言问题：

> 不能为了通过 Strict Smoke 去改 Gold 或 Prompt，而应该修 Harness 的 Dialect Validator，并永久保留 string `const` 的负向 regression。

这种做法能把“修一个 Bug”升级成“关闭一个系统性缺口”。

---

# 11. 反模式清单：看到这些迹象就应该停下来

> [!danger] 典型危险信号

### 架构层

- “先兼容旧 v1，以后再清理。”
- “模型先把正式状态都返回，程序再验证。”
- “DiceRecord 和 Program Outcome 各自算一次结果。”
- “UI 需要，所以直接给 Core 加一个 HP 字段。”
- “这个 Expansion Pack 可以执行一点 JS，比较灵活。”

### 测试层

- “mock 全绿，所以可以上真人 UAT。”
- “真实 Provider 以后最后再测。”
- “这个失败只是偶发，重跑一次看看。”
- “结果不够 95%，那把门槛改成 90%。”
- “JSON 不合法没关系，程序自动修一下。”

### 项目管理层

- “这个 Review 再开一个新任务号。”
- “一个字段一个补丁。”
- “让用户再点一次看看。”
- “为了并行，每个 Agent 都开长期分支。”
- “审核包先扔 AgentScratch 根目录再说。”

### 安全层

- “把 API Key 写进 README 方便点。”
- “把 raw Provider response 全部打包给 Reviewer。”
- “先 `git add .`，再看看提交了什么。”
- “旧 UAT 数据顺手迁移一下。”

---

# 12. 第二版必须坚持的工程戒律

可以把下面这组规则当作新版项目的“工程十诫扩展版”。

1. **真实模型验证优先于外围基础设施。**
2. **先纵向跑通，再横向扩展。**
3. **模型只能提出语义，不能拥有正式世界。**
4. **玩家代理权不能委托给模型。**
5. **Capability 与本次玩家授权分离。**
6. **Formal Turn 只在成功 commit 后存在。**
7. **失败执行不污染正式世界。**
8. **一个事实只保留一个权威来源。**
9. **Dice 是证据，不是第二 Outcome 权威。**
10. **Narrative 只能表达已经确定的正式结果。**
11. **未发布阶段，错误设计优先删除，不优先兼容。**
12. **Provider abstraction 在主 Provider 真跑通后再验证。**
13. **Schema 的 Provider 方言必须独立验证。**
14. **真实失败必须进入 regression corpus。**
15. **真人 UAT 用于产品验收，不用于基础协议调试。**
16. **恢复系统最后做，不要保护一个尚未稳定的流程。**
17. **UI 从 G2 起保持可玩，但正式产品化后置。**
18. **资产接入前保持 asset-ready，但不要提前猜资产协议。**
19. **动态 UI 必须声明式，资产不得执行任意前端代码。**
20. **项目负责人不承担 Agent 的逐字段调试循环。**
21. **任务中等粒度：批量实施，一次审核，一次集中返工。**
22. **Review ID 与正式任务 ID 分离。**
23. **当前规模优先单 `main`，避免任务分支森林。**
24. **审核证据脱敏、集中、可复现。**
25. **任何“为了方便”引入的第二事实源都要高度警惕。**

---

# 13. 从第一版带到第二版的“真正资产”

第一版最后最值得留下的，不是某个具体 Service 或 Handler，而是下面这些知识：

```text
我们知道模型在哪些地方会犯错；
我们知道哪些权力绝不能交给模型；
我们知道玩家表达应该怎样保护；
我们知道正式世界需要哪些权威结构；
我们知道 Save / Restore 应该是什么产品语义；
我们知道测试不能只依赖 mock；
我们知道真人 UAT 不应该承担集成调试；
我们知道 Recovery 应该在什么时候做；
我们知道资产与游戏实例应怎样分离；
我们知道 Agent 任务怎样写才更少返工；
我们知道哪些 Git / 审核习惯会制造不必要复杂度。
```

所以第一版最准确的总结不是：

> “我们白做了一个不能用的游戏。”

而是：

> **我们用第一版把酒馆游戏最难、最隐蔽的系统边界真实地撞了一遍，并把这些失败转换成了第二版可以直接使用的工程知识。**

如果第二版真正吸收这些经验，那么第一版的成本才算被完整回收。

---

# 14. 后续建议：把这篇复盘当成开发前检查表

每当新版准备开始一个较大的阶段，可以回来看三个问题：

### 问题 1：我们是不是又在横向扩展一个尚未真实验证的核心？

如果是，停。

### 问题 2：我们是不是为了兼容一个尚未发布的旧实现增加复杂度？

如果是，优先删除 / 重写。

### 问题 3：这个设计是不是在把程序应该承担的权威重新交给模型、UI 或资产？

如果是，重新画边界。

---

## Related Notes

- [[01_World_OS_v1.0_FC2_现行规则整合版_2026-07-12]]
- [[02_World_OS_FC2_补充裁定_Core与开发模式_2026-07-12]]
- [[07_酒馆游戏三项目架构与资产职责_v1.0]]
- [[08_酒馆游戏_H.0A_世界运行与信息架构_正式收口候选_v0.2]]
- [[09_酒馆游戏项目开发与Agent协作规则_v1.0]]
- [[10_酒馆游戏_补充裁定_代码层级与上帝权限命名空间_v1.0_2026-07-28]]
- [[酒馆游戏新版主体重建总路线 v1.1]]

