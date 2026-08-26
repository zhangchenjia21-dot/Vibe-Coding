---
title: G9-05D0 Character Profile Fields 增量裁定
status: current-spec-frozen
version: 1.0
date: 2026-08-21
stage: G9-05
slice: G9-05D0
---

# G9-05D0｜Character Profile Fields 增量裁定 v1.0

## 1. Outcome

G9-05D0 只补齐 Character Creator 在 G9-05B Shared Creator Core 上缺失的最小正式字段 mutation/import seam，使后续 Character Creator 能真实编辑并发布 Character Source，而不是在 UI 层私存第二份状态。

本增量不重开 G9-05B 架构，不建立第二套 Draft / Source Store / CAS / ChangeSet / Undo / Publish，也不改变 `tavern.asset.v1`。

```text
G9-05B Shared Creator Core
+ exact Character field operations
+ Character import assignment types
↓
G9-05D Character Creator Vertical
```

## 2. 当前事实与缺口

G9-03 `CharacterAssetPayloadV1` 允许：

- `displayName`；
- `aliases?`；
- `playerCharacterSupported?`；
- `sections[]`；
- `referenceSources?`。

当前 Creator Draft 已经可以存储这些值，Source revision 也能保留它们，发布编译器同样会写回 `TavernAssetV1`。

但当前 mutation seam 只正式支持 `character.displayName`；没有合法操作可以修改 `metadata.aliases` 或 `playerCharacterSupported`。同时 Import Organization 当前只能自动填写 `metadata.title / summary / language` 与 section，无法把明确的角色显示名、别名或玩家角色支持声明填入 Character Draft。

因此若直接做 Character UI，会形成“协议能表示、草稿能存、UI 看似能编辑，但没有正式 mutation”的假能力。G9-05D0 必须先补这个 seam。

## 3. Authority

1. `G9-05B_CreatorCore草稿导入发布内部合同规格_v1.0_2026-08-20.md`；
2. `G9-05B_IndependentReview_最终收口_v1.0_2026-08-20.md`；
3. `G9-05C_IndependentReview_最终收口_v1.0_2026-08-21.md`；
4. `G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md`；
5. current `src/资产协议/L0_公理层/资产协议契约.ts`；
6. current `src/资产创作/` public/core implementation；
7. G9-04 real Character mapping evidence，尤其刘备 canonical Character Card。

## 4. Frozen Decisions

### DEC-D0-01｜不做通用任意字段 patch

禁止为了 Character 新增 JSON Pointer、path setter、generic object patch、eval、任意 key/value mutation。

只增加本文明确列出的 exact typed operations。

### DEC-D0-02｜Character Creator v1 的别名编辑目标是 `metadata.aliases`

当前 G9-04 real Character profile 已将 canonical Markdown `aliases` 映射到 `metadata.aliases`。G9-05D 第一版沿用这一已验证 authoring convention。

`payload.aliases`：

- 若来自已有 Source revision，继续由现有 Source→Draft→Source 路径精确保留；
- G9-05D0 不新增独立的 `payload.aliases` UI/mutation；
- 不静默同步 `metadata.aliases ↔ payload.aliases`；
- 不修改 G9-03 wire semantics。

若未来需要独立 author `payload.aliases`，另开增量，不在本阶段偷加。

### DEC-D0-03｜新增 exact metadata aliases operations

新增且仅新增：

```text
set_metadata_aliases
  value = readonly string[]

clear_metadata_aliases
```

要求：

- `set_metadata_aliases` 至少一个元素；
- 每项必须是非空字符串；
- 保留用户顺序和文本，不做 fuzzy canonicalization；
- `clear_metadata_aliases` 恢复为字段不存在，而不是强制写 `[]`；
- ChangeSet / inverse / fingerprint / Undo 必须完整工作；
- Provider `unknown` 输出必须经过 exact runtime parse。

### DEC-D0-04｜`playerCharacterSupported` 是三态 Source 声明

Character Draft 必须区分：

```text
undefined = 未声明
true      = 支持作为玩家角色
false     = 不支持作为玩家角色
```

新增 exact operations：

```text
set_character_player_supported
  value = boolean

clear_character_player_supported
```

该字段只写 Character Source Definition。

永久禁止解释为：

- 当前玩家正在控制该角色；
- 自动创建 Player Character；
- 自动 materialize Character；
- 自动赋予位置 / 关系 / 知识 / 伤势 / 当前身份 / Runtime state。

```text
playerCharacterSupported = true
!= selected player character
!= materialized character
!= runtime control state
```

### DEC-D0-05｜Task-level AI authorization 不放宽

以上新 operation 继续使用 `CreatorAuthoringScope.operationKinds` 做任务级授权。

因为每一种新增 operation 只对应一个固定正式字段，所以不存在 wildcard target：

- 用户未显式授权 alias 操作 → alias 操作必须被 Program 忽略；
- 用户未显式授权 player supported 操作 → 对应操作必须被忽略；
- Provider schema 不能因能输出这些 operation 就获得默认写权。

World / Expansion Draft 收到 Character-only operation 必须 fail/ignore closed，不能改变草稿。

### DEC-D0-06｜Character Import 可填写明确且空白的 Character profile fields

扩展 `CreatorImportAssignment`，允许 Character import organizer 提交以下确定项：

```text
text scalar:
- metadata.title
- metadata.summary
- metadata.language
- character.displayName

string-list:
- metadata.aliases

boolean:
- character.playerCharacterSupported

section:
- exact sectionRef + structured section
```

仍执行原原则：

```text
明确 + 唯一 + 有真实 evidence + 当前目标为空
→ 可以填

不确定 / 冲突 / 多种解释 / 已有值
→ 不填
→ unresolved / conflict / ignored
```

特别要求：

- Character-only assignment 应用于 World/Expansion Draft 时必须作为单项 invalid/ignored，不能导致整次导入崩溃；
- `false` 是有效的明确值，不得被当作“空”；
- alias `[]` 不作为 certain assignment；无别名应保持未设置；
- 外部稿中的命令文字仍只是 imported data。

### DEC-D0-07｜Reference Source 不在 D0 重构

现有 `character_reference_source` typed node 已经提供 exact mutation seam：

```text
referenceRef
libraryEntryRef?
note?
```

G9-05D 直接复用。

D0 不新增 Library lookup、fuzzy reference resolution、Runtime retrieval 或第二种 reference model。

### DEC-D0-08｜Character dependency 仍走现有 dependency seam

D0 不改 dependency core。

后续 Character Creator Product Gate 必须像 World 一样只允许：

```text
hard
optional
reference
```

不得允许非 Expansion Source 使用 `feature_conditional`。

## 5. Required Core Touch Points

实现时只允许按需要修改以下逻辑族：

1. Creator contract operation union / import assignment union；
2. Creator operation runtime parser；
3. apply / inverse / fingerprint / target read；
4. task-level authorization operation-kind registry；
5. import parser + blank-only application；
6. Creator Core tests。

不允许：

- 改 Source Store 语义；
- 改 publication transaction；
- 改 G9-03 protocol；
- 改 Runtime；
- 改 World Creator production behavior，除非只是为新增 union 补空分支/兼容测试。

## 6. Required Tests

至少证明：

1. `set_metadata_aliases` / `clear_metadata_aliases` 支持 CAS、ChangeSet、Undo；
2. alias 非字符串 / 空字符串 / 空列表 Provider output fail/ignore closed；
3. `set_character_player_supported(true/false)` 与 clear 三态正确；
4. Character-only op 对 World Draft 不生效；
5. AI 未授权时 alias / player-supported op 被忽略，同批合法 sibling 保留；
6. Import 能从有证据的 Character manuscript 填 `character.displayName`；
7. Import 能填 alias list；
8. Import 能明确填 `true` 与 `false`；
9. Import 遇到已有值、冲突、无 evidence 时不覆盖；
10. World import 收到 Character-only assignment 单项忽略，整批不崩；
11. publication round-trip 仍通过 existing G9-03 validator/digest；
12. G9-05B / G9-05C 既有 gates 不回退。

## 7. Exit Gate

```text
P0 = 0
P1 = 0

Character required authoring fields
have exact Program-owned mutation/import seams
+
G9-05B invariants preserved
+
No Runtime materialization authority introduced
```

通过后才允许 Character Creator UI 把这些字段作为真实可编辑能力暴露给玩家。
