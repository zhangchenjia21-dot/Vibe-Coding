---
title: G9-05H｜Owner First Playable Independent Review｜correction-01
status: current-review-rework
version: 1.0
updated: 2026-08-22
---

# G9-05H｜Owner First Playable Independent Review｜correction-01

## 0. Review Object

```text
Repository: zhangchenjia21-dot/sillytavern
Task Branch: agent/g9-05h-owner-first-playable
Formal Code Base / protected main:
a97b4bae6a3bd9308ecb8c092b96bce81dd43700

Reviewed implementation:
9bed47059f721d7cf6cc4742922001eba007fdb3

Task Packet:
agent tasks/G9-05H_Kimi_OwnerFirstPlayableUAT准备执行包_v1.0_2026-08-22.md
packet commit:
1c4aabe10fae00de6a4d5f96af0851cd5de07360
```

Exact head 通过临时 Draft PR #7 锁定；PR 随后关闭，`merged=false`。`main` 在审查期间保持 `a97b4bae...`。

GitHub 对 reviewed SHA 无 external status / workflow run；本 Review 不声称 CI green。

## 1. Verdict

```text
P0 = 0
P1 = 3

G9-05H Engineering Gate
= CORRECTION-01 REQUIRED

READY FOR OWNER UAT
= NOT YET AUTHORIZED

OWNER UAT PASS
= 当然不属于 Engineering Review 可授予状态
```

本轮核心实现方向可保留，不要求重做：

- shared real-asset blob verifier；
- real Markdown → G9-04 Adapter → historical Source → formal Creator source_revision → production Source；
- missing DB only-if-missing compatibility bootstrap；
- existing game/save/draft/unrelated Source 保留；
- prepare repeat Source-level idempotency；
- same-version different-hash prepare fail closed；
- default production Host / proof ref isolation；
- NoProvider / external Provider calls = 0；
- thin launcher wrapper；
- Owner-facing UAT guide 基础结构。

Correction 只关闭以下三个边界。

---

## 2. P1-01｜越权删除 historical `grok build/` 任务资料

Reviewed branch 在 Task Packet 之后出现独立提交：

```text
0909740352462a0732b3be5ae51de7e3ef314932
chore: remove superseded G9-02B/02C/03/04 Grok build task packets
```

该提交删除 `grok build/` 下历史任务包与 README，并在 commit message 中称为 `Owner-directed cleanup`。

但 G9-05H 的唯一 Outcome 是 Owner UAT enablement；当前 Owner 决策还明确把 branch/handoff 历史清理列为低优先级卫生项，没有授权在本阶段执行 destructive cleanup。

因此：

```text
Historical task evidence cleanup
!= G9-05H UAT enablement
```

### Required

- 在同一 task branch 恢复 `090974...` 删除的全部 `grok build/` 文件，内容必须与 Formal Code Base `a97b4bae...` exact 一致；
- 不删除/改写其它历史资料；
- 不借 correction 清理 branch、handoff、archive；
- UAT implementation commit `9bed470...` 的功能代码保持。

允许使用显式、范围受限的 restore/revert；禁止 reset/rebase/force-push 改写已审查历史。

---

## 3. P1-02｜Owner preflight 的 Source readiness 不是 exact snapshot

当前 `buildOwnerUatPreflight()`：

```text
readSourceVersions()
→ raw SQL SELECT asset_ref, version
→ sourcePresent(assetRef, version)
→ PRESENT / ABSENT
```

这只证明 stable ref + version 字符串存在，没有证明：

```text
assetRef
+ assetType
+ version
+ contentHash
```

与 G9-05H 要求的 production snapshot 完全一致。

因此以下数据库状态可能被误报 `PRESENT / READY`：

```text
same assetRef + version
+ different valid contentHash
```

或 Source row/asset_json 在语义上已损坏、但 `asset_ref/version` 行仍存在。

H-P04 只证明 `prepare` 遇到 same-version/different-hash 会 fail closed；没有证明 `preflight` 会 BLOCKED。

### Required

为 Owner 首玩三枚 production Source 建立**单一共享 exact snapshot truth**，至少覆盖：

```text
World 0.2.3
刘备 Character 0.1.3
EP-CHAR-CORE 0.1.6
```

推荐做法：

1. 在 `G9主资产安装.ts` 或等价共享 Program helper 中冻结/导出三枚 `AssetSnapshotRefV1`（包含 exact `contentHash`）；
2. exact hash 必须来自当前 frozen real corpus + formal deterministic revision path；不得手工猜 hash；
3. installer regression 必须断言新生成资产仍等于这些 pins，未来若同 version 编译结果漂移则测试 FAIL，而不是静默重定义 Source version；
4. preflight 使用 `SQLiteSourceAssetLibraryStore.readExact()` 或等价完整校验，不能继续只查 `asset_ref/version`；
5. DB 中 same ref/version different hash → 对应 source status 必须 `ABSENT`（或更明确 INVALID），overall verdict = `BLOCKED`；
6. storage integrity invalid → `BLOCKED`，不能 `READY`；
7. `owner:uat:launch` 在不传 assets root 的后续日常启动中，仍必须能基于 frozen production snapshot pins 对 DB 做 exact readiness 检查；不能要求每次启动都重新提供 assets repo path。

不要把本 correction 扩成新的 Source migration system。

### Required tests

至少新增：

```text
preflight + exact correct snapshots
→ PRESENT / READY

preflight + LiuBei 0.1.3 same version different hash
→ sourceLiuBei013 != PRESENT
→ BLOCKED

preflight + EP-CHAR-CORE 0.1.6 same version different hash
→ sourceEpChar016 != PRESENT
→ BLOCKED

installer output
→ exact production snapshot pins match
```

World exact snapshot也必须进入同一机制。

---

## 4. P1-03｜Owner UAT 指南给出的关闭方式会留下 launcher-owned 进程

当前指南第 9 节写：

```text
直接关闭浏览器和启动器（或在终端 Ctrl+C）
→ 再次 owner:uat:launch
```

但正式 `start-playtest.ps1` 会使用 `Start-Process` 启动 backend/frontend，保存 `.local/g2-launcher-state.json` ownership record，然后 launcher 脚本自身结束；它明确提示 Owner 使用“关闭酒馆游戏.bat”结束本轮进程。

因此：

```text
Close browser
!= Stop game processes

Ctrl+C after launcher returned
!= Stop launcher-owned backend/frontend
```

错误指南可能导致第二次启动命中 `ALREADY_RUNNING`，直接阻断 G9-05H 必须人工验证的：

```text
关闭应用
→ 再次启动
→ 从 Game Library 继续同一游戏
```

### Required

- 修改 `docs/uat/G9-05H_OwnerFirstPlayable.md`：正式关闭路径必须明确使用仓库已有 `关闭酒馆游戏.bat`；
- 可以同时注明底层 `scripts/stop-playtest.ps1` 作为工程等价入口，但 Owner 主路径优先中文 `.bat`；
- 删除“只关浏览器 / Ctrl+C 即等价停止产品”的误导；
- 不需要为此重写 launcher；
- 如增加 `owner:uat:stop` 仅允许做薄 wrapper，非必需。

---

## 5. Preserve｜本 Correction 不得重做

以下已接受：

```text
G9真实资产核验.ts 单一 blob pinning truth
G9-05G gate / E2E / G9-05H 共用 verifier
formal Adapter + Creator revision install path
NoProvider installation
missing DB only-if-missing bootstrap
existing user state preservation
prepare Source idempotency
prepare same-version/different-hash fail closed
production capability module present
proof module absent
thin start wrapper
Library deferred
external Provider calls = 0
```

禁止：

- CI / Node pinning / SQLite driver migration；
- Creator UI convergence；
- Library Product；
- G10 / Release；
- Runtime bootstrap 重写；
- historical branch/archive cleanup；
- 真实 Provider 自动调用。

---

## 6. Correction Acceptance

```text
AC-HC01 090974 删除的 grok build 文件全部 exact 恢复；无其它卫生清理
AC-HC02 三枚 production Source 有共享 exact snapshot pins
AC-HC03 installer regression 证明 pins 与 frozen real corpus + formal revision 输出一致
AC-HC04 preflight 用 exact snapshot 校验，不再只看 ref/version
AC-HC05 same-version different-hash preflight BLOCKED
AC-HC06 owner:uat:launch 无 assets-root 时仍可 exact 检查已安装 Source
AC-HC07 UAT guide 使用“关闭酒馆游戏.bat”作为正式关闭路径
AC-HC08 prepare/idempotency/user-state/proof-host 原已通过行为不回归
AC-HC09 G9-05G real-assets gate 与 tests 不回归
AC-HC10 external Provider calls = 0
AC-HC11 main 保持 a97b4bae...，不得集成
AC-HC12 最终 branch/worktree clean，返回 exact Final SHA
```

---

## 7. Validation

在可用的真实 `sillytavern-assets` root 上运行：

```text
SILLYTAVERN_ASSETS_DIR=<real-assets-root> npm run owner:uat:test
npm run owner:uat:prepare -- <real-assets-root>   # 只允许指向隔离/任务专用 DB；不得碰 Owner 真实 UAT DB
npm run owner:uat:preflight -- <real-assets-root> # 同上，或以测试调用证明 CLI
npm run g9:05g:real-assets -- <real-assets-root>
npm run g9:05g:test
npm run g9:05g0:test
npm run g9:05e:test
npm run g9:05e:product-e2e
npm run g9:05f:test
npm run g8:test
npm run typecheck
npm run lint
npm test
npm run product:build
npm run g2:disclosure
npm run launcher:smoke
git diff --check
```

注意：CLI `owner:uat:prepare` 的真实默认 DB 是 `.local/g5-playtest.sqlite`。自动验证不得破坏 Owner 的真实本地 DB；应优先通过测试 API / 环境覆盖指向隔离 `.local/*.sqlite`，并在报告中明确实际路径与未触碰 Owner DB。

真实 Provider：

```text
0 calls
```

GitHub status/workflow 若为空，必须如实写无 external CI evidence。

---

## 8. Return

完成后只返回：

```text
Result
Changed
AC-HC01..AC-HC12
Tests: PASS / FAIL / NOT RUN
Provider calls
Reviewed Implementation SHA = 9bed47059f721d7cf6cc4742922001eba007fdb3
Correction Final SHA
Branch HEAD
origin/main
Working tree status
Remaining
```

完成口令：

`G9-05H CORRECTION-01 READY FOR INDEPENDENT REVIEW`
