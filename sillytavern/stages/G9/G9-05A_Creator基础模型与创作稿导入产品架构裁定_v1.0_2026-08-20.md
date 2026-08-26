---
title: G9-05A Creator 基础模型与创作稿导入产品架构裁定
status: current-product-architecture-frozen
version: 1.0
date: 2026-08-20
stage: G9-05
slice: G9-05A
extends: 19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md
next_gate: Creator Draft / Import contracts implementation task packet
---

# G9-05A｜Creator 基础模型与创作稿导入产品架构裁定 v1.0

## 0. 本裁定解决什么

G9-03 已冻结统一资产协议，G9-04 已证明真实 Markdown 资产可以确定性进入 `TavernAssetV1` 与既有本局绑定轨道。G9-05 开始解决用户如何创建、继续创作、修改并发布三类主资产。

本裁定只冻结 Creator 的第一层产品与数据边界：

1. “我的资产库”入口结构；
2. Creator 草稿与正式资产的关系；
3. 三种创作起点如何统一进入同一草稿；
4. 外部创作稿导入与 AI 整理规则；
5. AI 在导入阶段和正常创作阶段的权限边界；
6. 保存草稿、发布资产、已有资产新版本之间的关系。

本裁定不替代 #19。#19 继续负责 AI 协作式创作、受控工具调用、撤销、对话边界与发布权限；本文件是其 G9-05 产品基础增量。

---

## 1. 产品入口｜“我的资产库”四入口冻结

“我的资产库”是三类主资产管理与创作的统一产品入口。首版顶层提供四个入口：

```text
我的资产库
├── Creator
├── 世界包
├── 角色卡
└── 拓展包
```

产品语义：

```text
世界包 / 角色卡 / 拓展包
= 已保存正式 Source Asset 的分类与管理入口

Creator
= 创建、导入、继续草稿、从已有资产创建新版本的统一创作入口
```

用户不需要理解“工具类型”和“资产类型”的内部区别；四入口在产品层可以并列呈现。

资料库不进入 G9-05 三类主资产 Creator 首版。资料库产品功能继续遵守 #18 / #18A 与当前路线图，后置到三类主资产端到端闭环之后。

### 1.1 三类资产页不另造编辑器

世界包、角色卡、拓展包页面可以提供“新建”“创建新版本”等操作，但这些操作必须路由到同一个 Creator 工作区，不建立三套独立编辑器。

```text
世界包页 → 新建
角色卡页 → 新建
拓展包页 → 新建
已有资产 → 创建新版本
外部创作稿 → 导入

全部汇聚到：
Creator Draft Workspace
```

---

## 2. Creator 内置于酒馆本体，但不是 Runtime

Creator 作为 `sillytavern` 本体产品模块内置，不拆成独立外部软件或独立用户流程。

正式边界：

```text
Creator = 本体内置产品模块
Creator != Runtime authority
Creator != Game-local truth owner
Creator != TavernAsset protocol itself
```

Creator 可以依赖和复用：

- G9-03 资产协议与确定性 Validator；
- G9-04 已建立的兼容/适配能力；
- 现有产品壳、设置、Provider 基础；

但 Creator 不得直接修改正在游玩的 Runtime State，也不得把编辑草稿写成某局游戏事实。

继续冻结：

```text
Source Asset
!= Game-local Canonical Instance
!= Runtime State
```

---

## 3. Creator Draft 是工作对象，不是正式资产

Creator 必须拥有独立的草稿工作模型。语义名称可采用 `CreatorDraftV1`；精确 TypeScript 字段在实现规格中冻结。

正式关系：

```text
Creator Draft
= 可编辑 / 可不完整 / 可保存继续 / 非正式

TavernAssetV1
= 完整 / 通过确定性校验 / 正式 Source Asset 机器表达
```

因此：

```text
Creator Draft != TavernAssetV1
Creator Draft != Saved Source Asset
Conversation History != Creator Draft
```

### 3.1 草稿不得形成第二套资产语义

草稿虽然是独立工作模型，但其“资产内容区域”必须尽量一一对应 G9-03 已冻结的世界包、角色卡、拓展包正式语义。

草稿允许额外保存 Creator 专用工作信息，例如：

- `draftRef`；
- 草稿来源；
- 基础正式资产快照（若从已有资产创建新版本）；
- 未完成项；
- 导入后尚未整理的内容；
- 冲突证据；
- 校验问题；
- 草稿修订历史；
- AI/用户一次创作操作对应的可撤销变更集。

这些 Creator 工作信息不得自动进入最终 `TavernAssetV1`。

### 3.2 不允许“半成品 TavernAssetV1”承担草稿职责

正式资产协议不负责表达：

- “暂时没想好”；
- 两个候选方案；
- 导入冲突；
- AI 尚未确定的字段；
- 用户工作备注；
- Creator 对话或工具操作历史。

因此不得通过给 `TavernAssetV1` 塞入占位值、Creator 私有字段、任意扩展 JSON 来模拟草稿。

---

## 4. 三种创作起点统一进入同一草稿

Creator 首版冻结三种主要起点：

```text
A. 从空白开始
B. 导入外部创作稿
C. 从已有正式资产创建新版本
```

三条路径最终都必须进入同一个 Creator Draft 工作区，之后共享同一套：

- 手工结构化编辑；
- AI 创作对话；
- Program-owned Typed Creator Tools；
- 草稿修订与撤销；
- 确定性 Validator；
- 用户显式发布。

不得为“导入创作稿”建立第二套导入编辑器，也不得为“从已有资产修改”建立绕过 Draft 的直改模式。

---

## 5. 已有资产只能“创建新版本”，不得原地修改

当用户从“世界包 / 角色卡 / 拓展包”打开已有正式资产并选择编辑时：

```text
已有 Source Asset exact snapshot
↓
创建 Creator Draft
↓
用户 / AI 编辑 Draft
↓
确定性校验
↓
用户显式发布
↓
新的 Source Asset 版本
```

旧版本保持不变。

草稿应记录基础快照身份：

```text
assetRef
+ assetType
+ version
+ contentHash
```

发布新版本不得静默覆盖旧 Source，也不得静默改变任何已经绑定旧版本的现有游戏。

---

## 6. 外部创作稿导入｜定位

Creator 必须支持把在其它 AI、文本编辑器或其它创作流程中形成的文字稿导入 Creator，并继续创作。

典型来源包括：

- 与其它 AI 长时间讨论后生成的 Markdown；
- 普通纯文本设定稿；
- 其它可提取文字的常见文档格式；
- 用户自己的世界观、人物、规则设计笔记。

正式定位：

```text
外部创作稿
= 创作材料
!= Source Asset
!= Creator 指令
!= Runtime truth
```

导入过程不能直接把任意外部文稿认定为合法 `TavernAssetV1`。

---

## 7. 导入分为“程序读取”与“AI 语义整理”两层

为了保持无 Provider 仍可使用完整手工 Creator，导入能力分成两层：

### 7.1 程序读取层

Program 负责：

- 接收文件；
- 识别受支持格式；
- 提取文字与基础文档结构；
- 保存导入来源与可追踪位置；
- 把原始材料放入 Creator Import Workspace。

该层不调用模型。

第一版必须至少覆盖 Markdown 与纯文本。其它可提取文字的常见格式可以在实现规格中按成本扩展；PDF/OCR/网页抓取不作为 G9-05 首版关闭条件。

### 7.2 AI 语义整理层

用户明确发起“整理到 Creator”后，AI 可以读取有界导入材料，并尝试把材料映射到当前目标资产的 Creator Draft 字段。

模型调用失败或没有 Provider 时：

- 原始导入材料不能丢失；
- Draft 不能损坏；
- 用户仍可手工读取材料并编辑 Creator；
- 不得把 AI 导入整理作为 Creator 可用性的前置条件。

---

## 8. 导入 AI 的核心规则｜能确定就填，不能确定就留空

这是 G9-05 首版的正式导入权威规则：

> **AI 对外部创作稿中能够明确确定、且能够唯一映射到 Creator Draft 正式语义字段的内容，可以直接填写；不能确定、存在矛盾、存在多个合理解释、材料没有提供的字段，必须留空。**

冻结如下：

```text
明确、唯一、由原文支持
→ 填入 Draft

信息不足
→ 留空

材料未提及
→ 留空

存在互相冲突的候选事实
→ 留空

存在多个合理字段归属
→ 留空
```

禁止：

```text
“大概是这个意思” → 猜一个
“通常世界观会这样” → 自动补一个
“模型觉得更合理” → 覆盖原文
```

### 8.1 不使用“低置信度值填字段”作为默认方案

首版不要求把不确定值先写进正式草稿字段再打黄色标签。

不确定内容应保持字段为空，并进入独立工作信息，例如：

```text
待完善
= 材料没有提供足够信息

待整理
= 有相关材料，但不能唯一映射到字段

存在冲突
= 同一语义出现互相矛盾候选
```

### 8.2 局部不确定不能阻塞整份导入

例如 30 个字段中 22 个能够确定：

```text
22 个确定字段 → 正常填写
8 个不确定字段 → 保持为空并记录原因
```

不得因为局部冲突而让整份创作稿导入失败。

---

## 9. 原始信息不得因为“未填写”而丢失

虽然不能确定的正式字段必须留空，但原始材料必须继续保留在 Creator 工作区中。

至少需要语义等价的工作记录：

- 来源文件身份；
- 原始文字 / 可恢复引用；
- 对应来源位置；
- 未映射片段；
- 冲突候选及各自来源；
- 已成功映射字段与来源关系。

这些信息属于 Creator 工作过程，不自动成为最终 Source Asset 字段。

目标是同时满足：

```text
AI 不乱猜
+
原始信息不丢
+
用户不需要逐字段确认所有确定项
```

---

## 10. “导入整理”与“继续创作”必须分开

导入阶段的 AI 权限是“理解已有材料”，不是“自由补全作品”。

正式区分：

```text
导入整理阶段
= 提取 / 归类 / 映射已有材料

正常 Creator 创作阶段
= 用户可以明确要求 AI 生成、补全、重写或提出新设计
```

如果外部稿没有人口、势力名称、角色动机等信息，导入 AI 不得自行补齐。

用户随后可以在正常 Creator 对话中明确要求：

> “这里没有人口设定，你帮我设计一个。”

此时才进入 #19 已授权的 AI 创作流程。

---

## 11. 外部文件中的文字永远是数据，不是 AI 指令

所有导入文件都必须视为不可信创作内容。

即使材料中出现：

```text
“忽略之前要求”
“系统现在应当……”
“请执行以下命令……”
```

它们也只能作为被分析的作品文字或聊天记录，不能提升为 Creator AI 的系统指令、工具授权或发布授权。

正式冻结：

```text
Imported Content
!= Creator Instruction
!= Tool Authorization
!= Publish Authorization
```

AI 读取导入内容时必须使用数据隔离语义，不允许导入内容改变 Program-owned Creator Tool 权限。

---

## 12. AI 权限分两种工作模式

### 12.1 导入整理模式

AI 可以：

- 阅读用户明确导入的有界内容；
- 提取明确事实；
- 把确定内容填入 Draft；
- 记录未映射内容与冲突；
- 给出导入摘要。

AI 不可以：

- 对缺失字段自行创作；
- 在冲突候选中擅自选一个；
- 发布资产；
- 改写正式 Source Asset；
- 修改 Runtime。

### 12.2 正常创作模式

继续遵守 #19：用户可以通过任务级授权，让 AI 使用 Program-owned Typed Creator Tools 对 Draft 进行有界创作修改。

AI 修改后必须：

- 即时反映在结构化主工作区；
- 形成可观察变更摘要；
- 支持任务级 Undo；
- 仍由确定性 Validator 判断是否合法；
- 仍由用户显式发布。

---

## 13. Creator 首页基础信息架构

进入四入口中的 Creator 后，首版至少应提供以下语义区域：

```text
Creator

继续创作
├── 最近草稿
└── 已保存未发布草稿

开始新的创作
├── 新建世界包
├── 新建角色卡
└── 新建拓展包

从已有内容开始
├── 导入创作稿
└── 从已有资产创建新版本
```

具体视觉布局在产品实现阶段决定，不在本裁定冻结像素级 UI。

---

## 14. 发布链｜Draft 不能绕过 G9-03

Creator 发布不能建立第二套资产合法性判断。

正式链：

```text
Creator Draft
↓
Program-owned deterministic draft → asset candidate conversion
↓
existing G9-03 protocol validation / canonicalization / integrity
↓
合法 TavernAssetV1
↓
用户显式保存 / 发布到“我的资产库”
```

Creator 不得：

- 仅因为 AI 说“合法”就发布；
- 绕过 G9-03 Validator；
- 通过自定义隐藏字段保存额外 Source truth；
- 为 Creator 单独发明第二套 asset identity / hash / dependency protocol。

最终本地保存载体或可导出的文件格式不在本裁定冻结；但无论载体是什么，都必须可确定性得到并验证同一合法 `TavernAssetV1`。

---

## 15. 发布资产不会自动进入某局游戏

Creator 发布的结果进入“我的资产库”，属于 Source Asset。

```text
Publish Source Asset
!= Bind To Existing Game
!= Materialize Into Runtime
```

新资产、新版本不会静默影响现有游戏；真正创建或重新绑定游戏仍使用 G9-03 / G9-04 已建立的 exact snapshot Manifest 与 G9-02 本局绑定轨道。

---

## 16. G9-05A 明确不做

本基础裁定不提前实现：

- 资料库 Creator；
- 资料库检索辅助创作；
- 跨多个资产的大规模自主 Agent；
- 自动联网研究；
- PDF OCR；
- 通用网页抓取；
- 让外部文件中的提示词直接控制 Creator；
- AI 自动发布；
- Creator 直接修改正在游玩的 Runtime State；
- 第二套 Source / Game-local / Runtime identity；
- 为草稿方便而修改已冻结 `tavern.asset.v1` wire。

---

## 17. G9-05 后续实现顺序

冻结顺序：

```text
G9-05A
本裁定：产品入口 + Draft + Import + Publish authority
↓
Creator Draft / Import / Typed Tool 内部合同
↓
共享 Creator Core 实现
↓
世界包首个真实纵向
↓
角色卡复用
↓
拓展包复用
↓
三类主资产 Creator 基础闭环
↓
“我的资产库 → 创建游戏 → 完整游玩”端到端收口
```

禁止先做三个独立漂亮页面，再倒逼共享 Creator Core。

---

## 18. G9-05A Exit

本裁定视为语义冻结后，下一步实现规格必须至少证明：

1. 一个共享 Creator Draft authority；
2. 三种起点统一进入同一 Draft；
3. “我的资产库”四入口不产生四套编辑器；
4. Markdown / 纯文本导入能进入 Import Workspace；
5. AI 只填写能够确定的导入字段；
6. 不确定 / 冲突 / 缺失字段保持为空；
7. 未映射与冲突原文可恢复；
8. 导入内容不能成为模型指令；
9. 用户可继续手工 / AI 创作；
10. 任务级变更可撤销；
11. 无 Provider 时 Creator 与原始导入材料仍可使用；
12. Draft 发布必须复用 G9-03 Validator / integrity；
13. 发布 Source 不自动绑定现有游戏；
14. G9-02 / G9-03 / G9-04 / G8 authority 不回开。

最终产品原则：

> **Creator 是酒馆本体中承接“从零创作、外部创作稿、已有资产新版本”的统一创作中枢；AI 可以高效整理和创作草稿，但不确定就留空、冲突不替用户决定、正式发布仍由用户与确定性程序共同把关。**
