---
title: AI 驱动项目｜风险成本对称与 Recovery-first 补充规范
status: active-cross-project-addendum
version: 1.0
created: 2026-08-23
last_updated: 2026-08-23
scope: cross-project
related:
  - AI驱动项目全生命周期开发流程规范_v1.8_2026-08-23.md
  - 第二版_SillyTavern_补充复盘_DEC-P04_模型自由与恢复优先_2026-08-23.md
---

# AI 驱动项目｜风险成本对称与 Recovery-first 补充规范 v1.0

## 1. 定位

本规范补充全生命周期开发流程中的 Guardrail / Product Core 判断。

它不主张所有软件都“放开模型”，而是要求：

> **防错强度必须与错误真实成本、可发现性和可恢复性匹配。**

对于 AI 原生、生成型、创作型、Agent 型产品，尤其要防止把“模型理论上可能犯错”自动翻译成大规模前置限制。

---

## 2. Risk Economics Gate

任何明显限制模型能力、用户自由、主路径效率或产品核心体验的新 Guardrail，在进入正式架构前至少记录：

| 维度 | 必答问题 |
|---|---|
| Error Frequency | 错误现实中多常见，而不是理论上是否可能？ |
| Error Impact | 单次错误真实用户成本多大？ |
| Detectability | 用户 / 系统是否容易发现？ |
| Recoverability | 能否撤回、重试、重生成、回滚、回档？ |
| Recovery Cost | 恢复一次要花多少时间 / 数据 / 金钱？ |
| Prevention Cost | 防止错误需要多少架构、Schema、审批、状态机和测试？ |
| Core-value Damage | 防错是否削弱产品核心价值？ |
| Complexity Cost | 是否显著增加上下文、Tool、状态传播和 bug surface？ |

默认比较：

```text
Expected Error Cost + Recovery Cost
vs
Prevention Cost + Core-value Damage + Complexity Cost
```

若后者明显更高，应收窄或取消预防机制。

---

## 3. Recovery-first 适用条件

同时满足越多项，越应优先 Recovery-first：

- 单次错误低成本；
- 用户立即可见；
- 无第三方外部副作用；
- 状态可回滚；
- 可重生成 / 重试；
- 不涉及隐私、安全和权限；
- 不会破坏不可恢复的 canonical truth；
- 模型开放能力本身就是产品核心价值。

典型恢复机制：

- undo；
- regenerate；
- retry；
- checkpoint；
- save / restore；
- branch；
- revision history；
- human correction。

---

## 4. Prevention-first 适用条件

以下情况默认仍应前置预防：

- 不可逆外部副作用；
- 高额金钱 / 法律 / 安全成本；
- 隐私或秘密泄露；
- 用户授权 / 权限越权；
- 错误难以察觉；
- 数据损坏后无法可靠恢复；
- 高影响正式规则结果；
- 会系统性污染长期状态且无法重建。

---

## 5. AI 原生产品特别规则

AI 产品不得把以下推理当作充分架构理由：

```text
模型可能输出错误
→ 所以必须先把输出空间做成封闭白名单
```

必须继续证明：

1. 错误现实成本高；
2. Recovery 不足以处理；
3. 限制不会显著损害产品核心能力；
4. 没有更窄的安全边界。

对于创作、主持、开放式推理、世界模拟、Agent 执行等核心依赖模型主动性的产品，**过度 Prevention 本身属于一级产品风险。**

---

## 6. 与 Product Purpose Gate 的关系

正确顺序补充为：

```text
Primary Purpose
↓
Core Value / Core Loop
↓
Real Risks
↓
Risk Cost / Recoverability
↓
Minimum Necessary Guardrails
↓
Recovery Design
↓
Architecture
↓
Early Reality Check
```

禁止：

```text
列举所有理论错误
→ 每个错误建立机制
→ 最后再检查产品是否还能实现核心价值
```

---

## 7. Stage / Review Gate

在重大 AI 架构、权限系统、状态机、Schema Freeze 或产品面独立审核中，增加问题：

> **我们是在防一个真正昂贵的风险，还是在用昂贵系统消灭一个便宜且可恢复的错误？**

若无法回答，相关 Guardrail 不应自动升级为长期架构约束。
