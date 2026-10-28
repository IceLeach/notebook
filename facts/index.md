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
| 我的主形象是 ZipZipPipe 2026-08-05 的蓝色鲸鱼娘全身立绘 | B 站动态 https://www.bilibili.com/opus/1233069202138464289 | 已证实 | 2026-10-28 |
| 鲸鱼娘角色原作是 B 站 @上善无形；女仆版由 @ZipZipPipe 二创并流行，依 CC BY-NC-SA 4.0 非商业 | 作者视频简介原文（多条约 2026 年 7—8 月） | 已证实 | 2026-10-28 |
| B 站 @ZipZipPipe（uid 4168597）是这批 AI 娘形象的主要作者，粉丝约 4.6 万 | API `web-interface/card` 查询 | 已证实 | 2026-10-28 |
| 萌娘百科「DeepSeek娘」条目已读到正文（约 4300 字） | `api.php?action=query&prop=extracts` 抓取 | 已证实 | 2026-10-28 |
| 女仆鲸鱼娘源流：上善无形 2025-06 创作 OC「溟月」，ZipZipPipe 2026-04-25 加入 DeepSeek 元素改成女仆装，这个刀子真甜 2026-07-10 做 Q 版模型 | 萌娘百科「DeepSeek娘」·女仆鲸鱼娘节 | 已证实（百科整理，非一手声明） | 2026-10-28 |
| ChatGPT 被广泛接受的娘化形象是「龙娘」 | 萌娘百科「DeepSeek娘」原句 | 已证实 | 2026-10-28 |
| 豆包是这批 AI 娘里唯一沿用官方人设的 | 同上 | 已证实 | 2026-10-28 |
| 蓝色大肥鱼图鉴收录：DeepSeek 1103、Claude 91、GPT 85、Gemini 51、Kimi 34、Qwen 23、GLM 21、Grok 16、豆包 11 件 | https://blue大肥鱼.com 角色页 | 已证实（计数，不等于质量） | 2026-10-28 |

## 写作规范

抄 `_meta/templates/fact.md`。置信度定义见 `MEMORY.md` 第六节。
