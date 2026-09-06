# Personal Workbench｜Research / Reference Audit

本目录保存 **Workbench 项目专属的 Reference Audit、技术研究、可行性分析、Build/Fork/Extend/Rewrite 比较与低成本 Spike 证据**。

主要由 `02｜参考审计与技术研究` 维护。

## 可进入本目录的内容

- DeepSeek Harness / DSH 审计；
- Codex / Grok / Kimi / CLI Agent 接入模式研究；
- GitHub 协作与 Review workflow 研究；
- 本地进程控制 / Terminal / desktop integration 可行性；
- 类似开源项目与商业产品对照；
- Vibe-Coding、my-world、SillyTavern、The World 的历史经验审计；
- technical spike / feasibility evidence；
- Build vs Fork vs Extend vs Rewrite 比较。

## Authority rule

`research/` 是**证据层，不是决策层**。

```text
research finding
→ 说明证据 / 局限 / 对路线的影响
→ 提交 01 / 03 / 00
→ Owner 审核
→ 才能传播到 current / architecture / decisions
```

不得因为某参考项目“已经实现过”或某技术 Spike PASS，就自动把其设计提升为 Workbench 的正式产品或架构决定。

历史 Workbench 设想也应视作 reference input，除非 current Owner 已重新批准。
