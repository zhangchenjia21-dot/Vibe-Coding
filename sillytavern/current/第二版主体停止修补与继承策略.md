---
title: 酒馆游戏第二版｜停止继续修补与继承策略裁定
status: current-terminal-decision
version: 1.0
updated: 2026-08-23
project: 酒馆游戏第二版主体
decision: STOP_FURTHER_RUNTIME_REPAIR
supersedes_execution_status:
  - PLAY-01 correction-02 AUTHORIZED / NEXT
  - 第二版核心 Runtime 继续以修补方式恢复可玩性
---

# 酒馆游戏第二版｜停止继续修补与继承策略裁定 CURRENT

## 0. 决策

Project Owner 已明确停止 Kimi 当前 `PLAY-01 correction-02` 任务，并决定不再把“继续修补第二版核心 Runtime”作为默认下一步。

正式状态：

```text
第二版核心 Runtime 路线 = STOPPED
PLAY-01 correction-02 = CANCELLED / NOT COMPLETED
第二版继续 correction-03+ = NOT AUTHORIZED
sillytavern/main = 保持现有正式集成状态，不把未审核任务分支并入 main
第三版 / 后继项目 = 允许从新的产品与架构假设重新开始
```

该决定不是因为第二版没有工程成果，而是因为继续修补已经出现明显的结构性边际成本上升：旧产品哲学已经进入类型、Provider contract、Context compiler、Validator、Turn orchestration、测试 fixture 与持久化流程。继续逐层拆除隐式假设，成本可能高于在更成熟基础上重新建立核心玩法层。

---

## 1. 第二版终止点

截至本裁定：

- 正式 `sillytavern/main` 未合并 PLAY-01 / correction-01 的未审核实现；
- `agent/play-01-world-initiative` 上 correction-01 实现曾达到 `0b638ac940bc78dd083718c42c116513cda61eb6`；
- correction-02 任务包已发布，但 Project Owner 随后主动停止执行；
- 对 `0b638ac...` → 当前任务分支的比较只增加 correction-01 最终报告和 correction-02 任务包，没有 correction-02 实现代码；
- 因此 correction-02 不存在“待合并实现”，不得把任务包存在误读为功能完成。

第二版保留为：

```text
工程成果库
+
失败案例库
+
验证过的基础设施参考
+
第三版的反例与迁移来源
```

而不是继续承担“必须把现有 Runtime 修到最终可玩”的义务。

---

## 2. 停止继续修补的根因

### 2.1 不是 bug 数量，而是旧哲学渗透范围

连续独立审核发现的问题呈现同一模式：

```text
表面能力已经存在
↓
真实 Provider / 真实回合纵向继续向下追
↓
发现旧假设仍存在于另一层
```

典型包括：

- `创建新地点` 与 `玩家移动` 曾被错误耦合；
- Active Situation 已持久化，却一度没有进入模型工作集；
- Opening 已允许 World Initiative，却仍残留“不得创造新人物/地点”的旧 Prompt；
- 玩家行动已经发生，但 post-outcome World Initiative 一度仍读取 pre-player frame；
- Opening 可以生成叙事，却可能不留下下一回合可持续的局势对象。

这些不是互不相关的小缺陷，而是说明“玩家输入是世界唯一驱动力”“尽量预防模型错误”“正式世界变化必须经过大量窄门”已经进入多个基础抽象。

### 2.2 修补成本开始超过重建核心层成本

继续修复需要同时维护：

- 旧 Runtime 的大量契约与兼容假设；
- 1000+ 自动化测试的既有语义；
- 多层状态机、恢复与事务语义；
- Provider / Semantic / Narrative / Materializer 之间的历史职责分割；
- 为旧产品哲学设计的 typed mutation 与 validator。

而新的产品哲学要求：

- AI GM 默认拥有较高主持自由；
- 低成本错误优先 Recovery；
- 世界主动性不依赖玩家输入；
- 核心玩法先证明，再增加必要 Guardrail；
- 尽量复用成熟 Agent / Harness 的文件、工具、上下文与恢复能力。

两者之间已经出现明显结构摩擦。

---

## 3. 第二版哪些成果应继承

后继项目不默认复制第二版 Runtime，但以下成果仍有高价值：

### 3.1 产品与流程经验

- Primary Purpose / Core Value 必须先于架构；
- 复杂系统在核心用途上不能明显输给简单基线；
- Early Owner Reality Check；
- Engineering Correctness != Product Value；
- Fixture Pass != Real Product Vertical；
- Integration of Meaning；
- Decision Propagation；
- Agent 任务包、独立审核、exact-SHA Git 治理。

### 3.2 Recovery 相关经验

- Save / Restore；
- checkpoint / revision；
- crash recovery；
- stable identity；
- branch / rollback 思想；
- Source 与本局状态隔离。

这些能力的正确产品解释从“让模型绝不出错”转为：

> **让模型可以大胆工作，因为普通错误便宜、可撤回、可恢复。**

### 3.3 可复用资产与领域成果

- 已整理的汉末三国世界资产；
- Character / World / Expansion 的内容经验；
- 通用核心扩展应先于专用扩展的依赖教训；
- 世界包、角色包、拓展包的语义设计与素材可作为后继项目内容来源。

---

## 4. 哪些第二版抽象不得默认继承

以下能力若进入后继项目，必须重新证明必要性，而不是因为第二版已有实现就迁移：

- 当前 `FormalTurnSubmissionFlow` 主干；
- 大范围 Player Authorization 链；
- `RuntimeMaterializationNeed` 当前模型；
- Candidate Directory / Candidate exact-reference 思维；
- 复杂 Continuity decision contract；
- 大量剧情语义 Validator；
- 为防低成本模型错误建立的细粒度 typed mutation；
- 将 GM 创作拆成大量机械工具调用的流程；
- 任何“模型可能犯错，所以先限制输出空间”的默认设计。

继承证明至少回答：

1. 防的是真实高成本风险还是理论错误；
2. Recovery 为什么不足；
3. 是否伤害 GM 主动性与自然交互；
4. 是否有更窄、更简单的边界。

---

## 5. 后继项目启动假设：成熟 DeepSeek Harness + 游戏插件层

Project Owner 当前倾向的下一代方向记录为**启动假设，不在本裁定冻结最终架构**：

> 以可高度自定义、高度插件化、已经成熟稳定的 DeepSeek Agent Harness 作为基础运行环境，把通用 Agent 已经擅长的本地文件操作、上下文维护、资产读写、存档维护、工具调用与恢复能力交给 Harness；游戏项目自身主要开发真正具有产品差异化的游戏插件。

初步职责分割：

```text
成熟 DeepSeek Agent Harness
├─ 模型会话 / Context
├─ 本地文件与目录工具
├─ 资产读写
├─ 存档 / checkpoint / 恢复
├─ Agent 工具执行
├─ 通用错误恢复
└─ 插件 Host

The World 游戏层
├─ GM 主持流程插件
├─ RPG 规则 / 判定 / 数值元素插件
├─ 世界与角色资产使用约定
├─ 拓展包机制插件
├─ 必要的 Player Agency / 隐私 / 高影响规则边界
└─ 游戏专用 UI / 体验层（按真实需求逐步增加）
```

核心转变：

```text
第二版：
我们自己建设一个 Runtime，让模型在 Runtime 允许范围内主持

后继假设：
成熟 Agent Harness 已经能运行、读写、维护和恢复
→ 我们只建设“怎样成为一个好的 RPG / GM 系统”的增量能力
```

这有望显著减少自研基础设施、状态同步、Tool glue 与重复 Agent 能力，把工程预算重新集中到游戏核心价值。

---

## 6. 第三版启动时的强制原则

后继项目不得因为上述方向“看起来更轻量”就直接大规模实现。

第一阶段只需证明最小真实纵向：

```text
真实世界资产
+
真实玩家角色
+
强模型 GM
+
GM 流程插件
+
最小 RPG 规则插件
+
Harness 原生文件 / 存档能力
↓
连续真实游玩 10–20 回合
```

必须尽早验证：

- AI 是否主动主持；
- 新 NPC / 地点 / 局势能否自然出现；
- 玩家选择是否产生可感知后果；
- 模型是否能自然维护本地世界与存档；
- 回档 / 修正是否低摩擦；
- 插件是否增强游戏，而不是重新包住模型；
- 与“直接让强模型主持”相比是否真正增加价值。

只有真实失败反复出现且 Recovery 不足时，才为该失败增加新的 Prevention 机制。

---

## 7. 最终裁定

第二版不再以“尚未修完”定义，而以“已经完成其学习价值”定义。

它证明了：

> **可靠 Runtime 可以被造出来，但如果基础产品哲学错误，可靠性越深入，后续纠偏成本可能越高。**

后继项目的目标不是证明我们能再次设计一个更漂亮的 Runtime，而是尽快证明：

> **在成熟 Agent 基础上，用最少的自研游戏机制，能不能做出一个真正让人愿意持续玩的 AI RPG。**
