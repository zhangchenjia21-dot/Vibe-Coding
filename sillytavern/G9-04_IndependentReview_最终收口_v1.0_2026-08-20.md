---
title: G9-04 Adapter / Compiler / Binding Independent Review 最终收口
status: current-final-review-pass
version: 1.0
date: 2026-08-20
stage: G9-04
---

# G9-04｜Adapter / Compiler / Binding｜Independent Review 最终收口 v1.0

## 1. 审核对象

```text
Formal Code Base
5da2294a9d21585665167e69307d9c693427582d

Initial reviewed implementation
892867e72f44cb97557b944f7149d650d89a0abe

Correction Packet
97c103d7e896efb5c03469a7fbced820f87d145d

Final Tested / Reviewed Implementation
c492ac4a0eb33ec055f582a2a023066853e2c323

Integrated main
c492ac4a0eb33ec055f582a2a023066853e2c323

Real asset baseline
968175e6c3fb3545b7c2907b65089c7e1dbb40a0
```

## 2. 最终结论

```text
P0 = 0
P1 = 0

G9-04 implementation = PASS
G9-04 = PASS / CLOSED
G9-05 = AUTHORIZED / NEXT
```

G9-04 已把真实 Markdown 资产接入 G9-03 统一资产协议和 G9-02 本局绑定轨道，同时保持三层事实边界：

```text
Source Asset
!= Game-local Canonical Instance
!= Runtime State
```

## 3. 三个返修项最终关闭

### P1-01｜资料库不得进入本局主资产绑定｜PASS

最终实现只为 World / Character / Expansion 生成 `SourceAssetDescriptor` 与隐藏绑定锚点。Library 继续接受精确 Manifest / catalog 校验，但不进入 `runtime.asset-binding`、不进入 `sourceLineage`、不成为本局正式事实。

### P1-02｜资料库引用完整性｜PASS

`relatedRefs` 只允许精确解析到 catalog `assetRef` 或同一 Library 内精确 `entryRef`。不存在的引用和近似标题/别名均以 `LEGACY_REFERENCE_UNRESOLVED` 失败关闭；不使用文件名、标题或模糊匹配猜身份。

### P1-03｜主资产重复绑定｜PASS

Character / Expansion 的精确重复，以及同一逻辑身份不同版本同时进入主绑定，均以 `BINDING_DUPLICATE_PRIMARY` 失败关闭。程序不再静默去重或替用户选择版本。

## 4. 真实资产纵向证明

资产仓库：

```text
zhangchenjia21-dot/sillytavern-assets main
968175e6c3fb3545b7c2907b65089c7e1dbb40a0
```

样本：

- World：`世界包/汉末三国_天下未定_World_Pack_v0.2.3.md`，blob `0c27b7f6d252d8970191784eb930ca722f77d01e`；
- Character：`人物卡/汉末三国/CC-BATCH-01/刘备__Character_Card__v0.1.2.md`，blob `aa6fc6b1633f9cdaa4d0effd62986167369a3dd2`；
- Expansion：`拓展包/通用拓展包/人物能力与技艺_Expansion_Pack_v0.1.5.md`，blob `b165ddbd927eacc012f2edf5f8d81ad73d6e64a2`；
- Secondary：`世界包/埃瑟维亚_诸界余辉_World_Pack_v0.1.3.md`，blob `51193254cb924e358b60ad4f83f585ce1f02e148`。

完整链已证明：

```text
真实 Markdown
→ 显式 Legacy Adapter Profile
→ TavernAssetV1
→ G9-03 validator + canonical SHA-256
→ exact TavernGameAssetManifestV1
→ binding compiler
→ SourceAssetDescriptor
→ hidden Game-local binding anchors
→ SQLiteRuntimeStore.bootstrap()
→ exact sourceLineage
→ Save / Restore
```

刘备角色卡绑定不会自动物化 Runtime Character、People 或 Scene membership；No Phantom 保持成立。

Expansion Host 证明明确为 proof-only，只验证 `runtimeModuleRef` 到既有 Program Host 的接线，不冒充 EP-CHAR-CORE 玩法机制已经实现。

## 5. 最终测试证据

```text
g9:04:test                 17 / 17 PASS
g9:04:real                 PASS
g9:03:test                 36 / 36 PASS
G9-02 closure / A / BC / B / C core / breadth
                           PASS
G5                         207 / 207 PASS
G6                          17 / 17 PASS
G7                          20 / 20 PASS
G8                         208 / 208 PASS
full suite                 88 files / 806 tests PASS
typecheck                  PASS
lint                       PASS
product build              PASS
launcher smoke             PASS
disclosure                 PASS
git diff --check           PASS
Provider calls             0
```

G9-03 wire、G9-02 / G8 production contracts 均未修改。

## 6. 阶段出口

G9-04 已正式关闭。下一阶段 G9-05 负责三类主资产的 Creator 基础闭环，继续遵守：

```text
AI 对话 != Creator Draft
Creator Draft != Saved Source Asset
AI 可以编辑 Draft != AI 可以发布资产
```

默认代码执行 Agent = Codex；除非项目所有者明确指定 Grok。
