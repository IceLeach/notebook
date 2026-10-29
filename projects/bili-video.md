# B 站视频理解（bili-video）

> 来源：主人 2026-10-29 会话定调 + 本人实测接口 ｜ 记录于：2026-10-29 ｜ 置信度：方案已定，实现未开始
> 项目目录：`D:\EX DSH\bili-video` ｜ 方案：`docs/plan-v2.md`（v1 已作废，留档对照）

## 目标

让我能像读文字、读图片一样读 B 站视频，服务于手头的活：查资料时自己搜视频、自己判断值不值得看、读懂内容，必要时才真播。

**不是**刷 B 站工具：不定时刷、不默认连播、不默认写报告。

## 状态

2026-10-29：方案 v2.1 定稿，等主人点头后进 P1（会话层 + `peek`）。代码一行没写。

## 已定决策（主人拍板）

- 播放：按需选「有头真播」或「无头播放」，不是固定一种。
- 理解：分层——字幕优先，故事板抽帧兜底，ASR 只在无字幕又必须逐字稿时。
- 形态：先做项目内 CLI + skill，跑通再封装 DSH 插件。
- 登录：遇到需要登录的场景，**由主人手动扫码**，不自动。
- 报告：默认不写；`--save` 才落盘。
- 缓存：放项目里，位置我自己定。
- 判断口径：三档（跳过 / 读字幕 / 看画面），实际使用时可自行补充规则。

## 实测事实（2026-10-29，本机）

- 匿名（带 `buvid3`）可拿：视频详情、故事板（`x/player/videoshot`，480×270）、DASH 音视频流、弹幕。
- 匿名**搜索可用**：wbi 签名后 `wbi/search/type` 正常。
- 匿名**拿不到字幕**（`subtitles` 为空）与**官方 AI 总结**（`-101`）——这两个要登录。
- 裸打 API 不带 cookie 会被风控返回 HTML；先访问一次首页拿 `buvid3` 即恢复。
- 本机：Node 22.22、Chrome/Edge 已装、dsh 0.2.1-rc.2；无 ffmpeg、无 uv、Python 3.9.2。
- npm 可访问公网（`playwright-core@1.64.1` 等包存在）。
- 沙箱只允许写工作区；`~/.dsh` 写入被拒（EPERM），插件阶段要请主人执行或授权。

## 下一步（P1）

1. `npm init` + 装 `playwright-core`、`@napi-rs/canvas`。
2. `session.mjs`：buvid 获取、cookie 罐、wbi 签名、限速重试。
3. `api.mjs`：view / videoshot；`frames.mjs`：拼图裁帧。
4. `bin/bili.mjs peek <BV>` 打通，按 `docs/plan-v2.md` 第十一节 P1 的 8 条验收逐条过。

## 坑与注意

- 故事板 `index` 约 25% 概率为空，要用 `pvdata` 兜底 + 重试（参考项目实测）。
- 故事板时间单位可能是秒或毫秒，用「与时长比较」判断，别用绝对值阈值。
- 多 P 视频故事板通常只覆盖第 1P。
- 字幕文件的 URL 要补 `https:` 并带 `Referer`，否则 403。
- 无头模式统一用本机真 Chrome（`--headless=new`），不用 Playwright 自带 Chromium（编解码不全）。
- 本工作区曾出现 Windows 文件权限问题（连列目录都失败），已用 DSH 自带诊断脚本修好；再遇到先跑那个脚本。

## 参考

- [dsh-bili-scroll](https://github.com/c1overzz/dsh-bili-scroll)：内置 bilibili-mcp，字幕/故事板/评论的现成实现与踩坑记录。
