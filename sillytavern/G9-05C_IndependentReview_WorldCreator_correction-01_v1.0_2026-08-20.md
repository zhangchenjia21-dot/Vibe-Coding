---
title: G9-05C World Creator Independent Review correction-01
status: current-review-correction-required
version: 1.0
date: 2026-08-20
stage: G9-05
slice: G9-05C
---

# G9-05C｜World Creator｜Independent Review correction-01

## 1. 审核对象

```text
Formal Code Base
25286b2517cb26520109e3d8738671e53d88c861

Task Packet
a567a7f5f93fdf347982c398f69e268a8c595135

Reviewed Implementation
4f4f8449acb95b5270f0b4d21d65351129d9fe6a

Task Branch
agent/g9-05c-world-creator
= 4f4f8449acb95b5270f0b4d21d65351129d9fe6a

origin/main during review
25286b2517cb26520109e3d8738671e53d88c861
```

任务分支是 Formal Base 的严格前向后代，当前仅包含 G9-05C Task Packet 与一次实现提交；`main` 未被提前推进。

GitHub 当前没有该 SHA 的 CI status。由于本轮已经发现代码级 / 产品级 P1，独立审核无需以 CI 缺失作为阻断依据即可判定不得合并。

---

## 2. 最终结论

```text
P0 = 0
P1 = 4

P1-01 World composition / dependency advanced editor completeness = FAIL
P1-02 Import unresolved / conflict continuation navigation          = FAIL
P1-03 Existing sectionRef product edit semantics                    = FAIL
P1-04 World feature_conditional dependency semantic gate            = FAIL

G9-05C Implementation = FAIL / CORRECTION-01 REQUIRED
G9-05C CLOSED = NO
Character Creator Vertical AUTHORIZED = NO
main = unchanged at 25286b2517cb26520109e3d8738671e53d88c861
```

本轮主体方向成立：四入口、Creator 首页、空白/导入/Source revision 三起点、SQLite 持久化、G9-05B Draft/CAS/AI scope/ChangeSet/Undo/Publish 复用、世界包 Source 列表/版本历史、Provider 未配置降级、HTTP/DTO 边界均已建立。当前问题不是推翻纵向，而是补齐首个真实 Creator 工作区的产品完整性与 G9-03 语义门禁。

---

## 3. 已通过的主体能力

### 3.1 我的资产库 / Creator 导航

- `Creator` 与 `世界包` 为真实可用入口；
- `角色卡` / `拓展包` 明确为后续开放；
- Creator 保持在“我的资产库”内部，而非独立顶层产品；
- 原有游戏库 / 新游戏 / 设置 / Session 路由未被替换。

### 3.2 三种世界包创作起点

已接入现有 G9-05B Core：

```text
blank world Draft
imported_manuscript world Draft
exact World Source snapshot → source_revision Draft
```

`targetAssetRef` 仍由 Program 生成 / 继承，标题和文件名不参与身份计算。

### 3.3 导入 / AI / Undo / Publish 主链

- `.md/.txt` raw 原稿先写入 Import Artifact；
- Provider 不可用时仍可手工继续；
- Provider 可用时走 `organizeImport()`；
- AI authoring 由 Product 层将 UI selection 展开为 exact Core scope；
- Provider 输出仍经过 G9-05B runtime parser；
- ChangeSet / ignored operations / Undo 已投影到 UI；
- Publish 使用 `CreatorPublicationService.publish()`；
- Source version conflict 保持旧 Source、恢复 Draft editable；
- 发布不创建游戏，不直接修改 Runtime。

### 3.4 本地产品组装

正式本地服务已组装：

```text
SQLiteCreatorDraftStore
SQLiteCreatorImportArtifactStore
SQLiteSourceAssetLibraryStore
CreatorDraftFlow
CreatorPublicationService
DeepSeekCreatorProviderAdapter | NoProviderCreatorAdapter
```

三个 Store 使用现有本地数据库路径，不写研发资产仓库。

---

## 4. P1-01｜World composition / dependency 高级编辑没有达到冻结规格

G9-05C 冻结规格要求 World Creator 主工作区对 composition 支持：

```text
compositionRef
target.assetRef
target.assetType
disposition
```

并要求 dependency 区域允许：

```text
create
edit
delete
```

当前 UI：

### Composition

已有 entry 只能：

- 勾选 AI scope；
- 查看摘要；
- 删除。

不能编辑已有 entry。

新增 entry 只输入：

- `compositionRef`
- target `assetRef`

并在 UI 代码中固定：

```text
target.assetType = world
disposition = optional
```

因此用户无法选择 `character | expansion | library` target，也无法选择 `default | recommended | optional`。

### Dependency

已有 dependency 同样只可查看 / AI scope / 删除，不能编辑。

新增 dependency 只输入：

- `dependencyRef`
- target `assetRef`

并固定：

```text
kind = optional
target.assetType = world
requiredCapabilityRefs = []
```

没有完整的 target type / dependency kind / capability 编辑能力。

这不是视觉精修问题，而是规格要求的 World Source 语义无法从 UI 完成。

### Correction 要求

1. Composition 支持 create / edit / delete；已有 entry 编辑必须保留 Creator `nodeRef`；
2. Composition UI 暴露并校验全部四个正式字段；
3. Dependency 支持 create / edit / delete；已有 entry 编辑必须保留 `nodeRef`；
4. Dependency UI 至少暴露 World 合法 kind、target assetRef/type、requiredCapabilityRefs；
5. 所有写入继续调用现有 World Product API → G9-05B mutation seam；禁止页面私改数组；
6. 增加 UI 测试证明高级字段真实可编辑，不只是展示。

---

## 5. P1-02｜Import unresolved / conflict 缺少“回到相关创作位置”的入口

G9-05C §8 已明确冻结：对于 unresolved / conflict，除了“不自动选、不覆盖、证据可查看”，还必须：

> 提供跳转到相关字段 / 章节继续创作的入口。

当前 `ImportReview` 已正确显示：

- applied / ignored mapping；
- unresolved；
- conflicts；
- evidence segment / 原文。

但这些条目只使用折叠详情展示证据，没有任何“去相关字段 / 去相关章节”动作，也不会把对应字段或章节聚焦 / 滚动到主工作区。

这会把导入审阅做成只读报告，而不是“导入后接着创作”的工作流。

### Correction 要求

1. 对可以确定定位的目标提供确定性跳转：
   - `metadata.title / summary / language` → 对应基础字段；
   - `section:<sectionRef>` 或可解析 section target → 对应章节；
2. 点击后至少完成滚动 / focus / highlight 中一种明确产品反馈；
3. 无法解析的 unresolved target 可以显示“未定位”，不得猜目标；
4. conflict 仍不得自动采用任一 candidate；
5. evidence 保持可见；
6. 增加 UI 测试验证从 unresolved / conflict 条目能回到可继续编辑的位置。

---

## 6. P1-03｜existing `sectionRef` 被 UI 表现为可编辑，但 Core 明确禁止其身份迁移

当前 `SectionEditor` 对已有章节显示可编辑的“章节 Ref”输入框，并把整个 section 重新提交。

但 G9-05B 已冻结并实现 section identity invariant：

```text
同一个 Creator nodeRef
不得被 upsert 成另一个 sectionRef

同一个 sectionRef
不得被另一 nodeRef 占用
```

因此玩家在已有章节 UI 中修改 `sectionRef` 后点击“保存章节”，Core 会正确 fail closed；也就是说产品暴露了一个看起来可以编辑、实际上必然失败的正式控件。

本阶段不应为了 UI 重新打开 G9-05B identity contract。

### Correction 要求

1. 新建章节时允许用户定义 `sectionRef`；
2. 已有章节的 `sectionRef` 改为只读 identity 显示；
3. existing section 继续允许编辑 `sectionKind / title / body / visibility`；
4. Product UI 明确区分“章节身份”与“可编辑内容”；
5. 增加 UI 回归，证明 existing `sectionRef` 不再伪装成可变字段。

---

## 7. P1-04｜World Creator 可生成 `feature_conditional` dependency，但 G9-03 catalog 语义只对 Expansion source feature/module 成立

当前 World Product request validator 和 DeepSeek Creator dependency schema 都接受：

```text
kind = feature_conditional
```

G9-05B publication compiler 最终调用 G9-03 `validateAndVerifyAsset()`，该结构校验只要求 conditional dependency 有 `sourceScope`，所以这种 World Source 可以被编译、hash、写入 Source Asset Library。

但是 G9-03 catalog semantic gate 的 `conditionalScopeEnabled()` 明确要求 source asset 为 Expansion；World / Character 上的 `feature_conditional` 会以 `CONDITIONAL_DEPENDENCY_SCOPE_MISSING` fail closed。

因此当前 World Creator 能创建一种“可发布到我的资产库，但进入正式 catalog 时必失败”的 World Source，这违反“Creator 应直接产生同一种合法 Source Asset”的纵向目标。

同时当前 DeepSeek World dependency tool schema把 `sourceScope` 设为所有 dependency 的必填对象，甚至要求同时提供 `featureRef + moduleRef`，也与 World dependency 语义不匹配。

### Correction 要求

World Creator v1 对 dependency kind 收紧为：

```text
hard
optional
reference
```

不得让 World Product UI / HTTP / AI tool 产生 `feature_conditional`。

具体要求：

1. World Product DTO/request gate 拒绝 `feature_conditional`；
2. World UI 不提供该选项；
3. DeepSeek World authoring tool schema 不允许 `feature_conditional`；
4. World AI dependency schema 不应强制无意义的 `sourceScope`；
5. 已有 G9-05B generic Creator contract 不修改，因为 Expansion 后续仍需要 `feature_conditional`；
6. 增加负向测试：World request / AI 提交 conditional dependency 必须 fail closed / ignored；
7. 正向测试 hard / optional / reference 均可通过 World Product → publish → G9-03 catalog-compatible proof（至少覆盖结构与当前可验证语义）。

---

## 8. Correction 顺手收口项｜失败时不要清空未提交高级表单

当前新增 Section / Composition / Dependency 表单在 Promise resolve 后重置本地输入；上层 `run()` 会捕获服务端错误并 resolve `undefined`，因此 stale/invalid mutation 发生时子表单仍可能误以为成功并清空玩家尚未保存的数据。

该项本身不单列新 P1，但 correction-01 修改这些表单时应一起收口：

```text
只有得到新的 world_creator_workspace 成功结果
→ 清空新增表单

失败 / stale / invalid
→ 保留本地输入
→ 显示错误
```

---

## 9. 保持不变

correction-01 不得回滚本轮已成立能力：

- 四入口信息架构；
- blank / import / source revision 三起点；
- SQLite Creator / Source 持久化；
- targetAssetRef Program ownership；
- G9-05B revision/CAS；
- exact AI scope；
- Provider unknown runtime parse；
- certain-only + evidence + blank-only import；
- unresolved/conflict 不自动选择；
- ChangeSet / Undo；
- version conflict recovery；
- explicit Publish；
- Source list / exact version history；
- Provider 未配置时手工路径；
- Runtime 隔离；
- Character / Expansion 未开放状态。

不得：

- 新开 correction branch；
- 修改 G9-05B Core identity contract 来迁就 UI；
- 修改 G9-03 wire / validator semantics；
- 开始 Character / Expansion Creator；
- 创建第二套 Draft / Source Store / publish transaction；
- 产生真实外部 Provider 测试调用。

---

## 10. Gate

新的 exact implementation SHA 必须重新独立审核，并至少证明：

```text
P1-01 advanced composition/dependency editor = PASS
P1-02 import continuation navigation          = PASS
P1-03 existing section identity UX            = PASS
P1-04 World dependency semantic gate          = PASS

G9-05C focused product tests = PASS
G9-05B regression = PASS
G9-04 / G9-03 regression = PASS
G8 product regressions = PASS
full suite / typecheck / lint / product build / launcher / disclosure = PASS
Provider calls = 0
```

在 `P0=0 / P1=0` 之前：

```text
main 不推进
G9-05C 不关闭
Character Creator 不授权
```
