---
title: G9-05B Creator Core Independent Review correction-01
status: current-review-correction-required
version: 1.0
date: 2026-08-20
stage: G9-05
slice: G9-05B
---

# G9-05B｜Creator Core｜Independent Review correction-01

## 1. 审核对象

```text
Formal Code Base
c492ac4a0eb33ec055f582a2a023066853e2c323

Task Packet
63160fa9e4e0868195b468f745ae95d3ffb97e9b

Reviewed Task Final
0757f4674da23bcc2588b6265cc5c3d663e3667b

origin/main during review
c492ac4a0eb33ec055f582a2a023066853e2c323
```

`c492ac4a... → 0757f467...` 为严格前向后代，任务分支 ahead 3 / behind 0。变更限定在任务包、`src/资产创作/`、`src/资产库/`、G9-05B focused test 与 `package.json` 测试脚本；未修改 Runtime、G9-03 资产协议生产实现、G9-04 Legacy Adapter 或最终 Creator UI。

## 2. 最终结论

```text
P0 = 0
P1 = 4

G9-05B implementation = FAIL / CORRECTION REQUIRED
G9-05B CLOSED = NO
G9-05C AUTHORIZED = NO
main = unchanged at c492ac4a0eb33ec055f582a2a023066853e2c323
```

共享 Draft、Source Asset Library、CAS、原件保存、G9-03 发布编译与基本发布恢复方向成立；阻断集中在 AI 任务授权、导入 section identity、局部合法应用、版本冲突恢复四个正式产品边界。

---

## 3. P1-01｜普通 AI 创作缺少任务级编辑授权门禁

#19 已冻结：用户一次自然语言委托形成“任务级 Draft 编辑授权”，Program 必须校验允许修改的 asset / section / operation，并尊重用户明确保护的字段或区域。

当前 `CreatorDraftFlow.author()` 仅把 `allowedTargetsFor(assetType)` 传给 Provider。该集合等于该资产类型几乎全部可编辑面，而且始终包含 `targetVersion`。Provider 返回后，Core 直接执行整个 `operations`；Program 没有保存或校验“本次用户究竟授权了哪些目标”。

因此例如：

```text
用户：只改摘要，不要改标题和版本
↓
Provider 返回合法 typed operations：
metadata.summary
metadata.title
targetVersion
↓
当前 Program 可全部接受
```

这不是 bounded task authorization。

Correction：

- `author()` 必须接收 / 建立 Program-owned exact authoring scope；
- scope 至少表达本次允许的 scalar target、section/node ref / operation family；
- Provider 可见范围不得大于 Program 最终强制范围；
- Provider 返回操作必须逐项通过 scope gate；越权项进入 ignored/observable，不得落盘；
- `targetVersion` 默认不在 AI scope，只有用户当前明确授权“设置/修改发布版本”时才允许；
- 测试：summary-only 任务不能改 title/version/其它 section；显式版本授权时才可改版本；focused section 不能改未授权 section。

---

## 4. P1-02｜导入 section 的语义目标与 Draft node identity 混用，可绕过 blank-only

G9-05B 冻结：导入整理只填“当前空白目标”，不得静默覆盖已有用户内容。

当前导入整理将 section assignment 的目标构造成：

```text
section:<sectionRef>
```

但 `creatorTargetIsBlank()` / `readTarget()` 对 `section:<x>` 的解释实际是：

```text
按 nodeRef == x 查找
```

随后真正 `upsert_section` 又按 `proposedValue.nodeRef` 替换节点。

这使“空白检查使用 sectionRef、真正写入使用另一个 nodeRef”的两套 identity 发生错位。一个导入 assignment 可以用不存在的 `sectionRef` 通过空白检查，同时携带已有 section 的 `nodeRef`，从而替换玩家已有 section；反方向也可以制造同一个正式 `sectionRef` 的多个 Creator 节点，直到发布时才失败。

Correction：

- 冻结导入 section 的唯一语义目标 identity；若 typedTarget 是 `sectionRef`，blank-only 检查必须按 `sectionRef`；
- 新增 section 的 Creator `nodeRef` 由 Program 分配/归一化，或至少必须证明与已有 nodeRef 全局不冲突；Provider 不得利用 nodeRef 选择替换对象；
- existing sectionRef + different nodeRef → ignore/conflict，不覆盖；
- new sectionRef + existing nodeRef → ignore/conflict，不覆盖；
- exact new sectionRef + unique Program-owned node identity → apply；
- 增加上述三个负向/正向测试。

---

## 5. P1-03｜AI / Import 的“局部合法应用”没有真正成立

Task Packet AC-07 与 G9-05B §9.5 明确要求：同一个 AI 任务中合法项要保留，坏项进入 ignored；坏项不得迫使合法确定项全部回滚；最终持久化仍为一次原子提交。

当前基础函数：

```text
applyCreatorOperations(basis, operations)
```

按顺序严格执行；任意一个操作抛错，整个调用失败。普通 `author()` 直接使用它，因此一个非法 AI 操作会让同批合法操作全部丢失。

Import parser 也仍可把部分“结构形状正确但值非法”的 assignment 放入 operations，例如空字符串 scalar、空 section 核心字段；这些错误只在 `applyCreatorOperations()` 阶段抛出，同样会回滚同批合法确定项。

Correction：

- 用户手工 `mutate()` 可以继续 strict；
- AI-owned author/import 输出必须走 per-operation Program validation/application；
- 合法项汇总为一个 atomic ChangeSet；非法/越权/坏类型项进入 `ignoredOperations`；
- 所有合法项处理完成后一次 CAS 持久化；若持久化失败，则整批不落盘；
- 测试至少包含 `valid + invalid + valid` 的普通 AI 任务与 import 任务，验证两个合法项都落盘、非法项可观察且只有一个 ChangeSet。

---

## 6. P1-04｜Source 版本冲突会把 Draft 永久卡在 publishing

G9-05B §15.2 已冻结：

```text
SOURCE_ASSET_VERSION_CONFLICT
→ 不覆盖
→ Draft 返回可修复状态 / 明确错误
```

当前发布流程先：

```text
editable
→ persist publishing
→ library.appendValidated(asset)
```

若 `appendValidated()` 发现同 `assetRef + version` 不同 digest，会抛出版本冲突。但 `CreatorPublicationService` 没有把 Draft 从 `publishing` 恢复为 `editable`。

结果：

- Draft 已不可编辑；
- 重试仍使用同一 intended snapshot；
- 每次都再次版本冲突；
- 用户无法修改 `targetVersion` 修复，只能另建草稿或直接改数据库。

Correction：

- 明确区分“确定性的版本冲突”与“可重试的暂时存储失败”；
- 版本冲突发生且 exact Source 未成功写入时，使用 CAS 将 Draft 从 `publishing` 转回 `editable`，生成新 revision，保留全部内容与当前 targetVersion；
- 返回稳定 `SOURCE_ASSET_VERSION_CONFLICT` / 等价公共错误，让用户可直接修改版本后再次发布；
- Source store 原资产保持不变；
- 测试：预置相同 assetRef/version 的不同 digest → publish 冲突 → Draft editable → 改新版本 → publish PASS。

---

## 7. 非阻断发现

本轮没有形成额外 P1 的部分：

- Draft / Source / Runtime Owner 分离方向正确；
- Source Asset Library 只接受 G9-03 validated asset；
- 同 digest 幂等、不同 digest 不覆盖；
- Draft publication compiler 复用现有 `computeAssetDigest()` + `validateAndVerifyAsset()`，没有第二套 hash/validator；
- `.md/.txt` 原件 exact 保存、SHA-256 与 stable segment refs 方向正确；
- stale AI / Draft CAS / 持久化失败保护方向正确；
- Undo 使用 inverse operation + fingerprint conflict，而不是整份 Draft 回滚；
- Source-written / Draft-not-finalized 的 exact recovery 已有测试；
- 无 Provider 路径与 Runtime 隔离方向正确；
- `validUnresolved()` 对空 evidence refs 的约束可以在返修时顺手收紧，但目前只影响工作提示，不直接形成 Source/Runtime authority blocker。

---

## 8. 下一步

```text
same branch
agent/g9-05b-creator-core
↓
Codex correction-01
↓
focused + full regression
↓
new exact SHA
↓
GPT exact-SHA Independent Review
```

禁止新建 correction 分支、rebase、amend、force push、push main 或提前进入 G9-05C。
