# Blender QA 与独立验收

状态：current

## 1. Builder Claim != Independent Evidence

Agent 生成的：

- DESIGN_REPORT；
- CONNECTIVITY；
- manifest；
- PASS / DONE 声明；

只能视为 Builder Claim。

正式验收需要核对实际：

- `.blend`；
- 渲染；
- 脚本；
- 坐标；
- 几何；
- 连通性；
- 是否存在与参考设计的偏差。

原则：

> **报告说“存在”不等于场景里真的存在；报告说“连通”不等于几何上真的可通过。**

---

## 2. 建议 QA 分层

### A. 文件 / 工程层

检查：

- `.blend` 可重新打开；
- 脚本可重复执行；
- Collection / 对象命名清楚；
- 材质、相机、灯光没有丢失；
- 不依赖不可追踪的手工临时状态。

### B. Masterplan 层

检查：

- 总体边界；
- 中轴；
- 分区；
- 建筑 footprint；
- 墙；
- 门；
- 道路；
- 水域；
- 桥；
- 地形关系。

推荐使用严格正交顶视 + Overlay。

### C. Geometry 层

检查：

- 穿模；
- 悬空；
- 地面穿透；
- 门洞是否被墙或物体封闭；
- 桥是否真的跨水；
- 道路是否中断；
- 楼梯 / 坡道是否可用。

### D. Visual / Design 层

检查：

- 单体是否符合设计；
- 建筑等级是否清晰；
- 建筑是否过度同质化；
- 院落比例是否成立；
- 天际线是否符合意图；
- 局部设计是否被程序化简化。

---

## 3. 高价值自动化检查

Blender Python 可考虑建立通用 QA 组件。

### Ray Cast / Clearance

用于检查：

- 门洞净空；
- 人体通行空间；
- 道路上方障碍。

### Route Sampling

沿道路中心线连续采样：

- 是否存在地面；
- 是否突然落水；
- 是否被墙切断；
- 是否发生高度突变。

### Bridge-over-Water Check

检查桥梁：

- 是否跨越水 polygon；
- 两端是否真正接到道路 / 岸线。

### Gate Opening Check

检查：

- 墙体是否有真实开口；
- 开口宽高是否满足设定；
- 前后是否有可达空间。

### Reopen Validation

保存 `.blend` 后重新打开并再次运行关键检查，避免只验证内存中的临时状态。

---

## 4. Fidelity Report

对于 Reconstruction Task，建议单独输出：

`FIDELITY_REPORT.md`

至少检查：

```text
总体边界
中轴
墙体
门
核心建筑
功能区
水系
道路
桥梁
地形
```

每项标记：

- MATCH；
- MINOR DEVIATION；
- MAJOR DEVIATION。

禁止把明显不同描述成 MATCH。

---

## 5. 渲染视角不能只追求“好看”

正式 QA 至少应有：

### 严格顶视正交

用于总平面检查。

### 关键轴线

用于视线、门、道路和层级检查。

### 各主要宫区 / 街区鸟瞰

用于局部密度和组合关系检查。

### Hero Asset 近景

用于单体质量检查。

### 远景 / Skyline

用于整体尺度和天际线检查。

艺术渲染不能替代技术检查视图。

---

## 6. Owner UAT 与工程 QA 的边界

Agent 可以验证：

- 脚本；
- 文件；
- 几何；
- 连通；
- 渲染输出。

但“这个建筑是不是我想要的”“整体空间有没有美感”“是否值得作为最终设计”属于 Product / Design Acceptance，需要 Owner 判断。

因此：

```text
Engineering PASS
!=
Design PASS
!=
Owner PASS
```

三者应分开记录。

---

## 7. 推荐验收顺序

```text
Builder 自检
↓
自动 Geometry QA
↓
Independent Review
↓
严格顶视 / Overlay Fidelity Review
↓
设计质量 Review
↓
Owner UAT / Design Approval
↓
Freeze
```

只有通过对应 Stage Gate 后，成果才应成为后续阶段的 Authority。