---
title: my world｜SillyTavern 参考研究改进讨论通过清单补充记录
status: working-approved-candidate-list-addendum
version: 1.1
created: 2026-09-07
updated: 2026-09-07
parent_record: ./SILLYTAVERN_REFERENCE_IMPROVEMENT_DISCUSSION_2026-09-06.md
implementation_authorization: none
roadmap_authorization: none
---

# my world｜改进讨论通过清单补充记录

## 0. 用途

本文件续接主通过清单，记录后续讨论中 Owner 明确通过的提案。它与主记录共同构成本轮讨论结束后 Revised Task Axis 的输入。

重要边界不变：

- 通过 != 立即实施；
- 不据此创建新的 MW Work Item；
- 不修改 CURRENT Roadmap / Architecture / Status；
- 等 Owner 明确结束本轮讨论后，由 GPT 统一做重叠、依赖、冲突、阶段和 UAT 审计。

主记录此前累计通过：28 项。

---

## 1. 新增已通过提案

### P-29｜Player-known 世界大事 / World Chronicle｜原提案 46

**Owner verdict：通过。**

增加与“重要经历”分离的 player-safe 世界大事投影：重要经历回答“我的人生发生了什么”，World Chronicle 回答“我所了解的时代 / 世界发生了哪些重大变化”。

只记录玩家已经通过 accepted history 合法获知、且值得长期记住的外部事件；可复用 Provenance、Epistemic Status、Conflicting Evidence、Turn Freshness。后台 World Event 发生但玩家未知时不得展示，也不能把 Chronicle 当成 GM 剧情队列或新的 World Truth owner。

### P-30｜Game-local Frozen Manifest / 游戏构成｜原提案 48

**Owner verdict：通过。**

让玩家随时查看当前 Game 在 Final Create 时实际冻结了哪些 World / Entry / Character / Guaranteed NPC / Expansion / Composition 及其 exact generation / version。普通层显示作品名和版本，高级层按需显示 fingerprint / 机器标识。

Manifest 必须来自 Game-local frozen composition，不能重新查询 Source Library 当前最新版来替换旧局信息。该能力可为 Chronicle export、bug reproduction、Save/System、Creator UAT 和未来迁移提供稳定依据。

### P-31｜世界纠错模式 / Reality Correction Mode｜原提案 49

**Owner verdict：通过，并明确修正：应像 OOC 一样成为独立输入模式。**

当错误已经进入正式 Game Reality，而不是只存在于 Character / People 等派生展示时，玩家需要一个与“角色行动”“OOC 场外说明”并列、语义明确的 **世界纠错模式**。

候选模式：

```text
角色行动 | OOC / GM 指导 | 世界纠错
```

玩家在世界纠错模式中描述当前不一致及依据，例如：

> 折刀在第 73 回合已经交给陈安，此后不应仍属于张琛。

系统应形成可见、可审核的 Correction Proposal，明确：当前不一致、accepted-history 依据、拟修正的 authoritative state、可能受影响的 Surface / mechanics，再由玩家显式确认后执行正式 correction。

核心边界：

- 世界纠错模式不是普通角色行动，不送入 Narrative 伪装成剧情行为；
- 也不是 OOC 创作偏好，OOC 不能直接改 World Truth；
- 不允许自然语言直接任意写数据库；
- Correction 必须有正式 owner、currentness、影响范围与可追溯记录；
- 原始 accepted Narrative 不应被静默改写；当前 Reality 可以记录“该不一致已被明确纠正”；
- Correction 必须可被 Timeline / Restore 正确回退，保持 Reversibility over prevention；
- 若只是 player-side 派生表述错误，应继续走 P-10 派生信息纠正，不升级为 Reality Correction。

该提案需要独立 Architecture Audit，尤其要冻结它与 World / Inventory / NPC / Knowledge / Mechanics / Timeline 的 authority 和 atomicity。

### P-32｜本回合变化 / Player-visible Consequence Diff｜原提案 50

**Owner verdict：通过。**

每个 accepted 回合后允许玩家按需查看“这一回合真正留下了哪些 player-visible durable consequences”，例如 Character 更新、People 更新、事务关闭、行囊变化、组织认识变化、真实 mechanics 状态变化等。

优先采用**结构化 player-safe projection diff**，而不是为此再调用一次模型：比较上一 accepted current projection 与当前 projection，由各 authoritative owner 的安全投影形成变化摘要，并支持跳转到对应 Surface。

核心边界：

- 只展示玩家当前可知的 durable consequence；
- 不泄露隐藏 Agency / NPC-private / off-screen World truth；
- diff 不是新的事实源，不自行做语义推断；
- malformed / fail-soft 的后台能力不能伪装成已经发生的变化；
- Restore / Regenerate 后变化记录必须遵守对应 accepted-history currentness。

### P-33｜Model Profiles / 模型配置预设｜原提案 52

**Owner verdict：通过。**

允许用户保存和一键切换整套 AI 配置方案，例如 Narrative model、Background model、Provider endpoint、reasoning level、timeout 与其它明确的 lane assignment / compatibility settings，从而低摩擦管理多套真实 Provider / Model 工作方式。

核心边界：

- Profile 只保存可安全持久化的配置，不把 API Key / credential 打包进作品或共享资产；
- Credentials 与 Model Profile 分离，由本机 credential owner 管理；
- Source 可以记录“作者验证过哪些模型/方案”，但不能绑定或强制使用作者自己的 Provider；
- 切换 Profile 后应触发/复用 P-24 Compatibility Preflight，明确哪些 lane 支持、哪些未经验证；
- 与 P-08 Narrative / Background model 分离、P-18 使用可视化、P-24 Compatibility 统一设计，避免多套平行配置系统。

### P-34｜Debug Mode / 自动异常原因输出｜原提案 55（Owner 修正版）

**Owner verdict：通过，并明确把原“Bug Report Package”收敛为独立 Debug Mode。**

提供一个可切换的 **调试模式**。在没有异常时，调试模式与普通模式的玩家体验应尽量保持一致，不持续用开发日志打扰正常游玩；一旦关键 AI lane、结构校验、currentness、Save/Restore、mechanics、projection/persistence 等出现异常，调试模式自动输出可理解的错误原因和必要诊断信息。

候选异常输出可以包括：

```text
推荐行动失败
原因：结构化输出校验失败（actions 数量不等于 5）
回合：87
Provider / Model：Kimi / K3
结果：Narrative 已正常接受；推荐行动 fail-soft，不影响世界状态
```

或：

```text
人物整理结果被丢弃
原因：回调绑定的 accepted prefix 已过期
当前回合：91
原请求回合：89
结果：stale callback 未写入当前 People
```

核心边界：

- 普通模式继续保持简洁，不自动展示高级技术细节；
- Debug Mode 平时不改变游戏逻辑、模型输入、世界结果或成功/失败语义；
- 调试模式只提高 observability，不成为第二事实源，也不能为了“更容易调试”放宽结构校验或 currentness；
- 自动错误原因来自 Program 已知 terminal state / validation / currentness / persistence evidence，不要求模型生成“为什么自己这样想”；
- 不暴露 API Key、credential、无关本机隐私路径、NPC-private / GM-private 内容；
- 必要时允许按需展开更详细证据，但默认先给人能看懂的错误原因；
- 应与 P-01 生成状态/诊断、P-18 AI 使用可视化、P-24 Compatibility Preflight 统一观测模型，避免再造独立日志真相源。

该提案替代原 55 的“手工生成 Bug Report Package”作为当前通过方向；未来若真实协作需要可导出诊断包，可再作为 Debug Mode 的派生能力讨论，不在本提案中自动授权。

---

## 2. 当前累计

主记录 28 项 + 本补充记录 6 项 = **当前累计通过 34 项**。

本文件仍不是正式 Roadmap 或实现授权。
