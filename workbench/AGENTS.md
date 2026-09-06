# AGENTS.md｜Personal Workbench 治理目录读写协议

本文件适用于 `Vibe-Coding/workbench/`。

## 1. 最小读取顺序

```text
Vibe-Coding/AGENTS.md
→ workbench/README.md
→ workbench/current/README.md
→ 与当前问题直接相关的 current Owner
→ 必要的 research / architecture / decision
→ 需要跨项目执行方法时读取 Vibe-Coding/skill/ 下 current Skill
→ 实现问题再去 zhangchenjia21-dot/Workbench
```

禁止用“把整个 Vibe-Coding 或 Workbench 全读一遍”替代 Owner 定位。

## 2. Canonical Owner

- 项目基础定位、Authority、协作模型与 Stage 0 约束：`current/项目总纲.md`
- Current Stage / Goal / Blocker / Gate / Active Work / Next Action：`current/项目状态.md`
- 后续经 Owner 审核的 Product Definition：在 `current/` 建立稳定语义 Owner，并由 `current/README.md` 路由
- 后续经 Owner 审核的当前 Roadmap / Task Axis：在 `current/` 建立稳定语义 Owner，并由 `current/README.md` 路由
- 项目专属 Reference Audit / 技术研究 / Spike：`research/`
- 跨阶段长期有效的架构约束：`architecture/`
- Owner 正式裁定：`decisions/`
- 代码 / 测试 / runtime / CI / repository-native executable Task / implementation Review：`zhangchenjia21-dot/Workbench`
- superseded / closed process evidence：仓库根 `99_归档/workbench/`
- 跨项目可复用方法：仓库根 `skill/`

## 3. 00–05 聊天协作路由

```text
00｜项目总控
→ 维护 Stage / Gate / Current Status / 跨域冲突 / Decision Propagation
→ 主要写 current/项目状态.md 与已批准的 decisions

01｜产品定义
→ 形成 Primary Purpose / Core Value / Core Journey / Scope / Success Criteria
→ Owner 审核后写入 current 下唯一 Product Definition Owner

02｜参考审计与技术研究
→ 写 research/ 下项目专属证据
→ 研究结论不得自动升级为 Product / Architecture decision

03｜架构与开发路线
→ 维护 Draft / Revised Task Axis、Architecture Questions 与后续已批准架构
→ Route Freeze 前不得把 draft 写成 production commitment

04｜Agent 任务调度
→ 只有前置 Gate 满足后生成正式 executable work
→ repository-native Task Packet 与实现事实优先进入 zhangchenjia21-dot/Workbench

05｜代码审核与 UAT
→ 从 implementation repository 读取 diff / tests / CI / runtime evidence
→ Review 结论属于实现证据；只有长期生效的产品/架构/路线变化才传播回本目录
```

## 4. Stage Gate 约束

当前为 **Stage 0｜Pre-Implementation Alignment**。

除明确标记为 `EXPLORATION / SPIKE / PROTOTYPE` 的低成本探索外，在 Route Freeze / Product Definition Gate PASS 前：

- 不授权正式大规模编码；
- 不把框架、Harness、Agent、协议或技术栈假设写成冻结架构；
- 不假定 Workbench 已具备未来设计中的 Agent 调度、日志、自动 Review 等能力；
- 不因为历史聊天中曾讨论过某方案，就把它视为 current decision。

正式推进遵循仓库 current `skill/gpt/lifecycle-dev-process/SKILL.md`。

## 5. Authority 与冲突

项目内默认顺序：

1. 用户当前明确指令；
2. `workbench/` 下最新、已批准的 Product / Architecture / Roadmap / Decision；
3. `zhangchenjia21-dot/Workbench` 可验证的代码、测试、运行与 Git 事实；
4. Vibe-Coding current governance / Lifecycle；
5. Vibe-Coding current Skill；
6. `research/`、项目经验、历史聊天、Legacy Reference；
7. Agent 一般经验与模型记忆。

两个 current source 实质冲突时，不得静默拼接第三套方案；先按 authority / status / supersedes 解决，仍无法解决则回到 00 提交 Owner 裁定。

## 6. 写入链

```text
Freshness
→ 定位唯一 canonical Owner
→ 判断内容是 current / research / architecture / decision / implementation fact
→ 写入或更新唯一 Owner
→ 检查是否影响 Stage Gate / Task Axis / 已发任务 / UAT
→ 必要时 Decision Propagation
→ pre-push HEAD revalidation
→ 集中汇报
```

Current Owner 使用稳定语义文件名并原位更新。演变依靠 Git history 或 `99_归档/workbench/`，不要通过 `_CURRENT_2`、`FINAL_v2`、`最新版` 制造并行权威。
