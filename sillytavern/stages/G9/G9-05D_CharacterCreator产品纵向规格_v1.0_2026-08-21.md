---
title: G9-05D Character Creator 产品纵向规格
status: current-spec-frozen
version: 1.0
date: 2026-08-21
stage: G9-05
slice: G9-05D
requires:
  - G9-05D0 Character Profile Fields 增量裁定 v1.0
---

# G9-05D｜Character Creator 产品纵向规格 v1.0

## 1. 目标

G9-05D 在已关闭的 G9-05B Shared Creator Core 与 G9-05C World Creator 产品模式之上，实现第二个真实主资产 Creator：Character Creator。

目标链：

```text
我的资产库
→ Creator / 角色卡
→ 新建角色卡 / 导入角色创作稿 / 从已有角色卡创建新版本
→ Character Creator Draft Workspace
→ 手工编辑 + 受控 AI 协作
→ Import evidence / unresolved / conflict
→ ChangeSet / Undo
→ 确定性校验
→ 用户显式 Publish
→ Source Asset Library
→ 角色卡详情 / 版本历史 / 创建新版本
```

G9-05D 不复制 World Creator 的 World-only composition 语义；它必须依据 Character Source schema 单独设计。

## 2. 永久边界

继续保持：

```text
Creator Draft != Saved Source Asset
Saved Source Asset != Game-local Canonical Instance
Game-local Canonical Instance != Runtime State
AI Chat != Creator Draft
AI edit != Publish
```

Character 特别边界：

```text
Character Source Definition
!= materialized Character
!= current player character
!= current location
!= current relationship state
!= current knowledge
!= current injury / health
!= current office / faction / allegiance
!= current runtime state
```

`playerCharacterSupported = true` 只表示 Source 声明“该角色定义支持未来被选择为玩家角色”，不执行选择、控制或实例化。

G9-05D 禁止：

- 创建 Runtime Character；
- 修改任何当前游戏的人物状态；
- 因发布角色卡而把人物放进场景；
- 自动生成关系数值、位置、知识、伤势、能力当前值或当前承诺；
- 把 Character Creator 变成 Character materializer；
- 建第二套 Draft / Store / Provider settings / Publish transaction / asset protocol；
- 提前实现 Expansion Creator。

## 3. 产品信息架构

### 3.1 我的资产库

G9-05D 后：

```text
我的资产库
├── Creator       可用
├── 世界包        可用
├── 角色卡        可用
└── 拓展包        后续开放
```

角色卡入口进入已发布 Character Source Library。

### 3.2 Creator 首页

Creator 首页从“只展示 World Draft”升级为主资产 Creator Hub，至少展示：

- World Draft；
- Character Draft；
- 每份 Draft 的资产类型、标题/显示名、来源、生命周期、目标版本、更新时间。

开始创作区域至少提供：

```text
新建世界包
导入世界创作稿
新建角色卡
导入角色创作稿
从已有世界包创建新版本
从已有角色卡创建新版本
```

外部创作稿的目标资产类型必须由用户在入口层明确选择；AI 不得自行把同一文件偷偷判定成 World 或 Character 后直接发布。

## 4. 三种 Character 创作起点

### 4.1 空白创建

调用现有 Creator Core：

```text
createDraft({
  assetType: 'character',
  origin: { kind: 'blank' }
})
```

`targetAssetRef` 由 Program 生成并保持稳定，UI 不可编辑。

### 4.2 外部角色创作稿导入

第一版继续支持：

```text
.md
.txt
```

流程：

```text
用户明确选择“导入角色创作稿”
→ create character Draft(imported_manuscript)
→ importRaw()
→ 原稿持久化
→ Character Import Organizer（若 Provider 可用）
→ only certain + unique + evidence-backed + blank targets apply
→ unresolved/conflict 保留
→ Character Workspace
```

允许自动填写的第一版正式字段以 G9-05D0 为准：

- `metadata.title`；
- `metadata.summary`；
- `metadata.language`；
- `character.displayName`；
- `metadata.aliases`；
- `character.playerCharacterSupported`；
- semantic sections。

不自动从任意自然语言创作稿生成：

- Runtime position；
- current relationship；
- current knowledge；
- current injury / health；
- current capability profile；
- current allegiance / office；
- Library exact references；
- formal dependencies。

后两类如果无法 exact 映射，留在 unresolved / evidence，之后由玩家手工或受控 AI 在 Creator 中继续整理。

Provider 未配置时：原稿必须仍然保存，Character Draft 可手工继续编辑。

### 4.3 从已有角色卡创建新版本

使用现有 `CreatorPublicationService.createSourceRevisionDraft()`。

要求：

- exact base snapshot；
- 继承同一 `assetRef`；
- 旧 Source 永不修改；
- `metadata`、`displayName`、已有 `playerCharacterSupported`、sections、referenceSources、dependencies、compatibility 全部经现有 Source→Draft 路径保留；
- 新版本号不自动猜测。

## 5. Character Creator Workspace

采用与 World Creator 一致的产品骨架：

```text
结构化主工作区
+
AI 协作侧栏
+
导入审阅
+
发布区
```

但正式字段是 Character-specific。

### 5.1 基础信息

至少支持：

- `metadata.title`；
- `character.displayName`；
- `metadata.aliases`；
- `metadata.summary`；
- `metadata.language`；
- `character.playerCharacterSupported` 三态；
- `targetVersion`。

`targetAssetRef` 只读。

### 5.2 别名语义

G9-05D 第一版 UI 中“别名”对应 `metadata.aliases`，与当前 G9-04 canonical Character mapping 保持一致。

若 base Source 已包含 `payload.aliases`：

- 现有值继续由共享 Core 保留；
- 本阶段不提供第二套 payload alias 编辑器；
- 不静默同步或覆盖；
- 不因为 UI 不展示而删除该字段。

### 5.3 Player Character Support

UI 必须是明确三态：

```text
未声明
支持作为玩家角色
不支持作为玩家角色
```

不得使用普通 checkbox 将 `undefined` 和 `false` 混为一谈。

旁边必须有简短产品说明：该选项只描述资产能力，不会把角色加入当前游戏，也不会自动成为玩家角色。

### 5.4 Character Sections

复用 G9-05C 已验证 section 编辑模式：

- create；
- edit；
- delete；
- `sectionKind`；
- title；
- body；
- visibility；
- existing `sectionRef` 只读稳定 identity；
- 新 section 创建时定义一次 semantic `sectionRef`。

不冻结具体人物章节 ontology。Creator 可以用于历史人物、虚构 NPC、可接管角色等不同 Character Source。

### 5.5 Reference Sources

对应 `payload.referenceSources[]`，使用现有 `character_reference_source` typed node。

支持完整 create / edit / delete：

- `referenceRef`；
- `libraryEntryRef?`；
- `note?`。

规则：

- `referenceRef` 是正式 stable ref，不用显示标题猜 identity；
- existing typed node 的 Program `nodeRef` 不暴露为可编辑业务字段；
- `libraryEntryRef` 第一版只接受 exact 手工 ref；
- 不提供 Library fuzzy search；
- 不声称 Library entry 一定存在，除非现有 protocol/catalog 有对应 proof；
- 不触发 Runtime Library retrieval。

### 5.6 Character Dependencies

Character Source 可使用现有 dependency seam，但 Product/UI/AI 只允许：

```text
hard
optional
reference
```

禁止：

```text
feature_conditional
sourceScope
```

原因同 World：G9-03 conditional dependency 是 Expansion source feature/module 语义。

依赖编辑支持：

- create；
- edit；
- delete；
- dependencyRef；
- kind；
- exact target assetRef；
- target assetType；
- requiredCapabilityRefs。

真实用途包括角色卡对能力核心、世界、资料或其他资产的稳定依赖/引用，不表示当前角色已经物化或激活能力。

### 5.7 Compatibility

第一版继续以 Source revision 精确保留为主。没有现有正式 mutation seam 的 compatibility 对象不得在 UI 层绕过 Core 自建 patch。

## 6. 手工保存 / CAS

继续使用现有 revision/CAS。

所有字段、sections、referenceSources、dependencies 必须通过 Creator Core 正式 mutation seam。

UI mutation 失败时保留玩家未成功提交的表单内容，不得因服务端拒绝或 stale revision 清空输入。

## 7. Character AI 协作

### 7.1 独立 Character Provider Adapter

不得把当前 World-only DeepSeek adapter 改成“猜 assetType 的万能工具”。

优先建立 Character-specific adapter（命名可按项目约定），继续复用同一 Strict Tool Provider 网络边界和现有 API 设置。

Character adapter 只对 `assetType = character` 工作。

### 7.2 Character Authoring Tool Scope

允许的 typed operations 只包含 Character 当前产品真实支持的字段：

- text scalars：title / summary / language / targetVersion / character.displayName；
- G9-05D0 aliases operation；
- G9-05D0 player-supported operation；
- section upsert；
- Character dependency upsert（hard/optional/reference）；
- `character_reference_source` typed node upsert。

Program 仍执行：

- runtime parsing；
- exact task scope；
- partial apply；
- CAS；
- ChangeSet。

AI 无 publish 权限。

### 7.3 UI 精确作用范围

用户应能显式选择：

- 基础字段；
- alias；
- player-character support；
- exact section；
- exact existing reference source；
- exact existing dependency。

`targetVersion` 默认不授权。

不得使用 wildcard / 任意路径 / JSON Pointer。

### 7.4 AI 创建新复杂节点

第一版不要求 AI 自动创建新的 reference source / dependency 节点。玩家可先手工建立正式节点，再授权 AI 修改其 note/字段。

Import Organizer 也不自动猜 Library exact ref 或 dependency identity。

## 8. 导入审阅

复用 World Creator 已验证的 evidence / mapping / unresolved / conflict 模式。

可定位 formal targets 至少包括：

```text
metadata.title
metadata.summary
metadata.language
metadata.aliases
character.displayName
character.playerCharacterSupported
section:${sectionRef}
```

要求：

- exact existing identity 才显示“去这里继续创作”；
- unresolved 没有正式 target 时显示“未定位”；
- conflict candidate 只是证据，不可点击后直接写入 Draft；
- alias/boolean conflict 不自动选择；
- `section:${sectionRef}` 继续使用 G9-05C 已修正的 exact locator 规则。

## 9. ChangeSet / Undo

所有 AI 任务和 D0 profile field operations 都必须保持现有 ChangeSet / inverse / Undo 语义。

别名和三态 player-supported 必须能安全 Undo。

发生 `CREATOR_UNDO_CONFLICT` 时不得强覆盖后续玩家修改。

## 10. 发布与 Source Library

发布继续调用：

```text
compileCreatorDraftForPublication()
→ computeAssetDigest()
→ validateAndVerifyAsset()
→ SourceAssetLibraryStore.appendValidated()
```

Character 发布最低必填：

- targetVersion；
- metadata.title；
- character.displayName；
- protocol-valid sections（允许空数组，若协议允许）；
- 其它存在字段必须类型合法。

发布成功后进入 Character Source Library。

### 10.1 Character Source Library

至少提供：

- 已发布角色卡列表；
- displayName / title；
- version；
- summary；
- assetRef；
- 详情页；
- exact version history；
- 从 exact snapshot 创建新版本。

详情页至少展示：

- 基础 metadata；
- displayName；
- aliases；
- playerCharacterSupported 状态；
- sections；
- referenceSources；
- dependencies。

### 10.2 Catalog compatibility proof

至少提供 synthetic proof：

```text
published Character
+ exact hard target asset(s)
→ existing validateAssetCatalog()
→ PASS
```

同时证明：

- optional missing 只产生 gap；
- reference missing 不阻断；
- Character Product 无法产生 `feature_conditional`。

## 11. No-Phantom / Runtime Isolation Gate

这是 Character Creator 的强制验收，不是普通附加测试。

至少证明：

```text
创建 Character Draft
编辑
导入
AI authoring
发布 Character Source
打开 Character Source detail
创建新版本 Draft
```

全过程不会：

- 创建当前 Runtime Character；
- 改变当前 session turn/revision；
- 写入人物当前位置；
- 写入关系状态；
- 写入知识状态；
- 写入伤势/Health；
- 把 `playerCharacterSupported=true` 解释为 player selection。

G9-04 的 `Character Binding != Character materialization` 继续成立。

## 12. 路由建议

在当前产品路由体系内新增等价路由：

```text
/assets/creator/character/:draftRef
/assets/characters
/assets/characters/:assetRef/versions/:version
```

不迁移前端框架。

## 13. Required Acceptance Criteria

### AC-D-01
我的资产库：Creator / 世界包 / 角色卡可用，拓展包仍明确后续开放。

### AC-D-02
Creator 首页同时展示 World 与 Character drafts，并提供明确的 Character 新建/导入入口；不让 AI 猜目标资产类型。

### AC-D-03
空白 Character Draft 使用 Program-minted stable `targetAssetRef`。

### AC-D-04
`.md/.txt` 角色创作稿原文先持久化；无 Provider 时仍可手工继续。

### AC-D-05
Character Import 能对明确、有证据且空白的 title / summary / language / displayName / aliases / player-supported / sections 自动填写；不确定/冲突留空。

### AC-D-06
基础字段与 `playerCharacterSupported` 三态可真实保存、CAS、Undo；false 不等于 undefined。

### AC-D-07
Character sections 完整 create/edit/delete；existing sectionRef 只读。

### AC-D-08
referenceSources 完整 create/edit/delete，使用 existing typed node seam，无 fuzzy Library lookup。

### AC-D-09
Character dependencies 完整 create/edit/delete；只允许 hard/optional/reference，HTTP/UI/AI 均 fail closed 拒绝 feature_conditional/sourceScope。

### AC-D-10
Character AI 使用 exact task scope；越权/畸形 operation 单项 ignored，同批合法项保留。

### AC-D-11
ChangeSet / Undo 可见且不覆盖后续玩家修改。

### AC-D-12
发布为 exact `TavernAssetV1 character`，Source Library 可读取、查看版本历史并从 exact snapshot 创建新版本。

### AC-D-13
published Character + synthetic targets 通过 existing `validateAssetCatalog()`。

### AC-D-14
No Provider：手工创建、保存、发布、重开完整可用。

### AC-D-15
Runtime / No-Phantom：Character Creator 全链不 materialize Character，不修改当前 Runtime state；`playerCharacterSupported=true` 也不例外。

### AC-D-16
G9-05B / G9-05C existing gates、typecheck、lint、product build、launcher smoke 不回退；自动化 Provider calls = 0。

## 14. Out of Scope

本阶段不做：

- Expansion Creator；
- Character runtime materialization；
- 玩家角色选择流程；
- T0 Historical Resolver；
- Character Knowledge Runtime；
- Relationship Runtime；
- Health/Injury Runtime；
- Capability Bootstrap mechanics；
- Library product lookup / search / retrieval；
- avatar / image generation pipeline；
- arbitrary relationship graph editor；
- Creator UI framework migration。

## 15. Exit Gate

```text
P0 = 0
P1 = 0

G9-05D Character Creator Vertical = PASS / CLOSED
↓
Expansion Creator Vertical = AUTHORIZED / NEXT
```

只有通过 exact-SHA Independent Review 后才允许推进 Expansion Creator。
