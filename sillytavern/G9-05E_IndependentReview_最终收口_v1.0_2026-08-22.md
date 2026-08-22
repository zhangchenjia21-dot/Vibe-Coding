---
title: G9-05E Independent Review 最终收口
status: final-pass-closed
date: 2026-08-22
stage: G9-05E
reviewed_implementation: f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
integrated_main: f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
p0: 0
p1: 0
next_stage: G9-05F Expansion Creator Vertical
---

# G9-05E｜【使用我的资产库】创建游戏｜Independent Review 最终收口

## 1. Final Verdict

```text
P0 = 0
P1 = 0

G9-05E Use My Assets Game Creation = PASS / CLOSED
G9-05F Expansion Creator Vertical  = AUTHORIZED / NEXT
```

最终审核 / 集成实现：

```text
f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
```

Formal Code Base：

```text
dd67bd9c02f42717dab139e9c87b4fe7e25f0fc6
```

最终实现是 Formal Code Base 的纯向前后代：

```text
ahead 6
behind 0
merge base = dd67bd9c02f42717dab139e9c87b4fe7e25f0fc6
```

`main` 在 Gate 前保持 Formal Code Base 不变；PASS 后由 GPT 使用 `force=false` 精确 fast-forward 到 reviewed SHA，没有 merge / squash / rebase / 新 integration SHA。

最终验证：

```text
sillytavern/main == f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
```

## 2. Review History

### Initial implementation

```text
d0071141b54877853a2b0abadb110b7dcbc29d45
```

首轮判定：

```text
P0 = 0
P1 = 2
```

发现：

1. Final Create 成功但响应丢失后，原始 `basisRevision` 无法安全 replay 已创建的同一 game；
2. 切换 Source-backed player 模式时 UI 自动选择 `eligible[0]`，违反 explicit exact selection。

### correction-01

Reviewed implementation：

```text
abab72d184a65d9b26e8ffdc7ef363f5ad141a9f
```

原两个 P1 均关闭：

- Final Create 引入 Program-derived `createFingerprint`；
- `creationRef + exact createFingerprint` 在 `creating/created` 时可用原 pre-response revision replay 同一 game；
- mismatched fingerprint fail closed；
- Source-player mode 与 exact Character selection 分离，只有用户点击具体 exact snapshot 才写入 `player_character`。

最终回归扫描又发现两个多版本 exact-selection 产品问题，因此整体未关闭：

1. Expansion 同 `assetRef` v1/v2 按 stable ref 投影选中，可能让 sibling 版本继承选中/enablement；
2. Other Character 同 `assetRef` v1/v2 按 stable ref 投影 role，未选 sibling 可能误删已选版本。

### correction-02

Final implementation：

```text
f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
```

只修改：

- `玩家产品界面/界面/资产建局向导.tsx`
- `玩家产品界面/测试/资产建局产品纵向测试.test.tsx`

没有重新修改 Asset Game Creation Core、Manifest、G9-04 Binding 或 Runtime。

关闭：

- World / Player / Expansion / Other Character 的产品投影统一按 exact snapshot：

```text
assetRef
+ assetType
+ version
+ contentHash
```

- Expansion v1 选中时 v2 不显示为选中；v2 feature/module 控件只携带 v2 snapshot；
- unselected Expansion sibling 不会删除另一 exact version；
- Other Character v1/v2 只有 exact selected card 显示 role；
- unselected sibling 的“不加入”不可误删另一 exact version；
- Compatibility Review 显示 Character exact version + role；
- Compatibility Review 显示 Expansion exact version + enabled feature/module refs。

## 3. Final Architecture Proof

### 3.1 Selectable truth

```text
Creator Draft
!= selectable game asset

Published Source Asset exact snapshot
= selectable game asset
```

建局不会自动发布 Draft，也不会创建临时 Source。

### 3.2 Exact selection

World / Character / Expansion 均以 exact snapshot 为本局选择事实：

```text
assetRef
+ assetType
+ version
+ contentHash
```

不会按 stable ref 自动挑 latest，也不会因为同 identity sibling version 存在而继承 selection。

### 3.3 Dependency closure

hard dependency 必须由当前 selected-set 中 exact target 满足。

```text
asset exists in Source Library
!= hard dependency selected
```

optional gap 不阻断；首轮不支持的 hard Library dependency fail closed。

### 3.4 Character authority / No Phantom

Character role：

```text
bound_only
opening_character
player_character
```

只有 Final Create 中显式 `opening_character` / `player_character` 才 materialize。

`bound_only` 只进入 Manifest / Binding / lineage，不凭空获得位置或 live state。

`playerCharacterSupported=true` 只代表 Source capability；进入 Source-player chooser 不等于选择角色。只有用户点击具体 exact Character/version 才形成 `player_character`。

0 Character Source 合法；玩家可以显式提供本局 local player identity，不反向生成 Character Source。

### 3.5 Source → Game-local materialization

G9-05E 没有把 G9-04 hidden Source binding 冒充 Runtime semantic materialization。

```text
exact selected Source
→ deterministic Asset Game materialization
→ base RuntimeDefinition
→ existing compileAssetBindingPlan()
→ Runtime bootstrap
```

World 首轮只生成 Program-owned 最小 opening region/place/scene scaffold，不从自由 prose 猜多层 topology/relations/items。

### 3.6 Binding reuse

复用既有：

```text
compileAssetBindingPlan()
```

没有复制第二套 G9-04 binding / lineage 机制。

### 3.7 Public / private boundary

public Source 语义进入 player-safe setup fields；private Source 语义只进入 private seed context。Player-safe library/session 响应不暴露 private Source body。

### 3.8 Exactly-once Final Create

Final Create 的 durable identity 由 Program 对 authoritative exact intent 生成 `createFingerprint`。

```text
same creationRef
+ same exact createFingerprint
→ same game replay
```

即使成功响应丢失、浏览器仍持有原 `basisRevision`，也能返回同一 game；不同 fingerprint fail closed，不产生第二局。

### 3.9 Runtime lifecycle

已保留：

```text
Create
→ Session
→ formal turn
→ Save
→ Continue
→ Restore
→ continue
```

Source lineage 保持 exact version/hash；Source 后续发布新版本不会静默改变旧游戏；Runtime 不回写 Source。

## 4. Product Closure

【创建新游戏】现已真实支持：

```text
创建新游戏
├── 使用我的资产库   AVAILABLE
└── AI 辅助创建       保持既有行为
```

资产建局向导：

```text
选择世界包 exact version
→ 拓展包（可为空）
→ 玩家角色（local 或 exact Source）
→ 其他角色（不加入 / bound_only / opening_character）
→ 开场参数
→ 兼容性验证
→ Final Create
→ My Games / Session
```

Compatibility Review 必须让用户看见最终 exact selection，而不是只显示 stable ref。

## 5. Test / CI Evidence Boundary

仓库内已提交 focused backend / HTTP / UI regression tests，包括：

- exact World selection；
- zero-character local player；
- player eligibility；
- bound_only No-Phantom；
- hard dependency selected-set closure；
- exact Manifest / G9-04 binding reuse；
- public/private boundary；
- response-loss original-request replay；
- multi-version player explicit selection；
- multi-version Expansion exact selection / enablement；
- multi-version Other Character exact role / no cross-version remove；
- Compatibility Review exact version / role / feature / module；
- Create → turn → Save → Restore → continue exact lineage。

Independent Review 对 exact diff、frozen spec、committed tests 和 ancestry 逐项交叉审核后判定 P0/P1=0。

GitHub 在最终 SHA：

```text
f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
```

没有返回 external combined CI status，也没有 workflow run。因此本 Review **不声称 external CI green**；该事实限制必须保留。

## 6. Integration Proof

Gate 前：

```text
main = dd67bd9c02f42717dab139e9c87b4fe7e25f0fc6
```

Final reviewed SHA：

```text
f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
```

PASS 后：

```text
update main → f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
force = false
```

Post-write compare：

```text
main identical to reviewed SHA
ahead = 0
behind = 0
```

## 7. Next Authorization

G9-05E 已完成我们在 Expansion Creator 前要求的纵向证明：

```text
Creator published Source
→ exact My Assets selection
→ Manifest
→ explicit Game-local materialization
→ existing Binding
→ Runtime
→ Session / Save / Restore
```

因此：

```text
G9-05F Expansion Creator Vertical = AUTHORIZED / NEXT
```

但本 Review 不包含 G9-05F 产品/合同设计，也不授权 Agent 在没有新的 canonical spec + Task Packet 时自行实现。

Expansion Creator 完成后仍需继续三类主资产组合完整建局 / 游玩闭环，再进入 Primary Asset End-to-End Closure Gate。
