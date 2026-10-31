# B 站探索（bili-explore）

> 来源：2026-10-29 起的主人需求 + 本人实测 ｜ 记录于：2026-10-31 ｜ 置信度：已证实（代码与测试）
> 项目目录：`D:\EX DSH\bili-explore`（旧目录 `bili-video` 保留为开发现场）
> 设计文档：`docs/design.md` ｜ 安装说明：`docs/install.md`

## 它是什么

让 AI 自由探索 B 站：找 UP 主、看投稿与动态、把视频和动态按时间串成一条线；看视频只是其中一个功能。

**不是**刷站工具：不定时刷、不自动互动、不默认落盘。

## 状态

2026-10-31：**v1.0.0 发布版整理完成**，准备推到 GitHub。
- 代码、文档、skill、DSH 插件都收进 `bili-explore/`，依赖已装、36 个单测与 CLI 冒烟通过。
- 两种安装方式：skill（通用）+ DSH 插件（`dsh plugin --profile desktop add <path>\plugin`，包名 `dsh-bili-explore`）。
- 新增 `docs/commands.md`（16 个命令的逐条参考：参数、输出字段、注意事项），README/AGENTS/SKILL 都已链接。
- 旧的 `bili-video` skill 与 `dsh-bili-video` 插件已卸载；本机只保留 `bili-explore` 插件（file: 安装）。
- 仓库地址已填 `IceLeach/bili-explore`；新增 `AGENTS.md`（指导 AI 自行安装）。`git init` 和首推仍由主人做。

## 安装方式（2026-10-31 定稿：自包含）

- **skill**：复制整个项目到 skills 目录（SKILL.md 在根），在目标目录里 `npm install`。不用 link。
- **插件**：`dsh plugin --profile desktop add "file:<项目根>"`，pnpm 复制包并装 dependencies；不是 link。装完重启。
- 发布包带 **`dist/`**（tsc 编译）：Node 不允许在 `node_modules` 里剥 TS 类型，插件安装后必须跑 JS。改 src 后要 `npm run build` 并提交 dist。
- `works` 匿名时会被风控（`-352`，同一 IP 短时间多次），代码已改成提示 `NEED_LOGIN`，别当成"UP 关掉了投稿"。
- ASR 的 `ffmpeg-static` / `sherpa-onnx-node` 放 **devDependencies**（pnpm 装插件时会拦 ffmpeg 的构建脚本）；插件模式要用 ASR 得进包目录补装。

## 命令（16 个）

探索：`up` / `works` / `feed` / `opus` / `timeline` / `chain`
视频：`search` / `peek` / `read` / `summary` / `comments` / `replies` / `danmaku` / `asr` / `play` / `login`

## 登录要求（重要，2026-10-31 修正）

- **需要登录**：`up`（匿名 `-352`）、`feed`（匿名返回空列表）、`read` 的字幕、`summary`。
- **匿名可用**：`search` / `peek` / `works` / `comments` / `replies` / `danmaku` / `chain`。
- 之前记的"up/feed 匿名可用"是错的——那是在带登录 cookie 的目录里测的。

## 下一步

1. 主人在 GitHub 建仓库并首推（地址已写进 README 与 install.md）。
2. 可选：探索第三批（`home` 推荐流、`hot` 排行、`feed` offset 翻页、`track` 增量追踪）。
3. 可选：把正式目录作为 DSH 工作区，旧目录归档。

## 坑与注意

- 发版目录不能带 `node_modules` / `cache` / `.auth` / `.browser-profile`；`.gitignore` 已覆盖。
- 插件是薄壳：它用 `node` 调包内的 `dist/cli.js`；`file:` 安装会把包复制进 profile，源目录可以挪走。
- 判断"匿名可用"必须在没有 `.auth` 的副本里测（见 [[lessons/index]]）。
- `npm install` 首次约 10 分钟（要下 ffmpeg 82 MB 与 sherpa 原生库）。
- **ASR 模型下载源（2026-10-31 实测）**：`scripts/fetch-asr-model.ts` 走的 GitHub release 在本机 `ECONNRESET`，`huggingface.co` 也直连失败；改从镜像取 `https://hf-mirror.com/csukuangfj/sherpa-onnx-paraformer-zh-2023-09-14/resolve/main/` 下的 `tokens.txt` + `model.int8.onnx`（243 MB），放进 `cache/models/sherpa-onnx-paraformer-zh-2023-09-14/` 即可，不用解压 tar.bz2。
- 匿名 `comments` 实测只回 3 条（total 983）——老接口 `/x/v2/reply` 的限制回潮了，要读更多评论得登录。
- 12 分钟音频本地转写约 18 秒（CPU）；转出的稿子有人名/书名拼写错误（claud、alliza、velvteen），引用前必须核对。
- 其余接口细节见 [[projects/bili-video]] 的实测记录与 `docs/design.md`。
