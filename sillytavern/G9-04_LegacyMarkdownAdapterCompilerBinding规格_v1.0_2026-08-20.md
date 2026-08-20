---
title: G9-04 Legacy Markdown Adapter / Compiler / Binding 规格
status: current-spec-candidate-for-integration
version: 1.0
date: 2026-08-20
stage: G9-04
---

# G9-04｜Legacy Markdown Adapter / Compiler / Binding 规格 v1.0

## 0. Outcome

G9-04 的唯一主 Outcome：

> **让当前 canonical World Pack / Character Card / Expansion Pack 通过 AI-independent、可重复、fail-closed 的 Legacy Markdown Adapter 转换成 G9-03 已冻结的 `TavernAssetV1`，再由 exact `TavernGameAssetManifestV1` 进入既有 G9-02 Source Binding / Game-local lineage；Library 只做最小 parse / validate / canonical round-trip / cross-reference proof。**

本阶段不是新 Runtime Domain 开发，也不是 Creator 或 Library 产品阶段。

正式链：

```text
canonical Markdown Source
+ explicit Legacy Adapter Profile
↓ deterministic parse / mapping
TavernAssetV1
↓ G9-03 validate / canonicalize / SHA-256
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

---

## 1. Authority / Exact Baselines

Current product / architecture：

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

Current skills：

- `agent-task-packet v1.1`；
- `lifecycle-dev-process v2.1`；
- `tavern-asset v1.0`。

---

# 2. Frozen Decisions

## DEC-01｜Legacy Markdown 不自动等于 Machine Asset

现有 Markdown frontmatter / Wikilink 是当前语义资产的创作与治理来源；G9-04 不允许直接：

```text
frontmatter object
→ cast as TavernAssetV1
```

必须经过显式 Adapter Profile 和 deterministic mapping。

现有字段中的：

- `creator_binding: pending`；
- `asset_spec_binding: pending`；
- `workflow_mode / operation_mode / output_profile / skill / blueprint`；

属于 legacy governance / authoring metadata，不自动变成 v1 machine fields。

## DEC-02｜Stable identity 必须显式提供，不得猜

Legacy Adapter Profile 必须显式给出 `assetRef`。

禁止：

```text
filename → assetRef
Markdown title → assetRef
Wikilink display name → assetRef
alias fuzzy match → assetRef
```

文件路径和 Git blob SHA 只用于 source locator / evidence，不是 logical asset identity。

已有稳定 Expansion ID（例如 `EP-CHAR-CORE`）可以直接复用为 `assetRef`。

## DEC-03｜Legacy Adapter Profile 是迁移配置，不是第五种 Source Wire

G9-04 允许内部：

```ts
interface LegacyMarkdownAdapterProfileV1 {
  profileRef: string;
  sourcePath: string;
  expectedAssetType: 'world' | 'character' | 'expansion' | 'library';
  assetRef: string;
  expectedSourceVersion?: string;
  mapping: unknown; // owner-typed deterministic mapping configuration
}
```

精确实现 shape 可以调整，但必须满足：

- profile 是 Program / migration configuration；
- profile 不能成为 Runtime State；
- profile 不改变 G9-03 `tavern.asset.v1` wire；
- profile 中任何 machine-critical mapping 都必须显式、可测试、可审计；
- 同一 `{sourcePath, source version}` 只能命中一个 active profile；
- profile collision / missing profile fail closed。

## DEC-04｜Markdown/YAML 解析必须使用真实 parser，不写脆弱 YAML 子集

当前 assets frontmatter 已包含 quoted strings、arrays、Wikilinks 等结构。

允许新增一个小型、成熟的 YAML parser dependency（并更新 lockfile），若现有依赖不足；禁止为了少一个依赖而手写只覆盖 happy-path 的 YAML 解析器。

Markdown body 的结构解析必须 deterministic；不调用 LLM，不做语义猜测。

## DEC-05｜Legacy body 全量保留，公开内容只显式放行

为避免旧资产 private / backstage truth 被误公开：

```text
legacy full body
→ preserved as private Source semantic section
```

Adapter Profile 可以另外声明 exact structural selectors，将明确 public 内容复制 / 提取为 public sections。

安全默认：

```text
unclassified legacy body = private
```

禁止用“标题看起来像公开内容”或模型语义判断自动升级为 public。

因此：

```text
safe over-restriction
> accidental disclosure
```

但 Adapter 必须报告哪些 public selectors 成功/失败，不能静默丢内容。

## DEC-06｜Semantic fields 不得静默丢弃

Adapter 必须输出 deterministic diagnostics，至少区分：

- consumed machine fields；
- explicitly ignored governance fields；
- preserved raw semantic content；
- unmapped semantic fields / refs；
- unresolved dependency/reference targets。

以下 legacy 字段若存在，不能作为普通 governance key 静默忽略：

- `hard_dependencies`；
- `optional_integrations`；
- `providers`；
- `capability_core`；
- 以及 profile 明确声明为 dependency/reference 的字段。

需要通过显式 profile mapping 转成 G9-03 dependency / reference，或 fail / report unsupported；不得 fuzzy-resolve Wikilink。

## DEC-07｜Adapter 输出必须回到唯一 G9-03 Validator

正确链：

```text
Legacy Adapter
→ candidate TavernAssetV1
→ existing G9-03 validator
→ existing canonical serializer / digest
```

禁止复制第二套 asset validator/hash 实现。

同一 Markdown bytes + 同一 profile 必须得到完全一致 canonical bytes / digest。

## DEC-08｜World / Character / Expansion 各自 typed mapping

### World

至少映射：

- assetRef（profile explicit）；
- version（exact frontmatter value）；
- metadata title / aliases / language / tags；
- full private semantic body；
- optional explicit public extracts；
- explicit composition mapping（若 profile 提供）。

World body 不直接成为 Runtime State。

### Character

至少映射：

- assetRef（profile explicit）；
- version；
- displayName；
- aliases；
- playerCharacterSupported（只有 source/profile 有明确证据时）；
- full private semantic body；
- reference/dependency mappings；
- optional public extracts。

不得从 Character Markdown 直接生成 current position / current relationship / knowledge / role / injury / live state。

### Expansion

至少映射：

- stable `assetRef`；
- version；
- packageRef / Source ownerNamespace；
- Feature / Source Module declarations；
- `runtimeModuleRef`；
- typed dependency mapping；
- routing / projection / UI declaration seams；
- full private semantic body。

G9-03A 继续强制：

```text
Source moduleRef != Program Runtime moduleRef
RuntimeDomainModuleBinding.moduleRef = runtimeModuleRef
```

G9-04 **不实现新的 Politics / Capability / War / Economy 等 Domain mechanics**。真实 asset adapter 的 runtime binding profile只证明 mapping/Host compatibility，不代表对应 Domain gameplay 已在本阶段交付。

## DEC-09｜Game Asset Manifest exact pinning remains the creation boundary

Adapter 不直接把“当前 catalog 最新版本”塞进 Runtime。

必须：

```text
selected validated assets
↓
exact assetRef + type + version + digest
↓
TavernGameAssetManifestV1
```

已有 Manifest 在 Source v2 发布后仍解析到原 v1 snapshot。

## DEC-10｜G9-04 Binding Anchor 不是实体物化

为证明三个主资产真正进入 G9-02 lineage，允许增加 Program-owned internal binding anchor：

```text
Runtime ownerNamespace = runtime.asset-binding
visibility = hidden
sourceAsset = exact SourceAssetDescriptor
```

binding anchor 的职责只有：

- 记录本局绑定了哪个 exact primary Source snapshot；
- 进入 G9-02 Game-local lineage / Save-Restore rails；
- 给后续 T0 resolver / materializer / compiler 提供 durable referent。

它**不是**：

- World runtime state；
- materialized Character；
- Domain state；
- Player-known object；
- UI item。

因此不得因为绑定 Character Card 就在场景中生成角色；No Phantom 永久有效。

## DEC-11｜Expansion activation 必须继续经过 Program Host

如果 G9-04 binding plan 包含 active Expansion module：

```text
validated Source runtimeModuleRef
→ Program-known registry
→ RuntimeDomainModuleBinding
→ RuntimeDomainModuleHost.validateBindings()
```

unknown / unsupported / duplicate active runtime module fail closed。

G9-04 可以用 Program-owned fixture module 做 vertical binding proof，但不得把 fixture 冒充真实 Domain mechanics completion。

## DEC-12｜Library 只做最小协议 proof

G9-04 Library scope：

```text
fixture/source sample
→ deterministic parse
→ TavernAssetV1 library
→ validate
→ canonical round-trip
→ stable entry + provenance + audience + cross-reference validation
```

明确禁止：

- Game-local Library truth；
- Runtime retrieval/indexing；
- embedding/vector；
- Model Reference Provider；
- Library product UI；
- Creator Library editor。

## DEC-13｜Real Asset Gate 与 portable unit tests 分离

仓库内 unit tests 不能依赖开发者机器必然存在 sibling assets repo。

因此分两层：

```text
portable synthetic fixtures
→ npm test / CI style gates

real canonical asset gate
→ reads SILLYTAVERN_ASSETS_DIR or known sibling repo
→ records exact assets repo HEAD + exact source paths/blob SHAs
```

真实资产正文不得为了测试复制一整份进 `sillytavern` 仓库。

Real Asset Gate 必须 AI-independent、无 Provider。

---

# 3. Required Real Sample Corpus

G9-04 至少使用以下 current real sources：

### World

`世界包/汉末三国_天下未定_World_Pack_v0.2.3.md`

Initial stable `assetRef` assignment for this vertical：

```text
world:han-late-three-kingdoms
```

### Character

`人物卡/汉末三国/CC-BATCH-01/刘备__Character_Card__v0.1.2.md`

Initial stable `assetRef` assignment：

```text
character:han-late.liu-bei
```

### Expansion

`拓展包/通用拓展包/人物能力与技艺_Expansion_Pack_v0.1.5.md`

Existing stable ID is reused：

```text
EP-CHAR-CORE
```

### Secondary parser robustness sample

至少再读取一个结构差异明显的 current Markdown（推荐）：

- `世界包/埃瑟维亚_诸界余辉_World_Pack_v0.1.3.md`；或
- `人物卡/诸界余辉/CC-01_莉维娅·塞兰_Character_Card_v0.2.md`。

它用于证明 parser/profile 不是为单一 Han 文件硬编码；是否进入 binding vertical 不是 G9-04 Exit 的强制条件。

### Library

当前资产仓库没有已冻结为产品资产的 Library canonical body；因此使用**明确标记 fixture-only** 的最小 Library Markdown/normalized input做协议 proof，不得把 fixture 宣布为第四类主资产或正式汉末资料库。

---

# 4. Binding Compiler Foundation

G9-04 可以新增 internal：

```ts
interface CompiledGameAssetBindingPlanV1 {
  sourceDescriptors: readonly SourceAssetDescriptor[];
  primaryBindingAnchors: readonly AdditionalGameLocalAssetDefinition[];
  domainModuleBindings: readonly RuntimeDomainModuleBinding[];
}
```

精确命名可以按仓库分层调整，但语义必须满足：

1. input 只能是已经 validate 的 `TavernGameAssetManifestV1 + exact catalog`；
2. source descriptors 通过现有 G9-03 `mapAssetToSourceDescriptor()`；
3. primary binding anchors 一一对应 selected World / Character / Expansion snapshots；
4. anchor 使用 exact Source descriptor，不重新生成 identity/hash；
5. Library 不因本最小 proof 自动成为 Game-local canonical truth；
6. active Expansion binding 使用 G9-03A mapping，并经过 Program Host validation；
7. applying binding plan 只能扩展 `RuntimeDefinition` 的 existing seams，不替换 G9-02 bootstrap authority。

### 4.1 Real lineage proof

必须使用真实 `SQLiteRuntimeStore.bootstrap()` 或等价 production path 证明：

```text
exact manifest
→ compiled binding plan
→ bootstrap
→ gameLocalAssets[].sourceLineage
```

World / Character / Expansion 三个 primary source snapshot 的 stableRef/version/contentHash 必须与 manifest exact match。

### 4.2 No silent source update

在 game 已用 v1 manifest bootstrap 后，即使 catalog 新增同 `assetRef` v2：

- existing game sourceLineage 不变；
- old manifest 仍 exact resolve v1；
- 不自动 rebind。

### 4.3 Save / Restore

Binding anchors 必须进入 existing canonical Save / Restore semantics；Restore 后 lineage 不丢失、不升级、不重复。

---

# 5. Required Diagnostics / Fail-closed Classes

G9-04 可以增加新的 internal error codes；至少稳定区分：

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

不得把所有 Adapter failure 压成一个 generic parse error。

---

# 6. Acceptance Gates

## AC-01｜AI-independent parsing

Real Markdown → normalized legacy parse 全程无模型调用；重复输入得到相同 parse result。

## AC-02｜Explicit identity

World / Character `assetRef` 来自 explicit profile；改 filename/title 不重新生成 identity。

## AC-03｜No semantic body loss / safe disclosure

完整 Markdown body 被保留为 private semantic source；public section 只能通过 explicit selector 产生；selector mismatch fail / diagnostic。

## AC-04｜World real sample

汉末 World current source → valid `TavernAssetV1 world`，version/title/aliases/language/tags/body 不发生非授权丢失。

## AC-05｜Character real sample

刘备 current source → valid `TavernAssetV1 character`；不会创建 current position/role/knowledge/relationship state。

## AC-06｜Expansion real sample

EP-CHAR-CORE current source → valid `TavernAssetV1 expansion`；stable ID 保持 `EP-CHAR-CORE`；Source module / runtime module identity 分层符合 G9-03A。

## AC-07｜Dependency/reference discipline

Legacy Wikilinks / dependency fields 只能由 exact profile mapping resolve；no fuzzy name resolution；unresolved machine-relevant reference fail/report。

## AC-08｜G9-03 validator reuse

所有 adapter outputs 使用现有 G9-03 validator/hash；无第二 validator/digest path。

## AC-09｜Manifest exact pinning

World + Character + Expansion 形成 exact `TavernGameAssetManifestV1`；v2 catalog addition 不改写 old manifest。

## AC-10｜Real G9-02 binding lineage

manifest → binding plan → real bootstrap 后，三个 primary binding anchors 的 sourceLineage exact match manifest snapshots。

## AC-11｜No Phantom

Character Card binding anchor 不产生可见 Character entity、scene placement、player-known membership 或 runtime character state。

## AC-12｜Expansion Host proof

如 active module 被编译，真实 `RuntimeDomainModuleHost.validateBindings()` PASS；unknown/unsupported/duplicate active mapping fail closed。

## AC-13｜Save / Restore

binding lineage 在 Save / Restore 后 exact 保持，无 silent source upgrade。

## AC-14｜Library minimal proof

Library fixture parse / validate / canonical round-trip / stable entry / provenance / audience / cross-reference PASS；无 retrieval/UI/runtime truth。

## AC-15｜Real Asset Gate

记录并验证：

```text
assets repo HEAD = 968175e6c3fb3545b7c2907b65089c7e1dbb40a0
```

若执行时 HEAD 已变化，必须先做 Freshness diff；不得旧 profile 静默适配新 source。

## AC-16｜Regression

G9-03、G9-02 closure、G5/G6/G7/G8/full tests/typecheck/lint/build/launcher/disclosure/diff-check 全 PASS。

## AC-17｜No scope drift

没有 Creator、Library retrieval、Domain mechanics implementation、第二 Source identity、第二 Game-local binding authority、G9-05 scope。

---

# 7. Implementation Guidance

推荐模块边界：

```text
src/资产适配/
├─ L0_公理层/Legacy资产适配契约.ts
├─ L1_器件层/LegacyMarkdown解析器.ts
├─ L1_器件层/Legacy资产适配器.ts
├─ L1_器件层/游戏资产绑定编译器.ts
├─ L3_外交层/资产适配公开接口.ts
└─ 模块说明.md
```

可根据现有仓库分层做小幅调整。

跨模块依赖：

```text
资产适配 L3/L1
→ 资产协议 L3
→ 运行时 L3
```

禁止低层直接依赖对方内部 L0/L1 实现。

如果新增 YAML dependency，必须最小化、更新 lockfile、报告用途；不要新增 Markdown AST framework，除非真实 current source 证明结构解析无法用小型 deterministic parser完成。

---

# 8. Non-scope

- 不批量重写全部 canonical Markdown；
- 不把 current assets 全部迁成 JSON；
- 不实现 Creator；
- 不实现 Library product；
- 不实现 Runtime Library retrieval；
- 不实现新 Domain gameplay mechanics；
- 不改 `tavern.asset.v1` wire；
- 不回开 G9-02 authority；
- 不增加 arbitrary script/query/expression DSL；
- 不因 Wikilink / title 相似自动猜 identity。

---

# 9. Exit

G9-04 只有在 implementation + exact-SHA Independent Review 完成后才可 `PASS / CLOSED`。

本规格冻结后：

```text
G9-04 Spec             FROZEN
G9-04 Implementation   NEXT
G9-05                  NOT AUTHORIZED
```

实现 Agent 完成后只允许返回：

**G9-04 ADAPTER / COMPILER / BINDING READY FOR INDEPENDENT REVIEW**
