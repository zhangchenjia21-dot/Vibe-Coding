---
title: G9-05F Expansion Creator 产品纵向规格
status: current-spec-frozen
version: 1.0
date: 2026-08-22
stage: G9-05
slice: G9-05F
prerequisite:
  - G9-05E PASS / CLOSED
  - G9-05F0 focused seam PASS before product phase
---

# G9-05F｜Expansion Creator 产品纵向规格 v1.0

## 0. Outcome

把“我的资产库 → Creator → 拓展包”接成正式产品纵向：

```text
空白开始 / 外部材料 / 正式资产新版本
→ Expansion Creator Draft Workspace
→ 手工 + 受控 AI
→ Import evidence / unresolved / conflict
→ ChangeSet / Undo
→ Program Host Compatibility Gate
→ 显式 Publish
→ Source Asset Library
→ 拓展包列表 / 详情 / 版本历史 / 创建新版本
```

发布只产生 Source Asset，不自动激活当前游戏。

---

## 1. 永久边界

```text
Creator Draft
!= Expansion Source
!= Game Manifest enablement
!= Runtime binding
!= Runtime State
```

```text
Source moduleRef
!= Program runtimeModuleRef
```

Expansion Creator 只能声明并引用 Program 已注册能力，不能创建 executable plugin、运行任意代码、修改 Runtime Domain Owner 或绕过 Program validator。

---

## 2. 产品入口

“我的资产库”保持四入口：

```text
Creator
世界包
角色卡
拓展包
```

G9-05F 后：

- Creator 首页显示 Expansion Draft；
- “拓展包”成为正式 Source Asset 管理入口；
- World / Character 现有行为不得回退。

三种起点继续统一：

```text
空白开始 → 新建 Expansion Draft
外部材料 → Import Artifact → Expansion Draft
正式资产 → exact Source revision → 新版本 Draft
```

---

## 3. Workspace 信息架构

### 3.1 基础资料

必须包含：

- `metadata.title`
- `metadata.summary`
- `metadata.language`
- `targetVersion`
- read-only `targetAssetRef`
- `expansion.packageRef`
- `expansion.ownerNamespace`

`packageRef / ownerNamespace` 是 Source 声明，不取得 Program Runtime authority。

### 3.2 Features

每个 feature：

```text
nodeRef          Program-owned stable node identity
featureRef       Source semantic identity
label
defaultEnabled
```

规则：

- 新建时 `featureRef` 可编辑；
- existing node 的 `featureRef` 只读；
- label/defaultEnabled 可编辑；
- CRUD 只能走 `upsert_typed_node / remove_typed_node`；
- 删除被 module / feature_conditional dependency 引用的 feature 必须 fail closed 或先要求用户解除引用，不得留下悬空 Source。

### 3.3 Modules

每个 module 至少完整编辑：

```text
nodeRef
moduleRef
featureRef
label
defaultEnabled
runtimeModuleRef
runtimeModuleVersion?
config? { schemaRef, value }
routingMode
routingProfile? { label, scope, typicalSemantics[] }
projectionRef?
capabilityRefs[]?
definitionRegistryRefs[]?
acceptedHandoffKinds[]?
emittedHandoffKinds[]?
```

规则：

- existing `moduleRef` 只读；
- `featureRef` 必须指向当前 Expansion 真实 Feature；
- `runtimeModuleRef` 必须从 Program Capability Catalog 选择；
- `config.schemaRef` 必须从 Program Catalog 选择；`value` 作为 JSON-compatible data 编辑，禁止 eval/script；
- `projectionRef` 只允许 Program Catalog 中值；
- `routingMode` 只允许 `model_immediate | program_activated`；
- `moduleRef` 不得冒充 `runtimeModuleRef`。

### 3.4 Dependencies

Expansion 是首个允许完整四种 dependency kind 的 Creator：

```text
hard
optional
reference
feature_conditional
```

所有 dependency 完整编辑：

```text
nodeRef
dependencyRef
kind
target { assetRef, assetType }
requiredCapabilityRefs[]?
sourceScope? { featureRef?, moduleRef? }
```

`feature_conditional`：

- 必须有可验证 `sourceScope`；
- feature/module 通过当前 Draft 的真实节点选择，不用自由文本猜 ref；
- feature+module 同时存在时 parent 必须一致；
- 非 conditional dependency 不保存无意义 sourceScope。

existing `dependencyRef` 只读身份。

### 3.5 UI Extension Surface

`ownsExtensionSurfaces` 编辑：

```text
nodeRef
surfaceId
label
recommendedAfterSurfaceId?
secondaryViews[] { viewId, label }
```

existing `surfaceId` 只读。

这只是 Source 声明；不会在 Creator 中直接 mount 任意组件。

### 3.6 UI Contributions

完整编辑：

```text
nodeRef
contributionRef
title
hostCapability
targetRef?
componentKind
projectionRef?
staticData? Record<string, string|number|boolean>
```

规则：

- existing `contributionRef` 只读；
- `hostCapability` 只能从 frozen Program list 选择；
- `projectionRef` 只能选 Program Catalog 已注册 ref；
- `staticData` 只允许标量 JSON map；
- `componentKind` 是 declarative kind，不允许携带 code/callback/template script；
- Creator 不预览或执行未注册 Runtime/UI 能力。

### 3.7 Semantic Sections

与已验证 Creator 模式一致：

```text
nodeRef
sectionRef
sectionKind
title
body
visibility public/private
```

existing `sectionRef` 只读身份。

---

## 4. Program Capability Catalog UX

Workspace 必须有清晰的“可用宿主能力”辅助区或选择器，来自 G9-05F0 Program projection：

- Runtime Module refs；
- Config Schema refs；
- Projection refs；
- UI Host Capabilities。

产品语言必须区分：

```text
Source 声明名称
vs
Program 可绑定能力
```

没有 Program capability 时允许继续编辑 Draft，但相关 Module/Contribution 不能通过 Publish Gate；不得伪造“已绑定”。

---

## 5. AI Authoring

Expansion 使用独立 typed Provider adapter，可复用现有 DeepSeek Strict Tool transport，但必须：

- `author()` / `organize()` 都先验证 exact Character-like gate 的等价 Expansion type + revision；
- 默认作用范围是当前字段/节点；
- 用户可显式扩大到本次任务允许的多个 typed node；
- AI 输出必须进入 Creator Core parser / scope / CAS；
- 未授权节点忽略并进入 `ignoredOperations`；
- AI 不得 Publish；
- AI 不得发明 Program-owned `runtimeModuleRef / config.schemaRef / projectionRef / hostCapability`；只有 Catalog 中 exact ref 可接受；
- Provider 不可执行任意代码或查询 Runtime State。

首版继续允许 Provider 不可用时完整手工编辑/发布。

---

## 6. Import Review

`.md/.txt` 最小格式保持。

Expansion Import 允许 evidence-backed certain assignment：

- metadata；
- `packageRef / ownerNamespace`；
- sections；
- dependencies；
- features；
- modules；
- UI surfaces/contributions。

规则仍是：

```text
明确 + 唯一 + 原文支持 +（Program-owned ref 时）Catalog 可验证
→ 填入空白 Draft

信息不足 / 未提及 / 冲突 / 多种合理解释
→ unresolved / conflict
```

Import Review 必须能从正式 target 定位回：

- 基础字段；
- section；
- dependency；
- feature；
- module；
- UI surface；
- UI contribution。

候选值只读，不自动替用户采用 conflict candidate。

---

## 7. Validation / Publish

### 7.1 Save != Publish

所有编辑继续 CAS autosave / explicit mutation；保存 Draft 不等于发布。

### 7.2 Publish Gate

玩家点击 Publish 后：

```text
Draft validation
→ compileCreatorDraftForPublication
→ G9-03 structural/integrity
→ G9-03 catalog + Program Host Registry
→ default enablement Runtime binding proof
→ explicit confirmation / existing publication exactly-once lifecycle
→ append-only Source Store
```

必须展示可操作错误，例如 unknown runtime module / config schema / projection / invalid conditional scope。

### 7.3 No activation

成功 Publish 后：

```text
new Expansion Source snapshot exists
```

但必须同时证明：

```text
current games unchanged
Runtime module enablement unchanged
Session revision/turn unchanged
```

玩家只有之后在“使用我的资产库”创建游戏或其它合法 Game-local flow 中显式选择该 exact snapshot，才可能形成 Runtime binding。

---

## 8. Source Library

“拓展包”正式页最低能力：

- Source list；
- exact version history；
- detail；
- features/modules/default enablement 摘要；
- dependencies；
- UI declarations；
- exact snapshot identity；
- 创建新版本。

不得把当前某局 enablement 误显示为 Source truth。

---

## 9. Required Proof

G9-05F 至少证明：

1. blank Expansion 完整创建；
2. `.md/.txt` import 结构化整理；
3. exact Source revision → 新版本；
4. feature/module/UI/dependency CRUD + stable node identity；
5. feature_conditional 合法/非法 scope；
6. Program Catalog 选择与 unknown ref fail closed；
7. typed config valid/invalid；
8. AI exact scope + ignored op + stale result；
9. ChangeSet / Undo；
10. explicit Publish；
11. Source list/detail/history；
12. SQLite reopen；
13. No-Provider manual path；
14. published Expansion 通过 `validateAssetCatalog()`；
15. default active modules 通过真实 Runtime Host binding；
16. Publish 不激活 Runtime；
17. G9-05E 能选择新发布 Expansion exact version，并把 feature/module enablement 写入 Manifest；
18. World / Character Creator、资产建局、G8/G9 回归不变。

---

## 10. Non-goals

G9-05F 不做：

- executable plugin / user code；
- 新 Runtime module factory；
- arbitrary config schema designer；
- Library product/retrieval；
- 自动联网研究；
- 自动发布；
- 自动加入当前游戏；
- PDF/OCR；
- fuzzy Program capability matching；
- 第二套资产协议 / Draft Store / Source Store / Publish transaction。

---

## 11. Exit Gate

```text
G9-05F0 focused PASS
+
G9-05F P0=0 / P1=0
↓
Expansion Creator PASS / CLOSED
↓
三类主资产完整组合建局与游玩闭环
↓
Primary Asset End-to-End Closure Gate
```
