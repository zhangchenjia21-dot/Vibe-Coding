# 99_归档｜Archive

本目录保存已经被 current source 覆盖的历史版本、已关闭阶段的过程材料与阶段复盘。

## 归档原则

- active 目录只保留每个文档族的 current / latest 文件；
- 同一文档族的 superseded 旧版本移入本目录，不在 active 根目录并列；
- 独立且仍然有效的编号核心裁定，即使日期较早，也继续留在 active；
- 已关闭任务规格、Exit checklist、阶段更新 Ledger 等过程证据进入归档；
- 归档文件只作历史证据，不与 current source 竞争权威。

建议结构：

```text
99_归档/
├─ README.md
├─ sillytavern/
│  ├─ 版本历史/
│  ├─ 阶段过程/
│  └─ 阶段复盘/
└─ 项目经验/
```

## 人类可读文档版本号

本仓库的治理 / 规划 / 裁定文档版本号**不按 SemVer 的多位 minor 解释**，采用人类直觉的一位小版本序列：

```text
v1.0 → v1.1 → ... → v1.8 → v1.9 → v2.0 → v2.1
```

规则：

- `vM.N` 中 `N` 只允许 `0–9`；
- `vM.9` 的下一版本必须是 `v(M+1).0`；
- 禁止新建 `v1.10 / v1.11 / v1.12` 一类容易按十进制产生歧义的编号；
- 版本号仅表示文档演进顺序，不承担 SemVer 的 breaking-change 语义；重大变化由 `status / supersedes / change_class / ADR` 明确表达；
- 高频滚动 current 文档优先使用固定路径，例如 `*_CURRENT.md`，历史由 Git 与本目录承担。
