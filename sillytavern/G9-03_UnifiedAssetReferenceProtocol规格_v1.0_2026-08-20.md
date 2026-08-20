---
title: G9-03 Unified Asset / Reference Protocol 规格
status: current-spec-semantics-frozen
version: 1.0
date: 2026-08-20
stage: G9-03
next_gate: implementation + validation
---

# G9-03｜Unified Asset / Reference Protocol 规格 v1.0

## 0. 结论

G9-02 Runtime foundation 已 `PASS / CLOSED`。G9-03 正式冻结世界包、角色卡、拓展包与资料库 Reference Resource Layer 的 **v1 machine contract semantics**。

本规格冻结：

```text
Source Asset identity / version / integrity
+
四类 typed payload
+
typed dependency / reference / composition
+
Expansion Package → Feature → Module declarations
+
Runtime / UI binding declarations
+
Library stable entries / audience / provenance
+
Bundle transport manifest
+
Game Asset Manifest exact snapshot binding
+
compatibility / explicit migration seams
```

并永久保持：

```text
Source Asset
!= Game-local Canonical Instance
!= Runtime State
```

```text
三类主资产
= World + Character + Expansion

Library
= Reference Resource Layer
!= 第四类主资产
!= Runtime Truth
```

G9-03 v1 不要求重写现有 Markdown 资产正文。G9-04 才负责 adapter / compiler，把当前 canonical Markdown 资产转换成此 machine contract 并证明 parse / validate / round-trip / Game-local binding。

---

## 1. Authority / Evidence

本规格吸收并受以下 current authority 约束：

- `酒馆游戏项目开发核心总纲_CURRENT.md`；
- `酒馆游戏新版主体重建总路线 v2.3.md`；
- `G9-02_IntegratedClosure_IndependentReview_最终收口_v1.0_2026-08-20.md`；
- `G9-01_资产兼容性审计与G9-02基础门禁_v1.0_2026-08-18.md`；
- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.2_2026-08-19.md`；
- `18A_酒馆游戏_资料库资源层协议护栏增量裁定_v1.0_2026-08-19.md`；
- `15 / 16 / 17` Runtime / Game-local / Player-known current decisions；
- `sillytavern main@ab09c7ce6960a99b062d22fd49c143f9ae876f4e`；
- `sillytavern-assets main@bed1b4c93d84df2b83723ddeb3ff479203bb6f52`；
- `tavern-asset v1.0`。

现实样本包括：

- `汉末三国_天下未定_World_Pack_v0.2.3.md`；
- `刘备__Character_Card__v0.1.2.md`；
- `EP-POLITICS-CORE｜政治与公共权力核心 v0.1`；
- Generic Library Runtime Context Contract Pattern v0.6。

现有 Markdown frontmatter 是语义创作 / 治理事实，不自动等于本协议 machine fields。

---

# 2. Frozen Decisions

## DEC-01｜Canonical wire object

v1 的 canonical machine object 使用 JSON-compatible data model。

```text
schemaVersion = tavern.asset.v1
```

YAML / Markdown / Creator form 可以作为 authoring / import source，但进入 validator / compiler 前必须规范化成同一种 `TavernAssetV1` object。

```text
Authoring format
!= Canonical machine object
```

## DEC-02｜一个统一 Envelope，四种 typed payload

```text
TavernAssetV1
├─ world
├─ character
├─ expansion
└─ library
```

World / Character / Expansion 是三类主资产；Library 仅在统一协议中复用 Envelope，不因此成为第四类主资产。

## DEC-03｜Stable asset identity 不依赖文件名 / display name

`assetRef` 是稳定、不可由标题推断的 Source identity。

```text
filename change
!= asset identity change

title change
!= asset identity change
```

同一 `{assetRef, version}` 不允许对应不同内容。

## DEC-04｜Source Snapshot identity

一个精确 Source Snapshot 由：

```text
assetRef
+ assetType
+ version
+ sha256 content digest
```

共同识别。

它一一映射 G9-02 已冻结的：

```text
SourceAssetDescriptor.stableRef
SourceAssetDescriptor.assetType
SourceAssetDescriptor.version
SourceAssetDescriptor.contentHash
```

不得建立第二套 Runtime source identity。

## DEC-05｜Game Asset Manifest pins exact snapshots

Source Asset 的依赖 / 推荐可以指向 stable `assetRef`；但一局游戏真正创建时必须生成 exact resolved `Game Asset Manifest`：

```text
assetRef + version + sha256
```

该 Manifest 是创建 / binding input，不是 Runtime State。

因此：

```text
Source update
!= existing game silent update
```

## DEC-06｜Dependency Graph != Context Inclusion Graph

依赖协议表达组合与 capability availability，不表达模型 Prompt inclusion。

禁止任何机器字段等价于：

```text
hard dependency
→ always load entire dependency into model context
```

## DEC-07｜Expansion 声明 capability，Program owns executable implementation

External Asset 只能引用 Program 已注册的 `runtimeModuleRef / configSchemaRef / projectionRef / UI capability`。

禁止 Source Asset 携带：

- JS / TS / WASM；
- callback；
- eval / expression DSL；
- SQL / arbitrary query；
- arbitrary DB path；
- arbitrary state mutation script。

## DEC-08｜Feature / Module activation 是 v1 protocol first-class

必须区分：

```text
Package Included
!= Feature Enabled
!= Module Enabled
!= Runtime Relevant
!= Model Visible
```

## DEC-09｜Library retrieval profile 只冻结 identity / relevance hints，不冻结 search engine

v1 允许稳定 entry、summary、semantic tags、related refs、audience policy。

禁止冻结：

- embedding model；
- chunk size；
- vector dimensions；
- ranking algorithm；
- RAG provider；
- arbitrary query DSL。

## DEC-10｜Library audience eligibility != knowledge

```text
entry model-eligible
!= current model must see it

entry player-eligible
!= player knows it

entry character-eligible
!= character knows it
```

最终仍需 purpose / player-known / character-known / disclosure gate。

## DEC-11｜Library Reference Projection 不拥有现实

```text
Library Source
→ bounded retrieval
→ Reference Projection
→ candidate / authoring reference
→ Program / Domain Owner validation
→ canonical commit only when authorized
```

Reference Projection 本身永远不是 Game-local Truth、Runtime State 或 Formal Outcome。

## DEC-12｜Large Relation Graph 只保存 canonical source facts

协议允许大型 relation / registry 的 Source declarations，但 Runtime 不得因为 Source 可访问而把 whole graph 送入模型。

Program 必须按 G9-02 rails 生成 current-relevant、owner-safe、player-safe bounded subgraph / projection。

## DEC-13｜Asset Contract != Creator Tool Contract != Conversation Protocol

v1 Source Asset 禁止保存：

- chat history；
- prompt；
- provider；
- AI action log；
- Creator tool transport；
- undo transaction history。

Creator Draft 在 G9-05 另有工作区状态；Save / Publish 后才生成合法 Source Asset。

---

# 3. Common Machine Contract

以下 TypeScript-like 定义是 v1 的 normative semantic shape。实现可以使用 TypeScript types + JSON Schema / AJV，但字段名与边界不得擅自改变。

```ts
type TavernAssetTypeV1 = 'world' | 'character' | 'expansion' | 'library';

type SourceVisibilityV1 = 'public' | 'private';

type AssetDependencyKindV1 =
  | 'hard'
  | 'optional'
  | 'feature_conditional'
  | 'reference';

interface AssetSnapshotRefV1 {
  assetRef: string;
  assetType: TavernAssetTypeV1;
  version: string;
  contentHash: string; // sha256:<64 lowercase hex>
}

interface AssetStableRefV1 {
  assetRef: string;
  assetType: TavernAssetTypeV1;
}

interface AssetMetadataV1 {
  title: string;
  aliases?: readonly string[];
  language?: string;
  tags?: readonly string[];
  summary?: string;
}

interface AssetDependencyV1 {
  dependencyRef: string;
  kind: AssetDependencyKindV1;
  target: AssetStableRefV1;
  sourceScope?: {
    featureRef?: string;
    moduleRef?: string;
  };
  requiredCapabilityRefs?: readonly string[];
}

interface AssetCompatibilityV1 {
  requiredHostCapabilities?: readonly string[];
  supersedes?: readonly AssetStableRefV1[];
  migrationFrom?: readonly {
    source: AssetSnapshotRefV1;
    migrationRef: string;
  }[];
}

interface SemanticSectionV1 {
  sectionRef: string;
  sectionKind: string;
  title: string;
  body: string;
  visibility: SourceVisibilityV1;
}

interface TavernAssetV1 {
  schemaVersion: 'tavern.asset.v1';
  assetRef: string;
  assetType: TavernAssetTypeV1;
  version: string;
  integrity: {
    algorithm: 'sha256';
    digest: string; // sha256:<64 lowercase hex>
  };
  metadata: AssetMetadataV1;
  dependencies?: readonly AssetDependencyV1[];
  compatibility?: AssetCompatibilityV1;
  payload:
    | WorldAssetPayloadV1
    | CharacterAssetPayloadV1
    | ExpansionAssetPayloadV1
    | LibraryAssetPayloadV1;
}
```

### 3.1 Discriminated payload rule

Validator 必须强制：

```text
assetType = world      → WorldAssetPayloadV1
assetType = character  → CharacterAssetPayloadV1
assetType = expansion  → ExpansionAssetPayloadV1
assetType = library    → LibraryAssetPayloadV1
```

不得接受 type/payload mismatch。

### 3.2 assetRef rules

v1 `assetRef`：

- non-empty；
- case-sensitive；
- immutable for the logical Source Asset；
- 不从 title / filename 自动生成或重新计算；
- import collision 时若 identity 相同但历史不一致，fail closed；
- assetRef rename 等价于新 identity，必须显式 migration / supersession。

v1 不强迫使用 UUID，也不强迫 GitHub namespace；现有稳定 ID 如 `EP-POLITICS-CORE` 可以合法迁入。

### 3.3 version rules

`version` 是 Source publisher 管理的 opaque non-empty version token。

v1 不在 Runtime 内实现 SemVer range solver。

```text
exact game binding
→ always pins exact version + hash
```

依赖的 long-lived identity 使用 `assetRef`；版本兼容由 import / catalog validator 与 explicit compatibility metadata 处理，不由 Runtime 猜测。

### 3.4 integrity rules

规范化 machine object 的完整内容（除 `integrity.digest` 自身）必须经过 deterministic JSON serialization 后计算 SHA-256。

同一：

```text
assetRef + version
```

若出现不同 digest：

```text
INTEGRITY_CONFLICT
```

不得“最后写入者覆盖”。

---

# 4. World Asset Payload

```ts
type CompositionDispositionV1 = 'default' | 'recommended' | 'optional';

interface WorldCompositionEntryV1 {
  compositionRef: string;
  target: AssetStableRefV1;
  disposition: CompositionDispositionV1;
}

interface WorldAssetPayloadV1 {
  kind: 'world';
  sections: readonly SemanticSectionV1[];
  composition?: readonly WorldCompositionEntryV1[];
}
```

World 继续负责：

- 世界身份 / 历史社会地理基线；
- 世界级规则与限制；
- T0 / opening 语义来源；
- 默认 / 推荐组合；
- public/private Source semantic sections。

World 不直接保存：

- 当前人物 / 地点 /政治 / 战争 live state；
- Runtime revision / turn；
- Formal Outcome；
- current Player-known；
- Creator conversation。

### 4.1 composition semantics

World 可以推荐 / 默认：

- Character；
- Expansion；
- Library。

`default / recommended / optional` 只影响新游戏组合 UX / resolver，不自动把依赖内容变成 Prompt。

---

# 5. Character Asset Payload

```ts
interface CharacterAssetPayloadV1 {
  kind: 'character';
  displayName: string;
  aliases?: readonly string[];
  playerCharacterSupported?: boolean;
  sections: readonly SemanticSectionV1[];
  referenceSources?: readonly {
    referenceRef: string;
    libraryEntryRef?: string;
    note?: string;
  }[];
}
```

Character Source 保存稳定人物 Definition / authoring evidence，例如：

- identity；
- personality；
- decision logic；
- capability evidence；
- voice / expression；
- player takeover guidance；
- resolver handoff semantics。

但：

```text
Character Source Definition
!= T0 resolved instance
!= current character state
!= player-known dossier
```

具体官职、位置、关系、知识、伤病、能力成长、政治状态等必须由 T0 / Domain Owner / Runtime 在一局内形成。

`referenceSources` 只能表达 provenance / inspiration，不让 Library 成为 Character owner。

---

# 6. Expansion Asset Payload

```ts
interface ExpansionFeatureV1 {
  featureRef: string;
  label: string;
  defaultEnabled: boolean;
}

interface ExpansionRoutingProfileV1 {
  label: string;
  scope: string;
  typicalSemantics: readonly string[];
}

interface TypedConfigV1 {
  schemaRef: string;
  value: unknown;
}

interface ExpansionModuleV1 {
  moduleRef: string;
  featureRef: string;
  label: string;
  defaultEnabled: boolean;
  runtimeModuleRef: string;
  runtimeModuleVersion?: string;
  config?: TypedConfigV1;
  routingMode: 'model_immediate' | 'program_activated';
  routingProfile?: ExpansionRoutingProfileV1;
  projectionRef?: string;
  capabilityRefs?: readonly string[];
  definitionRegistryRefs?: readonly string[];
  acceptedHandoffKinds?: readonly string[];
  emittedHandoffKinds?: readonly string[];
}

interface ExpansionUiSurfaceV1 {
  surfaceId: string;
  label: string;
  recommendedAfterSurfaceId?: string;
  secondaryViews?: readonly { viewId: string; label: string }[];
}

interface ExpansionUiContributionV1 {
  contributionRef: string;
  title: string;
  hostCapability:
    | 'player_status'
    | 'player_character_detail'
    | 'entity_detail'
    | 'core_surface'
    | 'map_overlay'
    | 'narrative_contextual'
    | 'global_notice'
    | 'game_creation_settings'
    | 'extension_surface';
  targetRef?: string;
  componentKind: string;
  projectionRef?: string;
  staticData?: Readonly<Record<string, string | number | boolean>>;
}

interface ExpansionAssetPayloadV1 {
  kind: 'expansion';
  packageRef: string;
  ownerNamespace: string;
  features: readonly ExpansionFeatureV1[];
  modules: readonly ExpansionModuleV1[];
  ui?: {
    ownsExtensionSurfaces?: readonly ExpansionUiSurfaceV1[];
    contributions?: readonly ExpansionUiContributionV1[];
  };
  sections: readonly SemanticSectionV1[];
}
```

## 6.1 Program-owned Runtime module gate

`runtimeModuleRef` 必须解析到 Program 注册的 built-in module。

```text
Source Expansion
→ references built-in Runtime module
→ Program validates module / config schema / capabilities
→ creates RuntimeDomainModuleBinding
```

Source 不得上传代码。

`config.value` 只有在 `schemaRef` 对应 Program-known typed validator 时才有效；未知 `schemaRef` fail closed。

## 6.2 Package / Feature / Module mapping

编译后至少能够无歧义构造：

```text
RuntimeDomainModuleBinding.packageRef
RuntimeDomainModuleBinding.featureRef
RuntimeDomainModuleBinding.moduleRef
```

Feature / Module disabled 时：

- 不产生 active binding；
- 不进入 immediate Router directory；
- 不触发 conditional dependency；
- 不拥有 active UI contribution / state。

## 6.3 Routing profile is semantic directory, not authorization

`routingProfile` 只允许描述：

- label；
- one-line scope；
- typical semantics。

不得把关键词表当玩家行为白名单。

## 6.4 Runtime Context Contract boundary

Semantic Asset 可以在 `sections` 中保存完整 19 项 Context Contract 说明。

machine v1 只冻结执行所需的最小声明 seam：

- routing mode / profile；
- built-in runtime module；
- projection capability；
- capability / registry refs；
- handoff kinds。

State-mandatory activation、graph traversal、background progression、Formal Outcome、candidate validation 等仍由 Program module 实现；本协议不引入条件表达式 DSL。

## 6.5 UI declaration boundary

Expansion UI declaration 只能选择 G8 Host capability 与 Program-known projection/data capability。

禁止：

- arbitrary DOM / React / CSS / script；
- state object path；
- callback；
- runtime query expression。

G9-04 compiler 将其验证并映射到 G8 `PlayerProductRuntimeUiDefinition` / Host seam。

---

# 7. Library Asset Payload

```ts
type LibraryAudienceV1 =
  | 'creator_reference'
  | 'model_reference'
  | 'player_reference'
  | 'character_reference';

type LibraryAccessV1 = 'eligible' | 'gated' | 'forbidden';

interface LibraryProvenanceV1 {
  provenanceRef: string;
  sourceLabel: string;
  locator?: string;
  note?: string;
}

interface LibraryEntryV1 {
  entryRef: string;
  entryKind: string;
  title: string;
  summary?: string;
  body: string;
  provenance: readonly LibraryProvenanceV1[];
  audience: Readonly<Record<LibraryAudienceV1, LibraryAccessV1>>;
  semanticTags?: readonly string[];
  relatedRefs?: readonly string[];
}

interface LibraryAssetPayloadV1 {
  kind: 'library';
  sections?: readonly SemanticSectionV1[];
  entries: readonly LibraryEntryV1[];
}
```

## 7.1 Stable entry identity

`entryRef` 在同一 Library `assetRef` 内必须稳定且唯一。

Library version 可以更新 entry 内容，但既有游戏仍绑定旧 Library snapshot。未来显式 rebind / migration 后才能读取新 snapshot。

## 7.2 Audience semantics

四种 audience 必须显式存在；缺字段 fail closed。

推荐安全默认：

```text
creator_reference   eligible
model_reference     gated
player_reference    gated
character_reference gated
```

含义：

- `eligible`：该职责原则上可使用，但仍受 current purpose / selection 约束；
- `gated`：必须再经过对应 disclosure / knowledge / authorization gate；
- `forbidden`：不得进入该职责投影。

即使是 `eligible`：

```text
Library bound
!= Library entry relevant
!= Library entry model-visible
```

## 7.3 Retrieval hints are declarative only

`semanticTags / relatedRefs / summary` 只是未来索引 / bounded retrieval 的稳定语义 hints。

它们不冻结：

- 分块方式；
- embedding；
- 排序；
- query DSL；
- Provider。

## 7.4 Reference object != Game definition object

允许：

```text
Library historical-person entry
+
Character Card for same historical person
```

两者 identity 与 Owner 独立。

Character / World 可以引用 Library entry 作为 provenance，但不得降级成“只存一个 library pointer 的壳”。

---

# 8. Bundle Manifest｜Transport / Import / Export

Bundle 不是第五类资产，也不是 Runtime Truth。

```ts
interface TavernBundleEntryV1 {
  snapshot: AssetSnapshotRefV1;
  source:
    | { kind: 'embedded'; path: string }
    | { kind: 'catalog_reference' };
}

interface TavernBundleManifestV1 {
  schemaVersion: 'tavern.bundle.v1';
  bundleRef: string;
  version: string;
  integrity: {
    algorithm: 'sha256';
    digest: string;
  };
  entries: readonly TavernBundleEntryV1[];
  rootAssetRefs?: readonly string[];
}
```

Bundle 的职责：

- import / export transport；
- asset snapshot inventory；
- embedded path mapping；
- integrity closure。

Bundle 不拥有：

- dependency truth；
- runtime enabled state；
- player knowledge；
- game save state。

Asset 自身 dependencies / composition 仍是 Source truth。

---

# 9. Game Asset Manifest｜Resolved Creation Binding

新游戏真正进入 G9-02 source binding 前，必须先解析为 exact snapshot manifest。

```ts
interface ResolvedExpansionBindingV1 {
  snapshot: AssetSnapshotRefV1; // expansion
  packageIncluded: boolean;
  enabledFeatureRefs: readonly string[];
  enabledModuleRefs: readonly string[];
}

interface ResolvedLibraryBindingV1 {
  snapshot: AssetSnapshotRefV1; // library
  bindingRef: string;
}

interface TavernGameAssetManifestV1 {
  schemaVersion: 'tavern.game-assets.v1';
  world: AssetSnapshotRefV1;
  characters: readonly AssetSnapshotRefV1[];
  expansions: readonly ResolvedExpansionBindingV1[];
  libraries: readonly ResolvedLibraryBindingV1[];
}
```

## 9.1 Manifest authority

Game Asset Manifest 是：

```text
resolved creation / binding input
```

不是：

```text
Game-local Canonical Instance
Runtime State
Save Snapshot replacement
```

Bootstrap / compiler 对每个 snapshot 生成 / 复用 G9-02 `SourceAssetDescriptor`，再走现有 bind path。

## 9.2 Exact pinning

所有 Manifest entries 必须 exact：

```text
assetRef + assetType + version + hash
```

不允许在 Runtime 启动时偷偷“查最新版本”。

---

# 10. Dependency / Reference Validation

## 10.1 Hard dependency

`hard` 缺失：asset composition invalid，fail closed。

## 10.2 Optional dependency

`optional` 缺失：asset 仍可合法存在；相关 optional capability 不激活。

## 10.3 Feature conditional dependency

必须携带 `sourceScope.featureRef` 或 `sourceScope.moduleRef`。

只有对应 Feature / Module enabled 时才要求 target availability。

## 10.4 Reference

`reference` 只表达 read-only semantic / provenance relation。

```text
Reference dependency
!= Runtime owner dependency
!= Prompt inclusion
```

## 10.5 Cycle rules

Hard Dependency Graph 禁止 cycle。

Optional / Reference relation 可以形成图，但不得通过 fallback 产生第二 truth。

---

# 11. Runtime Mapping｜必须复用 G9-02

G9-04 compiler 必须证明以下 mapping，不得创造替代轨道。

## 11.1 Source identity

```text
TavernAssetV1
assetRef / assetType / version / integrity.digest
↓
SourceAssetDescriptor
stableRef / assetType / version / contentHash
```

## 11.2 Game-local lineage

```text
Game Asset Manifest exact snapshot
↓ bind
GameLocalSourceLineage
↓
GameLocalAssetMetadata.definitionRevision
```

Source 不被 Runtime 反写。

## 11.3 Expansion binding

```text
Expansion package / feature / module
↓ validate built-in runtimeModuleRef
↓
RuntimeDomainModuleBinding
```

Domain Canonical Record 必须继续通过 `recordRef` 复用 GameLocalAssetMetadata identity。

## 11.4 Runtime state

External Asset 不直接创建 arbitrary state table。

只有 Program-known built-in module / codec 可以把 validated config / definitions materialize 为：

```text
RuntimeDomainCanonicalRecord
RuntimeDomainState
```

## 11.5 Context

```text
Source dependencies / Context Contract
!= model prompt
```

实际模型上下文继续由 G9-02：

```text
bounded Routing Catalog
→ selected modules
→ selected-only JIT Projection
→ owner-preserving join
```

## 11.6 UI

External declarations 编译到 G8 Host-safe declarations；Host 仍拥有布局、安全、player-safe live data、响应式与 accessibility。

Source Asset 不获得浏览器执行权。

---

# 12. Compatibility / Update / Migration

## 12.1 Immutable published snapshot

发布后的 `{assetRef, version, hash}` 视为 immutable snapshot identity。

修订内容必须产生新 version / hash。

## 12.2 Existing game

```text
asset v1 game binding
+
asset author publishes v2
→ existing game stays on v1 snapshot
```

## 12.3 Explicit rebind / migration

未来既有游戏升级 Source 版本必须是：

```text
explicit request
→ compatibility validation
→ migration plan
→ owner-aware rebind
→ Save / Restore / Recovery regression
→ atomic commit
```

本 G9-03 只冻结 seam；不实现完整 rebind UX。

## 12.4 Supersession

`compatibility.supersedes` 是 Source lineage / catalog hint，不自动删除旧游戏 binding。

---

# 13. Integrity / Canonicalization

G9-03 implementation 必须提供 deterministic canonical serializer。

Hash material：

```text
entire normalized TavernAssetV1
minus integrity.digest
```

规则：

- UTF-8；
- object key order deterministic；
- array order preserved；
- no non-JSON values；
- no timestamps generated during serialization；
- no filesystem path in Asset digest unless path is authored asset content；
- hash output lowercase `sha256:<hex>`。

Bundle 同理，digest 计算时排除自己的 `integrity.digest`。

---

# 14. Validation Error Classes

v1 validator 至少稳定区分：

```text
INVALID_SCHEMA_VERSION
INVALID_ASSET_TYPE
PAYLOAD_TYPE_MISMATCH
INVALID_ASSET_REF
DUPLICATE_ASSET_REF
DUPLICATE_SECTION_REF
DUPLICATE_DEPENDENCY_REF
INTEGRITY_MISMATCH
INTEGRITY_CONFLICT
DEPENDENCY_MISSING
HARD_DEPENDENCY_CYCLE
CONDITIONAL_DEPENDENCY_SCOPE_MISSING
UNKNOWN_RUNTIME_MODULE
UNKNOWN_CONFIG_SCHEMA
UNKNOWN_PROJECTION
INVALID_FEATURE_REF
INVALID_MODULE_REF
INVALID_MODULE_FEATURE_PARENT
UNKNOWN_UI_CAPABILITY
UI_SURFACE_OWNERSHIP_CONFLICT
DUPLICATE_LIBRARY_ENTRY_REF
INVALID_LIBRARY_AUDIENCE
BUNDLE_SNAPSHOT_MISSING
BUNDLE_SNAPSHOT_MISMATCH
GAME_MANIFEST_UNRESOLVED_SNAPSHOT
```

实现可以增加更细错误，但不得把关键错误压成 generic `INVALID_ASSET`。

---

# 15. Security / Information Boundary

Source / Bundle / Library validator 必须 fail closed：

- unknown executable field；
- script / callback / eval；
- arbitrary object path；
- arbitrary runtime query；
- unknown module / projection / config schema；
- hidden/private material 被声明为无条件 player-visible；
- Library audience 缺失或非法。

Asset Source 中的 `private` section / Library restricted entry 只代表 Source-side boundary；实际进入 Player / Model Context 仍必须经过 Runtime disclosure gate。

---

# 16. G9-03 Implementation Deliverables

Grok Build implementation phase 应在 `sillytavern` 中建立：

1. v1 TypeScript contract；
2. deterministic validator；
3. deterministic canonical serializer + SHA-256 integrity；
4. four payload validators；
5. dependency graph validator；
6. Bundle validator；
7. Game Asset Manifest resolver / validator foundation；
8. mapping proof 到 `SourceAssetDescriptor`；
9. Expansion binding proof 到 `RuntimeDomainModuleBinding`；
10. UI declaration safe validation proof；
11. Library entry / audience / provenance validation；
12. fixture corpus：World / Character / Expansion / Library / Bundle / Game Manifest；
13. negative corpus 覆盖本规格 error classes。

G9-03 不要求：

- 完整 Markdown parser；
- 把全部现有资产迁成 JSON；
- Creator UI；
- Library product UI；
- Runtime Library retrieval；
- AI Creator tool protocol。

完整 current asset adapter / compiler 属于 G9-04。

---

# 17. Acceptance Gates

## AC-01｜Single Envelope / Typed Payload

四种合法样本通过；type mismatch fail closed。

## AC-02｜Identity / Integrity

相同 `assetRef + version` 不同 digest fail closed；canonical serialization 可重复。

## AC-03｜G9-02 Source Mapping

每种主资产和 Library snapshot 都能无损生成 `SourceAssetDescriptor`；不得创建第二 Source identity。

## AC-04｜Dependency Semantics

Hard / optional / conditional / reference 四类差异可自动验证；Hard graph cycle fail closed；依赖不触发 Context inclusion。

## AC-05｜Expansion Hierarchy

Package / Feature / Module identity、parent、enablement 与 built-in module refs 可验证；disabled module 不进入 active binding。

## AC-06｜No Executable Asset

任意 script / callback / unknown config schema / unknown runtime module fail closed。

## AC-07｜Context Boundary

Routing Profile 可编译为最小 directory metadata；无字段可以要求 whole dependency / whole registry / whole relation graph prompt inclusion。

## AC-08｜UI Safety

UI declaration 只能命中 G8 Host-safe capabilities；unknown capability / conflicting surface owner fail closed；无 arbitrary code/state path。

## AC-09｜Library Protocol

Library stable entry / provenance / 4-audience policy 可验证；retrieval hints 不具备 Runtime mutation authority。

## AC-10｜Reference != Truth

World / Character 引用 Library entry 时，仍保留自己的 Source identity；Library entry 不可直接映射成 Game-local truth without explicit compiler/domain commit。

## AC-11｜Bundle

embedded / catalog-reference snapshots 可校验；path / snapshot mismatch fail closed；Bundle round-trip 不丢 identity / hash。

## AC-12｜Game Asset Manifest

创建时 resolved manifest pins exact snapshot；Source 更新后旧 Manifest 仍解析到旧 version/hash。

## AC-13｜No Scope Drift

不实现 G9-04 full Markdown adapter，不实现 G9-05 Creator，不实现 Library Runtime retrieval。

## AC-14｜Regression

G5–G9 existing tests、typecheck、lint、build、disclosure、launcher 不回滚。

---

# 18. Non-scope / Deferred

明确不属于 G9-03 v1：

- arbitrary plugin runtime；
- user-uploaded executable modules；
- asset script engine；
- generic condition/expression DSL；
- arbitrary query DSL；
- vector DB；
- embedding/chunk/ranking contract；
- Library Runtime retrieval implementation；
- Creator chat/tool transport；
- autonomous multi-asset agent；
- full Objective Engine；
- external marketplace / signing infrastructure。

---

# 19. Decision Propagation

G9-03 semantics 现已冻结：

```text
G9-02                         PASS / CLOSED
↓
G9-03 semantic protocol      FROZEN / PASS
↓
G9-03 implementation         NEXT
↓
GPT exact-SHA Independent Review
↓
G9-03 PASS / CLOSED
↓
G9-04 Adapter / Compiler / Binding
```

最新 `sillytavern-assets` Politics / Kinship / Large Relation Graph 结论属于 P2 protocol constraint，已吸收到本规格，不新增额外 Stage。

G9-03 implementation 必须复用 `sillytavern main@ab09c7...` 的 G9-02 Runtime rails；如果实现发现必须修改 G9-02 identity / routing / Program authority / Save-Recovery authority 才能完成，应停止并返回 GPT 做架构裁定。

---

# 20. Final Freeze

> **G9-03 v1 冻结的不是“把 Markdown 换成 JSON”，而是建立一个能长期承载 World / Character / Expansion / Library 的稳定 Source Snapshot 协议，并让所有具体资产最终只通过同一 `Source Snapshot → Resolved Game Manifest → G9-02 Game-local Binding → Runtime` 链进入游戏。**
