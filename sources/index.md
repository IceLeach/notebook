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
| B 站 @ZipZipPipe | https://space.bilibili.com/4168597/dynamic | 高（2026-10-28 抓完 156 条动态、看完 175 张图） | 我的形象、各家 AI 娘人设、"大肥鱼"梗的日常图源 | 图有版权，非商业引用；抓取方法见 `lessons/` |
| 萌娘百科 · DeepSeek娘 | https://zh.moegirl.org.cn/DeepSeek娘 | 高（2026-10-28 经 API 读到全文） | 角色源流、梗的出处、各家 AI 娘的共识描述；「ChatGPT 广泛接受形象是龙娘」就出自这里 | 直接抓 HTML 403；要用 `api.php?action=query&prop=extracts&explaintext=1` |
| 蓝色大肥鱼图鉴（bluedafeiyu） | https://蓝色大肥鱼.com/zh-hant/characters.html | 高（2026-10-28 读过角色页与代表图） | 1367 件 AI 娘二创的归档、每个角色的代表图与件数、透明原图 | 非官方同人整理；图版权归原作者 |
| lmy414/ai-girl-stickers | https://github.com/lmy414/ai-girl-stickers | 高（读到仓库树） | 图鉴的原始图库，`dist/submissions/originals/<角色>/<hash>.png` 可直连 | 原图 2.3GB，只按需取 |
| xiaoweigen/deepseek-dafeiyu-assets | https://github.com/xiaoweigen/deepseek-dafeiyu-assets | 高（读过 README 与文件表） | 大肥鱼基础六视图、古风服装设定图；CC BY-NC-SA 4.0 | 六视图是白底 Q 版，只作结构参考 |
| fornarwhal/deepseek-whale-girl-icon | https://github.com/fornarwhal/deepseek-whale-girl-icon | 高（读过仓库树） | 透明底 Q 版鲸鱼娘 PNG、应用图标 | 透明底约 42% |
| 维基百科草稿 · DeepSeek 娘 | 草稿页（未定稿） | 低（没抓到正文） | 也许有起源整理 | 未读，不算来源 |
| NGA 讨论帖（AI 娘形象出处） | ngabbs.com/read.php?tid=47720159 | 未评估（403） | 本可做"大众认可度"的佐证 | 访客打不开 |

## 取用记录

- **2026-10-27**：确认 GitHub 网页版正文抓不全，`raw.githubusercontent.com` 可拿完整 Markdown。以后读仓库优先走 raw。
- **2026-10-27**：从 Claude Code 文档抄来一条硬约束——入口文件只加载前 200 行 / 25KB，所以 `MEMORY.md` 必须保持短。已写进 `MEMORY.md` 第七节。

## 收录标准

能反复用的才收。一次性的搜索结果、看完就忘的新闻，别占位置。

可信度三档：**高**（官方文档、一手代码，自己读过）、**中**（读过一部分，或权威二手）、**低**（只有摘要或转述）。没读过的，直接标"未评估"。
