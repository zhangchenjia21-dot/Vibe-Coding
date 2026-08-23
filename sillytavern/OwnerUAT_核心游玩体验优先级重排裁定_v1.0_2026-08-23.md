---
title: Owner UAT｜核心游玩体验优先级重排裁定
status: current-decision-frozen
version: 1.0
updated: 2026-08-23
project: 酒馆游戏新版主体
---

# Owner UAT｜核心游玩体验优先级重排裁定 v1.0

## 0. 当前决定

Project Owner 在真实进入产品并开始体验后，明确给出新的产品判断：

> 当前产品最大问题不是缺少资料库、更多资产类型、更多通用机制或更多外围功能，而是进入游戏之后不好玩。

因此立即调整当前产品优先级：

```text
Owner First Playable
= 已产生有效产品反馈

Owner UAT
= BLOCKED / NEEDS PRODUCT EXPERIENCE FIXES

当前最高优先级
= 核心游玩体验收敛

Library Product
= DEFERRED

通用核心能力扩展
= DEFERRED

G10 Provider Expansion
= NOT AUTHORIZED

G11 Alpha / 全系统验收
= NOT AUTHORIZED until core playability closes

G12 Windows Release
= NOT AUTHORIZED
```

本裁定不宣布当前工程基础失败。现有 Runtime、Save / Restore、Crash / Resume / Recovery、资产协议、Creator、Use My Assets 与 Host 等已关闭能力继续作为上游基础；新的问题属于产品层“是否值得持续游玩”的核心验收缺口。

---

## 1. 这不是 UI 美化任务

“进入游戏后不好玩”不得被降格解释成：

- 改颜色、字体、间距；
- 增加更多面板；
- 增加更多 Suggested Actions；
- 增加更多资产；
- 增加更多机制数量；
- 单纯让模型输出更长；
- 单纯减少一个 Confirmation；
- 单纯换 Provider / Model。

这些都可能改善局部体验，但不能证明核心游玩循环成立。

当前必须优先回答：

> 玩家为什么愿意继续下一回合？

---

## 2. 当前诊断对象：核心游玩循环

下一阶段先做产品诊断与体验规格，不直接派发大规模实现任务。

至少审查以下维度：

1. **目标感**：玩家进入局内后是否知道自己当前可以追求什么，是否存在短期与中期目标。
2. **选择质量**：玩家输入是否形成真正不同的路线，而不是不同措辞换来近似叙事。
3. **行动—反馈闭环**：玩家行动后，世界是否迅速给出可感知、可理解、可追踪的变化。
4. **后果与代价**：重要选择是否改变资源、关系、局势、机会、风险或未来可选项。
5. **世界主动性**：NPC、组织、事件、承诺和背景行动是否会主动推进，而不是永远等待玩家聊天。
6. **不确定性与风险**：是否存在足够但可理解的不确定性，让判断、筹划和冒险有意义。
7. **节奏**：是否存在铺垫、决策、结果、新局势的循环，而不是持续均匀对话。
8. **信息回报**：玩家是否能从人物、地图、目标、知识、物品等界面看见自己造成了什么影响。
9. **角色感**：使用角色资产进入游戏后，玩家是否真的感觉自己在扮演该角色，而不是只换了一个名字和背景文本。
10. **持续动机**：连续数回合后是否自然产生“我还想看看接下来会怎样”的动力。

这些维度的目的是定位主要乐趣缺口，不是要求一次性把所有维度都做成复杂系统。

---

## 3. 必须保留的上游不变量

核心游玩体验改进不得以破坏已证明基础为代价：

```text
Program owns formal facts / outcomes
Model interprets / proposes / narrates
Player Agency violations = 0
No Phantom World Change
Hidden Knowledge 不泄露
Atomic Commit
Save / Continue / Restore exactly-once
Crash / Resume / Recovery exactly-once
Source Asset != Game-local Instance != Runtime State
```

允许重新设计体验和玩法循环；不允许为了“更像游戏”而把正式世界提交权直接交给模型，或绕过已关闭的一致性、安全和恢复边界。

---

## 4. 对既有体验债的重新评价

G5 已登记但后置的体验问题现在重新进入诊断范围，包括：

- 过度 Confirmation；
- 自然语言多动作不顺；
- 缺少玩家可见撤回 / 重新输入；
- 聊天式输入与程序行动之间的摩擦。

但它们现在只是“核心不好玩”可能原因的一部分，不自动成为主要根因。

不得恢复到“一个句式失败就加一个规则”的局部修补循环。

---

## 5. 下一阶段顺序

当前授权顺序冻结为：

```text
真实试玩观察 / 现状诊断
↓
核心游玩循环产品定义
↓
最小可验证乐趣假设
↓
小范围实现
↓
Project Owner 真人连续试玩
↓
依据实际乐趣反馈继续迭代或否决
```

在“核心游玩体验”达到可接受水平前，不恢复资料库、更多资产类型、通用核心大扩展、Provider 扩展、Alpha 收口或 Release 优先级。

---

## 6. 当前成功标准

下一阶段的成功标准不再是“所有接口和测试都通过”本身，而至少需要 Project Owner 能够在真实局内确认：

```text
我知道现在想做什么
+
我的选择真的会改变事情
+
世界会回应并继续发展
+
结果让我产生新的判断或目标
+
我愿意主动再玩下一回合
```

工程 Gate 仍然必须通过，但：

```text
Engineering PASS
!= Fun / Playability PASS
```

核心游玩体验最终仍由 Project Owner 真人试玩裁定。
