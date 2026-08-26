# G9-02C Breadth｜Independent Review 真实 Provider 证据阻塞 v1.0

状态：`CODE REVIEW PASS / EVIDENCE REQUIRED / NOT MERGEABLE`
日期：2026-08-19

## 1. Review Identity

```text
Implementation repo
zhangchenjia21-dot/sillytavern

Formal Task Base
182740801b48c2edc2399e4e4dd8b6ae5a43ccaa

Reviewed Breadth SHA
8a481ef16737e2c36310668b61b40e29b82ee1f7

Commit
feat: complete G9-02C routing context breadth

Topology
Base → Final = ahead 3 / behind 0
merge-base = exact Base

main
182740801b48c2edc2399e4e4dd8b6ae5a43ccaa
unchanged
```

前两个 ahead commit 为 GPT Breadth execution docs；第三个为 Grok Breadth implementation。

## 2. Verdict

```text
Static / architecture review
P0 = 0
P1 = 0

B1 Routing Breadth                 PASS by code/test inspection
B2 Projection / Scale Breadth      PASS by code/test inspection
B3 Continuation / Background       PASS by code/test inspection
B4 Long-session / Recovery         PASS by code/test inspection
Real Provider routing evidence     UNVERIFIED

G9-02C Breadth                     NOT PASS YET
main merge                         BLOCKED BY EVIDENCE
G9-02 Integrated Closure           NOT STARTED
G9-03                              NOT AUTHORIZED
```

本阻塞是验收证据缺失，不是已发现代码 P1。

## 3. Code / Test Findings

### B1

- 1,000 enabled module leaves；
- Package → Feature → Module 三层 bounded refinement；
- target 不在首项；
- 3 Router calls 定位 exact target；
- model-visible working sets 始终受 Core count / serialized-size 上限约束；
- unselected projection load = 0；
- multi-branch / multi-select breadth 仍保持 bounds。

### B2

- 1,000 Player-known records：unrelated Turn 不带 dossier，targeted 只带 bounded last-known facts；
- 10,000 relationship graph：只返回 allowed-character/player-safe bounded subgraph，hidden/unseen/reverse mirror 不泄漏；
- 4 owner cross-owner join：只包含 selected projections，owner identity 保留，joined context < 4,000 chars；
- unrelated owner 不进入 join。

### B3

- no-trigger recipient projection = 0；
- valid exact Handoff → only exact recipient / `authoritative_continuation`；
- missing/wrong/duplicate/over-bound/disabled continuation fail closed；
- runtime 新增 duplicate continuation sourceRef rejection；
- 100 deterministic wait turns：Router calls = 0；Domain Candidate calls = 0。

### B4

- 100 committed observation turns；
- routing working-set serialized size 在长 session 中不随 history 线性增长；
- Save at turn 50 → continue to 100 → Restore → branch evidence；
- semantic-ready crash/resume：Recovery 前 Router/Candidate 各 1 次，Recovery 后仍各 1 次；
- committed turn exactly once。

## 4. Real Provider Gate

Breadth implementation 已把：

```text
npm run g9:02c:core:smoke
```

升级为 production-path 1,000-leaf smoke，要求：

```text
RuntimeDomainRoutingCatalog
→ bounded Package profile
→ DeepSeekRuntimeDomainRouter
→ bounded Feature refinement
→ DeepSeekRuntimeDomainRouter
→ exact Module selection
```

并输出 sanitized metrics：

- enabledLeaves；
- providerCalls；
- groupProfileKinds；
- maxProfilesPerRequest；
- maxSerializedCharsPerRequest；
- selectedModule；
- verdict。

但 Reviewed SHA 没有提交本次真实运行 evidence artifact；GPT 也没有可独立读取的 Grok Final Report。因此：

```text
Smoke code exists
!=
Real Provider PASS evidence exists
```

不得据此宣布 Breadth PASS。

## 5. Evidence Closure

继续原：

```text
branch
agent/g9-02c-breadth

worktree
D:\AI\Projects\.worktrees\sillytavern-agent
```

只做 evidence closure，不重做 Breadth，不新建 fix branch。

必须：

1. 在 exact code SHA 上重跑 Breadth focused + full offline gates；
2. offline 全 PASS 后运行真实 DeepSeek routing smoke；
3. 将 sanitized 结果写入仓库 evidence file，包含 tested SHA / command / exit status / metrics；
4. 禁止写入 API Key、raw prompt、raw provider response、hidden state；
5. 若缺 Key，则明确 `REAL_PROVIDER_NOT_RUN / MISSING_CREDENTIALS`，Breadth 保持 BLOCKED；不得伪造 PASS。

Evidence-only commit 后 GPT 将重新核对 ancestry 与 evidence，再决定是否 fast-forward main。

## 6. Final statement

> **Reviewed Breadth code currently has no P0/P1 finding. Merge remains blocked only because the required real Model-first routing run is not independently verifiable.**
