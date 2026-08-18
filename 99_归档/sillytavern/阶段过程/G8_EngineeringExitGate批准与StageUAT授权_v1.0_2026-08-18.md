# G8｜Engineering Exit Gate 批准与 Stage UAT 授权 v1.0

状态：`APPROVED FOR STAGE UAT`  
日期：2026-08-18  
代码基线：`sillytavern@3ad5b419e9fb289488a18ded82c5a77f2cdd376c`

> 本文件记录 ChatGPT 作为 Product / Architecture / Independent Review Owner 对 G8 Engineering Exit Gate 的正式批准。
>
> 本批准只表示工程 Gate 已通过、可以进入项目所有者 Stage UAT；**不等于 Stage UAT 已通过，也不等于 G8 已 CLOSED。**

---

## 1. Freshness / Decision Propagation

批准前已重新执行 Cross-Chat Freshness Preflight 与 Decision Propagation：

- `Vibe-Coding/main` 最新相关 current 为 G8 Exit Gate 文档与阶段收口更新；
- `sillytavern/main` 当前实现基线仍为 `3ad5b419e9fb289488a18ded82c5a77f2cdd376c`；
- `Skill/main` 当前相关上游保持有效；
- 未发现新的 superseding architecture / stage decision；
- 未发现改变 G8 Exit Gate 或 Stage UAT 前置关系的新决策。

结论：

> `Freshness PASS + Decision Propagation PASS + Roadmap Consistent`

---

## 2. Engineering Exit Gate 裁定

正式批准：

> **ENGINEERING EXIT GATE = PASS**
>
> **READY_FOR_STAGE_UAT**

批准依据包括：

- G5：203/203 PASS；
- G6：15/15 PASS；
- G7：20/20 PASS；
- G8：179/179 PASS；
- full suite：638/638 PASS；
- typecheck / lint / product build / launcher smoke / disclosure / diff-check PASS；
- Creation Project → Final Create → Game Instance → Product Session vertical PASS；
- typed Runtime UI Definition → Host → player-safe DTO → Product UI vertical PASS；
- single action one Formal Turn PASS；
- controlled multi-action transient preflight → aggregate FormalDelta → one atomic Formal Turn PASS；
- invalid second step zero partial commit PASS；
- Resolution-required sequence zero RNG / zero mutation PASS；
- G7 Durable Recovery exactly-once regression PASS；
- no-key / configured-key critical paths PASS；
- Desktop / narrow / keyboard 无 P0/P1 blocker；
- P0 = 0；
- P1 = 0。

GitHub 当前未为该 commit 返回独立 CI status，因此具体测试数字属于 Executor-side Gate evidence；但关键 Runtime / Contract / Recovery / Product 路径已完成 GitHub exact-SHA Independent Review。

---

## 3. Non-blocking backlog

以下不阻塞本次 Stage UAT：

### P2 / Deferred

- WEB-06 Save Center / Recovery Product UX；
- WEB-07 Undo / Re-input；
- Creation Preview；
- 完整 Creation Semantic Review；
- 高级 Timeline / Recovery UX。

### P3 / Polish

- deep mobile / WCAG；
- 视觉 polish；
- 无调用、无 authority 的旧命名查询辅助函数 `readNewGameDraftId` 清理。

这些不得被解释为永久取消；按既有路线在 Alpha / G11 或对应 cleanup 时重新拉回。

---

## 4. Stage UAT 权限边界

下一步由项目所有者执行**短版产品 Stage UAT**。

项目所有者只验证玩家可感知核心路径，不承担内部工程测试。

建议覆盖：

1. 正式 Launcher 启动 → Main Menu；
2. 创建新游戏 → AI 辅助创建 → Final Create → 开始游戏；
3. 普通单动作自然语言 → 正常产生一个 Formal Turn；
4. 两步自然语言动作，例如“回到酒馆，然后等到第二天早晨” → 玩家体验正常，仍表现为一次输入 / 一个正式回合；
5. Core five / Extension Surface / Settings 等关键界面无明显阻塞。

不要求项目所有者人工测试：

- crash stage；
- idempotency；
- SQLite transaction；
- RNG zero-call；
- hidden disclosure scan；
- transient reducer；
- Durable Execution internal stage。

这些已经属于 Engineering Gate 责任。

---

## 5. Final G8 Closure Gate

只有项目所有者完成 Stage UAT，并明确给出产品侧 PASS 后，才允许：

```text
Engineering Exit Gate PASS
+
Stage UAT PASS
↓
G8 PASS / CLOSED
↓
G9-01 Compatibility Audit
```

若 Stage UAT 发现：

- P0 / P1 玩家阻塞 → 开集中修复任务；
- P2 / P3 → 记录 backlog，不重新扩大 G8 关键路径。

---

## 6. 当前正式结论

截至本文件：

```text
G8 Engineering Exit Gate     PASS / APPROVED
Stage UAT                     AUTHORIZED / NEXT
G8                            NOT YET CLOSED
G9-01                         NOT YET STARTED
```

正式批准语句：

> **批准当前 `sillytavern@3ad5b419e9fb289488a18ded82c5a77f2cdd376c` 进入 G8 Stage UAT。**
>
> **在项目所有者 Stage UAT 明确 PASS 前，不宣称 G8 CLOSED，不启动正式 G9 machine implementation。**
