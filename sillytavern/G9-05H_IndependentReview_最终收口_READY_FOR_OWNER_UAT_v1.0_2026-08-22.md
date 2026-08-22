---
title: G9-05H｜Owner First Playable Independent Review｜最终收口
status: PASS-READY-FOR-OWNER-UAT
version: 1.0
updated: 2026-08-22
---

# G9-05H｜Owner First Playable Independent Review｜最终收口

## 0. Review Object

```text
Repository: zhangchenjia21-dot/sillytavern
Task Branch: agent/g9-05h-owner-first-playable
Protected Main / Formal Base:
a97b4bae6a3bd9308ecb8c092b96bce81dd43700

Reviewed Final Implementation SHA:
fb264be9fa8878230949d3b371222c2ed8254f6c
```

Exact head 通过临时 Draft PR #8 锁定，随后立即关闭，`merged=false`。该 PR 仅作为 exact-SHA audit anchor，不参与集成。

GitHub 对 reviewed SHA：

```text
combined statuses = []
workflow runs = []
```

因此本 Review 不声称 external CI green。

## 1. Final Verdict

```text
P0 = 0
P1 = 0
P2 = 1

G9-05H Engineering Gate
= PASS / CLOSED

Owner First Playable
= READY FOR OWNER UAT

OWNER UAT PASS
= NOT GRANTED BY ENGINEERING
= only Project Owner actual play can grant
```

## 2. Accepted Implementation

已确认：

- pinned real corpus 由共享 `G9真实资产核验.ts` 统一持有；
- G9-05G real-assets gate / E2E 与 G9-05H prepare/preflight 共用同一 corpus truth；
- Owner prepare 走 `real Markdown → G9-04 Adapter → historical Source → Creator source_revision → production Source`；
- World 0.2.3、刘备 0.1.3、EP-CHAR-CORE 0.1.6 为首次 Owner UAT production snapshots；
- production snapshots 已冻结 exact `assetRef + assetType + version + contentHash` pins；
- preflight 对正常/不同 hash Source 可做 exact readiness，same ref/version different hash → INVALID/BLOCKED；
- 无 assets-root 的日常 `owner:uat:launch` 仍可基于 frozen production pins 对已安装 DB 做 exact readiness；
- missing DB only-if-missing compatibility bootstrap；
- existing game/save/Creator Draft/unrelated Source preservation；
- repeat prepare Source-level idempotency；
- same-version different-hash prepare fail closed；
- production Program Host 包含 `builtin:character-capability.evidence.v1`，不包含 G9-04 proof module；
- automated external Provider calls = 0；
- Owner UAT Guide 的正式关闭路径改为 `关闭酒馆游戏.bat` / `scripts/stop-playtest.ps1`；
- Library 继续 deferred；本阶段没有进入 G10 / Release。

## 3. Owner-authorized Historical Cleanup

提交：

```text
0909740352462a0732b3be5ae51de7e3ef314932
```

删除旧 `grok build/` G9-02B/02C/03/04 task packets。

Project Owner 已明确确认这些文件由 Owner 本人决定删除，属于授权 repository hygiene。最终 Review 接受该提交，不要求恢复，不计 P1/P2。

## 4. P2｜Preflight 对 SQLite 行级元数据损坏的诊断还未完全等价于正式 Store

最终 correction 的 `checkExactSourceSnapshot()` 会：

```text
SELECT asset_json by asset_ref + version
→ JSON parse
→ validateAndVerifyAsset
→ snapshotOfSourceAsset
→ compare exact production pin
```

这足以覆盖正常 Owner prepare 路径，以及 same ref/version different asset content 的 INVALID/BLOCKED。

但正式 `SQLiteSourceAssetLibraryStore.parseStoredAsset()` 还会额外核对 SQLite 行：

```text
row.asset_ref
row.asset_type
row.version
row.digest
```

与 `asset_json` 内部字段完全一致。

因此若数据库被外部/手工直接破坏为：

```text
asset_json 自身仍合法且 exact
但 row.digest 或 row.asset_type 与 asset_json 不一致
```

当前 preflight 可能误报 PRESENT，而正式 Store 会报 `STORAGE_INVALID`。

### Severity / Timing

```text
P2 / Post-UAT Hardening
```

理由：正常 Owner prepare 通过正式 Store 写入，行元数据与 asset_json 保持一致；该缺口需要外部/手工 SQLite corruption 才触发，不阻断首次 Owner UAT 主路径。

为避免 correction 链无界扩散，本轮不再发 correction-02。后续 Engineering Hardening 可让 preflight 复用/抽取正式 Store 的 read-only row integrity semantics。

## 5. Correction Budget Decision

G9-05H correction-01 的两个 Owner-facing P1 均已关闭：

```text
exact production Source readiness
formal stop / restart path
```

剩余问题降为非阻断 P2。按照当前 correction budget：不为边缘 hardening 继续派生 correction-02，不再延迟首次真人 UAT。

## 6. Git Integration Gate

集成前确认：

```text
base main = a97b4bae6a3bd9308ecb8c092b96bce81dd43700
reviewed final = fb264be9fa8878230949d3b371222c2ed8254f6c
ahead = 6
behind = 0
merge base = exact a97b4bae...
```

允许集成方式仅为：

```text
main exact fast-forward → fb264be9...
force = false
```

禁止 merge / squash / rebase / integration SHA。

## 7. Current Product State

G9-05H Engineering Closure 后：

```text
Primary Asset E2E = PASS / CLOSED
G9-05H Owner First Playable Engineering = PASS / CLOSED
Owner First Playable = READY FOR OWNER UAT
Library Product = DEFERRED / LATER EXTENSION
G10 Provider Expansion = NOT AUTHORIZED
Release = NOT AUTHORIZED
```

下一动作不是继续 Agent 功能开发，而是 Project Owner 本人实际：

```text
prepare
→ preflight
→ launch
→ 我的资产库
→ 使用我的资产建局
→ 真实 Provider Session
→ Save / Continue / Restore
→ 正式 stop
→ restart
→ continue same game
```

Owner 实际反馈决定后续产品修复与 Engineering Hardening 优先级。