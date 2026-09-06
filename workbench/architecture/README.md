# Personal Workbench｜Architecture

本目录保存 **跨阶段长期有效的 Workbench 架构约束、contract、ownership/state model 与关键设计裁定**。

当前项目仍处于 Stage 0，因此本目录暂不预置具体技术架构。

## 写入条件

只有满足以下之一时才新增架构文档：

- 已有足够 Product / Reference evidence，需要固定高沉没成本设计边界；
- 某 contract / state model / ownership 会被多个后续任务依赖；
- 需要独立演化且放入 current 核心文件会明显污染主线；
- Owner 已批准将某 Draft Architecture 提升为长期约束。

Route Freeze 前的探索必须明确标记：

```text
DRAFT / EXPLORATION
NOT ROUTE-FROZEN
NOT PRODUCTION COMMITMENT
```

## Canonical rule

- Product / Roadmap 当前状态仍由 `../current/` 拥有；
- Reference evidence 由 `../research/` 拥有；
- Owner 正式裁定由 `../decisions/` 拥有；
- 代码与 runtime implementation fact 由 `zhangchenjia21-dot/Workbench` 拥有。

不要为了“以后可能复用”在真实 consumer 出现前提前冻结大型 platform / protocol / plugin architecture。
