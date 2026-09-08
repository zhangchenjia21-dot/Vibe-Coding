# 设计图到数字 Masterplan

状态：current

## 1. 核心认识

AI 生成的概念图，即使带有：

- 比例尺；
- 网格；
- 图例；
- 编号；
- 宫墙；
- 建筑标签；

也不等于工程意义上的 Blueprint。

位图主要表达的是**语义和视觉关系**，而不是机器可直接执行的精确几何。

一张“看起来像施工图”的 PNG，如果没有明确记录坐标、轮廓和尺寸，Agent 仍然需要自己推断，因此很容易进行二次设计。

---

## 2. 图片可以表达什么

图片擅长表达：

- 哪个区域在东、西、南、北；
- 主次关系；
- 大体规模；
- 风格；
- 轴线；
- 建筑密度；
- 水、墙、园林之间的视觉关系。

图片不天然提供：

- 精确 XY 坐标；
- footprint polygon；
- 建筑长宽；
- 旋转角；
- 院墙节点；
- 水岸控制点；
- 道路中心线与宽度；
- 桥梁跨度；
- 地形高程；
- 建筑间距。

因此：

> **要求 Agent“忠实复现”时，PNG 不应成为唯一事实源。**

---

## 3. Topology 与 Geometry 分开管理

### Topology / 拓扑

回答：

- 哪个门连接哪个区域？
- 哪条道路通向哪里？
- 哪些节点必须互通？

例如：

```text
G01 → G02 → G03 → 外朝 → G04 → 内廷 → G05 → 御苑
```

### Geometry / 几何

回答：

- G04 在哪里？
- 宫墙沿哪些点折转？
- 道路宽多少？
- 建筑 footprint 多大？
- 水岸曲线在哪里？

交通图可以冻结 Topology，但不能替代 Geometry。

---

## 4. Digital Masterplan

大型场景在正式建模前，应建立结构化总平面。

推荐至少输出：

```text
masterplan.json
masterplan.svg
```

后续需要时可扩展：

- DXF；
- GeoJSON；
- 其它 CAD / GIS 兼容格式。

### 建筑示例

```json
{
  "id": "H001",
  "name": "主殿",
  "type": "great_hall",
  "x": 0,
  "y": 510,
  "width": 142,
  "depth": 72,
  "rotation": 0,
  "level": "L1",
  "source_reference": "02",
  "confidence": "high"
}
```

### 宫墙示例

```json
{
  "id": "W_CORE_E01",
  "type": "ceremonial_wall",
  "points": [[195, 300], [195, 720]],
  "height": 14
}
```

建议至少包含：

```text
site
terrain
zones
walls
gates
buildings
courtyards
roads
bridges
water
garden_zones
landmarks
```

---

## 5. Masterplan 的 Authority

一旦 Owner / Independent Review 确认 Digital Masterplan：

> **后续 Agent 不得为了程序实现方便、视觉美化或个人判断静默重新布局。**

若需要调整，必须回到 Masterplan 层显式修改，再向下传播。

正确关系：

```text
Approved Design
→ Digital Masterplan
→ Freeze
→ Blender / 目标平台读取
```

而不是：

```text
PNG
→ 每个 Agent 各自理解
→ 每个 Agent 各自生成一套坐标
```

---

## 6. Footprint Stage Gate

Digital Masterplan 完成后，先生成 Whitebox：

- 建筑 = 0.3–1m 高的 footprint proxy；
- 墙 = 简化墙体；
- 门 = 清晰开口；
- 道路 / 广场 / 水 / 桥 = 实际位置；
- 相机 = 严格正交顶视。

然后做：

- 与参考设计并排；
- 半透明 Overlay；
- 坐标和尺寸检查；
- 分区检查。

只有总平面 PASS，才进入正式建筑建模。

---

## 7. 大型总图必须配套局部图

将城市级 / 宫殿级设计压缩进一张有限分辨率 PNG 后，宏观结构通常可辨，但局部会丢失：

- 小建筑轮廓；
- 门洞；
- 回廊；
- 院墙折线；
- 水岸；
- 建筑间距。

单纯裁剪、放大总图不会产生新的信息。

因此推荐三层图纸制度：

### L0 总体图

负责总体轴线、分区、地形、水系和大边界。

### L1 分区局部图

针对重点宫区 / 街区重新高分辨率设计，而不是简单裁图。

### L2 单体 / 院落图

提供单体平面、立面、侧面、必要剖面和关键尺寸。

原则：

> **总图告诉 Agent“在哪里”，局部图告诉 Agent“这一片如何组织”，单体图告诉 Agent“这个资产本身怎么做”。**