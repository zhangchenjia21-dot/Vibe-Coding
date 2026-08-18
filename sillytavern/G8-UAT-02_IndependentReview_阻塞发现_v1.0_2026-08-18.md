# G8-UAT-02｜Independent Review 阻塞发现 v1.0

状态：`CURRENT REVIEW FINDING / NARROW RETURN REQUIRED`
日期：2026-08-18
当前代码对象：`sillytavern@ce6c05ffc89f31a39400f7069c9e7503e4c86d9a`
上游规格：`G8-UAT-02_GameLocalSemanticMaterialization与活世界收口规格_v1.0_2026-08-18.md`

## 1. Review Rerun 结论

```text
G8-UAT-02 Narrow Return        REVIEWED / RETURN REQUIRED
Independent Review Rerun      FAIL
P0                             0 confirmed
Original P1                    5
Closed P1                      4
Remaining P1                   1
Stage UAT                      NOT AUTHORIZED
G8                             ACTIVE / UAT FIX
G9                             NOT AUTHORIZED
```

`ce6c05f...` 已实质关闭原 5 个 P1 中的 4 个：

- Save / Restore 已纳入 Game-local topology / provenance / typed Item placement，并可回滚 future-only entity；
- Materialization Need 已由每 Turn Semantic AI 判断，World Materializer 只在 typed need 成立时调用；Program 不做关键词 / 正则 / 通用 NLP；
- Creation public materialization 与 private seed raw source 已形成输入隔离，private marker 不进入 public provider path；
- Item 已支持 player / character / scene typed placement，并贯通 Runtime / SQLite / Product。

Opening Beat 也已从 Creation Materializer final prose 改为 structured beat → independent Opening Narrative Realizer → persisted narrative；当前没有确认新的 P0/P1 authority failure。

---

## 2. Remaining P1｜Scene-present Item 在最终 Narrative-safe projection 中被过滤

### 现状

当前三条链不一致：

```text
Runtime / Product item visibility
→ 支持 scene-present Item

ContinuityContext initial projection
→ 支持 scene-present Item

projectNarrativeSafeCurrentContext final projection
→ 仍只把 holderRef 属于 player / visible Character 的 Item 纳入 current.items
→ 没有把 placementKind=scene && holderRef=finalSceneRef 纳入
```

因此合法的场景物品（例如学院庭院里的“学院公告”）可以：

- 存在于 authoritative Runtime；
- 出现在 Product 【物品】Surface；
- 被 Semantic Candidate Directory 看见；

但在正式 Narrative realization 前的最终 `NarrativeSafeContext.current.items` 中消失。

随后 `compileNarrativeSafeFormalOutcome()` 又从该 final context 生成 `interactableAuthority.itemNames`，于是 Narrative Authority 与 Runtime / Product 对同一个 scene-present Item 产生不一致。

这违反 G8-UAT-02 要求的：

```text
Runtime authoritative item
== Product visible item
== Narrative-safe current item
```

### 必修

修正 `projectNarrativeSafeCurrentContext()` 的 Item 投影，使 final Scene 下：

```text
public scene-present Item
→ placementKind = scene
→ holderRef = finalSceneRef
→ included
```

同时继续包括：

```text
player-held Item
visible current-participant-held Item
```

并保证移动后使用 **finalSceneRef**，旧 Scene 的 scene-present Item 不得残留。

优先复用 / 抽取统一的 current-scene Item visibility helper，避免 `visibleItemsInCurrentScene`、`compileContinuityContext`、`projectNarrativeSafeCurrentContext` 三份规则再次漂移。

---

## 3. Required Proof

必须新增 focused regression：

```text
Scene A:
  Item = 学院公告 (scene-present)

Player at Scene A
→ PlayerSession.visibleItems contains 学院公告
→ ContinuityContext.current.items contains 学院公告
→ final NarrativeSafeContext.current.items contains 学院公告
→ Narrative Formal Outcome interactableAuthority.itemNames contains 学院公告

Player moves A → B
→ final Narrative-safe context no longer contains Scene A 学院公告
→ Scene B scene-present Item（如有）正确出现
```

再增加至少一个 Narrative fixture / Provider-boundary test：玩家观察或查看 scene-present Item 时，Narrative 可以使用该 canonical Item，不需要 phantom fallback，也不会把它当作玩家持有物。

---

## 4. 已关闭项目不要重做

以下原 P1 在 `ce6c05f...` 独立复核中已通过，继续保持：

```text
P1-01 Save/Restore topology          CLOSED
P1-02 AI Semantic-gated materialize  CLOSED
P1-03 Opening Beat authority         CLOSED
P1-04 Public/private boundary        CLOSED
P1-05 typed Item placement core      CLOSED except final Narrative projection seam
```

当前只修 final Narrative scene-item projection，不重新设计 Creation、Save、Semantic Router、Opening、Product 或 Item persistence。

---

## 5. Gate

返修后至少运行：

```text
focused scene-item Narrative tests
G5
G6
G7
G8
G8-UAT-01
G8-UAT-02
full
Typecheck
Lint
Product build
Launcher smoke
Disclosure
Diff-check
Targeted Real DeepSeek UAT2 smoke
```

Real Provider Gate 必须增加或明确证明：scene-present Item 在普通正式 Turn Narrative 中可被正确引用，且移动离场后不再出现在 Narrative working set。

---

## 6. Current Next

> `scene-present Item Narrative projection narrow fix → Independent Review rerun`。

修复通过前不授权 Project Owner Stage UAT，不启动 G9。
