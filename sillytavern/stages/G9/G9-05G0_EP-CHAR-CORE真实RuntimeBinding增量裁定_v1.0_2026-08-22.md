---
title: G9-05G0｜EP-CHAR-CORE 真实 Runtime Binding 增量裁定
status: current-spec-frozen
version: 1.0
updated: 2026-08-22
---

# G9-05G0｜EP-CHAR-CORE 真实 Runtime Binding 增量裁定 v1.0

## 0. Outcome

在进入 Primary Asset End-to-End Closure 前，先关闭当前唯一会让“三类主资产真实闭环”形成假阳性的缺口：

> G9-04 已证明真实 Expansion Source 可以 exact binding 到 `RuntimeDomainModuleHost`，但 `EP-CHAR-CORE｜人物能力与技艺 v0.1.5` 当前绑定的是 proof-only Program module；它不能被当作真实 Expansion gameplay/runtime behavior 的证明。

G9-05G0 只建立一个最小、真实、可长期扩展的 production slice：

```text
真实 EP-CHAR-CORE Source
→ production RuntimeDomainModule binding
→ materialized Character 的 public capability evidence
→ Game-local Canonical Record
→ bounded Runtime context projection
```

不在本阶段实现完整六层成长、技能等级、训练成长、Dice 或 Formal Outcome。

---

## 1. Canonical semantic evidence

真实语义资产：

```text
sillytavern-assets/main
拓展包/通用拓展包/人物能力与技艺_Expansion_Pack_v0.1.5.md
通用资产库/01_维护/ContextContracts/EP-CHAR-CORE_RuntimeContextContract_v0.1.md
人物卡/汉末三国/CC-BATCH-01/刘备__Character_Card__v0.1.2.md
```

冻结语义：

1. EP-CHAR-CORE Owner 是人物长期 Capability，不拥有伤势、关系、物品、魔法、政治等其它 Domain；
2. Runtime Context Contract 明确要求 bounded capability projection，而不是整份 Character Profile 常驻模型上下文；
3. Character Card 的“能力与局限”明确是 `T0 Capability Bootstrap` 的稳定语义证据，不是正式数值或当前 Runtime State；
4. Program / Runtime 仍拥有最终能力状态、正式判定、RNG、Formal Outcome 与提交权。

因此 G0 的最小真实切片选择“Capability Evidence Projection”，而不是凭空发明技能数值。

---

## 2. G9-04 historical proof must remain historical

当前 G9-04 profile：

```text
EP-CHAR-CORE@0.1.5
→ proof feature/module
→ builtin:g9-04.ep-char-core-proof.v1
```

只作为历史 Adapter / Binding 证据。

永久禁止：

```text
修改同一 assetRef + version 的 runtimeModuleRef
却仍宣称它是原来的 exact Source snapshot
```

因为：

```text
same assetRef + same version + different payload
→ different contentHash
→ violates exact snapshot identity
```

G9-05G0 不原地改写历史 `EP-CHAR-CORE@0.1.5`。

---

## 3. Production Source revision

Primary E2E 使用的新 production-bound Source 必须来自显式 Source revision：

```text
EP-CHAR-CORE@0.1.5
→ Creator source_revision
→ remove historical proof feature/module declaration
→ add production feature/module declaration
→ targetVersion = 0.1.6
→ Host Gate
→ explicit Publish
→ EP-CHAR-CORE@0.1.6
```

Source stable identity 保持：

```text
assetRef = EP-CHAR-CORE
packageRef = package:EP-CHAR-CORE
```

G0 冻结新的 production declarations：

```text
featureRef       = feature:character-capability
source moduleRef = module:character-capability.evidence
runtimeModuleRef = builtin:character-capability.evidence.v1
ownerNamespace   = runtime.character-capability
routingMode      = model_immediate
```

`Source moduleRef != Program runtimeModuleRef` 永久保持。

不新增 config schema；v1 不需要 projectionRef DSL；Program code 本身拥有 projection 实现。

---

## 4. Character capability evidence Source revision

为了证明 Character Source 与 EP-CHAR-CORE 真正发生语义连接，E2E 使用刘备的显式 Source revision：

```text
character:han-late.liu-bei@0.1.2
→ Creator source_revision
→ add public semantic section
   sectionKind = capability-evidence
   body = 真实角色卡「3. 能力与局限」原文对应内容
→ targetVersion = 0.1.3
→ explicit Publish
```

要求：

- 不把形容词机械翻译成数值；
- 不生成 Skill level；
- 不创建 Runtime Character；
- 只是把 real manuscript 中已经明确存在的 Capability evidence 纳入新的正式 Character Source version；
- `playerCharacterSupported=true` 保持 Source capability 声明。

历史 `0.1.2` 不改写。

---

## 5. Program Runtime module

新增 production Program module：

```text
builtin:character-capability.evidence.v1
```

建议 Owner：

```text
runtime.character-capability
```

### 5.1 Descriptor

最低：

```text
moduleKind = character_capability_evidence
moduleVersion = v1
supportedFeatureRefs = [feature:character-capability]
hardDependencyModuleRefs = []
routingMode = model_immediate
```

routing capabilities/tags 只表达静态能力目录，例如：

```text
character-capability:read
character-capability
skill
training
specialty
experience
```

Router 仍只负责选择，不获得状态写权限。

### 5.2 v1 behavior boundary

v1 是 read-only capability evidence provider：

- `validateCanonicalRecord()`：验证本 Owner 的 capability evidence record；
- `projectContext()`：只投影当前请求相关的 bounded public capability evidence；
- `validateCandidate()`：默认不接受 mutation candidate；
- `validateCandidateAuthorization()`：不得因为 module selected 获得新授权；
- `buildFormalChange()`：v1 没有合法 mutation candidate，必须 fail closed；
- 不创建成长事件；
- 不写 Attribute / Skill / Specialty；
- 不替代 Program Judge / Dice / Formal Outcome。

未来成长系统必须作为本 Owner 的后续增量，不在 G0 偷渡。

---

## 6. Game-local capability evidence foundation

新增窄的 Program-owned bootstrap compiler；不要新增 generic executable plugin/bootstrap registry。

触发条件必须同时满足：

```text
EP-CHAR-CORE package selected
+
feature:character-capability enabled
+
module:character-capability.evidence enabled
+
对应 Program runtimeModuleRef 已注册
```

然后只处理已经 materialized 的 Character：

```text
player_character
opening_character
```

永久禁止：

```text
bound_only Character Source
→ capability record
```

因为 `bound_only` 没有 Runtime Character；否则就是 No-Phantom 违规。

### 6.1 Evidence source

对每个 materialized Character：

1. 根据 Runtime Character 的 exact `sourceAsset` 找到当前 Game Manifest 已选 exact Character Source；
2. 只读取 `visibility=public` 且 `sectionKind=capability-evidence` 的 semantic section；
3. 不读取 private section；
4. 没有明确 evidence section 时不猜、不生成。

### 6.2 Canonical record

建议每个 materialized Character 生成一个稳定 record：

```text
recordRef = character-capability:evidence:<runtime-character-ref>
ownerNamespace = runtime.character-capability
moduleRef = builtin:character-capability.evidence.v1
recordKind = character_capability_evidence
recordVersion = v1
visibility = public
```

payload 必须 bounded，至少含：

```text
characterRef
sourceAssetRef
sourceVersion
sourceContentHash
evidenceText
```

`evidenceText` 必须服从现有 Domain bounded payload 限制；不要因为 Source section 很长就突破 Host 上限。

对应 `GameLocalAssetMetadata` 必须与 record identity/Owner/visibility 对齐并保留 Character Source lineage。

---

## 7. Projection rules

`projectContext()` 只能从该 module 自己的 Canonical Records 读取。

优先根据：

```text
request.currentRefs
+
authorized turn safe refs
```

选择相关 Character；不得把所有人物能力证据整批加载。

输出示意：

```text
projectionKind = character_capability_evidence
purpose = <current purpose>
relevantRefs = [characterRef]
facts = {
  characterRef,
  capabilityEvidence,
  provenance
}
```

不得泄露：

- privateProfile；
- private semantic section；
- NPC 未公开能力上限；
- 未物化 Character；
- 全世界角色能力表。

---

## 8. Production Host registration

G9-05F 已冻结 `Creator / Publication / Runtime` 共用 Program capability truth。

G0 后 production Program Host 必须真实注册：

```text
builtin:character-capability.evidence.v1
```

因此：

- Creator Capability Catalog 能看到该 exact runtimeModuleRef；
- Expansion manual/import/AI 只能从 Program Catalog 使用它；
- Publish Host Gate 能验证它；
- Game Creation Runtime Host 使用同一个 module implementation。

旧测试“默认 production Host 当前没有任何 Expansion runtime module”被 G0 的新事实明确 supersede；测试应改为：

> 默认 Host 不含 fixture/proof module，但包含正式 Program built-ins。

禁止把 `tests/**` fixture registry 复制进 production。

---

## 9. Required G0 proofs

必须证明：

1. historical proof Source `EP-CHAR-CORE@0.1.5` 未被原地改写；
2. explicit Creator revision 发布 `EP-CHAR-CORE@0.1.6`；
3. production Host Catalog exact 暴露新 runtimeModuleRef；
4. Host Gate 接受 production Source，unknown ref 仍 fail closed；
5. materialized player/opening Character + public `capability-evidence` → exact Canonical Record；
6. bound_only → no capability record；
7. private capability section → no player-safe record/projection；
8. enabled module → routing directory / bounded projection 可观察；
9. disabled package/feature/module 任一 → module 不参与；
10. Source publish 不自动激活当前游戏；
11. Save/Restore 后 record/binding/source lineage 不丢；
12. Provider external calls = 0。

---

## 10. Scope lock

Allowed：

- 一个真实 production `RuntimeDomainModule`；
- 一个窄的 EP-CHAR-CORE Game-local capability evidence bootstrap compiler；
- production Program Host built-in registration；
- real Source revision / integration gate；
- focused tests。

Not allowed：

```text
NO full six-layer capability engine
NO numeric attribute/skill invention
NO XP/level system
NO new Dice system
NO generic executable plugin framework
NO runtime reads sillytavern-assets repository directly
NO Source mutation from Runtime
NO G9-03 wire/schemaVersion change
NO Library Product
NO G10 Provider work
```

---

## 11. Exit Gate

只有 G0 独立证明真实 Source Expansion 已产生 production Runtime behavior，才能进入 G9-05G Primary Asset E2E 的最终 PASS 判定。

```text
G9-05G0 PASS
!=
Primary Asset End-to-End Closure PASS
```
