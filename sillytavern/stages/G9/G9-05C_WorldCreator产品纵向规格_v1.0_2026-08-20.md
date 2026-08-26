---
title: G9-05C World Creator 产品纵向规格
status: current-spec-frozen
version: 1.0
date: 2026-08-20
stage: G9-05
slice: G9-05C
---

# G9-05C｜World Creator 产品纵向规格 v1.0

## 1. 目标

G9-05C 是 G9-05B Creator Core 关闭后的第一个真实产品纵向。

它不再建立新的草稿、导入、AI patch、Source Store、发布事务或资产协议，而是把已经关闭的共享核心接到玩家可实际操作的世界包 Creator 工作区。

目标链：

```text
我的资产库
→ Creator
→ 新建世界包 / 导入外部创作稿 / 从已有世界包创建新版本
→ World Creator Draft Workspace
→ 手工编辑 + 受控 AI 协作
→ 导入未决 / 冲突可见
→ ChangeSet / Undo
→ 确定性校验
→ 用户显式 Publish
→ Source Asset Library
→ 世界包详情 / 版本历史 / 创建新版本
```

## 2. 永久边界

继续保持：

```text
AI Chat != Creator Draft
Creator Draft != Saved Source Asset
Saved Source Asset != Game-local Canonical Instance
Game-local Canonical Instance != Runtime State
```

以及：

```text
AI can edit authorized Draft scope
!= AI can publish
```

本阶段不得：

- 建第二套 Creator Draft；
- 建第二套 Source Asset Store；
- 建第二套发布事务；
- 建第二套资产协议；
- 让 Creator 直接写 Runtime；
- 自动绑定现有游戏；
- 让外部创作稿中的命令性文字获得系统/工具权限；
- 因 UI 方便而放宽 G9-05B task-level authoring scope；
- 提前实现 Character / Expansion Creator 正式纵向。

## 3. 我的资产库信息架构

顶层固定为四个入口：

```text
我的资产库
├── Creator
├── 世界包
├── 角色卡
└── 拓展包
```

G9-05C 只要求：

- `Creator`：真实可用；
- `世界包`：真实可用；
- `角色卡`：入口存在，但明确标记后续阶段，不伪造功能；
- `拓展包`：入口存在，但明确标记后续阶段，不伪造功能。

不得把 Creator 做成顶层独立产品入口；它属于“我的资产库”。

## 4. Creator 首页

Creator 首页至少包含：

### 4.1 继续创作

读取 `CreatorDraftStore.list()`，只展示当前 `assetType = world` 的草稿。

至少展示：

- 标题（无标题时显示“未命名世界”）；
- Draft 状态；
- 最近更新时间；
- 当前目标版本（若有）；
- 来源：空白 / 外部创作稿 / 已有 Source 新版本。

点击进入对应 World Creator Workspace。

### 4.2 开始新的创作

提供：

- 新建世界包；
- 导入创作稿；
- 从已有世界包创建新版本。

角色卡 / 拓展包创建入口可以显示“后续开放”，但不得假装已经可用。

## 5. 三种世界包创作起点

### 5.1 空白创建

程序调用现有 `CreatorDraftFlow.createDraft({ assetType: 'world', origin: { kind: 'blank' } })`。

`targetAssetRef` 继续由 Program 生成，一次生成后稳定。

UI 不允许用户通过标题/文件名重写 `targetAssetRef`。

### 5.2 外部创作稿导入

第一版支持：

```text
.md
.txt
```

流程：

```text
选择文件
→ 创建 world Draft(origin = imported_manuscript)
→ importRaw()
→ 原稿立即持久化
→ Provider 可用时 organizeImport()
→ certain assignments 填空白项
→ unresolved/conflict 保留
→ 进入 World Creator Workspace
```

若 Provider 未配置：

- 原始文件仍必须成功保存；
- 草稿仍可打开；
- UI 明确显示“AI 尚未配置，原稿已保存，可稍后整理”；
- 不得因为 AI 不可用阻止手工继续创作。

导入阶段只整理原文，不自动创造缺失设定。

### 5.3 从已有世界包创建新版本

世界包详情页提供“创建新版本”。

必须使用现有 `CreatorPublicationService.createSourceRevisionDraft()`。

要求：

- 精确继承 base snapshot 的 `assetRef`；
- 旧 Source 永不修改；
- 新 Draft 内容来源可追溯到 base snapshot；
- 新 `targetVersion` 可先留空，由用户在 Creator 中明确填写；
- 不自动猜下一个版本号。

## 6. World Creator Workspace

第一版工作区采用“结构化主工作区 + AI 协作区”，具体视觉形式可沿用现有产品技术栈，不冻结前端框架。

不得为本阶段迁移产品 UI 框架。

### 6.1 主工作区

至少支持以下 World Draft 内容：

#### 基础信息

- `metadata.title`
- `metadata.summary`
- `metadata.language`
- `targetVersion`

`targetAssetRef` 只读显示或隐藏，不允许普通用户编辑。

#### 世界章节

对应 `payload.sections[]`：

- `sectionRef`
- `sectionKind`
- `title`
- `body`
- `visibility`

支持：

- 新增章节；
- 编辑已有章节；
- 删除章节；
- public/private 明确可见；
- section identity 使用 Creator node identity + semantic `sectionRef`，不得用显示标题猜 identity。

第一版不强制实现章节拖拽排序；若当前 Core 没有确定性 reorder contract，不得在 UI 层私自直接重排底层数组绕过 mutation seam。

#### 世界组成

对应 `payload.composition[]`。

作为“高级 / 组成”区域，支持：

- `compositionRef`
- target `assetRef`
- target `assetType`
- disposition

必须走现有 typed operation。

#### 依赖

作为“高级 / 依赖”区域，允许创建、编辑、删除 World Draft dependencies。

必须继续走 G9-05B typed dependency operation 和运行时类型校验。

Compatibility 第一版以保留现有 Source 值为主；除非现有 Creator Core 已有正式 mutation seam，否则不要为了 UI 新增任意对象 patch。

### 6.2 草稿保存语义

所有正式修改继续使用 `revision / CAS`。

UI 应显示：

- 已保存；
- 正在提交；
- 发生并发冲突 / 已有更新，需要刷新最新草稿。

不得把“草稿自动保存”与“发布为正式资产”混为同一个按钮。

## 7. AI 协作区

### 7.1 产品交互

AI 区至少提供：

- 输入创作要求；
- 当前作用范围；
- 发送任务；
- 本次变更摘要；
- 被忽略 / 越权 / 非法操作数量；
- 对最近任务执行 Undo 的入口。

### 7.2 作用范围

默认范围应是：

```text
当前选中字段 / 当前选中章节
```

用户可显式扩大到多个区域。

若提供“整份世界包”选项，也必须由 Program 展开成当前草稿中明确的 exact targets / sectionRefs / typedNodeTargets；不得向 Core 传 wildcard、任意路径、JSON Pointer、eval 或通用 patch。

`targetVersion` 默认不属于 AI scope；只有用户明确要求 AI 修改版本且 UI 明确授权时才可加入。

### 7.3 Provider 接线

本阶段允许在 Creator L3 / 产品组装层增加适配器，把现有配置的语义 Provider / DeepSeek 公共能力适配为：

- `CreatorAuthoringProvider`
- `CreatorImportOrganizer`

约束：

- 不修改 G1 Provider 公共协议；
- 不建立第二 Provider 设置系统；
- 复用现有 API 设置 / Provider 生命周期；
- 自动化测试默认使用 deterministic fake / offline provider，不产生真实付费调用；
- Provider 未配置时 AI 区显示不可用，但手工创作、原稿保存、重新打开、发布路径仍完整可用。

模型只能返回 G9-05B 已允许的 typed Creator operations / import organization；Program 继续做最终 scope gate 与 runtime parsing。

AI 无 Publish 工具。

## 8. 导入审阅体验

导入完成后，Workspace 必须让用户看见：

- 已自动填写的内容；
- 未决项；
- 冲突项；
- 原始 evidence segment。

规则继续冻结：

```text
能确定 → 填
不能确定 / 冲突 → 留空
```

对于 unresolved/conflict：

- 不自动选候选；
- 不静默覆盖玩家已有内容；
- 提供跳转到相关字段/章节继续创作的入口；
- 原文证据可查看。

## 9. ChangeSet / Undo

每次 AI 任务必须继续形成一个 Program-owned ChangeSet。

UI 至少显示最近一次任务：

- 修改了哪些区域；
- 忽略了哪些操作；
- 是否可撤销。

Undo 必须调用 G9-05B inverse mutation path。

若发生 `CREATOR_UNDO_CONFLICT`，UI 应明确告知“相关内容后来已被修改，不能直接撤销此任务”，不得强制覆盖。

## 10. 发布

发布必须是显式用户动作。

流程：

```text
当前 exact Draft revision
→ compileCreatorDraftForPublication()
→ existing computeAssetDigest()
→ existing validateAndVerifyAsset()
→ SourceAssetLibraryStore.appendValidated()
→ published Draft
```

### 10.1 发布前状态

UI 至少展示：

- 世界标题；
- 版本；
- 当前校验是否通过；
- 校验错误摘要。

发布失败不得丢草稿。

### 10.2 版本冲突

若 `SOURCE_ASSET_VERSION_CONFLICT`：

- 保持旧 Source 不变；
- G9-05B Core 会恢复 Draft 为 editable；
- UI 明确提示版本已被占用；
- 用户可修改版本后重试。

### 10.3 发布成功

发布后：

- 进入世界包详情页或提供明确跳转；
- Source Asset Library 中能按 exact assetRef/version 找到；
- Draft 显示 published；
- 不自动创建游戏；
- 不自动更新现有游戏。

## 11. 世界包资产页

“世界包”入口读取 `SourceAssetLibraryStore.listByType('world')`。

至少支持：

- 列表世界包；
- 按 assetRef 识别同一资产的多个版本；
- 查看标题 / 版本 / summary / assetRef；
- 查看版本历史；
- 打开具体版本详情；
- 从指定 exact snapshot 创建新版本 Draft。

不得把 display title 当成资产 identity。

## 12. HTTP / Product Contract

Creator UI 如果需要新增 HTTP 接口：

- 必须进入现有产品 HTTP 门面 / 产品 DTO 体系；
- Product 层只调用 `资产创作/L3_外交层` 与 `资产库/L3_外交层`；
- 不从产品层直接 import Creator L0/L1/L2 内部实现；
- 浏览器 DTO 不暴露 Runtime hidden state、raw Provider response 或任意执行对象；
- stale revision / validation / AI unavailable / AI failed / undo conflict / version conflict 必须稳定映射为产品错误状态。

## 13. 持久化

桌面/本地产品运行时使用已有 SQLite Creator Draft Store / Import Artifact Store / Source Asset Library Store。

要求：

- 重启后 Creator Draft 可继续；
- 导入原件仍可恢复；
- 已发布世界包仍在资产库；
- 发布中断沿用 G9-05B recovery；
- 不把用户资产写入研发仓库 `sillytavern-assets`。

## 14. 首轮视觉要求

第一版目标是“清楚、可用、稳定”，不是最终美术精修。

至少满足：

- 桌面常用宽度可完整操作；
- 重要状态不只靠颜色表达；
- 发布与普通保存有明显区分；
- AI 修改范围在发送前可见；
- unresolved / conflict 不隐藏在日志里；
- Provider 未配置有明确空状态；
- 角色卡 / 拓展包未实现入口不能误导成可用。

不得为了视觉效果引入新的大型 UI 依赖，除非现有产品已经使用且必要。

## 15. 验收纵向

至少证明以下真实产品路径：

### A. 无 Provider 手工路径

```text
我的资产库
→ Creator
→ 新建世界包
→ 手工填写标题 / 版本 / 章节
→ 自动保存
→ 发布
→ 世界包列表看到正式 Source
→ 打开详情
→ 创建新版本 Draft
```

### B. 外部创作稿路径

```text
导入 .md
→ exact raw artifact 保存
→ AI organizer fixture
→ 确定内容自动填入
→ 冲突/未决保持空白并可见
→ 玩家继续修改
→ 发布
```

### C. AI scope 路径

```text
选择一个章节
→ 要求 AI 只修改该章节
→ Provider 返回合法本章节修改 + 越权其它区域修改
→ Program 只应用授权项
→ UI 展示忽略项
→ Undo
```

### D. 失败恢复路径

至少覆盖：

- stale revision；
- Provider unavailable；
- Provider malformed operation；
- Undo conflict；
- Source version conflict；
- publish retry / recovery。

## 16. 测试

除 G9-05C focused tests 外，至少回归：

```powershell
npm run g9:05b:test
npm run g9:04:test
npm run g9:03:test
npm run g8:test
npm run g8:product-e2e
npm test
npm run typecheck
npm run lint
npm run product:build
npm run launcher:smoke
npm run g2:disclosure
git diff --check
```

自动化 Provider 真实外部调用 = 0。

## 17. 阶段退出

只有在 exact-SHA Independent Review 达到：

```text
P0 = 0
P1 = 0
```

并完成 World Creator 真实产品纵向后，才能进入 Character Creator Vertical。

G9-05C 不因为“页面能打开”而 PASS；必须证明 Draft → Import/AI → Undo → Publish → Source Library → New Version 的完整闭环。