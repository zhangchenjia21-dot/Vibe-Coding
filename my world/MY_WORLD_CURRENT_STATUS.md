---
title: my world｜当前状态
status: current-project-status
version: 1.7
created: 2026-08-26
updated: 2026-08-26
phase: G2 AI Conversation Spine
current_task: G2-04 Turn / Conversation Domain v0.1
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. 文档职责

本文件只拥有当前执行状态：Current Phase、Current Task、已完成 Gate、Owner UAT 结论、当前 blocker / waiting。

其它事实 owner：

- 产品目的：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 架构地图：`MY_WORLD_架构_CURRENT.md`
- 阶段 / Task DAG：`MY_WORLD_总体规划路线图_CURRENT.md`

---

## 2. 当前状态

```text
Current Phase                 G2 — AI Conversation Spine
G2-01 Application/Game Shell PASS — Owner UAT
G2-02 Provider Adapter v0.1  PASS — Engineering
G2-03 Narrative View         PASS — Owner UAT
Current Task                  G2-04 — Turn / Conversation Domain v0.1
G2-04 implementation         0bf1f012366db7271664a192c1c30e60947cc5c9
Independent Review            RETURNED — IR-03 regenerate provider context defect
G2-05 Context Assembly        HOLD
G2-GATE                       NOT YET
```

---

## 3. G2-03｜CLOSED / PASS

Owner 已在导出 EXE 中完成真实 UAT，并明确 PASS。G2-03 已成立的交互、布局、Composer、Narrative richness 与字体 carry-forward 不因本次 G2-04 review 回退。

字体偏小的非阻塞观察已由 G2-04 implementation 调整到中等可读基线；未来玩家可选字号仍留给 G6 UI Preference。

---

## 4. G2-04 Independent Review

### 已通过的部分

`0bf1f012366db7271664a192c1c30e60947cc5c9` 已建立最小纯内存 Conversation / Turn Domain：

- Conversation Domain 拥有 Turn ordering / accepted player+GM truth / Generation State；
- UI 已移除旧 `_history` / duplicated generation flags，主要作为 Domain projection；
- Provider Adapter 仍保持 transport-only；
- completed Regenerate / latest-turn correction 的 accepted truth 在 Domain 内遵守“成功前保留旧稳定结果、成功后原子替换”；
- cancel / fail 不破坏旧 accepted pair；
- Retry / Regenerate / correction 有 focused domain tests；
- typography medium baseline 已完成；
- 未越界实现 G2-05 / G3。

这些部分暂不要求重做。

### IR-03｜Regenerate Provider Context includes superseded assistant — BLOCKING

当前 `src/domain/会话.gd::build_provider_messages()` 在一个已 completed 的最新 Turn 进入 Regenerate 时：

```text
active turn has accepted response
pending_player_text == player_text
→ append current user
→ append current old accepted assistant
```

因此第一次 completed Turn 的 regenerate 请求会成为：

```text
[system, user(current), assistant(old)]
```

多 Turn 情况则为：

```text
[system,
 previous user, previous assistant,
 current user, current old assistant]
```

这不是“同一 player input 重新生成新的 GM response”的正确 request context。旧 assistant 应继续作为 **Domain accepted truth** 稳定保留，供 Cancel/Fail rollback；但它不应作为当前 Regenerate attempt 的输入消息继续条件化新生成。

正确分离：

```text
Accepted Domain Truth
→ old assistant remains stable until replacement succeeds

Regenerate Provider Request
→ previous accepted turns
→ current player user exactly once
→ request ends with current user
→ current old assistant excluded
```

当前 G2-04 离线 T04 反而把错误行为编码成测试，明确期待 `[system,user,assistant]`。真实 GUI test 只检查 regenerate context 中 player input / user count，没有检查最后 role 和旧 assistant 是否被排除，因此没有抓住 IR-03。

DeepSeek 当前官方 Chat Prefix Completion 文档也把“以 assistant 作为最后消息继续补写”定义为专门 prefix 场景，需要 `prefix=true` 与 beta endpoint；当前正式 Adapter 没有采用该模式。G2-04 不应引入 prefix continuation 来绕过本问题。

---

## 5. IR-03 Required Repair

只做 G2-04 focused repair：

1. `build_provider_messages()` 对 completed latest Turn 的 Regenerate：
   - 保留所有更早的 accepted `user + assistant` pairs；
   - 当前 Turn 只发送 `pending/current user` 一次；
   - **不得发送该 Turn 的旧 accepted assistant**；
   - request 最后一条必须是当前 `user`。
2. Domain 内旧 accepted assistant 继续稳定存在；只有新 generation completed 才原子替换。
3. Regenerate Cancel/Fail 后旧 accepted pair 继续完整；随后 direct new Turn context 合法。
4. Latest-turn correction 继续使用 corrected user，不带该 Turn 的旧 assistant；不得因修 IR-03 破坏 AC-07/08。
5. 修正 focused test：

```text
first completed turn
→ regenerate
→ provider roles == [system, user]
→ current old assistant occurrence == 0
→ last role == user
→ accepted old assistant still unchanged in Domain
→ success atomically replaces assistant
```

多 Turn regenerate 还必须证明：

```text
previous accepted pairs preserved
+ current user exactly once
+ current old assistant absent
+ last role == user
```

6. 真实 DeepSeek GUI completed→Regenerate 路径增加同样 context assertion，再证明新 generation completed。
7. 原 IR-01 / IR-02 / correction / typography / G2-03 regression 全部保持通过。
8. 不修改 Provider Adapter，不实现 G2-05 Context Assembly，不引入 beta prefix completion。

---

## 6. 当前核心约束

- `Model freedom first. Reversibility over prevention.`
- `Narrative richness over artificial brevity.`
- `Context stays bounded, not starved.`
- `UI is a projection, not a second truth source.`
- `Transcript != Timeline.`
- Regenerate 的旧 accepted response 可以保持稳定 truth，但不能错误进入 replacement request。
- G2-04 不得提前实现 G2-05 / G3。

---

## 7. 当前 waiting

```text
Blocking: IR-03 regenerate provider context includes superseded assistant
Waiting: KimiCode K3 focused repair + regression evidence + revised Final Report
Owner UAT: not required
Next only after Independent Review PASS: G2-05 Context Assembly v0.1
```
