---
title: G9-05H｜先 Owner First Playable 后 Library 阶段重排裁定
status: current-decision-frozen
version: 1.0
updated: 2026-08-22
---

# G9-05H｜先 Owner First Playable 后 Library 阶段重排裁定 v1.0

## 0. Decision

Project Owner 明确要求：在继续资料库（Library Product）之前，先亲手使用已经完成的真实资产链创建一局游戏并实际游玩。

因此从本裁定起：

```text
Primary Asset End-to-End Closure Gate
= PASS / CLOSED

↓

G9-05H Owner First Playable
= AUTHORIZED / NEXT

↓ Owner UAT 后再决策

Library Product Increment
= DEFERRED / LATER EXTENSION
```

Library 不再是当前阻塞 Gate，也不因 G9-03 已存在 `assetType=library` / `manifest.libraries[]` 就获得当前开发优先级。

G10 Provider Expansion 与 Release 仍未授权。

---

## 1. Why

当前已经具备：

```text
World Creator
Character Creator
Expansion Creator
Use My Assets
真实 EP-CHAR Runtime binding
真实三主资产 E2E
Formal Turn
Save / Restore
Crash / Resume / Recovery
```

在继续增加第四类产品纵向之前，必须由 Project Owner 从真实玩家视角验证：

```text
能否顺利启动
→ 能否看到自己的真实资产
→ 能否自然完成资产建局
→ 能否进入真实 AI Session
→ 连续回合是否像游戏
→ Save / Continue / Restore 是否可理解
→ UI / 文案 / 错误反馈是否存在产品级阻塞
```

Owner UAT 发现的核心体验问题优先于 Library、更多资产类型或外围扩展。

---

## 2. Library Position

Library 的架构方向保留，但降为后续扩展：

```text
Library = versioned Source Reference Asset
Library Source != Game Truth
Library Binding != Runtime materialization
Library Access Eligibility != Current Knowledge
Library Retrieval != Runtime Authority
```

当前不实现：

- Library Product UI；
- Library Creator；
- Runtime Library Retrieval Gateway；
- Player Encyclopedia；
- Vector / RAG；
- `manifest.libraries[]` 产品选择纵向。

这些均不得成为 Owner First Playable 的前置条件。

---

## 3. G9-05H Scope Principle

G9-05H 不是新的玩法阶段，也不是 Release 工程。

只允许做：

```text
Owner UAT enablement
真实资产本地安装 / seed
首次试玩 preflight
最少必要启动包装
UAT checklist / feedback capture
```

不得借机：

```text
重写 Runtime
重做 Game Creation
重做 Creator
引入 Library
扩展完整 EP-CHAR 成长系统
新增 Provider 能力
做 Release installer
做大规模 UI redesign
```

---

## 4. Owner UAT Authority

自动测试可以证明工程 Gate，但不能代替本阶段的产品结论：

```text
Engineering PASS
!= Owner UAT PASS
```

G9-05H 最终结果必须由 Project Owner 实际游玩后给出：

```text
PASS
或
BLOCKED / NEEDS PRODUCT FIXES
```

若 Owner 发现阻塞问题：

1. 先按 UAT 问题修复；
2. 同一根因遵守 correction budget；
3. 在核心试玩体验关闭前，不恢复 Library Product 优先级。

若 Owner UAT PASS：下一阶段重新做产品优先级决策，Library 仍不自动成为 NEXT。
