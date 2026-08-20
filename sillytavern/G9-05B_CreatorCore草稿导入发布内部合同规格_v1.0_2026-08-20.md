---
title: G9-05B Creator Core 草稿 / 导入 / 发布内部合同规格
status: current-spec-frozen
version: 1.0
date: 2026-08-20
stage: G9-05
slice: G9-05B
depends_on:
  - G9-05A_Creator基础模型与创作稿导入产品架构裁定_v1.0_2026-08-20.md
  - 19_酒馆游戏_CreatorConversationalAuthoring与AI协作式结构化创作裁定_v1.0_2026-08-19.md
  - G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md
  - G9-04_LegacyMarkdownAdapterCompilerBinding规格_v1.0_2026-08-20.md
next_gate: shared Creator Core implementation + exact-SHA independent review
---

# G9-05B｜Creator Core 草稿 / 导入 / 发布内部合同规格 v1.0

## 0. Outcome

G9-05A 已冻结产品入口与权限边界。本规格继续冻结“共享 Creator Core”内部合同，使后续世界包、角色卡、拓展包 Creator 复用同一套草稿、导入、AI 修改、撤销、发布与持久化基础。

正式主链：

```text
我的资产库 / Creator
↓
blank | imported_manuscript | source_revision
↓
CreatorDraftV1
↓
manual edit / import organize / normal AI authoring
↓
revisioned change-set + autosave
↓
Program-owned draft publication compiler
↓
existing G9-03 validation / canonicalization / SHA-256
↓
validated TavernAssetV1
↓
SourceAssetLibraryStore
↓
我的资产库：世界包 / 角色卡 / 拓展包
```

本规格不实现 Runtime bind，不修改现有游戏，也不重新设计 `tavern.asset.v1`。

---

## 1. Owner Map

G9-05B 新增两个长期 Owner，必须分开：

### 1.1 Creator Draft Owner

```text
资产创作模块
= Creator Draft / Import Artifact / AI edit / Change-set / Undo / Publish Flow Owner
```

它拥有创作期工作状态，但不拥有正式 Source Asset authority，也不拥有 Runtime State。

### 1.2 Source Asset Library Owner

```text
资产库模块
= 已通过 G9-03 校验的本地 Source Asset 存储 / 枚举 / 精确快照读取 Owner
```

该“资产库”是用户产品数据，不等于开发仓库 `sillytavern-assets`，也不等于 G9-02 Game-local Source Binding。

### 1.3 Runtime Owner 不变

```text
Creator Draft
!= Source Asset Library
!= Game-local Canonical Instance
!= Runtime State
```

Creator 发布只把 Source 写入“我的资产库”；真正创建游戏或重新绑定游戏仍走 G9-03 / G9-04 / G9-02 的精确快照轨道。

---

## 2. 模块边界

建议生产代码模块：

```text
src/资产库/
src/资产创作/
```

二者均采用现有 L0 / L1 / L2 / L3 分层。

### 2.1 资产库模块

允许依赖：

- `资产协议/L3_外交层/资产协议公开接口`

不得依赖：

- Runtime 内部状态；
- Creator 内部状态；
- UI 组件。

### 2.2 资产创作模块

允许依赖：

- `资产协议/L3_外交层/资产协议公开接口`；
- `资产库/L3_外交层/资产库公开接口`；
- Provider 公开端口。

不得：

- 跨模块读取 `游戏创建` L0/L1/L2 内部类型；
- 直接访问 Runtime SQLite / hidden state；
- 直接写正式资产存储表；
- 直接发布未经 G9-03 验证的对象。

旧 `游戏创建` 模块只作为已经证明过的设计经验，不成为 Creator 的隐式数据模型。

---

## 3. CreatorDraftV1｜草稿身份与生命周期

Creator Draft 必须拥有独立 schema，例如：

```text
creator-draft.v1
```

最低身份合同：

```text
schemaVersion
+ draftRef
+ assetType
+ targetAssetRef
+ targetVersion?          // 发布前可尚未确定
+ origin
+ lifecycle
+ createdAt
+ updatedAt
+ revision
+ content
+ workState
```

### 3.1 assetType

G9-05 首版只允许：

```text
world
character
expansion
```

`library` 不进入本阶段 Creator。

`assetType` 在草稿创建后不可由 AI 修改，也不可通过普通编辑操作变更。若用户选错类型，应新建另一份草稿。

### 3.2 targetAssetRef

对新建资产（blank / imported_manuscript）：

```text
Program 在 Draft 创建时生成稳定 targetAssetRef
```

禁止：

- 从标题生成；
- 从文件名生成；
- 从别名生成；
- 由导入 AI 猜测；
- 因用户修改标题而改变。

对 `source_revision`：

```text
targetAssetRef = baseSnapshot.assetRef
```

并保持不变。

具体 ref 字符串格式由 Program identity port 负责，但必须满足 G9-03 `assetRef` 合同并全局稳定。

### 3.3 targetVersion

草稿可以在创作阶段暂时没有发布版本。

发布前必须存在非空 `targetVersion`。

权限：

- 用户可以设置；
- Program 可以提供可见默认建议；
- 导入 AI 不得从外部文稿标题/文件名自行决定版本；
- 普通 AI 创作任务除非用户明确要求“设置发布版本”，否则不得修改。

对 `source_revision`，发布版本不得与基础快照版本相同。

### 3.4 origin

必须是以下三类之一：

```text
blank
imported_manuscript
source_revision
```

语义：

```text
blank
= 用户从空白创建

imported_manuscript
= 草稿由一个或多个外部创作稿起步

source_revision
= 从一个 exact 已发布 Source snapshot 创建新版本
```

`source_revision` 必须保存：

```text
baseSnapshot
= assetRef + assetType + version + contentHash
```

不得只保存 display title。

### 3.5 lifecycle

冻结：

```text
editable
publishing
published
```

`published` 草稿本身只读；继续修改必须从已发布 Source Asset 创建新的 `source_revision` 草稿。

---

## 4. revision｜所有修改均为 CAS 自动保存

Creator 必须继承现有 Creation Project 已证明的 revision/CAS 模式。

每次被接受的变更：

```text
basisRevision = N
↓
Program validate mutation
↓
produce revision N+1
↓
store.write(next, basisRevision=N)
```

只有持久化成功后，调用方才能收到成功结果。

若 CAS 失败：

```text
CREATOR_DRAFT_STALE_REVISION
```

并返回最新持久化草稿；不得覆盖较新的玩家修改。

### 4.1 迟到 AI 结果

所有 AI 请求必须携带：

```text
draftRef
basisRevision
taskRef
```

AI 返回时若当前 `revision != basisRevision`：

```text
丢弃旧结果
不得自动重放
不得覆盖新草稿
```

用户可显式重新发起该任务。

---

## 5. Draft Content｜可不完整，但不得形成第二资产本体

`content` 是“面向未来 `TavernAssetV1` 的可不完整结构化内容”。

原则：

1. 字段语义尽可能与 G9-03 同名、同类型、同层级；
2. 允许发布必需字段暂时缺失；
3. Creator 工作信息不混入 `content`；
4. `integrity` 不属于 Draft，由发布时 Program 计算；
5. 不允许 arbitrary JSON sidecar 作为隐藏 Source truth。

### 5.1 metadata

草稿可不完整表达：

```text
title?
aliases?
language?
tags?
summary?
```

发布时 `title` 必须满足 G9-03。

### 5.2 dependencies / compatibility

草稿可以编辑正式 G9-03 dependency / compatibility 语义，但列表项必须有稳定 Creator node identity 以支持修改与撤销。

Creator node identity：

```text
只用于 Draft 工作
!= published dependencyRef / featureRef / moduleRef 本身
```

发布时 Creator-only node identity 被剥离。

### 5.3 三类 payload

分别使用可不完整的：

```text
World Draft Payload
Character Draft Payload
Expansion Draft Payload
```

它们必须镜像 G9-03 正式 payload 语义，而不是重新定义“Creator 专用世界观字段体系”。

对于列表型正式对象（sections / composition / referenceSources / features / modules / UI declarations 等），Draft 可以暂时存在不完整节点，但发布转换必须在构造 `TavernAssetV1` 前确认所有被发布节点可形成合法正式对象。

---

## 6. Program-owned Typed Creator Targets

Creator 不允许 AI 直接发送 JSON Patch、任意对象路径或脚本。

所有编辑目标必须来自 Program-owned typed registry / typed operation union。

正式原则：

```text
AI describes intended typed change
→ Program resolves exact allowed target
→ Program validates lifecycle / ownership / type / cardinality
→ Program mutates Draft
```

禁止：

- arbitrary `statePath`；
- arbitrary JSON Pointer；
- `eval` / expression / callback；
- AI 直接提交完整替换 Draft；
- AI 自行添加未知字段。

### 6.1 Creator 核心操作族

共享核心至少需要表达以下语义操作：

```text
set / clear scalar draft value
upsert / remove semantic section
upsert / remove dependency
upsert / remove typed list node
set targetVersion（仅显式授权）
```

Expansion 的 Feature / Module / UI 节点、World composition、Character reference source 等在各自资产纵向中通过同一 typed operation foundation 扩展，不另建工具系统。

### 6.2 永久保护字段

普通 AI 工具不得修改：

```text
draftRef
assetType
targetAssetRef
origin
baseSnapshot
createdAt
revision
lifecycle
raw import artifact bytes/hash
published snapshot
```

---

## 7. ChangeSetV1｜每个创作任务形成一个可观察变更集

用户手工动作、AI 创作任务、AI 导入整理都必须最终形成 Program-owned change set。

最低合同：

```text
changeSetRef
taskRef?
actor = user | ai | system
basisRevision
resultRevision
operations
touchedTargets
createdAt
undoState
```

AI 一次任务即使修改多个字段，也形成一个任务级 change set。

UI 应能显示：

- 修改了哪些内容；
- 哪些输出被忽略；
- 哪些字段因保护/冲突未修改。

### 7.1 Undo

Undo 不回退 `revision` 数字。

正式语义：

```text
undo(changeSetRef)
= 应用该 change set 的 Program-owned inverse 变更
→ 创建新的 revision
```

若 change set 所触及的目标在之后已被其它 change set 改动，且 inverse 会覆盖较新内容：

```text
CREATOR_UNDO_CONFLICT
```

失败关闭，不删除后续修改。

第一版 UI 可优先提供“撤销上一项 AI 任务”，但底层合同不得通过直接把整份 Draft 回滚到旧 revision 来抹掉其它修改。

---

## 8. ImportArtifactV1｜外部创作稿原始证据独立保存

导入文件不直接塞进正式 Draft content。

Program-owned Import Artifact 最低包含：

```text
importRef
contentHash (sha256)
originalFileName
format
createdAt
rawContent / exact recoverable bytes
extractedText
segments[]
```

### 8.1 第一版格式

关闭条件至少支持：

```text
.md
.txt
```

其它文本格式后续增量；PDF / OCR / 网页抓取不属于 G9-05B 关闭条件。

### 8.2 segment

每个可供 AI 引用的片段必须有稳定 `segmentRef`。

Markdown 至少保留标题层级/正文位置；纯文本至少保留段落或行区间。

AI 输出只能引用真实存在的 `segmentRef`，不得伪造来源。

### 8.3 导入内容是数据

继续冻结：

```text
Imported Content
!= Creator Instruction
!= Tool Authorization
!= Publish Authorization
```

Provider request builder 必须把导入文本置于明确的数据区，而不是拼入系统指令区。

---

## 9. Import Organization Output｜确定项 / 待整理 / 冲突三分

AI 导入整理输出必须是结构化结果，不接受自由文本作为直接修改命令。

最低语义：

```text
certainAssignments[]
unresolvedItems[]
conflicts[]
```

### 9.1 certainAssignments

每项必须包含：

```text
typedTarget
proposedValue
evidenceSegmentRefs[]
```

Program 只允许：

- target 存在；
- 类型正确；
- target 当前允许 AI 导入填写；
- evidence refs 全部真实存在；
- 不违反 identity / lifecycle / protected field 边界。

### 9.2 只填空白目标，不静默覆盖已有内容

导入整理模式冻结：

```text
Draft target 当前为空
+ AI 给出确定映射
→ 可填写
```

若目标已有用户或先前创作内容：

```text
不得静默覆盖
→ 保持原值
→ 若新材料不一致，记录 unresolved / conflict
```

这与正常 Creator 创作模式不同。用户在正常创作中可以明确要求 AI 重写已有字段。

### 9.3 unresolvedItems

原因至少区分：

```text
insufficient_information
ambiguous_target
unsupported_structure
```

对应正式字段保持为空或原值不变。

### 9.4 conflicts

冲突必须保留多个候选及各自 evidence refs。

Program / AI 不替用户选胜者。

### 9.5 局部应用

同一 AI 导入任务中：

- 合法确定 assignments 正常应用；
- 非法/未知 assignments 被忽略并记录；
- unresolved / conflicts 正常保存到 `workState`；

整个结果形成一个 task-level change set。

局部坏项不得迫使合法确定项全部回滚；持久化本身仍必须原子成功。

---

## 10. workState｜创作期信息不进入 Source

Draft `workState` 至少拥有：

```text
importRefs[]
mappings[]
unresolvedItems[]
conflicts[]
validationIssues[]
changeSetHistory / refs
```

这些信息：

```text
Creator work metadata
!= TavernAssetV1 fields
```

发布时全部剥离，除非用户明确把其中某段内容通过正常 Creator 编辑写入正式资产语义字段。

### 10.1 未解决项默认不自动阻止发布

“待完善 / 待整理 / 存在冲突”本身是创作工作提示，不自动成为第二套发布 Validator。

发布是否具备正式资产合法性，最终由：

1. Draft publication compiler 能否构造完整候选；
2. G9-03 validator / integrity；
3. Source Asset Library identity/version conflict gate；

共同决定。

如果某个空字段不是正式协议必需，用户可以选择不补完并发布；Creator 不强迫“世界必须写满所有栏目”。

---

## 11. CreatorDraftStore｜草稿持久化

端口最低合同：

```text
read(draftRef)
list()
create(draft)
write(draft, basisRevision)
```

`write` 必须 CAS。

生产实现可与现有产品数据共用同一个 SQLite 文件，但必须使用独立 Creator 表；该表不是 Runtime State 表。

最低要求：

- WAL；
- `draftRef` 主键；
- revision 列；
- lifecycle 列；
- created/updated 时间；
- schema version 验证；
- JSON 结构读取失败时 fail closed；
- 不把 Provider key / prompt history 塞进 Draft JSON。

---

## 12. CreatorImportArtifactStore｜原始材料持久化

导入原件必须独立于 Draft revision 保存，避免每次编辑复制整份大文件。

端口至少支持：

```text
put(artifact)
read(importRef)
listByDraft(draftRef) 或显式 linkage
```

`contentHash` 使用 SHA-256。

同一 exact 原件可以去重，但用户在某个 Draft 中的 linkage 必须稳定。

删除 Draft 是否删除原始 Import Artifact 不在首版自动执行；禁止因为草稿的一次字段修改就丢掉原件。

---

## 13. SourceAssetLibraryStore｜“我的资产库”正式 Source Owner

这是 G9-05 新增长期产品能力。

只接受：

```text
validateAndVerifyAsset(asset) PASS
```

后的 `TavernAssetV1`。

端口最低能力：

```text
readExact(snapshot)
listByType(world | character | expansion)
listVersions(assetRef)
appendValidated(asset)
```

### 13.1 append-only 版本语义

Key：

```text
assetRef + version
```

规则：

```text
不存在
→ append

存在且 digest 完全相同
→ idempotent success

存在但 digest 不同
→ SOURCE_ASSET_VERSION_CONFLICT
```

绝不覆盖。

### 13.2 存储载体

首版可以使用 SQLite 保存 canonical `TavernAssetV1` JSON 与 snapshot identity；这是产品数据，不是代码仓库文件。

将来增加导出 Markdown / JSON / Bundle 不改变这个 canonical Source authority。

### 13.3 不等于开发资产仓库

```text
SourceAssetLibraryStore
!= zhangchenjia21-dot/sillytavern-assets
```

开发资产仓库仍是项目研发的 canonical semantic samples；用户本地“我的资产库”是产品数据。

---

## 14. 发布编译器｜Draft → TavernAssetV1

Creator 发布必须由 Program 执行确定性转换。

输入：

```text
one exact CreatorDraftV1 revision
```

输出：

```text
TavernAssetV1 candidate
```

步骤：

```text
Draft exact revision
↓
strip Creator-only work metadata
↓
construct target world / character / expansion payload
↓
set schemaVersion = tavern.asset.v1
↓
set targetAssetRef + targetVersion
↓
set placeholder integrity only for digest computation
↓
existing computeAssetDigest()
↓
existing validateAndVerifyAsset()
↓
validated TavernAssetV1
```

禁止实现第二套 hash、第二套资产结构 Validator 或 Creator-only hidden Source fields。

### 14.1 G9-04 Adapter 的关系

G9-04 Adapter 继续负责 Legacy Markdown → `TavernAssetV1`。

Creator Draft 已是结构化工作对象，因此发布不应绕回 Legacy Markdown 再解析。

```text
Creator Draft
→ direct deterministic publication compiler
→ G9-03
```

不是：

```text
Creator Draft
→ Markdown
→ G9-04 Legacy Adapter
```

---

## 15. 发布 exactly-once / Crash Recovery

发布是跨 Draft Store 与 Source Asset Library Store 的产品级提交过程，必须可安全重试。

冻结状态机：

```text
editable
↓ candidate compiled + G9-03 validated
publishing(publicationRef, intendedSnapshot, basisRevision)
↓ appendValidated(asset)
published(snapshot, publishedAt)
```

### 15.1 publicationRef

由 Program 生成并稳定保存，不能由 AI 提供。

### 15.2 重试

若崩溃发生在 Source 已写入、Draft 尚未标记 published：

```text
resume publishing
→ readExact(intendedSnapshot)
→ exact asset exists
→ mark Draft published
```

不得重复产生另一个版本。

若 Source 尚未写入：

```text
retry appendValidated(exact same candidate)
```

若同 `assetRef + version` 已存在不同 digest：

```text
SOURCE_ASSET_VERSION_CONFLICT
→ 不覆盖
→ Draft 返回可修复状态 / 明确错误
```

### 15.3 发布后

`published` Draft 不再原地修改。

用户选择“继续编辑”时：

```text
published snapshot
→ new source_revision Draft
```

---

## 16. AI Provider Port｜Creator 不绑定具体模型

共享 Creator Core 只依赖公开 Provider 端口。

至少区分：

```text
ImportOrganizer
CreatorAuthoringProvider
```

二者可以由同一个真实模型适配器实现，但请求/权限语义不同。

### 16.1 无 Provider

必须存在 `not_configured` 路径：

- 可以新建 Draft；
- 可以导入 `.md/.txt` 并读取原文；
- 可以手工填写；
- 可以保存 / 恢复 Draft；
- 可以确定性校验和发布合法资产；
- AI 按钮返回明确不可用，不破坏 Draft。

### 16.2 Provider 失败

任何 AI 调用失败：

```text
Draft 保持调用前持久化版本
Import Artifact 保留
用户可以继续手工创作
```

---

## 17. AI 导入与正常 AI 创作权限分离

### 17.1 导入整理

只允许：

- 填当前空白且允许导入填写的字段；
- 新增有明确原文证据的 Draft 节点；
- 记录待整理 / 冲突；
- 生成导入摘要。

不允许：

- 创造原文没有的事实；
- 覆盖已有玩家内容；
- 设置身份字段；
- 设置发布状态；
- 发布。

### 17.2 正常 AI 创作

用户明确发起创作任务后，AI 可以对允许的 Draft 内容进行生成、重写、补全。

仍必须：

```text
Typed Creator Operations
+ basisRevision
+ Program validation
+ task-level ChangeSet
+ Undo
```

AI 仍无发布权。

---

## 18. 产品 / HTTP 边界

浏览器 UI 只能消费 `src/产品界面` / Creator 公开门面提供的玩家安全 DTO / HTTP 接口。

前端不得：

- 直接打开 SQLite；
- 直接调用 SourceAssetLibraryStore 内部实现；
- 直接改 Draft JSON；
- 自己实现 G9-03 Validator；
- 自己决定发布是否成功。

UI 的 loading / autosaved / conflict / publish success 必须反映真实服务端 outcome。

---

## 19. 错误族

实现必须提供稳定错误码，至少覆盖：

```text
CREATOR_DRAFT_NOT_FOUND
CREATOR_DRAFT_NOT_EDITABLE
CREATOR_DRAFT_STALE_REVISION
CREATOR_DRAFT_INVALID_TARGET
CREATOR_DRAFT_PROTECTED_TARGET
CREATOR_DRAFT_INVALID_VALUE
CREATOR_IMPORT_UNSUPPORTED_FORMAT
CREATOR_IMPORT_NOT_FOUND
CREATOR_IMPORT_EVIDENCE_INVALID
CREATOR_AI_UNAVAILABLE
CREATOR_AI_FAILED
CREATOR_AI_STALE_RESULT
CREATOR_UNDO_CONFLICT
CREATOR_PUBLISH_INVALID_DRAFT
CREATOR_PUBLISH_PERSISTENCE_FAILED
SOURCE_ASSET_VERSION_CONFLICT
SOURCE_ASSET_NOT_FOUND
```

错误文本可以产品化，但程序逻辑不得依赖本地化 message。

---

## 20. G9-05B 首次实现范围

第一次 Codex 实现只建立共享基础，不先做完整 Creator UI。

必须实现：

1. `src/资产库/` Source Asset Store 公共合同 + 内存/SQLite 实现；
2. `src/资产创作/` Draft / Import / ChangeSet / Publish 公共合同；
3. Draft Store 内存/SQLite 实现与 revision CAS；
4. Markdown / text Import Artifact 提取与持久化；
5. Import Organizer typed output 的 Program 校验与局部应用；
6. protected target / blank-only import rules；
7. stale AI result 丢弃；
8. task-level ChangeSet + Undo conflict gate；
9. Draft publication compiler，复用 G9-03 digest / validator；
10. Source Asset Library append-only collision gate；
11. publish retry / recovery；
12. no-Provider 手工路径；
13. 世界包用 synthetic fixture 做第一个“共享核心可发布”证明；
14. 不改 G9-02 Runtime / G9-03 wire / G9-04 Adapter authority。

第一次实现不要求：

- 四入口最终视觉页面；
- 完整世界包产品编辑器；
- 角色卡产品编辑器；
- 拓展包产品编辑器；
- 真实 Provider UI；
- 资料库 Creator；
- PDF / OCR / Word；
- 创建游戏端到端接线。

---

## 21. 验收矩阵

共享 Core 实现必须至少证明：

### Draft

- blank / imported / source_revision 三 origin 创建成功；
- Program-minted `targetAssetRef` 不受 title/file/AI 改变；
- source_revision exact 保留 baseSnapshot；
- CAS stale write fail；
- AI stale result fail；
- persistence failure 保持旧 Draft。

### Import

- `.md` / `.txt` 原件可恢复；
- segmentRef 可精确引用；
- certain + evidence → 填空白目标；
- missing evidence → ignore/fail stable；
- ambiguous / insufficient → 字段空；
- conflict → 字段空/原值不变 + conflict retained；
- existing player value 不被 import overwrite；
- prompt-like imported text 无工具/发布权限。

### ChangeSet / Undo

- one AI task multi-field → one change set；
- partial valid assignments retained；
- ignored assignments observable；
- undo creates new revision；
- touched target changed later → undo conflict fail closed。

### Source Asset Library

- only validated asset accepted；
- exact same asset/version/digest idempotent；
- same assetRef/version different digest → conflict；
- list world/character/expansion；
- Library not exposed as G9-05 primary Creator category。

### Publish

- incomplete invalid Draft → fail before Source write；
- valid World Draft → exact `TavernAssetV1(world)`；
- digest uses existing G9-03 function；
- published Source store contains exact snapshot；
- retry after Source write before Draft finalization → exactly-once close；
- published Draft immutable；
- published Source does not alter Runtime or existing Game binding。

### No Provider

- manual edit / save / reopen / validate / publish PASS；
- import raw `.md/.txt` PASS；
- AI request reports unavailable without mutating state。

---

## 22. 回归与禁止事项

必须回归：

- G9-04；
- G9-03；
- G9-02 integrated closure；
- G8 Product contracts；
- G5 / G6 / G7；
- full suite；
- typecheck；
- lint；
- product build；
- launcher smoke；
- disclosure；
- `git diff --check`。

Provider calls for shared Core implementation：

```text
0
```

禁止借实现：

- 修改 `tavern.asset.v1` wire；
- 修改 G9-02 Source/Game-local/Runtime 三层权威；
- 修改 G9-04 Legacy Adapter 去迎合 Creator；
- 直接把 Creator Draft 存到 Runtime 表；
- 让 AI 生成 `assetRef`；
- 让 AI 发布；
- 引入 arbitrary JSON patch / eval / script；
- 把 imported prompt 当指令；
- 先开发三套重复 UI。

---

## 23. Exit

当共享 Creator Core implementation 经 exact-SHA Independent Review 满足：

```text
P0 = 0
P1 = 0
```

则：

```text
G9-05B Creator Core Foundation = PASS / CLOSED
↓
G9-05C World Creator Vertical = AUTHORIZED / NEXT
```

最终原则：

> **Creator Core 的任务不是替用户决定作品，而是提供一个可恢复、可追踪、可撤销、不会污染正式 Source 或 Runtime 的创作工作区；AI 只能通过受控结构化操作帮助填写和创作，最终发布必须由 Program 精确编译并通过现有 G9-03 资产协议。**
