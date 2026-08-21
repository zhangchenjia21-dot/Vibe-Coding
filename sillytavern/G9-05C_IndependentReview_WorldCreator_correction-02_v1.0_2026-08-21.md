---
title: G9-05C World Creator Independent Review correction-02
status: current-review-correction-required
version: 1.0
date: 2026-08-21
stage: G9-05
slice: G9-05C
---

# G9-05C｜World Creator｜Independent Review correction-02

## 1. 审核对象

```text
Formal Code Base
25286b2517cb26520109e3d8738671e53d88c861

Grok active packet
2ef03f745dcde3682b9fdc0a158e29bf200301dd

Reviewed Implementation
19f97f6210c1a5165c5e09a192f0bce7ee936983

Task Branch
agent/g9-05c-world-creator
= 19f97f6210c1a5165c5e09a192f0bce7ee936983

origin/main during review
25286b2517cb26520109e3d8738671e53d88c861
```

分支仍是 `main` 的严格前向后代；Grok packet 后只有一个实现提交。GitHub 当前没有该 SHA 的 CI status，因此本审核以 exact diff、当前代码、直接测试与冻结规格交叉核对为依据。

## 2. 结论

```text
P0 = 0
P1 = 2

P1-01 advanced composition/dependency editor = PASS
P1-02 import continuation section locator    = PARTIAL / FAIL
P1-03 existing sectionRef identity UX        = PASS
P1-04 World dependency semantic gate         = PARTIAL / EVIDENCE INCOMPLETE

G9-05C Implementation = CORRECTION-02 REQUIRED
G9-05C CLOSED = NO
Character Creator AUTHORIZED = NO
main = unchanged at 25286b2517cb26520109e3d8738671e53d88c861
```

这不是重做 correction-01。Grok 已把主体产品收口完成，correction-02 只剩一个真实 UI 定位 bug 与一个 G9-03 catalog 兼容性 Gate 证据缺口。

## 3. 已关闭项

### 3.1 P1-01 PASS

World composition 已支持 create/edit/delete，并允许：

```text
compositionRef
target.assetRef
target.assetType = world | character | expansion | library
disposition = default | recommended | optional
```

existing edit 保留 `nodeRef`；新增项继续由 Product/Core 生成 node identity。

World dependency 已支持 create/edit/delete，World-specific kind 收紧为：

```text
hard | optional | reference
```

并支持 target type 与 `requiredCapabilityRefs`；失败时新增表单只在成功拿到新 workspace 后 reset。

### 3.2 P1-03 PASS

existing section 的 `sectionRef` 已改成只读“章节身份”；保存继续使用原 semantic ref，`sectionKind/title/body/visibility` 可编辑。新章节仍可输入 `sectionRef`。没有修改 G9-05B identity contract。

### 3.3 P1-04 主体实现已成立

World HTTP/request gate 已拒绝：

```text
feature_conditional
sourceScope
```

World UI 不再暴露 conditional；DeepSeek World authoring schema 只允许：

```text
hard | optional | reference
```

且不要求 `sourceScope`。DeepSeek adapter 对恶意/越界 conditional operation 做额外 Program-side 丢弃，generic G9-05B union 与 G9-03 protocol 未修改。

## 4. P1-02｜章节 continuation locator 仍不符合 formal target 编码

G9-05B import mapping 对章节目标的 canonical label 是：

```text
section:${assignment.target.sectionRef}
```

例如真实 `sectionRef = section:overview` 时，formal target 为：

```text
section:section:overview
```

当前 UI `parseFormalLocateTarget()` 对任何 `section:` 字符串直接把**整段 target**当成 `sectionRef`。因此：

```text
target = section:section:overview
→ parsed sectionRef = section:section:overview
→ DOM id = world-creator-section-section:section:overview

实际章节 sectionRef = section:overview
→ DOM id = world-creator-section-section:overview
```

两者不一致，按钮会显示，但无法定位真实章节。

当前 UI 测试只用了 `target = section:overview`，因此没有覆盖冻结 correction packet 要求的 `section:<sectionRef>` formal encoding。

### Correction 要求

定位必须基于当前 workspace 中**真实存在的 section identity**，不得仅做字符串前缀判断。

推荐确定性解析：

1. metadata 三个 exact target 直接定位；
2. 对 section target：
   - 若 `view.sections` 中存在 `sectionRef === target`，使用 exact target；
   - 否则若 target 以 `section:` 开头，且存在 `sectionRef === target.slice('section:'.length)`，使用去掉一层前缀后的 exact ref；
   - 否则返回“未定位”；
3. 不得按 title / description / 文本相似度猜章节。

`LocateAction` 只有在 resolver 真正解析到当前 workspace 的现有字段/章节时才显示可定位按钮。

必须增加测试：

```text
sectionRef = section:overview
formal target = section:section:overview
→ 点击后 focus/scroll 到 section:overview

不存在的 section target
→ 显示未定位，不显示假按钮
```

## 5. P1-04｜catalog-compatible proof 尚未完成

本 P1 的原始根因不是“Source 结构能不能写入”，而是：

```text
World feature_conditional
→ 可以通过基础 Source validation / append
→ 但 validateAssetCatalog() 永久因非 Expansion source fail closed
```

当前 Grok 测试已经证明：

- World request conditional fail closed；
- DeepSeek conditional 被过滤；
- `hard / optional / reference` 可写入 Draft；
- 带这些 dependency 的 World 可以 Publish 进 Source Store。

但尚未执行 correction-01 明确要求的最后一跳：

```text
published World Source
+
synthetic exact dependency targets
→ validateAssetCatalog(...)
→ PASS
```

因此还不能把“Source 可发布”再次当成“catalog 语义已证明兼容”。

### Correction 要求

新增 focused semantic test：

1. 通过 World Creator 正式发布一个带 World 合法 dependency 的 Source；
2. 从 `SourceAssetLibraryStore` 读取 exact published World；
3. 构造最小 synthetic target assets，使 hard target exact resolve；optional/reference 可按 G9-03 语义构造；
4. 复用现有 G9-03 `validateAssetCatalog()` 与已有 test fixture/registry 写法；
5. 证明 catalog PASS，且没有 `CONDITIONAL_DEPENDENCY_SCOPE_MISSING`；
6. 不修改 G9-03 validator，也不新增特殊 Creator-only catalog 规则。

如果这个测试暴露新的协议级问题，停止并返回 GPT；不得为了让测试通过改 G9-03。

## 6. 保持不变

correction-02 不得重开已经通过的部分：

- 四入口与 Creator / World routes；
- blank/import/source revision 三起点；
- SQLite persistence；
- G9-05B Draft/CAS/AI scope/ChangeSet/Undo/Publish；
- composition/dependency 完整编辑；
- existing sectionRef 只读；
- World-specific dependency kind；
- DeepSeek World schema；
- no-provider manual path；
- Source list/detail/version history；
- Runtime isolation。

不得修改：

```text
src/资产创作/L0-L2 production semantics
src/资产协议/** production semantics
src/运行时/**
G9-04 Adapter
Character / Expansion Creator
```

## 7. Gate

新的 exact SHA 至少证明：

```text
P1-02 section formal target locator = PASS
P1-04 catalog-compatible proof      = PASS
G9-05C focused tests                = PASS
G9-05B / G9-03 / G9-04 regressions  = PASS
G8 product regression               = PASS
full suite/typecheck/lint/build      = PASS
launcher/disclosure                  = PASS
Provider calls                       = 0
```

在 `P0=0 / P1=0` 前：

```text
main 不推进
G9-05C 不关闭
Character Creator 不授权
```
