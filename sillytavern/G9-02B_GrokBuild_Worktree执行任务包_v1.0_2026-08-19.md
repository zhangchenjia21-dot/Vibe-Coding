# TASK｜G9-02B｜长期玩家已知人物目录与人物页面迁移

状态：`CURRENT EXECUTION PACKET`
类型：implementation
执行者：Grok Build
主要 Owner：Runtime Player Knowledge / Product Projection

## 1. Base / Worktree

Repo：

```text
D:\AI\Projects\sillytavern
```

Task Base：

```text
5962e6f5933f245693e090cbdfd2f79791820ef1
```

Task Branch：

```text
agent/g9-02b
```

Agent Worktree：

```text
D:\AI\Projects\.worktrees\sillytavern-agent
```

**禁止直接在原 `D:\AI\Projects\sillytavern` main worktree 修改代码。**

原 main worktree 只允许执行只读 Git 检查和 `worktree add/remove` 管理命令。

---

## 2. Outcome

完成 G9-02B breadth 的核心真实产品结果：

```text
【人物】Surface
从“当前场景人物”
迁移为
“玩家长期已知人物目录”
```

同时保持：

```text
visibleCharacters
= 当前场景可感知 / 可互动人物

knownCharacters（或语义等价字段）
= 玩家长期已知人物
```

两者必须是独立 DTO 集合和独立生命周期。

---

## 3. Authority / Source Manifest

1. 用户当前明确指令
2. `G9-02B_RuntimeDomainBreadth与PlayerKnownDirectory规格_v1.0_2026-08-19.md`
3. `17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`
4. `G9-02BC_SharedRuntimeFoundationConvergence规格_v1.0_2026-08-19.md`
5. `G9-02BC_IndependentReview_共享运行时基础_v1.0_2026-08-19.md`
6. `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`
7. 当前代码：`sillytavern@5962e6f5933f245693e090cbdfd2f79791820ef1`
8. `Skill/main/skill/gpt/agent-task-packet/SKILL.md`

Discussion-only Opening Scenario 不属于本任务 authority。

---

## 4. 启动协议

开始时在原仓库只执行：

```powershell
git -C D:\AI\Projects\sillytavern fetch origin
git -C D:\AI\Projects\sillytavern rev-parse origin/main
git -C D:\AI\Projects\sillytavern status --short
git -C D:\AI\Projects\sillytavern worktree list
git -C D:\AI\Projects\sillytavern branch --list agent/g9-02b
```

必须确认：

```text
origin/main = 5962e6f5933f245693e090cbdfd2f79791820ef1
```

然后创建：

```powershell
git -C D:\AI\Projects\sillytavern worktree add `
  D:\AI\Projects\.worktrees\sillytavern-agent `
  -b agent/g9-02b `
  origin/main
```

从此所有代码编辑、测试、构建、commit 都必须在：

```text
D:\AI\Projects\.worktrees\sillytavern-agent
```

创建后确认：

```powershell
git branch --show-current
git rev-parse HEAD
git status --short
```

Expected：

```text
branch = agent/g9-02b
HEAD = 5962e6f5933f245693e090cbdfd2f79791820ef1
worktree = clean
```

若固定 worktree 或任务分支存在但来源不明，不得 reset/clean/rebase，返回 BLOCKED。

---

## 5. Read First

按顺序：

1. root `AGENTS.md`
2. `src/运行时/AGENTS.md`
3. `src/产品界面/AGENTS.md`
4. `玩家产品界面/AGENTS.md`（若需要改 UI）
5. `tests/AGENTS.md`
6. G9-02B 当前规格
7. `docs/runtime/领域模块与上下文扩展指南.md`
8. `src/运行时/L0_公理层/玩家会话契约.ts`
9. `src/运行时/L1_器件层/玩家会话投影器.ts`
10. `src/产品界面/L3_外交层/玩家产品界面公开契约.ts`

只有证据不足时扩大读取，并在 Final Report 说明原因。

---

## 6. Decision Digest

### INV-B01

```text
Canonical Character Truth
!= Player-known Character Directory
!= Current Scene Visible Character Set
```

### INV-B02

目录不是第二份 Character Truth；每条目录记录必须引用稳定 `characterRef`。

### INV-B03

未认识的 public NPC 不得自动出现在 People Surface。

### INV-B04

合法进入目录的证据至少支持：

- encounter；
- introduction / typed knowledge；
- creation-known。

不得使用名字相似度猜身份。

### INV-B05

角色离场、远行、失联、死亡、当前不可见都不能仅因此删除目录 membership。

### INV-B06

`currentPresence` 必须从当前 Runtime 可见集合派生，不决定 membership。

### INV-B07

`last-known` 只能表达玩家最后合法知道的事实，不得自动跟随 NPC 秘密实时状态。

### INV-B08

People Surface 必须消费独立长期目录 DTO；即时行动和 Narrative 继续消费 `visibleCharacters`。

### INV-B09

不得自动把全部 known characters 注入普通 Turn prompt。完整相关性选择属于 G9-02C。

### INV-B10

不得重新设计 G9-02BC 已冻结的 Domain Host / Formal Change / Save-Restore authority。

如果正确实现必须改变这些 shared contracts，返回 `BLOCKED / ARCHITECTURE QUESTION`。

---

## 7. Required Vertical Cases

必须至少跑通：

1. 开局当前场景可识别 NPC → visible + known + People；零新增模型调用；
2. 玩家离场 → NPC 从 visible 消失，但仍在 People；
3. 再次相遇 → 同一 `characterRef`，不重复 entry；
4. typed 异地身份知识 → 可进入 known directory，但不泄露 live hidden truth；
5. public 但从未认识 NPC → People 不可见；
6. 已知人物死亡/失踪/远行 → membership 不因不可见删除；状态只在玩家合法得知时更新。

---

## 8. Persistence / Recovery

必须证明：

```text
Save-before-knowing
→ 认识 NPC
→ Restore
→ entry/evidence 消失
```

```text
认识 NPC
→ Save-after-knowing
→ 后续变化
→ Restore
→ 恢复当时 dossier/evidence
```

以及：

- Restore 后可形成不同认识历史；
- retry/recovery 不重复 entry/evidence/event/formal turn；
- exactly-once。

---

## 9. Product Migration

Runtime L3 新增独立玩家安全 known-character DTO。

Product：

```text
surface.people
→ known directory
```

如仍存在兼容 `right.people`：也必须迁移到 known directory。

浏览器不得读取 Runtime / SQLite 自行推导。

不做视觉大改。

---

## 10. Non-scope

禁止实现：

- G9-02C 完整模型路由；
- 路由目录大规模预筛最终算法；
- 结果驱动的下游继续处理完整系统；
- People 分类/搜索/筛选/排序；
- Objective Engine；
- external asset-spec；
- Adapter / Compiler；
- Creator；
- Opening Scenario Runtime；
- unrelated UI redesign；
- arbitrary JS/eval/query/statePath。

---

## 11. Validation

建立：

```text
npm run g9:02b:test
```

然后至少运行：

```powershell
npm run g9:02b:test
npm run g9:02bc:test
npm run g9:02a:test
npm run g8:ui-host:test
npm run g8:product-e2e
npm run g5:test
npm run g6:test
npm run g7:test
npm run g8:test
npm test
npm run typecheck
npm run lint
npm run product:build
npm run launcher:smoke
npm run g2:disclosure
git diff --check
```

真实 Provider 调用：`0`。

---

## 12. Git Rules

在 Agent worktree 内只 stage task-owned exact files。

禁止：

```text
git add .
git add -A
git reset --hard
git clean
git stash
git pull
git merge
git rebase
git commit --amend
force push
push origin main
```

建议主要提交：

```text
feat: add persistent player-known character directory
```

Gate 通过后：

```powershell
git push -u origin agent/g9-02b
```

**不要合并 main，不要删除 worktree，不要删除任务分支。**

等待 GPT Independent Review。

---

## 13. Review / Correction Protocol

如果 GPT Review FAIL：

- 保持同一 `agent/g9-02b`；
- 保持同一 worktree；
- 追加 bounded correction commit；
- push 同一分支；
- 返回新的 Final SHA。

禁止新建 fix branch。

如果 GPT Review PASS：

GPT 将先检查 `origin/main` 仍等于 Task Base，并确认 Task Final SHA 可快进；满足后由 GPT 将 `main` 快进到已审核 Final SHA。

只有收到“main 已集成”后，才执行 worktree/branch cleanup。

---

## 14. Cleanup（仅在 GPT 确认 main 已集成后）

```powershell
git -C D:\AI\Projects\sillytavern worktree remove `
  D:\AI\Projects\.worktrees\sillytavern-agent

git -C D:\AI\Projects\sillytavern branch -d agent/g9-02b

git -C D:\AI\Projects\sillytavern push origin --delete agent/g9-02b

git -C D:\AI\Projects\sillytavern worktree prune
```

不要求 Project Owner 手工执行 Git 清理。

---

## 15. Final Report

必须返回：

```markdown
## Result
PASS | PARTIAL | BLOCKED

## Freshness / Worktree
- origin/main
- task branch
- worktree path
- base HEAD
- main worktree untouched

## Player-known Directory
- owner / evidence / membership
- currentPresence
- last-known boundary

## Product Migration
- Runtime DTO
- People Surface
- visibleCharacters separation

## Persistence
- Save-before / after
- Restore / Branch
- Recovery exactly-once

## Safety
- unseen NPC
- hidden identity/location/status
- no second Character truth
- no prompt auto-growth

## Regression
- commands + results

## Git
- base SHA
- final SHA
- task branch push
- main NOT modified

## Remaining
- G9-02C model-first routing
- routing working-set pruning
- outcome-gated continuation
- People scale stress
```

最终成功状态只能写：

```text
G9-02B IMPLEMENTATION READY FOR INDEPENDENT REVIEW
```

不得自行宣布 G9-02B CLOSED / G9-02C ACTIVE / G9-03 AUTHORIZED。
