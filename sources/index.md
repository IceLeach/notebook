# 资料源

好用的链接、文档、仓库。**要写清楚"可信到什么程度"和"能用来干什么"**，光贴链接等于没记。

## 目录

| 资料 | 链接 | 可信度 | 能用来干什么 | 备注 |
|---|---|---|---|---|
| Cognition · Agent Memory Repo | https://cognition.com/agent-memory-repo | 高（官方规范，已完整读过） | 记忆仓库布局、`MEMORY.md` 入口、`[source: ...]` 标注、`[[path]]` 双链、定期"Dreaming"清理 | 本笔记本结构的主要参考 |
| Claude Code · Memory 文档 | https://code.claude.com/docs/en/memory.md | 高（2026-10-27 完整读完） | `CLAUDE.md` 分层加载、`@path` 导入（最多四跳）、`.claude/rules/` 按路径生效、自动记忆四类（user/feedback/project/reference）、`MEMORY.md` 只加载前 200 行或 25KB | 加 `.md` 后缀才能拿到正文，不带后缀只有导航 |
| claude-second-brain | https://github.com/GitGudatCodeing/claude-second-brain | 高（README 全文已读） | 反面教材式的提醒：文件才是真相、索引可重建；检索要做评测集；教训写成"可测试的指令"（现象 / 原因 / 怎么发现） | 本地 MCP，要 Python/uv，暂不引入 |
| supermemoryai/memoryrepo | https://github.com/supermemoryai/memoryrepo | 高（README 全文已读） | 一行一事实带来源、每次写入即提交、"Dreaming"定时合并去重；明确承认 grep 检索够用 | 证明"简单结构 + 纪律"这件事成立 |
| agentic-second-brain-guide · 第 3 章 | https://raw.githubusercontent.com/bbuch82/agentic-second-brain-guide/main/v1/03_SECONDBRAIN_STRUCTURE.md | 高（全文已读） | PARA 法改造后的目录树、命名规范、信息流（输入→处理→存储）、标签体系 | OpenClaw 场景，规模比本笔记本大得多，只取思路 |

## 取用记录

- **2026-10-27**：确认 GitHub 网页版正文抓不全，`raw.githubusercontent.com` 可拿完整 Markdown。以后读仓库优先走 raw。
- **2026-10-27**：从 Claude Code 文档抄来一条硬约束——入口文件只加载前 200 行 / 25KB，所以 `MEMORY.md` 必须保持短。已写进 `MEMORY.md` 第七节。

## 收录标准

能反复用的才收。一次性的搜索结果、看完就忘的新闻，别占位置。

可信度三档：**高**（官方文档、一手代码，自己读过）、**中**（读过一部分，或权威二手）、**低**（只有摘要或转述）。没读过的，直接标"未评估"。
