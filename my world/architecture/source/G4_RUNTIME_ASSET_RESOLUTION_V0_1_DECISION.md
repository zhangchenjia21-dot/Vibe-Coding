---
title: my world｜G4 Runtime Asset Resolution v0.1 Decision
status: current-canonical-architecture-decision
created: 2026-09-02
updated: 2026-09-02
phase: G4 Primary Source Assets & Local Game Creation
parent_task: G4-10 Runtime Asset Resolution
semantic_owner: GPT
---

# G4 Runtime Asset Resolution v0.1｜Canonical Decision

## 0. Decision

G4-10 建立一个**最小、exact-generation-aware 的 authored visual asset resolution seam**，让已经属于 immutable Primary Source generation 的 portrait / scene / authored map 能在真实 Windows Godot Runtime 中被安全解析和加载。

它解决的是：

```text
exact immutable Source generation
+ declared authored visual
→ safe runtime resolution
→ real Godot image load
```

它**不**建立地图玩法系统、旅行系统、GIS、procedural map、通用媒体平台或 G6 完整 RPG UI。

---

## 1. Primary purpose

G4 的目标是让 Primary Source 不只是文本 identity，而是能够可靠携带并复现其 authored visual identity。

G4-10 的核心价值：

> **旧 Game 必须继续看到它当初 pin 的 exact Source visual，而不是随着 Source current 更新、文件路径变化或 fallback 猜测而悄悄换图。**

因此：

```text
stable Source identity
!= exact immutable generation
!= authored visual declaration
!= runtime visual projection
```

视觉资源是 Source-authored presentation/reference，不自动成为 gameplay truth。

---

## 2. Canonical ownership

### 2.1 Source owns authored bytes

World / Character Source contract 已允许：

- World `authored_assets[]`；
- optional Character `portrait`；
- declared asset bytes 进入 exact generation fingerprint。

这些 bytes 继续由 **Managed Source Library immutable generation** 拥有。

G4-10 **不把视觉 bytes 复制成第二份 Game-owned canonical asset store**，也不把图片塞进 SQLite。

### 2.2 Game owns exact ancestry, not Source current

已有 Game 的 visual resolution 必须从该 Game 已 pin 的 exact Source generation provenance 出发。

禁止：

```text
old Game
→ stable asset_id
→ Source current generation
```

正确：

```text
old Game
→ pinned exact Source generation
→ declared visual in that exact generation
```

Source 更新后，新 Game 可以 pin 新 generation；旧 Game 不得静默换图。

### 2.3 UI owns presentation only

UI / Godot Texture / cached Image 只是 projection。

```text
Texture / Image / placeholder / cache
!= Source truth
!= Game truth
```

缓存丢失可重建，不得反向成为 authored identity owner。

---

## 3. G4-10 v0.1 consumer set

本阶段只消费已经存在的 Source contract，不扩展 universal asset schema。

### Character

若 Character manifest 存在：

```json
{
  "portrait": {
    "path": "portrait.webp",
    "alt_text": "..."
  }
}
```

则 G4-10 可以解析该 exact-generation portrait。

若 `portrait` 字段 canonical absent，则是合法 absence，不是损坏。

### World

World `authored_assets[]` 中本阶段消费：

```text
kind = portrait | scene | map
```

每项继续使用 Source contract 已冻结的 package-local `asset_id / kind / path`。

`document` 即使属于 Source generation / fingerprint，也不在 G4-10 的 image runtime consumer scope 内；不要为了 G4-10 建通用 document viewer。

### Expansion

除非 current Expansion contract 已经显式声明 authored visual seam，否则 G4-10 不替 Expansion 猜测或新造一套视觉 schema。

---

## 4. Exact resolution identity

Runtime resolution 至少必须语义绑定：

```text
source asset identity/type
+ exact generation fingerprint / exact generation record
+ declared visual identity
```

其中 declared visual identity：

- World authored asset：manifest 中的 `authored_assets[].asset_id`；
- Character portrait：canonical role `character_portrait` + exact Character generation（不要求为了统一而修改 Character Source schema 新增假 asset_id）。

Resolver 不允许仅靠文件名、display name、stable Source asset_id 或 current-generation lookup 猜测 visual。

---

## 5. Safe path boundary

所有 authored visual path 都是 package-local relative path，并继续服从 Source 已有安全边界：

- 禁止 absolute path；
- 禁止 drive path；
- 禁止 URI；
- 禁止 `.` / `..` traversal；
- 禁止 symlink / canonical-path escape；
- 只能解析 exact managed Source generation package 内正式声明的文件。

Consumer 不应自行字符串拼接 Managed Source Library 路径。应通过一个受控 resolver seam 获取 resolution result。

G4-10 不建立任意本地文件浏览权限。

---

## 6. Resolution states / fallback semantics

必须区分三种语义：

```text
RESOLVED
ABSENT
UNAVAILABLE
```

### RESOLVED

只有当：

- exact generation 可验证；
- declaration 属于该 exact generation；
- path 安全且文件存在；
- bytes 未被 tamper；
- Godot 能真实解码/load 为本阶段支持的视觉资源；

才算 resolved。

### ABSENT

Source canonical 没有声明 optional visual，例如 Character 没有 `portrait` 字段。

这是正常状态，不应报 Source corruption。

### UNAVAILABLE

例如：

- pinned exact generation 不可取得；
- generation integrity/tamper check 失败；
- declared file missing；
- unsafe path；
- visual bytes 无法由真实 Godot image loader 解码；
- declared visual identity 不存在于该 exact generation。

UNAVAILABLE 必须带 bounded safe reason code / diagnostics，但不得加载未经验证的替代 bytes。

---

## 7. Missing fallback is presentation fail-soft, never identity fallback

视觉资源不是继续 Narrative/World truth 所必需的 authoritative mutation，因此 visual resolution failure **不得把整局游戏变成不可玩的 blocking gate**。

允许：

```text
ABSENT / UNAVAILABLE
→ UI 使用 application-owned neutral placeholder
或省略该视觉 surface
→ gameplay continues
```

但 placeholder：

- 不写入 Source manifest；
- 不写成 Game-authored visual truth；
- 不进入 Source fingerprint；
- 不伪装成 historical/authored visual；
- 不允许改变 exact Source ancestry。

严格禁止这些“方便的 fallback”：</n
```text
missing old-generation portrait
→ use Source current portrait

missing authored map
→ use another map with same filename

missing scene
→ scan neighboring folders / another Source package

broken image
→ network-download replacement
```

> **Fallback may preserve presentation; fallback must never rewrite identity.**

---

## 8. Godot runtime boundary

G4-10 必须证明真实 Godot 4.7.2 Runtime 能从 managed exact-generation Source bytes 加载视觉资源。

Mechanism 可以选择安全的 bytes→Image/Texture 或 managed-path→Image load 方法，但不得要求 Source visual 先变成 `res://` imported Resource 才可工作。

这是本地可安装 Source Library；Windows exported application 必须能够加载安装在 managed Source generation 中的 authored visual bytes。

L3/public seam 的 consumer 不应获得通用 filesystem authority。返回形态可以由 Mechanism 设计，但至少应投影：

- success / status；
- exact source generation provenance；
- declared visual identity/kind；
- safe non-secret diagnostics；
- 可供 Godot consumer 使用的 resolved visual result。

---

## 9. Authored map boundary

G4-10 的 `kind = map` 只表示 authored map **image/reference presentation**。

明确不是：

- map topology graph；
- current location truth；
- travel/pathfinding；
- distance rules；
- fog of war；
- interactable POI；
- GIS coordinate system；
- procedural generation；
- world simulation spatial owner。

这些属于后续真实 Runtime / G5 / G6 consumer 拉出的能力。

同理，`scene` visual 不等于 Godot SceneTree gameplay state。

---

## 10. Source integrity / Game playability

Existing Source rules仍要求 declared bytes 纳入 generation fingerprint，且 exact generation missing/tamper 不得被静默接受。

G4-10 的新增裁定是：

> **不要加载被破坏的 visual bytes，但也不要因为纯 presentation 缺失而阻止 Narrative Game 继续。**

因此 resolver 可以 fail-loud in diagnostics、fail-soft in presentation。

如果未来某个视觉资源本身成为 authoritative mechanic input，必须由那个真实 consumer 另行冻结更强 Gate；G4-10 不提前假设。

---

## 11. Validation reality gate

G4-10 mechanism 至少必须证明：

1. World authored `scene` / `map` 中至少一种、以及 Character portrait，能够从 task-owned Managed Source Library exact generation 真实 resolve；
2. real Godot image load 成功，不是只检查文件存在；
3. unsafe traversal / external path 被拒绝；
4. optional portrait absence → ABSENT，不伪造 Source placeholder；
5. missing/tampered/un-decodable visual → UNAVAILABLE + neutral presentation fallback eligibility，而不是 current-generation/neighbor fallback；
6. Source current 更新后，旧 Game/exact provenance 仍 resolve 旧 generation visual；
7. Windows exported application / canonical export validation 能通过真实 load seam；
8. no Source/Game/SQLite writeback is introduced solely to display a visual。

G4-10 是 Engineering Reality Gate，不要求 Owner 再做一次独立产品 UAT。G4-11 才使用两组真实 Primary Source 做跨 family 的产品现实测试。

---

## 12. Non-scope

G4-10 明确不做：

- image generation / editing pipeline；
- asset authoring UI；
- Reference Library；
- map gameplay engine；
- topology / travel / GIS；
- procedural map；
- scene graph authoring；
- arbitrary media/plugin protocol；
- online asset download/store；
- G6 full RPG visual redesign；
- G5 World Semantics。

---

## 13. Task route

```text
G4-10S0 Runtime Asset Resolution Semantic Freeze   PASS / CLOSED — GPT
↓
G4-10M1 Runtime Asset Resolution Mechanism         next — CODEX
↓
GPT Independent Review
↓
G4-11 Two Primary Asset Families Reality Test
```

G4-GATE 仍在 G4-11 之后。不得提前启动 G5。
