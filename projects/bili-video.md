# B 站视频理解（bili-video）· 已归档

> **2026-10-31 起项目正式命名为 `bili-explore`，当前档案见 [[projects/bili-explore]]。** 本文件保留开发过程记录，不再更新。

> 来源：主人 2026-10-29 会话定调 + 本人实测接口 ｜ 记录于：2026-10-29 ｜ 置信度：方案已定，实现未开始
> 项目目录：`D:\EX DSH\bili-video` ｜ 方案：`docs/plan-v2.md`（v1 已作废，留档对照）

## 目标

让我能自由探索 B 站：找 UP 主、看投稿与动态、追一条内容线；看视频只是其中一个主要功能。

**不是**刷 B 站工具：不定时刷、不默认连播、不默认写报告、不自动互动。

2026-10-30 起定位扩展：信息不全在视频里（可能在动态），前后关联的视频不一定在合集里，所以要能「认识一个人」和「把线串起来」。

## 状态

2026-10-30：**定位扩展为「B 站探索」，两批探索功能完成，版本 1.0.0**。
- 探索命令：`up` / `works` / `feed` / `opus`（第一批）+ `timeline` / `chain`（第二批）。
- 视频命令：`peek` / `read` / `search` / `summary` / `comments` / `replies` / `danmaku` / `asr` / `play` / `login`。
- `timeline`：投稿+动态按时间合并（视频动态去重）；`chain`：给一条视频的位置、前后、合集、提到的 BV。
- 落盘：`read --save report|transcript|both --note` 才写 `out/<bvid>/`，默认什么都不写。
- skill：`bili-video` 已改写成「B 站探索」，含全部探索命令。
- 插件：`dsh-bili-video` link 进 desktop profile；新增 action 的 schema 要重启桌面版才生效。
- 36 个单测全过；类型检查通过。
- 待办：目录改名 `bili-explore`。

## 已定决策（主人拍板）

- 播放：按需选「有头真播」或「无头播放」，不是固定一种。
- 理解：分层——字幕优先，故事板抽帧兜底，ASR 只在无字幕又必须逐字稿时。
- 形态：先做项目内 CLI + skill，跑通再封装 DSH 插件。
- 登录：遇到需要登录的场景，**由主人手动扫码**，不自动。
- 报告：默认不写；`--save` 才落盘。
- 缓存：放项目里，位置我自己定。
- 判断口径：三档（跳过 / 读字幕 / 看画面），实际使用时可自行补充规则。
- 语言：用 TypeScript 写（主人 2026-10-30 建议），Node 24 原生类型剥离，无构建步骤。

## 实测事实（2026-10-29 起，本机）

- 匿名（带 `buvid3`）可拿：视频详情、故事板（`x/player/videoshot`，480×270）、DASH 音视频流、弹幕。
- 匿名**搜索可用**：wbi 签名后 `wbi/search/type` 正常。
- 匿名**拿不到字幕**（`subtitles` 为空）与**官方 AI 总结**（`-101`）——这两个要登录。
- 裸打 API 不带 cookie 会被风控返回 HTML；用 `x/frontend/finger/spi` 拿 `buvid3/buvid4` 最轻（首页请求 4 s，spi 也慢但更小）。
- 本机：Node **24.22**（2026-10-30 主人升级）、Chrome/Edge 已装、dsh 0.2.1-rc.2；无 ffmpeg、无 uv、Python 3.9.2。
- npm 可访问公网（`playwright-core@1.64.1` 等包存在）。
- 沙箱只允许写工作区；`~/.dsh` 写入被拒（EPERM），插件阶段要请主人执行或授权。
- 首次请求到 B 站约 4.5 s（TLS 冷启动），之后单请求 0.7–1.2 s——这是 peek 8 s 的主要成本。

## 下一步

已归档：项目正式化与后续计划见 [[projects/bili-explore]]。

## 坑与注意

- 故事板 `index` 约 25% 概率为空，要用 `pvdata` 兜底 + 重试（参考项目实测）。
- 故事板时间单位可能是秒或毫秒，用「与时长比较」判断，别用绝对值阈值。
- 多 P 视频故事板通常只覆盖第 1P；`--page` 可选，但故事板仍可能只有第 1P。
- 字幕文件的 URL 要补 `https:` 并带 `Referer`，否则 403。
- 无头模式统一用本机真 Chrome（`--headless=new`），不用 Playwright 自带 Chromium（编解码不全）。
- 本工作区曾出现 Windows 文件权限问题（连列目录都失败），已用 DSH 自带诊断脚本修好；再遇到先跑那个脚本。
- **`node --test` 在本机沙箱里会 spawn EPERM**：测试直接 `node tests/unit.test.ts` 跑。
- **`login` 在沙箱里也启动不了 Chrome（spawn EPERM）**：要放宽权限跑；它只写项目内的 `.browser-profile/` 和 `.auth/`。
- 浏览器 profile 一旦登录，`login` 打开就带 `SESSDATA`，「扫码前按回车」的分支无法复现（P2 的 2.2 未实测）。
- B 站 AI 字幕覆盖率不低：抽样 9 条只有 2 条无字幕，找无字幕样本得去音乐区/短片（如 BV1zKZrYAEi8）。
- `read` 未登录时直接报 `NEED_LOGIN`，不区分「真没字幕」；只有登录后才降级成 `NO_SUBTITLE`。
- 一张故事板拼图只 `loadImage` 一次（3 帧常同图），重复解码很慢。
- npm 缓存要指到项目内（`.npmrc` 的 `cache=./.npm-cache`），否则用户目录写不进去。
- 搜索的 `--order` 映射：`relevance→totalrank`、`pubdate→pubdate`、`click→click`；实测 click 按播放降序、pubdate 按时间降序。
- 搜索标题里带 `<em class="keyword">` 和 HTML 实体，必须清洗；`duration` 是 `"12:34"` 字符串，要转秒。
- 相关推荐 `archive/related` 匿名就能取，不用 wbi，也不额外登录。
- `conclusion/get` 返回结构是 `data.model_result.{summary,outline[].part_outline[].{timestamp,content}}`，字段都按可选解析——B 站改过一次结构。
- **别点 video 画面**：B 站里点一下视频就是暂停/继续，自动化点它会把自己刚播的视频点停。只调 `video.play()`，需要按按钮时点 `.bpx-player-ctrl-play`。
- 连播必须复用同一个 page 导航，`newPage()` 开新页播第二条会卡住。
- 卡死判定用「30 秒窗口内进度推进 < 3 秒」，不能用「瞬时不动」——缓冲时可能极慢前进，也可能被 kick 推一下。
- 排障工具有用：`node scripts/diagnose-play.ts <BV> headless` 会截图并打印播放器状态（paused/readyState/页面文字）。
- 报告的「结论」不归脚本写：脚本只做机翻式的取数与排版，结论由调用者用 `--note` 传进去；别让脚本编内容判断。
- 报告里的帧要复制到 `out/<bvid>/frames/` 并用相对路径引用，否则 `out/` 拿走就断图。
- `read` 会先找 `peek.json` 里现成的帧（文件都在才命中），没有才重抓；`--refresh` 强制重抓。
- **skill 的安装位置**：`%USERPROFILE%\.dsh\skills\<name>\SKILL.md`，frontmatter 必须有 `name` 和 `description`；装完技能目录会热更新，不用重启。
- **DSH 桌面版的插件 profile 是 `desktop`**（不是 web）；`dsh plugin --profile desktop add <本地目录>` 用 link 方式装，改代码立即生效。
- 插件热加载：装完 `bili` 工具当场就出现在工具表里，这次没重启桌面版（和 README 里说的"必须重启"不一样，以实测为准）。
- 插件依赖 `@deepseek-ai/dsh-tools` 的 peer 警告可以忽略（宿主运行时解析）；`defineTool` 用了动态 import + 兜底，拿不到就传普通对象。
- **插件里不要用 `process.execPath` 起子进程**：桌面版的 execPath 是 Electron（DeepSeek Harness.exe），要用 PATH 里的 `node`。
- `dsh plugin` 自带 pnpm（11.28.5），不用自己装。
- **插件工具 schema 不热更新**：改 `parameters` 后必须重启桌面版；改 `execute` 里的逻辑也一样（模块缓存）。CLI 侧改了立即生效。
- 评论用新版 `/x/v2/reply/main`（匿名可用、cursor 翻页），旧的 `/x/v2/reply` 匿名只回 3 条；`wbi/main` 匿名 -403。
- 弹幕用 `/x/v1/dm/list.so`，返回**明文 XML**（不是压缩也不是 protobuf），一次拿全片，正则 `<d p="...">` 解析即可。
- 评论正文里的 `[doge]` 这类表情是文本占位符，保留原样，别当脏数据删。
- ASR 模型解开后文件名是 `model.int8.onnx`（量化版），不是 `model.onnx`——代码两种都探。
- Windows 自带 `tar` 不支持 bz2（会调不到 bzip2），解压退回 `python -m tarfile`；下载脚本已加回退。
- ASR 要 spawn ffmpeg 拉音频，沙箱里必须放宽权限；`ffmpeg-static` 的二进制在 `node_modules/ffmpeg-static/ffmpeg.exe`。
- 模型 234 MB 放 `cache/models/`，和缓存一起可删（删了重跑下载脚本即可）。
- 楼中楼的 `--root` 必须是**主楼**的 rpid，在楼里再点「回复」拿到的 id 是二级评论，接口会返回空。
- `comments --with-replies` 只展开前 5 条有回复的主楼——20 条全展开就是 20 次额外请求，会慢到不可用。
- 动态的 `pub_time` 是相对时间（「7 分钟前」），过滤/排序要用 `pub_ts` 转 ISO。
- 图文动态的正文在 `opus/feed/space`（按 id 对齐），`feed/space` 里的 `desc.text` 是空的。
- 视频动态没有正文，`text` 要用 `major.archive.title` 兜底。
- `up` / `works` / `feed` 的 mid 支持数字、空间链接、昵称；昵称走用户搜索，可能重名。
- 合集/系列目前只列不展开（`up` 的 `seasons[]`），拿内容要另接 `seasons_archives_list`。
- 投稿列表是「新 → 旧」：取"前后"时 `index-1` 是**更新**的视频，别搞反（`chain` 的 `older`/`newer` 就是为此改的）。
- `chain` 只能在第一页 50 条投稿里定位；更老的视频 `position` 会是 null。

## 参考

- [dsh-bili-scroll](https://github.com/c1overzz/dsh-bili-scroll)：内置 bilibili-mcp，字幕/故事板/评论的现成实现与踩坑记录。
