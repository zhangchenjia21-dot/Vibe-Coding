---
title: G9-05H｜Owner First Playable UAT 准备规格
status: current-spec-frozen
version: 1.0
updated: 2026-08-22
---

# G9-05H｜Owner First Playable UAT 准备规格 v1.0

## 0. Product Outcome

让 Project Owner 不需要理解历史 G5/G9 测试脚手架，就能在本机完成：

```text
准备真实资产
→ 启动正式 Player Product UI
→ 【我的资产库】看到真实三国资产
→ 【使用我的资产】创建游戏
→ 进入真实 AI Session
→ 连续游玩
→ Save / Continue / Restore
→ 关闭并再次启动后继续
→ 提交 Owner UAT 反馈
```

本阶段不要求自动化替 Owner 判定“好玩”；只负责把真实试玩入口准备到足够低摩擦、可重复、不会破坏已有存档。

Formal Code Base：

```text
zhangchenjia21-dot/sillytavern
main@a97b4bae6a3bd9308ecb8c092b96bce81dd43700
```

真实资产基线继续使用 G9-05G frozen exact blobs：

```text
World:
世界包/汉末三国_天下未定_World_Pack_v0.2.3.md
blob 0c27b7f6d252d8970191784eb930ca722f77d01e

Character:
人物卡/汉末三国/CC-BATCH-01/刘备__Character_Card__v0.1.2.md
blob aa6fc6b1633f9cdaa4d0effd62986167369a3dd2

Expansion:
拓展包/通用拓展包/人物能力与技艺_Expansion_Pack_v0.1.5.md
blob b165ddbd927eacc012f2edf5f8d81ad73d6e64a2
```

Owner 最终应选择 production revisions：

```text
World 0.2.3
刘备 Character 0.1.3
EP-CHAR-CORE 0.1.6
```

---

## 1. Current Friction To Remove

当前正式 launcher / local server 仍有历史 UAT 脚手架：

1. launcher 要求 `g5-playtest.sqlite` 预先存在；
2. 用户需要知道并手工运行 `npm run g5:uat:reset`；
3. Source Asset Library 与 `sillytavern-assets` 仓库分离，真实三国资产不会自动出现在【我的资产库】；
4. G9-05G 的 real-assets Gate 只验证，不向 Owner 的 SQLite Source Library 安装资产；
5. 真实游戏需要 DeepSeek Provider；无 Key 时应用可启动，但不能完成真实 AI 游玩。

G9-05H 只消除这些首次试玩摩擦，不重写整个 Bootstrap 架构。

---

## 2. Owner UAT Preparation Command

新增一个明确的 Owner UAT 准备入口，例如：

```text
npm run owner:uat:prepare -- <assets-root>
```

或等价的 Windows 友好脚本。

必须满足：

### 2.1 Safe DB bootstrap

- 使用当前正式本地产品实际读取的 SQLite 数据库；
- 数据库不存在时，可创建必要 schema / compatibility anchor；
- 数据库已经存在时，绝不 reset、rm、覆盖已有游戏、Save、Creator Draft 或 Source Asset；
- 不要求 Project Owner 手工运行历史 `g5:uat:reset`；
- 可复用既有 G5 bootstrap helper，但必须有“only-if-missing”安全包装，不得每次准备都 destructive reset。

### 2.2 Exact real-asset verification

- `<assets-root>` 缺失 → 明确 BLOCKED / NOT READY；
- exact source 文件缺失或 blob 不符 → FAIL CLOSED；
- 不允许 synthetic fallback；
- blob 计算逻辑应复用/抽取 G9-05G real-assets Gate，而不是复制第二套 pinning truth。

### 2.3 Formal Source installation

把真实资产安装到同一 SQLite `SourceAssetLibraryStore`：

```text
World manuscript
→ existing G9-04 Adapter
→ World 0.2.3 Source

刘备 0.1.2
→ existing G9-04 Adapter
→ formal Creator source_revision
→ 刘备 0.1.3

EP-CHAR-CORE 0.1.5
→ existing G9-04 Adapter
→ formal Expansion Creator/Core source_revision
→ EP-CHAR-CORE 0.1.6 production binding
```

禁止：

- 直接手写 final `TavernAssetV1` 绕过已有 adapter/revision contract；
- 直接改写历史 0.1.2 / 0.1.5；
- 把 G9-04 proof module 放回 production Host；
- 调用外部 Provider 来 seed 资产。

### 2.4 Idempotency

重复执行 prepare 必须：

```text
同 exact snapshot 已存在
→ no-op / replay
→ 不重复 Source
→ 不改变 digest
→ 不改变已有游戏
→ 不改变已有 Save
```

同 assetRef + version 不同 hash 必须 fail closed。

---

## 3. Owner UAT Preflight

新增只读 preflight，例如：

```text
npm run owner:uat:preflight -- <assets-root>
```

输出至少包括：

```text
node/npm dependencies
SQLite health
Player Product launcher prerequisites
DeepSeek API configuration PRESENT / ABSENT
World 0.2.3 exact Source present
刘备 0.1.3 exact Source present
EP-CHAR-CORE 0.1.6 exact Source present
production Host contains builtin:character-capability.evidence.v1
production Host does not contain G9-04 proof ref
```

不输出 API Key、玩家文本、Provider 原始响应或 hidden state。

若 Key 缺失：

- prepare 仍可完成；
- preflight 必须明确提示“可启动产品，但真实 AI 游玩尚未 READY”；
- 用户可以经现有 API Settings 页面配置，按产品提示重启；
- Agent 自动测试不得为了 preflight 发起真实 Provider call。

---

## 4. Launch Experience

优先复用现有正式 launcher：

```text
scripts/start-playtest.ps1
→ backend
→ 玩家产品界面
```

G9-05H 可以增加一个薄的 Owner UAT wrapper / `.bat`，但不得复制第二套 server/bootstrap。

理想 Owner 操作：

```text
首次：Owner UAT Prepare
→ 启动酒馆游戏
→ 浏览器打开

以后：直接启动酒馆游戏
```

现有 G5 test game 若仍作为内部 compatibility anchor 存在，不是本阶段必须清除的架构债；但不得阻断 Owner 从【我的资产库】创建新游戏。若 UI 把测试游戏呈现得足以造成明显误导，可做最小标识/过滤，不得因此重写 Runtime Directory。

---

## 5. Required Owner Play Path

准备完成后，UAT 文档必须要求 Owner 亲手完成：

1. 打开正式 Player Product；
2. 检查 API Settings，确认真实 DeepSeek 已配置；
3. 进入【我的资产库】；
4. 确认能看到 exact：
   - World 0.2.3；
   - 刘备 0.1.3；
   - EP-CHAR-CORE 0.1.6；
5. 进入【使用我的资产】；
6. 选择：
   - World = 汉末三国；
   - Player Character = 刘备；
   - Expansion = EP-CHAR-CORE；
   - `feature:character-capability` enabled；
   - `module:character-capability.evidence` enabled；
7. 创建游戏并进入 Session；
8. 至少进行若干真实自由输入回合，不使用 fixture mode；
9. 创建 Save；
10. Continue；
11. Restore；
12. 关闭应用；
13. 再次启动；
14. 从 Game Library 继续同一游戏；
15. 记录产品问题。

本规格不规定必须玩固定 N 回合才能“好玩”；但至少应覆盖正常行动、一次信息询问/观察、一次与能力证据相关的行动或判断，以触发真实 EP-CHAR routing/projection 的机会。

---

## 6. UAT Feedback Categories

提供简单记录模板，不要求用户写技术诊断：

```text
启动 / 配置
我的资产库
资产选择 / 建局
开场
输入与响应速度
叙事质量
玩家行动自由度
角色一致性
世界一致性
Expansion 行为是否有感
错误 / 卡死 / 重试
Save / Continue / Restore
关闭后再启动
整体“像不像游戏”
```

Owner 只需描述现象、期望和严重程度；技术根因由 GPT Independent Review / Agent 后续处理。

---

## 7. Stage Gate

Engineering Gate：

```text
G9-05H implementation PASS
= prepare / preflight / asset installation / launcher wrapper / offline regression 成立
```

但阶段最终状态仍是：

```text
READY FOR OWNER UAT
```

只有 Project Owner 亲手试玩后才能给：

```text
OWNER UAT PASS
或
OWNER UAT BLOCKED / NEEDS FIXES
```

Library Product、G10 Provider Expansion、Release 均不因 Engineering PASS 自动授权。

---

## 8. Correction Budget

同一 Owner-UAT 根因链继续遵守：

```text
correction-01 = focused fix
correction-02 = root-neighbor / crash-window / sibling-boundary review
correction-02 后仍失败 = STOP patching → redesign seam
```
