---
title: G9-05G｜Primary Asset End-to-End Closure 规格
status: current-spec-frozen
version: 1.0
updated: 2026-08-22
---

# G9-05G｜Primary Asset End-to-End Closure 规格 v1.0

## 0. Product Outcome

关闭 G9 主资产链的第一条真实、完整、可重复端到端证明：

```text
真实 World manuscript
+
真实 Character manuscript
+
真实 Expansion manuscript
↓
正式 published Source exact snapshots
↓
【使用我的资产库】exact selection
↓
selected-set dependency closure
↓
TavernGameAssetManifestV1
↓
Source → Game-local deterministic materialization
↓
G9-04 binding / lineage
↓
真实 Program RuntimeDomainModule binding
↓
可观察的 Expansion Runtime behavior
↓
Playable Session / Formal Turn
↓
Save / Continue / Restore
↓
Crash / Resume / Recovery
↓
Source version isolation
```

这一步完成后，Primary Asset End-to-End Closure Gate 才可 PASS。

---

## 1. Formal Code / Asset Base

Formal Code Base：

```text
zhangchenjia21-dot/sillytavern
main@26d23d47c5f5ac42d3e1029654a64eda831c4db1
```

当前 real semantic corpus 基线：

```text
zhangchenjia21-dot/sillytavern-assets
main@5fb7886d51d7ddb2235d9cf0a52f8530dc8a827e
```

Primary E2E 选择：

```text
World:
世界包/汉末三国_天下未定_World_Pack_v0.2.3.md
Git blob: 0c27b7f6d252d8970191784eb930ca722f77d01e

Character:
人物卡/汉末三国/CC-BATCH-01/刘备__Character_Card__v0.1.2.md
Git blob: aa6fc6b1633f9cdaa4d0effd62986167369a3dd2

Expansion:
拓展包/通用拓展包/人物能力与技艺_Expansion_Pack_v0.1.5.md
Git blob: b165ddbd927eacc012f2edf5f8d81ad73d6e64a2
```

若 asset repo HEAD 后续变化，但这三个 exact blob 未变化，可继续；若任一正文 blob 改变，必须 Freshness Gate 重审，不得静默使用新内容。

G9-04 的历史 semantic proof baseline `968175e6...` 继续保留为历史证据；G9-05G 使用当前已迁移后的 canonical paths / exact blobs。

---

## 2. Real Source preparation

禁止直接手写三个 `TavernAssetV1` fixture 冒充真实资产。

### 2.1 World

World 可复用现有 G9-04 explicit Legacy Adapter Profile：

```text
real Markdown
→ LegacyMarkdownAssetAdapter
→ validated TavernAssetV1
→ SourceAssetLibraryStore
```

exact Source：

```text
world:han-late-three-kingdoms@0.2.3
```

### 2.2 Character

先保留 G9-04 base：

```text
character:han-late.liu-bei@0.1.2
```

然后通过共享 Creator Core 做 explicit Source revision：

```text
0.1.2
→ source_revision Draft
→ 从 real manuscript exact heading「3. 能力与局限」加入 public capability-evidence section
→ targetVersion 0.1.3
→ explicit Publish
```

新的 product Source：

```text
character:han-late.liu-bei@0.1.3
playerCharacterSupported = true
```

不得把形容词变成虚构数值。

### 2.3 Expansion

先保留 G9-04 historical proof Source：

```text
EP-CHAR-CORE@0.1.5
```

再通过 Expansion Creator / shared Creator Core explicit Source revision：

```text
0.1.5
→ remove proof-only feature/module
→ add G9-05G0 production feature/module
→ targetVersion 0.1.6
→ current Program Host Gate
→ explicit Publish
```

新的 product Source：

```text
EP-CHAR-CORE@0.1.6
feature:character-capability
module:character-capability.evidence
→ builtin:character-capability.evidence.v1
```

必须证明旧 `0.1.5` Source 仍可 exact read，且内容没有被 revision 原地覆盖。

---

## 3. Product game creation path

必须使用 G9-05E 已关闭的正式 Asset Game Creation Core / Product Contract，不得直接 hand-build RuntimeDefinition 绕过建局。

最低流程：

```text
inventory
→ create intent
→ select exact World 0.2.3
→ select exact Liu Bei 0.1.3 as player_character
→ select exact EP-CHAR-CORE 0.1.6
→ enable feature:character-capability
→ enable module:character-capability.evidence
→ explicit opening parameters
→ compatibility validate
→ Final Create fingerprint
→ create game
```

刘备可作为 Source-backed player 的依据只来自：

```text
playerCharacterSupported === true
```

不因为“刘备很重要”或测试方便而绕过 eligibility。

---

## 4. Manifest truth

Final Manifest 必须 exact pin：

```text
assetRef + assetType + version + contentHash
```

至少断言：

- World exact 0.2.3；
- Character exact product Source 0.1.3；
- Expansion exact product Source 0.1.6；
- enabled feature/module 与用户 selection 一致；
- 无 implicit latest；
- hard dependency 只由 selected set 满足；
- Source Library 中 sibling/newer version 存在也不改变 Manifest。

---

## 5. Runtime binding and materialization

创建后必须区分并同时证明：

```text
Source binding lineage
!=
semantic materialization
!=
Runtime Domain behavior
```

### 5.1 World

World Source 产生 Game-local world/opening location/scene 语义，并保留 exact source lineage。

### 5.2 Character

刘备因为明确 `player_character` 而 materialize 为当前 Game-local player Character；Source stable identity 不等于 Runtime characterRef。

不得把其它 bound-only Character phantom materialize。

### 5.3 Expansion

Expansion Source 通过 existing G9-04 binding compiler 产生：

```text
RuntimeDomainModuleBinding
moduleRef = builtin:character-capability.evidence.v1
```

然后 G9-05G0 bootstrap compiler 从刘备 exact Character Source 的 public `capability-evidence` 创建 Game-local Canonical Record。

禁止 Runtime 回读 external Markdown 仓库。

---

## 6. Real Expansion behavior proof

单纯：

```text
binding exists
```

不算 PASS。

必须证明 enabled EP-CHAR-CORE 对 Runtime 有可观察真实效果。

推荐 deterministic turn：玩家输入涉及“我擅长什么 / 当前行动应依靠哪类长期能力证据 / 基于自己的既有能力采取行动”等 Capability 语义。

允许使用 deterministic fixture Router / Narrative Provider 以避免外部模型，但 Router 必须从正式 routing directory 选择真实 module，不得直接调用 `projectContext()` 绕过 orchestration。

必须观察：

```text
Runtime routing directory contains production module
→ selection contains production module
→ joined context contains bounded character capability projection
→ projection provenance points to exact Game-local capability record / Character Source lineage
```

同时建立 negative control：

```text
same exact Sources
but moduleEnabled=false
→ routing directory does not expose module
→ no capability projection
```

这证明 enablement 真正影响 Runtime，而不是测试直接调用模块。

---

## 7. Formal Turn / authority

G0 v1 module是 read-only evidence provider，所以 G9-05G 不要求它自己产生 Formal Change。

但整局必须执行至少一个 existing Formal Turn，证明：

- Player action 经过正式授权；
- Domain selection / context composition 发生在正式回合链；
- Narrative Provider 只消费 player-safe context；
- Program 仍拥有最终提交；
- turn / revision 正常推进；
- Expansion projection 不获得新的行动授权；
- 无 private capability leakage。

Fixture Narrative Provider 允许；external Provider calls 必须 0。

---

## 8. Save / Continue / Restore

在至少一个正式回合后创建 Save，并证明 Restore：

```text
Game Manifest lineage unchanged
World source descriptor unchanged
Character source descriptor unchanged
Expansion source descriptor unchanged
RuntimeDomainModuleBinding unchanged
capability Canonical Record unchanged
turn/revision restored exactly
```

Restore 不重新解析 latest Source，不重新跑外部 repo，不把新 Source 版本灌入旧游戏。

---

## 9. Crash / Resume / Recovery

至少覆盖一个正式 Runtime durable execution interruption window，并证明：

- 恢复后同一 authorized turn 不重复提交；
- 不产生第二份 capability record；
- 不重复创建 game；
- domain projection / narration 的恢复行为符合已有 durable execution contract；
- game revision / turn exactly-once；
- Source Library 不被 Runtime 写入。

同时保留 Final Create response-loss replay 的 G9-05E regression。

---

## 10. Source version isolation

Game A 创建并运行后，向 Source Library append：

```text
EP-CHAR-CORE@0.1.7
或 Character sibling newer version
```

不要求该新版本成为新的 canonical semantic manuscript；它只用于 exact Source isolation regression，可由 Creator explicit revision 生成。

必须证明：

```text
Game A
still pins old exact snapshot
still uses old binding/record
Save/Restore unchanged
```

新游戏只有在玩家明确选择新 exact version 时才变化。

禁止：

```text
stable assetRef → implicit latest
```

---

## 11. Information boundary

Primary E2E 必须检查：

- private Source section 不进入 player-safe `RuntimeGameSetupField`；
- private section 不进入 capability evidence record；
- `privateProfile` 不进入 EP-CHAR projection；
- Router 只看到静态 module directory；
- projection 只读取 module-owned bounded records；
- Runtime state / Save 不回写 Source；
- Character Source ability evidence ≠ Player knowledge of all NPC hidden abilities。

---

## 12. No-Phantom Gate

必须证明：

```text
bound_only Character
→ binding only
→ no Runtime Character
→ no capability record
```

并证明：

```text
player_character Liu Bei
→ exactly one Runtime player Character
→ exactly one relevant capability record
```

不得因 Expansion enabled 而把 Source catalog 全部 Character 物化。

---

## 13. Product / HTTP proof

至少一条测试必须走正式 Product/HTTP vertical，而不是只调 L1/L2：

```text
GET inventory
→ selection mutations
→ compatibility review / validate
→ Final Create
→ session route/projected playable session
```

不要求浏览器视觉 UAT 覆盖所有步骤，但产品接口必须是真接线。

---

## 14. Real asset Gate

新增一个 Primary Asset real gate（名称由实现按 repo conventions 决定），最低要求：

```text
assets root missing → NOT RUN / explicit failure，不 silently synthetic fallback
exact paths/blob/content mismatch → fail closed
Provider external calls = 0
```

不得在真实资产不可用时自动改用 fixture 然后声称 E2E PASS。

---

## 15. Required regression surface

最低：

```text
G9-05G0 focused tests
G9-05G real E2E test
G9-05F tests/product-e2e
G9-05E tests/product-e2e
G9-05D
G9-05C
G9-05B
G9-04 real gate/tests
G9-03
G8
G7 crash/recovery relevant gates
typecheck
lint
npm test
product build
launcher smoke
g2 disclosure
git diff --check
```

未运行必须报告 `NOT RUN`，不能从代码阅读推断 PASS。

---

## 16. Scope lock

Allowed：

- G9-05G0 production EP-CHAR runtime slice；
- real asset preparation/revision gate；
- Primary Asset E2E tests / minimal product wiring gaps；
- necessary narrow regression fixes directly exposed by this E2E。

Prohibited：

```text
NO Library Product implementation
NO G10 Provider expansion
NO Release work
NO second Asset Game Creation system
NO second Binding compiler
NO new generic Runtime plugin framework
NO full EP-CHAR capability engine
NO rewriting G9-03 schemaVersion
NO changing existing game to latest Source silently
NO external Provider call in automated E2E
```

---

## 17. Acceptance Criteria

```text
AC-G01 real World/Character/Expansion manuscript evidence is exact and fail-closed
AC-G02 historical Source versions remain immutable; production revisions are new exact versions
AC-G03 production EP-CHAR RuntimeDomainModule is real and registered in production Program Host
AC-G04 Creator Host Gate publishes EP-CHAR-CORE@0.1.6 with exact production binding
AC-G05 Liu Bei@0.1.3 carries public capability-evidence without invented numeric state
AC-G06 Product Asset Game Creation creates game from exact three Source snapshots
AC-G07 Manifest exactness / dependency closure / no latest guessing
AC-G08 World/Character materialization and Source binding remain distinct
AC-G09 enabled EP-CHAR produces real bounded Runtime projection through formal routing/orchestration
AC-G10 disabled module negative control produces no projection
AC-G11 bound_only No-Phantom and player_character exactly-one capability record
AC-G12 private Source information does not leak
AC-G13 formal turn commits normally with Program final authority
AC-G14 Save / Continue / Restore preserves exact Source + domain lineage
AC-G15 Crash / Resume / Recovery remains exactly-once
AC-G16 Source newer version does not mutate old game
AC-G17 Runtime never writes Source
AC-G18 product/HTTP vertical reaches playable session
AC-G19 external Provider calls = 0
AC-G20 G9-05B/C/D/E/F + G9-04/03 + G8 regressions no P0/P1
```

---

## 18. Exit

只有 GPT 对 exact Tested Implementation SHA 独立审核 `P0=0 / P1=0` 后：

```text
G9-05G0 = PASS / CLOSED
G9-05G Primary Asset End-to-End Closure = PASS / CLOSED
```

然后才允许进入下一路线阶段。
