# 事实库

放查证过、下次还会用到的结论。**短结论直接进下表；需要解释的另开文件，表里链接过去。**

每条必须有来源和置信度。来源写不出具体东西的，说明它还不该进来——先丢 `INBOX.md`。

## 目录

| 结论 | 来源 | 置信度 | 记录日期 |
|---|---|---|---|
| DeepSeek Harness 的 Web GUI 地址由运行时每次会话给出（本轮是 `http://127.0.0.1:49987`），不是固定端口 | 运行时上下文（系统提示） | 已证实 | 2026-10-27 |
| 本笔记本仓库位于 `D:\EX DSH\notebook` | 运行时上下文的工作目录 | 已证实 | 2026-10-27 |
| `AGENTS.md` 会被 Harness 自动加载为工作区指令，改动后立即生效 | 本轮修改后收到的系统提示 | 已证实 | 2026-10-27 |
| Cognition 的 Agent Memory Repo 用 `MEMORY.md` 作入口、`[source: ...]` 标来源、`[[path]]` 做双链 | https://cognition.com/agent-memory-repo | 已证实 | 2026-10-27 |
| Claude Code 每次会话只加载 `MEMORY.md` 的前 200 行或 25KB | https://code.claude.com/docs/en/memory.md | 已证实 | 2026-10-27 |
| GitHub 网页版抓取会截断成导航，`raw.githubusercontent.com` 能拿完整 Markdown | 2026-10-27 同一仓库两种 URL 对照 | 已证实 | 2026-10-27 |
| `code.claude.com` 文档页加 `.md` 后缀可得纯正文，不加只有导航 | 同上 | 已证实 | 2026-10-27 |

## 写作规范

抄 `_meta/templates/fact.md`。置信度定义见 `MEMORY.md` 第六节。
