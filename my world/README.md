# my world

`my world` 是基于 The World / DSH 长期真实试玩经验启动的独立单人 AI RPG 项目治理目录。

## 项目事实

- 本地项目目录：`D:\AI\Projects\my-world`
- 实现仓库：`https://github.com/zhangchenjia21-dot/my-world`
- Godot：`4.7.2.stable.official.ed1daf0bf` Standard / non-.NET Windows x64
- 当前：`G1-04 — Real Provider Streaming / Cancel Foundation Spike`
- G1-04 当前必须真实接通：**DeepSeek + Kimi Code**

## Authority

1. [`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md)
2. [`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md)
3. [`MY_WORLD_G1新聊天交接指令_CURRENT.md`](./MY_WORLD_G1新聊天交接指令_CURRENT.md)
4. [`MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`](./MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md)
5. [`MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`](./MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md)

```text
Current Phase = G1
Current Task = G1-04
G1-01 = PASS
G1-02 = PASS
G1-03 = PASS
```

G1-01 已证明本机 Git / Godot runtime 正常；Codex 权限失败为 sandbox-only。

G1-02 已证明 Godot CLI、Windows x86_64 export templates 与 ICU Data 可用；GDScript 只是 provisional spike candidate，最终语言 / Runtime boundary 留给 G1-06。

G1-03 已由用户人工 Windows UAT 确认 PASS：中文、长文本滚动、批量 / 持续追加、UI 响应、输入、选择 / 复制、正常退出与 Git clean 均通过。

## G1-04

Outcome：验证 Godot Foundation surface 对**两个真实 Provider**都能完成真实网络请求、SSE 增量输出、cancel、错误态和 UI 非冻结。

```text
DeepSeek
POST https://api.deepseek.com/chat/completions
stream = true
default model = deepseek-v4-pro
key env = DEEPSEEK_API_KEY

Kimi Code API
POST https://api.kimi.com/coding/v1/chat/completions
stream = true
default model = k3
key env = KIMI_CODE_API_KEY
```

这是 G1-04 exploratory execution scope，不冻结最终产品 Provider 架构。实现只保留一个极薄的共同 HTTP/SSE seam，并显式区分两家的 host/path/key/model；Kimi Code 不保留兼容 fallback，不建设自动路由、fallback mesh、负载均衡或 Provider 平台。

G1-04 只有在 **DeepSeek 与 Kimi Code 都获得真实 HTTP 2xx + 增量 stream 证据**，并验证真实 cancel / UI responsiveness 后才能 PASS。当前状态为 NOT PASS，最高结果为 READY FOR OWNER UAT。

## 原则

> **迁移经验，不迁移宿主债务。**

> **Commodity Foundation, Owned Game Semantics.**

> **Engine-native, not engine-semantic-coupled.**
