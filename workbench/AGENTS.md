# AGENTS.md｜Personal Workbench 治理目录读写协议

本文件适用于 `Vibe-Coding/workbench/`。

## 1. 最小读取顺序

```text
Vibe-Coding/AGENTS.md
→ workbench/README.md
→ workbench/current/README.md
→ 与当前问题直接相关的 current Owner
→ 必要的 discussion / research / architecture / decision
→ 需要跨项目执行方法时读取 Vibe-Coding/skill/ 下 current Skill
→ 实现问题再去 zhangchenjia21-dot/Workbench
```

禁止用“把整个 Vibe-Coding 或 Workbench 全读一遍”替代 Owner 定位。

## 2. Canonical Owner

- 项目基础定位、Authority 与协作模型：`current/项目总纲.md`
- Current Stage / Goal / Blocker / Gate / Active Work / Next Action：`current/项目状态.md`
- Current Product Definition：`current/产品定义.md`
- Frozen Roadmap / Task Axis：`current/开发路线.md`
- Frozen V0 Architecture：`architecture/架构方案.md`
- 未经 Owner 审核的 Product / Architecture / Roadmap / Task Axis 提案：默认只存在于聊天；只有 Owner 明确要求保存草案时才进入 `discussion/`
- 项目专属 Reference Audit / 技术研究 / Spike：`research/`
- Owner 正式裁定：`decisions/`
- 代码 / 测试 / runtime / CI / repository-native executable Task / implementation Review：`zhangchenjia21-dot/Workbench`
- superseded / HOLD / closed process evidence：仓库根 `99_归档/workbench/`
- 跨项目可复用方法：仓库根 `skill/`

## 3. 00–05 聊天协作路由

```text
00｜项目总控
→ 维护 Stage / Gate / Current Status / 跨域冲突 / Decision Propagation
→ 只有 00 可以正式记录 Stage Gate PASS / FAIL / NEXT
→ 主要写 current/项目状态.md 与已批准的 decisions

01｜产品定义
→ 形成 / 重新评估 Primary Purpose / Core Value / Core Journey / Scope / Success Criteria
→ 默认先在聊天中与 Owner 讨论
→ Owner 明确批准后，才可写入 / 更新 current Product Definition Owner

02｜参考审计与技术研究
→ 提供 research / evidence / feasibility / spike 结果
→ 研究结论不得自动升级为 Product / Architecture decision
→ 只有任务本身明确授权保存研究证据时，才可直接写 research/

03｜架构与开发路线
→ 形成 Architecture Questions、Draft / Revised Task Axis 与架构提案
→ 默认先在聊天中向 Owner 展示：发现、选项、推荐、风险、待裁定点
→ 未经 Owner 明确批准，不得把新路线 / 新架构写入 current/、architecture/、decisions/
→ 未经 Owner 明确批准，不得把新的路线 / 架构判断标为 DECIDED
→ Route Freeze 后进入 maintenance：implementation 若发现 frozen contract 需要重大修改，先回 Owner Discussion，再由 03 形成 change proposal，最后回 00 Gate

04｜Agent 任务调度
→ 只有前置 Gate 满足后生成正式 executable work
→ 当前已授权的首个 production target 是 TA-1A First Usable Core Vertical
→ repository-native Task Packet 与实现事实优先进入 zhangchenjia21-dot/Workbench
→ 不得把 TA-1B / Future Scope 偷渡进 TA-1A Task

05｜代码审核与 UAT
→ 从 implementation repository 读取 diff / tests / CI / runtime evidence
→ Review 结论属于实现证据；只有长期生效的产品/架构/路线变化才传播回本目录
→ TA-1A 必须完成 Independent Review + Reality Gate + Owner real-data UAT
→ Stage Exit / Gate 结论仍回 00 + Owner
```

## 4. Cross-chat Routing Prompt Protocol

当 00 或其它专业聊天需要把工作交给另一个已经完成初始化的专业聊天时，**默认使用短指令，只传递增量，不重复项目背景**。

专业聊天应主动读取 GitHub current source，而不是依赖上游 Prompt 复制项目事实。

标准交接指令只需包含：

```text
1. 当前要做什么 / Goal
2. 从哪里读 current input
3. 本轮必须交付什么
4. 关键限制 / 禁止事项
5. 完成后回到哪里 / Return condition
```

默认不要重复：

- 已经存在于 `current/` 的 Product / Architecture / Roadmap 全文；
- 已经存在于 `AGENTS.md` 的 Authority / Promotion / Gate 规则；
- 已经存在于专业聊天初始化 Prompt 的角色职责；
- 大段历史背景和已归档路线；
- 专业聊天能够通过 current source 自行解析的信息。

只有在以下情况才允许长指令：

- 新聊天尚未初始化；
- current source 不足以表达一次性特殊约束；
- 跨项目 / 跨仓库任务存在容易误解的临时上下文；
- 用户明确要求生成完整 Task / Prompt Artifact。

原则：

> **GitHub 保存项目事实，Prompt 只传递本轮增量。**

> **不要把交接 Prompt 变成第二份 Product / Architecture specification。**

## 5. Owner Discussion & Promotion Gate

凡是会新增或改变以下任一事实：

- Product Definition / Scope；
- Architecture / contract / ownership / state model；
- Roadmap / Task Axis / stage ordering；
- Stage Gate / Route Freeze；
- implementation authorization；
- 其它会被后续任务依赖的重大项目级决定；

默认流程必须是：

```text
专业聊天形成 Working Draft
→ 在聊天中向 Owner 展示与讨论
→ Owner 明确：批准 / 修改 / 延后 / 否决
→ 获得明确批准后才允许 GitHub Promotion
→ 如涉及 Gate，再由 00 独立做 Gate Review
```

### 明确规则

- **Owner 沉默、未反对、离开聊天、或“任务已生成”均不构成批准。**
- 标记 `DRAFT` / `EXPLORATION` **不等于获得了 GitHub 写入许可**。
- 未经 Owner 审核的新结论只能使用 `PROPOSED` / `HYPOTHESIS` / `OPEN` / `DEFERRED` / `HOLD` 等状态，不得使用 `DECIDED` 表示已经生效。
- 专业聊天框可以自行完成分析，但不能自行完成“决策升级”。
- 专业聊天框可以说 `READY FOR OWNER REVIEW`；在 Owner 审核前不得说“已完成并进入下一 Gate”。
- 只有 00 可以正式记录 `Gx.y = PASS / FAIL / NEXT`；需要 Owner Approval 的 Gate，00 也不得从沉默推定批准。

### 非 Authority 草案持久化

默认草案留在聊天中。

只有 Owner 明确要求保存草案时，才写入 `discussion/`。其中内容：

- 不是 current Owner；
- 不是 Architecture Authority；
- 不是 Roadmap；
- 不得触发下游任务；
- 不得作为 implementation authorization。

### 允许直接写入的窄例外

1. 00 对**已经明确批准的决定**做机械 Decision Propagation / Status 更新；
2. 用户在当前任务中已明确要求“写入 / 保存 / 更新 GitHub”；
3. 02 被明确要求持久化客观 research / spike evidence，但 evidence 仍不能自动升级为 decision；
4. Gate 已满足后，04 / 05 按已经批准的 Task / Review protocol 写 implementation evidence。

## 6. 当前 Route Freeze / Implementation 约束

当前状态：

```text
G0.1 = PASS
G0.2 = PASS
G0.3 = PASS
G0.4 = PASS
G0.5 = PASS
G0.6 Route Freeze = PASS
Implementation = AUTHORIZED FOR TA-1A DISPATCH
```

当前 frozen architecture / route 以：

- `current/产品定义.md`
- `current/开发路线.md`
- `architecture/架构方案.md`
- `decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md`

为准。

正式实现期规则：

- 04 只能按 frozen Route 为 TA-1A 生成 executable work；
- implementation Agent 不得自行改变 Electron / SQLite / identity / date / recurrence / backup / restore / migration 等 frozen contracts；
- 具体 SQLite binding / ORM、UI component library、字段级 production DDL 等未冻结 implementation detail，可以在不改变 frozen contract 的前提下由 Task/Agent 选择并接受 Review；
- TA-1B 不因 Route Freeze 自动授权，必须等待 TA-1A Engineering / Independent Review / Owner real-data UAT Stage Exit；
- Owner Windows notification-area / tray 鼠标 focused check 已后移为 TA-1A Exit 前必须完成的 residual；
- Future Milestones / AI / Sync / Custom / Plugin / SDK / Knowledge / Habit / Finance / Health / AI Collaboration 等继续 Deferred；
- 如果实现证据证明 frozen contract 本身错误，不得静默修补路线，必须回到 03 + Owner + 00 做显式 change / Gate。

正式推进继续遵循仓库 current `skill/gpt/lifecycle-dev-process/SKILL.md` 与适用 Task Packet Skill。

## 7. Authority 与冲突

项目内默认顺序：

1. 用户当前明确指令；
2. `workbench/` 下最新、**已经 Owner 审核 / 批准 / Freeze** 的 Product / Architecture / Roadmap / Decision；
3. `zhangchenjia21-dot/Workbench` 可验证的代码、测试、运行与 Git 事实；
4. Vibe-Coding current governance / Lifecycle；
5. Vibe-Coding current Skill；
6. `discussion/`、`research/`、`99_归档/workbench/`、项目经验、历史聊天、Legacy Reference；
7. Agent 一般经验与模型记忆。

归档资料永远不能因为“以前曾 PASS / DECIDED”而自动恢复 current authority。

implementation fact 与 frozen contract 冲突时，不能让代码事实静默改写 contract：应先确认是实现 bug 还是 frozen contract 被现实反证；后者必须回 03 / Owner / 00。

两个 current source 实质冲突时，不得静默拼接第三套方案；先按 authority / status / supersedes 解决，仍无法解决则回到 00 提交 Owner 裁定。

## 8. 写入链

```text
Freshness
→ 定位事实类型与唯一 canonical Owner
→ 判断是否触发 Owner Discussion & Promotion Gate
→ 若触发：先在聊天中讨论，等待 Owner 明确裁定
→ 获批后再写入 / 更新唯一 Owner
→ 检查是否影响 Stage Gate / Task Axis / 已发任务 / UAT
→ 必要时 Decision Propagation
→ pre-push HEAD revalidation
→ 集中汇报
```

Current Owner 使用稳定语义文件名并原位更新。演变依靠 Git history 或 `99_归档/workbench/`，不要通过 `_CURRENT_2`、`FINAL_v2`、`最新版` 制造并行权威。
