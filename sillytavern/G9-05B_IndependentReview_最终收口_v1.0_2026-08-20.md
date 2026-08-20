---
title: G9-05B Creator Core Independent Review 最终收口
status: current-final-review
version: 1.0
date: 2026-08-20
stage: G9-05
slice: G9-05B
next_gate: G9-05C World Creator Vertical
---

# G9-05B｜Creator Core｜Independent Review 最终收口 v1.0

## 1. 审核对象

```text
Formal Code Base
c492ac4a0eb33ec055f582a2a023066853e2c323

Initial Task Packet
63160fa9e4e0868195b468f745ae95d3ffb97e9b

Initial Reviewed Implementation
0757f4674da23bcc2588b6265cc5c3d663e3667b

correction-01 Packet
21bf495bbbb1d1dcfec7714ac8c76e059740a431

correction-01 Reviewed Implementation
f789150d584f8f2e538558c0129e0b25e5bbb73e

correction-02 Packet
eb9502b177f289bf5ee8956a454dca4a63e8cd2c

Final Tested / Reviewed / Integrated Implementation
25286b2517cb26520109e3d8738671e53d88c861
```

最终任务分支：

```text
agent/g9-05b-creator-core
= 25286b2517cb26520109e3d8738671e53d88c861
```

独立审核前 `main`：

```text
c492ac4a0eb33ec055f582a2a023066853e2c323
```

最终集成后 `main`：

```text
25286b2517cb26520109e3d8738671e53d88c861
```

`main` 采用 `force=false` 纯 fast-forward，且复核结果与最终审核 SHA 完全 identical；无 merge / squash / rebase / 新集成 SHA。

---

## 2. 最终结论

```text
P0 = 0
P1 = 0

G9-05B Semantics               PASS / FROZEN
G9-05B Creator Core Foundation PASS / CLOSED
G9-05C World Creator Vertical  AUTHORIZED / NEXT
```

G9-05B 已建立可长期复用的 Creator 共享核心，不再允许下游世界包、角色卡、拓展包分别复制草稿、AI 编辑、导入、撤销、正式资产库或发布事务。

---

## 3. 最终关闭的长期边界

### 3.1 Creator Draft / Source / Runtime 分层

继续保持：

```text
Creator Draft
!= Saved Source Asset
!= Game-local Canonical Instance
!= Runtime State
```

Creator Draft Store 只拥有创作期草稿、导入证据、ChangeSet、Undo 与发布工作状态；Source Asset Library Store 只拥有已通过 G9-03 结构/完整性验证的本地正式 Source Asset。Creator 发布不自动修改既有游戏或 Runtime。

### 3.2 稳定资产身份

新建资产的 `targetAssetRef` 由 Program 在草稿创建时生成并保持稳定；标题、文件名、别名和 AI 输出均不能改写身份。已有 Source 创建新版本时精确继承基础快照 `assetRef`。

### 3.3 revision / CAS / stale AI

所有 Draft mutation 使用 revision/CAS。AI 任务绑定 exact `basisRevision`；迟到结果不能覆盖玩家较新的修改。持久化失败不产生半成功草稿。

### 3.4 导入创作稿

`.md` / `.txt` 原件独立保存 exact 原文、SHA-256、稳定 `segmentRef`。

导入整理继续冻结：

```text
确定 + 有真实 evidence + 当前目标为空
→ 可以填写

不确定 / 冲突 / 信息不足 / 已有玩家内容
→ 不覆盖
→ 保留 unresolved / conflict / evidence
```

section 目标以正式语义 `sectionRef` 判空；Provider 不能借 Creator `nodeRef` 覆盖已有章节，新导入章节的内部 node identity 由 Program 生成。

### 3.5 任务级 AI 编辑授权

普通 AI 创作不因为进入 Creator 而获得整份 Draft 默认写权限。

当前 Program-owned scope 能限制：

- operation family；
- scalar target；
- semantic section；
- dependency / list node identity；
- typed node 的 exact `nodeKind + nodeRef + allowedOperations`。

`targetVersion` 只有本次任务明确授权时才可由 AI 修改。

### 3.6 Provider unknown output runtime gate

Provider 输出按不可信 `unknown[]` 处理，不依赖 TypeScript interface 充当运行时验证。

复杂操作在进入 mutation 前逐层解析并校验：

- section；
- dependency 及 nested target/sourceScope；
- world composition；
- character reference source；
- expansion feature；
- expansion module / routing / config shape / arrays；
- expansion UI surface / contribution；
- static scalar data。

非法枚举、错误布尔类型、未知嵌套字段、错误 stable ref shape、越权 typed node 等均局部忽略并记录；同一任务中的合法 sibling 继续形成一个 ChangeSet，最后仍只做一次 CAS persist。

### 3.7 ChangeSet / Undo

一次用户或 AI 创作任务形成一个 Program-owned ChangeSet。Undo 通过 inverse mutation 创建新 revision；若目标后来已被其它修改改变则 `CREATOR_UNDO_CONFLICT`，不得把后续新内容一起回滚。

### 3.8 Source Asset Library append-only

正式 Source Asset Library：

```text
assetRef + version 不存在
→ append

存在且 digest 相同
→ idempotent success

存在但 digest 不同
→ SOURCE_ASSET_VERSION_CONFLICT
→ 永不覆盖
```

### 3.9 发布与恢复

Creator Draft 直接确定性编译为 G9-03 `TavernAssetV1`，复用现有 `computeAssetDigest()` / `validateAndVerifyAsset()`；不绕回 Markdown / G9-04 Legacy Adapter。

发布生命周期：

```text
editable
→ publishing
→ Source append
→ published
```

若 Source 已写入而 Draft 尚未 final，可 exact resume；若发生确定性版本冲突且 Source 未写入 intended snapshot，Draft 会 CAS 恢复为 `editable`，保留内容与版本，允许玩家修改版本后重试。

---

## 4. correction 历史结论

### correction-01

初次审核发现 4 个 P1：

1. 普通 AI 缺少任务级授权；
2. Import section `sectionRef/nodeRef` 错位；
3. AI 一坏项拖掉同批合法项；
4. Source 版本冲突后 Draft 卡在 `publishing`。

`f789150...` 关闭了 section identity 与版本冲突恢复，并建立首版 scope + partial apply。

### correction-02

第二次审核只剩 2 个同源 P1：

1. typed node scope 未绑定 `nodeKind`；
2. Provider 复杂 payload runtime parse 不完整。

最终 `25286b25...` 已通过 exact typed-node authorization、完整复杂 operation runtime parsing、跨 typed-kind node identity gate 与相应负向测试关闭上述问题。

---

## 5. 范围审计

最终 correction-02 仅修改：

- `src/资产创作/L0_公理层/资产创作契约.ts`
- `src/资产创作/L1_器件层/创作稿状态器.ts`
- `src/资产创作/L2_流程层/创作稿流程.ts`
- `tests/g9/CreatorCore草稿导入发布基础测试.test.ts`

整个 G9-05B 实现没有修改：

- G9-03 `tavern.asset.v1` wire；
- G9-04 Legacy Adapter 语义；
- Runtime production contracts / authority；
- 最终 Creator UI；
- G9-05C 世界包产品纵向。

Provider 生产调用仍不属于本阶段实现。

GitHub 对最终 SHA 未配置独立 CI status；本 Independent Review 以 exact-SHA source/spec diff、已提交 focused positive/negative tests、既有阶段 gate 约束与任务分支纯前向关系完成技术判定，不把“有远端 CI”虚构为证据。

---

## 6. G9-05C 授权边界

下一阶段正式授权：

```text
G9-05C World Creator Vertical
```

其目标是让第一个真实世界包走通：

```text
我的资产库
→ Creator
→ blank / imported manuscript / source revision
→ World structured Draft workspace
→ manual + bounded AI authoring
→ validation / change summary / undo
→ explicit publish
→ Source Asset Library world asset
→ reopen / create new revision
```

G9-05C 必须复用本阶段共享 Core，不得另建第二套 Draft、AI Patch、Source Store 或发布事务。

角色卡与拓展包继续后置到 World Creator 真实纵向证明之后。

---

## 7. Closure

```text
G9-05B = PASS / CLOSED
main = 25286b2517cb26520109e3d8738671e53d88c861
G9-05C = AUTHORIZED / NEXT
```

最终原则：

> **Creator 的共享核心已经从“AI 能改草稿”收口为“AI 只能在用户本次授权范围内，通过 Program 完整验证过的 typed operation 修改 exact Draft revision；正式 Source 仍由 Program 确定性发布，任何创作过程都不能绕过 Source / Game-local / Runtime 的长期权威边界”。**
