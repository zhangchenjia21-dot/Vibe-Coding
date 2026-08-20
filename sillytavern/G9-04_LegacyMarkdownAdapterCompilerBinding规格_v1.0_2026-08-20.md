---
title: G9-04 Legacy Markdown Adapter / Compiler / Binding 规格
status: current-spec-frozen
version: 1.0
date: 2026-08-20
stage: G9-04
next_gate: implementation + exact-SHA independent review
---

# G9-04｜Legacy Markdown Adapter / Compiler / Binding 规格 v1.0

## 0. Outcome

G9-04 的唯一主 Outcome：

> **让当前 canonical World Pack / Character Card / Expansion Pack 通过 AI-independent、可重复、fail-closed 的 Legacy Markdown Adapter 转换成 G9-03 已冻结的 `TavernAssetV1`，再由 exact `TavernGameAssetManifestV1` 进入既有 G9-02 Source Binding / Game-local lineage；Library 只做最小 parse / validate / canonical round-trip / cross-reference proof。**

正式链：

```text
canonical Markdown Source
+ explicit Legacy Adapter Profile
↓ deterministic parse / mapping
TavernAssetV1
↓ existing G9-03 validate / canonicalize / SHA-256
TavernGameAssetManifestV1
↓ G9-04 binding compiler
SourceAssetDescriptor
+ hidden Game-local binding anchors
+ validated Expansion RuntimeDomainModuleBinding where applicable
↓
existing G9-02 bootstrap / lineage / Save-Restore rails
```

永久保持：

```text
Markdown Source != TavernAssetV1 Runtime State
TavernAssetV1 != Game-local Instance
Game Asset Manifest != Runtime State
Adapter Profile != second Source truth
```

本阶段不是新 Runtime Domain 开发，也不是 Creator 或 Library 产品阶段。

---

## 1. Authority / Exact Baselines

Current authority：

- `酒馆游戏项目开发核心总纲_CURRENT.md`；
- `酒馆游戏新版主体重建总路线 v2.3.md`；
- `G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md`；
- `G9-03A_RuntimeModuleBinding与TypedConfig增量裁定_v1.0_2026-08-20.md`；
- `G9-03_IndependentReview_最终收口_v1.0_2026-08-20.md`；
- #18 / #18A Library current decisions。

Implementation baseline：

```text
zhangchenjia21-dot/sillytavern main
5da2294a9d21585665167e69307d9c693427582d
```

Asset baseline：

```text
zhangchenjia21-dot/sillytavern-assets main
968175e6c3fb3545b7c2907b65089c7e1dbb40a0
```

Current skills：`agent-task-packet v1.1`、`lifecycle-dev-process v2.1`、`tavern-asset v1.0`。

---

# 2. Frozen Decisions

## DEC-01｜Legacy Markdown 不自动等于 Machine Asset

现有 Markdown frontmatter / Wikilink 是语义资产的创作与治理来源；禁止直接 `frontmatter → cast TavernAssetV1`。必须经过显式 Adapter Profile 和 deterministic mapping。

`creator_binding / asset_spec_binding / workflow_mode / operation_mode / output_profile / skill / blueprint` 等 legacy governance 字段不自动成为 v1 machine fields。

## DEC-02｜Stable identity 必须显式提供，不得猜

Legacy Adapter Profile 必须显式给出 `assetRef`。禁止 filename/title/Wikilink display name/alias fuzzy match 推断 identity。文件路径和 Git blob SHA 只用于 source locator / evidence。

已有稳定 Expansion ID（例如 `EP-CHAR-CORE`）直接复用。

## DEC-03｜Legacy Adapter Profile 是迁移配置，不是第五种 Source Wire

允许内部 profile，至少表达：

```text
profileRef
sourcePath
expectedAssetType
assetRef
expectedSourceVersion
owner-typed deterministic mapping config
```

要求：

- profile = Program/migration configuration；
- profile != Runtime State；
- 不改变 `tavern.asset.v1` wire；
- machine-critical mapping 必须显式、可测试、可审计；
- 同一 `{sourcePath, source version}` 只能命中一个 active profile；
- missing/collision fail closed。

## DEC-04｜Markdown/YAML 必须真实解析

当前 frontmatter 已包含 quoted strings、arrays、Wikilinks。若现有依赖不足，允许新增一个小型成熟 YAML parser dependency并更新 lockfile；禁止手写只覆盖 happy-path 的 YAML 子集。

Markdown body 结构解析必须 deterministic、无 LLM、无语义猜测。

## DEC-05｜Legacy body 全量保留，公开内容只显式放行

安全默认：

```text
full legacy Markdown body
→ preserved as private Source semantic section

explicit structural selector
→ optional public section

unclassified legacy body
→ private
```

禁止根据“标题看起来公开”或模型判断自动升级 public。Public selector mismatch 必须 fail/diagnostic，不能静默丢内容。

## DEC-06｜Semantic fields 不得静默丢弃

Adapter diagnostics 至少区分：consumed machine fields、explicitly ignored governance fields、preserved raw semantic content、unmapped semantic fields/refs、unresolved dependency/reference targets。

`hard_dependencies / optional_integrations / providers / capability_core` 及 profile 标为 semantic relation 的字段不得当普通治理字段静默忽略；必须 exact mapping 或 fail/report。禁止 fuzzy Wikilink resolution。

## DEC-07｜唯一 Validator / Digest Path

```text
Legacy Adapter
→ candidate TavernAssetV1
→ existing G9-03 validator
→ existing canonical serializer / digest
```

禁止复制第二套 validator/hash。同一 Markdown bytes + 同一 profile 必须产生相同 canonical bytes/digest。

## DEC-08｜Typed mapping

### World

至少映射 explicit assetRef、exact version、title/aliases/language/tags、full private body、optional public extracts、explicit composition（若 profile 提供）。World body 不直接成为 Runtime State。

### Character

至少映射 explicit assetRef、version、displayName、aliases、playerCharacterSupported（仅有明确证据时）、full private body、reference/dependency mappings、optional public extracts。不得从 Card 直接生成 current position/role/knowledge/relationship/injury/live state。

### Expansion

至少映射 stable assetRef、version、packageRef、Source ownerNamespace、Feature/Source Module、runtimeModuleRef、typed dependency、routing/projection/UI seam、full private body。

G9-03A 继续强制：

```text
Source moduleRef != Program Runtime moduleRef
RuntimeDomainModuleBinding.moduleRef = runtimeModuleRef
```

G9-04 不实现新的 Politics / Capability / War / Economy 等 Domain mechanics。Adapter 的 Runtime binding proof 不等于对应 Domain gameplay 完成。

## DEC-09｜Manifest exact pinning

selected validated assets 必须先解析为 exact `assetRef + type + version + digest` 的 `TavernGameAssetManifestV1`，再进入 binding。Source v2 发布不得静默改写旧 manifest/game。

## DEC-10｜Binding Anchor 不是实体物化

允许 Program-owned internal hidden binding anchor：

```text
ownerNamespace = runtime.asset-binding
visibility = hidden
sourceAsset = exact SourceAssetDescriptor
```

职责只有：记录本局 exact primary Source snapshot、进入 G9-02 lineage/Save-Restore、提供 durable binding referent。

它不是 World state、materialized Character、Domain state、Player-known object 或 UI item。因此绑定 Character Card 不得在场景中生成角色；No Phantom 永久有效。

## DEC-11｜Expansion activation 继续经过 Program Host

若 binding plan 含 active Expansion module：

```text
validated runtimeModuleRef
→ Program-known registry
→ RuntimeDomainModuleBinding
→ RuntimeDomainModuleHost.validateBindings()
```

unknown/unsupported/duplicate active runtime module fail closed。允许 Program-owned fixture module 做 vertical binding proof，但不得冒充真实 Domain mechanics completion。

## DEC-12｜Library 仅最小 proof

```text
fixture/source sample
→ deterministic parse
→ TavernAssetV1 library
→ validate
→ canonical round-trip
→ stable entry + provenance + audience + cross-reference validation
```

禁止 Game-local Library truth、Runtime retrieval/indexing、embedding/vector、Model Reference Provider、Library UI、Creator Library editor。

## DEC-13｜Real Asset Gate 与 portable unit tests 分离

仓库 unit tests 不依赖 sibling assets repo。分层：

```text
portable synthetic fixtures
→ npm test / CI-style gates

real canonical asset gate
→ SILLYTAVERN_ASSETS_DIR or known sibling repo
→ exact assets repo HEAD + paths + blob SHAs
```

不得为了测试把真实资产全文复制进 code repo。Real Asset Gate 无 Provider。

---

# 3. Required Real Sample Corpus

### World

`世界包/汉末三国_天下未定_World_Pack_v0.2.3.md`

Stable `assetRef` for this vertical：

```text
world:han-late-three-kingdoms
```

### Character

`人物卡/汉末三国/CC-BATCH-01/刘备__Character_Card__v0.1.2.md`

Stable `assetRef`：

```text
character:han-late.liu-bei
```

### Expansion

`拓展包/通用拓展包/人物能力与技艺_Expansion_Pack_v0.1.5.md`

Reuse existing stable ID：

```text
EP-CHAR-CORE
```

### Secondary parser robustness sample

至少再读取一个结构差异明显的 current Markdown，推荐：

- `世界包/埃瑟维亚_诸界余辉_World_Pack_v0.1.3.md`；或
- `人物卡/诸界余辉/CC-01_莉维娅·塞兰_Character_Card_v0.2.md`。

它用于证明 parser/profile 不是 Han 单文件硬编码；是否进入 binding vertical 不作为 Exit 强制条件。

### Library

当前资产仓库没有正式 Library canonical body；使用明确标记 `fixture-only` 的最小 Library Markdown/normalized input做协议 proof，不得宣布为第四类主资产或正式汉末资料库。

---

# 4. Binding Compiler Foundation

允许 internal `CompiledGameAssetBindingPlanV1` 或等价结构，语义至少包含：

- `SourceAssetDescriptor[]`；
- primary `AdditionalGameLocalAssetDefinition[]` hidden binding anchors；
- `RuntimeDomainModuleBinding[]`。

要求：

1. input 只能是 validated `TavernGameAssetManifestV1 + exact catalog`；
2. Source descriptor 复用现有 `mapAssetToSourceDescriptor()`；
3. primary binding anchors 一一对应 selected World / Character / Expansion snapshots；
4. anchor 使用 exact Source descriptor，不重新生成 identity/hash；
5. Library 不因本最小 proof成为 Game-local canonical truth；
6. active Expansion binding 使用 G9-03A mapping并经过 Host validation；
7. binding plan 只扩展 `RuntimeDefinition` existing seams，不替换 bootstrap authority。

### Real lineage proof

必须使用真实 `SQLiteRuntimeStore.bootstrap()` 或等价 production path证明：

```text
exact manifest
→ compiled binding plan
→ bootstrap
→ gameLocalAssets[].sourceLineage
```

World / Character / Expansion 三个 primary snapshots 的 stableRef/version/contentHash 与 manifest exact match。

### No silent source update

Game 用 v1 manifest bootstrap 后，catalog 新增同 assetRef v2：existing lineage 不变，old manifest仍解析 v1，不自动 rebind。

### Save / Restore

Binding anchors 进入 existing canonical Save / Restore；Restore 后 lineage 不丢失、不升级、不重复。

---

# 5. Required Fail-closed Diagnostics

至少稳定区分：

```text
LEGACY_FRONTMATTER_INVALID
LEGACY_PROFILE_MISSING
LEGACY_PROFILE_COLLISION
LEGACY_ASSET_TYPE_MISMATCH
LEGACY_VERSION_MISMATCH
LEGACY_PUBLIC_SELECTOR_MISSING
LEGACY_UNMAPPED_SEMANTIC_FIELD
LEGACY_REFERENCE_UNRESOLVED
BINDING_PRIMARY_SNAPSHOT_MISSING
BINDING_DUPLICATE_PRIMARY
```

不得把全部 Adapter failure 压成 generic parse error。

---

# 6. Acceptance Gates

- **AC-01** AI-independent real Markdown parse deterministic。
- **AC-02** World/Character assetRef 来自 explicit profile；filename/title change 不重算 identity。
- **AC-03** full body preserved private；public only explicit selector；selector mismatch fail/diagnostic。
- **AC-04** 汉末 World current source → valid `TavernAssetV1 world`，version/metadata/body 无非授权丢失。
- **AC-05** 刘备 current source → valid `TavernAssetV1 character`；不生成 current live state。
- **AC-06** EP-CHAR-CORE current source → valid `TavernAssetV1 expansion`；stable ID = `EP-CHAR-CORE`；module/runtime identity 符合 G9-03A。
- **AC-07** Legacy relation/Wikilink 只 exact profile resolve；no fuzzy resolution；unresolved semantic reference fail/report。
- **AC-08** outputs 复用 existing G9-03 validator/hash，无第二 path。
- **AC-09** World + Character + Expansion exact manifest pinning；v2 catalog不改 old manifest。
- **AC-10** manifest → binding plan → real bootstrap 后三个 primary binding anchors lineage exact match。
- **AC-11** Character binding 不产生 visible Character/entity placement/player-known/runtime character state。
- **AC-12** active Expansion binding 真实 Host validation；unknown/unsupported/duplicate active fail closed。
- **AC-13** binding lineage Save/Restore exact保持，无 silent upgrade。
- **AC-14** Library fixture parse/validate/canonical round-trip/stable entry/provenance/audience/cross-ref PASS，无 retrieval/UI/runtime truth。
- **AC-15** Real Asset Gate记录 assets repo exact HEAD；若执行时不是 `968175e6c3fb3545b7c2907b65089c7e1dbb40a0`，先做 Freshness diff，不得旧 profile静默适配新 source。
- **AC-16** G9-03、G9-02 closure、G5/G6/G7/G8/full tests/typecheck/lint/build/launcher/disclosure/diff-check PASS。
- **AC-17** 无 Creator、Library retrieval、Domain mechanics implementation、第二 Source identity、第二 Game-local authority、G9-05 scope drift。

---

# 7. Recommended Module Boundary

```text
src/资产适配/
├─ L0_公理层/Legacy资产适配契约.ts
├─ L1_器件层/LegacyMarkdown解析器.ts
├─ L1_器件层/Legacy资产适配器.ts
├─ L1_器件层/游戏资产绑定编译器.ts
├─ L3_外交层/资产适配公开接口.ts
└─ 模块说明.md
```

跨模块只通过 `资产协议 L3` 与 `运行时 L3`。如果新增 YAML dependency，必须最小化、更新 lockfile并报告用途；不要新增 Markdown AST framework，除非真实 source证明必要。

---

# 8. Non-scope / Exit

禁止：批量重写全部 canonical Markdown、把全部资产迁成 JSON、Creator、Library product/retrieval、新 Domain mechanics、修改 `tavern.asset.v1` wire、回开 G9-02、arbitrary script/query/expression DSL、fuzzy identity resolution。

本规格冻结后：

```text
G9-04 Spec             FROZEN
G9-04 Implementation   NEXT
G9-05                  NOT AUTHORIZED
```

Implementation + exact-SHA Independent Review PASS 后才能 `G9-04 PASS / CLOSED`。

实现 Agent 完成后只允许返回：

**G9-04 ADAPTER / COMPILER / BINDING READY FOR INDEPENDENT REVIEW**
