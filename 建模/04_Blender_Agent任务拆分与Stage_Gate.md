# Blender Agent 任务拆分与 Stage Gate

状态：current

## 1. 不要混合 Reconstruction 与 Design

Agent 任务首先要明确是哪一种：

### Reconstruction Task

目标是忠实复现已有设计。

约束：

- 自主权低；
- 不得为了“更合理”“更好看”“更好编码”重构布局；
- 遇到局部歧义只做最小必要推断；
- 偏差必须显式报告。

### Design Task

目标是提出或深化设计。

允许：

- 调整比例；
- 比较方案；
- 重新组织空间；
- 做设计取舍。

两种任务不能在同一阶段含混混用。

---

## 2. 不要让一个 Agent 同时承担全部角色

大型 Blender 任务若同时要求 Agent 担任：

- 建筑师；
- 总规划师；
- 景观设计师；
- Blender 建模师；
- Python 工程师；
- 渲染师；
- QA；

会产生目标竞争。

推荐职责分离：

### Owner / GPT

负责：

- 真实需求；
- 设计方向；
- 关键取舍；
- Review；
- Freeze / Stage Gate。

### Design Agent

负责：

- 概念；
- 总平面；
- 局部设计；
- 单体设计。

### Implementation Agent（如 Codex）

更适合：

- Blender Python；
- 参数化实现；
- 资产装配；
- 场景工程化；
- 自动验证。

施工 Agent 应尽量少在施工过程中重新承担核心设计决策。

---

## 3. 推荐 Stage Gate

### G0：Requirement Freeze

确认：

- 用途；
- 风格；
- 尺度；
- 关键功能；
- 硬约束。

### G1：Concept / Masterplan Draft

确认总体空间关系。

### G2：Digital Masterplan

把设计转换为 JSON / SVG 等结构化几何事实。

### G3：Whitebox / Footprint

只建立地形、水、墙、门、路、桥、建筑 footprint。

**G3 未 PASS，不允许进入正式建筑建模。**

### G4：Hero Asset

单独完成核心建筑资产。

### G5：Spatial Module

完成院落 / 宫区样板。

### G6：Full Assembly

按冻结资产和 Masterplan 总装。

### G7：Landscape / Lookdev

地形、水系、植被、材质、光照深化。

### G8：Independent QA + Owner Review

独立检查后由 Owner 决定最终是否通过。

---

## 4. 为什么“一条超级任务”容易失真

如果同一任务同时要求：

- 完整场景；
- 所有功能存在；
- 高质量建筑；
- 高忠实度；
- 脚本模块化；
- 完整 QA；
- 多张渲染；

Agent 会选择最容易形式化验证的目标优先执行，例如：

- 对象存在；
- 门存在；
- 路能通；
- 脚本能跑；
- 渲染数量达标。

但“设计味道是否忠实”“建筑是否真正独特”更难自动量化，容易被牺牲。

因此每个 Task 应有一个主要 Outcome，而不是同时压入所有阶段目标。

---

## 5. Reconstruction Task 的推荐指令原则

明确告诉 Agent：

1. 哪份输入是 Geometry Authority；
2. 哪份输入是 Topology Authority；
3. 哪些旧坐标不可复用；
4. 哪些设计已冻结；
5. 哪些位置允许最小推断；
6. 不允许为了实现方便规则化重排；
7. 必须生成 Fidelity Report；
8. 必须输出严格正交顶视用于 Overlay Review。

建议增加：

```text
source_reference
confidence
```

记录每个数字化对象的来源和置信度。

---

## 6. 中间检查点比最终长报告更重要

大型 Agent 工作不要只在最后验收。

典型高价值检查点：

```text
参考图
↓
masterplan.svg/json
【Review】
↓
Whitebox Top View
【Review】
↓
单体 Asset Render
【Review】
↓
院落 / 宫区
【Review】
↓
总装
```

越早发现布局错误，返工成本越低。

---

## 7. 设计自主权要显式控制

任务中应明确：

### Frozen

Agent 无权修改。

### Bounded Discretion

Agent 可在明确边界内推断。

### Open Design

Agent 可自由提出方案。

不要同时写：

> “严格忠实设计，不得修改”

又写：

> “所有不明确之处自行决定并完整执行到底”

却不给出什么属于局部、什么属于核心设计。

核心经验：**Agent 自主权必须和阶段目标匹配。**