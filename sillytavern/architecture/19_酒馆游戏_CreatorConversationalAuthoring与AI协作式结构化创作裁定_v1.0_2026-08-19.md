---
title: 酒馆游戏｜Creator Conversational Authoring 与 AI 协作式结构化创作裁定
status: current
version: 1.0
date: 2026-08-19
project: 酒馆游戏新版主体
scope:
  - creator
  - ai-authoring
  - asset-authoring
  - g9-05
  - product-ux
  - draft-authority
---

# 19｜酒馆游戏：Creator Conversational Authoring 与 AI 协作式结构化创作裁定 v1.0

> [!abstract] 核心裁定
> 新版 Creator 不再定义为“传统结构化表单 + 若干零散 AI 按钮”，而定义为 **结构化 Creator 主工作区 + 持续 AI 创作对话区** 的协作式创作环境。
>
> 用户既可以完全手工编辑，也可以在对话区自然描述需求、回答 AI 的最少必要追问，并授权 AI 通过 Program-owned Typed Creator Tools 直接修改当前 Creator Draft。AI 可以操作 Draft，但不拥有正式 Source Asset；最终 Save / Publish 仍由用户显式触发并经过确定性 Validator。
>
> 本裁定进入 G9-05 首版产品范围。跨多个资产的大规模自主操作、长时间自主 Agent、自动联网研究等不属于当前产品目标，只保留在 `Creator_AI后续可选增量备忘录_v1.0_2026-08-19.md`，未来是否实现取决于真实 Creator UAT，而非当前 Roadmap 承诺。

---

## 0. Product Definition

Creator 的核心用户问题不再只是：

> “如何把资产字段填完？”

而是：

> **“如何让不会理解全部底层资产协议的创作者，也能用自然语言表达创作意图，并始终在可见、可控的结构化资产上完成创作？”**

正式产品形态：

```text
用户
├─ 直接编辑 Creator 主工作区
└─ 在 AI 对话区表达需求
        ↓
   AI 理解 / 最少必要追问
        ↓
   结构化 Creator Tool Call
        ↓
   Program 校验
        ↓
   Creator Draft
        ↓
   主工作区即时可见
        ↓
   Validator / AI Review / 用户继续迭代
        ↓
   用户 Save / Publish
        ↓
   Source Asset
```

因此：

```text
AI Chat
!= Creator Draft

Creator Draft
!= Saved Source Asset
```

---

## 1. 主界面正式采用“双工作面”

桌面端 Creator 默认包含两个协同区域：

```text
主工作区 / 设定区
= 当前结构化资产 Draft 的可见编辑器

AI 创作对话区
= 持续对话、需求澄清、解释、规划与受控编辑入口
```

AI 对话区不是独立的资产真相，也不是另一个 Creator State。

主工作区始终是当前 Draft 的可见结构化表达；用户可以随时不使用 AI，直接编辑同一份 Draft。

未来移动端可以采用抽屉 / Tab / 浮层等交互，不改变双工作面产品语义。

---

## 2. CREATOR-AI-01｜AI 可以直接修改 Draft，但不能发布资产

当用户明确委托编辑任务，例如：

> “帮我完善这个世界的政治结构，但不要改魔法设定。”

该指令可以形成**任务级 Draft 编辑授权**。

正式链：

```text
User Intent
→ AI interprets bounded authoring task
→ Typed Creator Edit Plan / Tool Calls
→ Program validates allowed asset/section/operation
→ atomic / bounded Draft mutation
→ UI reflects change
→ change summary + Undo
```

正式冻结：

```text
AI can edit Draft
!= AI can Save / Publish Source Asset
```

默认不允许 AI：

- 自动发布；
- 自动导出；
- 静默替换整个资产；
- 绕过 Validator；
- 绕过用户明确保护的字段 / 区域。

---

## 3. CREATOR-AI-02｜AI 不操作 DOM，只调用 Typed Creator Tools

禁止把 Creator AI 做成：

```text
模型看网页
→ 猜第几个输入框
→ click / type / DOM automation
```

必须通过 Program-owned typed tool seam，例如语义等价能力：

```text
read_current_asset
read_current_section
update_fields
add_entry
remove_entry
add_feature
add_module
set_dependency
run_validation
explain_validation
undo_authoring_action
```

精确 API 名称在 G9-05 internal design 时冻结；G9-03 不把 Creator Tool API 写进 Source Asset external schema。

每个 tool 必须：

- 明确 asset / section / stable ref；
- 使用 typed payload；
- 经过 Program validation；
- 无 arbitrary state path；
- 无 eval / script / callback；
- 不能绕过 Source Asset schema / Validator。

---

## 4. CREATOR-AI-03｜Chat Conversation 不是资产事实源

聊天记录只拥有：

- 创作过程；
- 用户意图上下文；
- AI 澄清问题与回答；
- 本次编辑任务的解释与摘要。

它不拥有资产事实。

例如用户先说“考虑删除魔法”，之后又说“还是保留”，最终事实只由当前 Creator Draft 决定。

正式冻结：

```text
Conversation History
!= Asset Canonical State
```

AI 需要知道“当前作品是什么”时，应读取当前 Draft / 当前 section / 当前 validation state，而不是仅凭长聊天历史回忆。

Source Asset protocol 不增加 `chat_history` / `prompt` / `model_name` 等 Creator 过程字段。

---

## 5. CREATOR-AI-04｜最少必要追问，尽快产生可见第一稿

Creator AI 不得变成需求访谈问卷。

用户表达创作目标后：

- 只追问会显著改变资产结构、核心语义、玩法组合或关键 Owner 的问题；
- 可安全采用默认值的细节优先生成第一稿并标明假设；
- 尽快让用户在主工作区看到真实 Draft，再围绕可见结果继续迭代。

目标体验：

```text
natural idea
→ few high-value questions
→ visible structured first draft
→ iterative refinement
```

而不是：

```text
natural idea
→ long questionnaire
→ no visible artifact for a long time
```

---

## 6. CREATOR-AI-05｜Current Focus 是对话上下文的一部分

AI 对话允许读取 bounded Creator interaction context：

- current asset identity / kind；
- current selected section；
- current focused entry / feature / module；
- current validation issues；
- bounded Draft summary / exact requested fields；
- relevant stable refs。

因此用户可以自然说：

> “这里太复杂了，帮我简化。”

但 AI 不因为能够读取当前 Draft，就获得整个资产库、所有其它资产或任意文件的默认读取权。

---

## 7. CREATOR-AI-06｜任务级 Undo / 变更可见性是首版要求

AI 一次任务可以修改多个相关字段，因此不能要求用户逐字段点确认；但必须保留可观察性与撤销能力。

首版至少提供语义等价能力：

```text
AI task
→ 3–N bounded Draft mutations
→ one authoring action / change set
→ user can inspect summary
→ user can undo the whole AI action
```

用户手工编辑和 AI 编辑都进入同一 Draft revision / history discipline，不建立第二份 AI Draft。

高风险 destructive bulk replacement 默认不在 G9-05 首版开放。

---

## 8. CREATOR-AI-07｜Validator Authority 不改变

AI 可以：

- 生成内容；
- 补全字段；
- 改写；
- 解释结构；
- 提出依赖建议；
- 做语义 Review；
- 解释 Validator 错误。

但正式合法性仍由确定性 Validator / Program contract 决定。

```text
AI says valid
!= Asset valid
```

正确链：

```text
AI / User edits Draft
→ deterministic validation
→ errors / warnings
→ user / AI iterates
→ user Save / Publish
```

AI Review 与 deterministic Validator 必须分开显示：

- Validator：结构、协议、引用、类型、明确规则；
- AI Review：语义完整性、重复 Owner、创作一致性、可读性、设计建议。

---

## 9. CREATOR-AI-08｜无 API 仍必须是完整 Creator

正式产品承诺：

```text
No API Key / Provider unavailable
→ full manual Creator remains usable
```

AI 是创作增强，不是 Creator availability dependency。

以下操作不得自动触发模型：

- 打开资产；
- 手工编辑；
- 保存 Draft；
- Validator；
- 导入 / 导出；
- 浏览 Creator 页面。

模型调用由用户明确发起的对话 / AI 操作触发。

Provider 失败时：

- 不损坏 Draft；
- 不推进错误 revision；
- 保留手工编辑能力；
- 给出可恢复的非阻塞反馈。

当前游戏创建模块已经存在 AI Fill / Review、字段级部分应用、无 Key 非阻塞的经验；G9-05 应复用其成熟边界，而不是再创造一套相反规则。

---

## 10. G9-05 首版 Must-have

G9-05 正式从“传统 Creator + AI 辅助”升级为：

> **Conversational Authoring Core + Structured Creator Workspace。**

阶段首版必须建立共享 Creator AI rails，并让世界包 / 角色卡 / 拓展包三类主资产使用同一套 Draft / Tool / Validation authority。

至少包含：

1. 结构化主工作区；
2. 持续 AI 对话区；
3. AI 读取当前 asset / section / focus 的 bounded context；
4. natural-language authoring request；
5. 最少必要追问；
6. typed Creator tool / patch contract；
7. AI 可受控修改 Draft；
8. Draft 修改即时反映到主工作区；
9. 任务级变更摘要 / Undo；
10. deterministic Validator；
11. AI Review / Validator explanation；
12. 用户显式 Save / Publish；
13. 无 Provider 时完整手工创作；
14. 导入 / 导出 / 返回“我的资产库”。

### 首个真实纵向

优先使用世界包证明：

```text
用户自然描述世界
→ AI 进行少量关键追问
→ AI 通过 Creator Tools 填充结构化世界包 Draft
→ 主工作区显示结果
→ 用户手工/对话继续修改
→ Validator
→ Undo 可用
→ 用户保存
→ 导入 / 导出 / 再打开
```

随后角色卡与拓展包复用同一 Creator rails；G9-05 关闭前，三类主资产都必须能使用共享框架完成基础创作链。

---

## 11. G9-05 明确不做

以下不属于 G9-05 首版完成条件：

- 资料库真实检索增强创作；
- 跨多个资产的大规模自主操作；
- 长时间自主 Agent；
- 自动联网研究；
- 通用 autonomous-agent runtime；
- 跨资产万能事务；
- 长任务调度器；
- 通用网页抓取平台；
- speculative vector/RAG platform；
- AI 自动 Save / Publish；
- Creator 资产协议中的 prompt / chat / model metadata。

前三项高级能力只记录在：

`Creator_AI后续可选增量备忘录_v1.0_2026-08-19.md`

其状态是：

```text
DEFERRED OPTIONAL
NOT ROADMAP COMMITMENT
```

只有真实 Creator UAT 后出现明确需求，才重新进入 Product Definition / Architecture Gate。

---

## 12. 与资料库 #18 / #18A 的关系

G9-05 不因为 Creator AI 而提前实现资料库产品功能。

当前仍保持：

```text
资料库协议
→ G9-03 同批冻结

资料库产品功能
→ 三类主资产端到端闭环后
```

未来资料库 Creator 上线后可增加：

```text
Creator Chat
→ bounded library retrieval
→ Reference Projection
→ AI authoring proposal
→ Creator Draft
```

但始终保持 #18A：

```text
Retrieved Library Slice
!= Source Asset Truth
```

G9-05 首版不以资料库检索为依赖。

---

## 13. 对 G9-03 / G9-04 的传播

### G9-03

必须确保三类主资产协议具有稳定 identity / field / ref / validation semantics，使 Creator Tool 能可靠编辑；但不得把 Creator AI 实现细节写进 Source Asset protocol。

正式区分：

```text
Asset Contract
!= Creator Tool Contract
!= Conversation Protocol
```

G9-03 不冻结：

- prompt；
- chat history；
- model provider；
- Creator tool transport；
- AI action history external wire。

### G9-04

Parser / Adapter / Compiler 必须完全独立于 AI 工作。

```text
Manual Creator
and
AI-assisted Creator
→ produce same valid Source Asset contract
```

因此 Creator AI 不能成为 G9-04 编译器的依赖。

---

## 14. Product UAT Gate

G9-05 至少需要两类真实使用证明。

### UAT-A｜新手自然语言创作

一个不了解底层资产协议的用户能够：

```text
描述世界创意
→ 回答少量问题
→ 得到结构化可见 Draft
→ 继续聊天 / 手工调整
→ 成功验证与保存
```

不能要求用户理解底层 module / dependency / context field 才能开始。

### UAT-B｜高级用户直接控制

高级作者可以：

- 完全不用 AI；
- 精确手工编辑；
- 使用 AI 做局部改写 / Review / 解释；
- 明确保护不希望 AI 修改的区域；
- 撤销 AI 的任务级修改。

AI 不得降低高级作者的可控性。

---

## 15. Decision Propagation

```text
Current Stage
G9-02C
= NO REOPEN / NO SCOPE CHANGE

G9-03
= Asset contract only; reserve stable editable identities

G9-04
= AI-independent parser/compiler/binding

G9-05
= Conversational Authoring Core is REQUIRED

Creator advanced autonomous capabilities
= DEFERRED OPTIONAL

资料库 Creator / Runtime retrieval
= DEFERRED by #18
```

本裁定不改变当前 G9-02C Sol Core 的实现任务与 Base。

---

## 16. 最终冻结

```text
新版 Creator
= Structured Creator Workspace
+ Conversational AI Authoring

AI
= can discuss
+ can ask bounded questions
+ can call typed Creator tools
+ can mutate Draft
+ can review / explain

AI
!= Source Asset Owner
!= Validator Authority
!= automatic Publisher

User
= final Save / Publish authority

No Provider
= Creator still fully usable
```

> **让用户用自然语言表达创作意图，让 AI 帮他操作结构化 Creator；但始终让用户看得见、改得动、撤得回，并由正式协议与 Validator 决定什么能成为资产。**