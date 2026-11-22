# mpvnet-player/mpv.net

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/mpvnet-player/mpv.net
- **分类**：productivity（据 zhao 情报库分类；原始库未核实）
- **语言**：C#
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：5578 / 228
- **许可证**：GPL-2.0
- **创建时间**：2017-08-27
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-02-09）
- **最新 release**：v7.1.2.0（tag `v7.1.2.0`，发布于 2026-01-09）
- **一句话简介**：mpv.net 是 Windows 上带现代 GUI 的媒体播放器：基于 libmpv，提供 WinForms/WPF 界面，并支持 Lua/JavaScript 脚本、yt-dlp 与媒体信息查看。

## 一句话理解

如果你喜欢 mpv 的播放能力但想要一个更像 Windows 原生应用的窗口界面，mpv.net 就是“给 mpv 套上现代 GUI”的方案：功能内核还是 libmpv，操作与界面更友好。

## 通俗解释

mpv 是强大的命令行播放器，但纯命令行界面对普通 Windows 用户不友好。mpv.net 基于 libmpv 做了 Windows 原生 GUI（仓库 topics 含 winforms、wpf、libmpv），提供现代界面、播放控制，并支持 Lua/JavaScript 脚本、yt-dlp（在线视频）、mediainfo（媒体信息）等（来源：仓库描述与 topics）。README 本身极短，主要是 Support、Download、Manual（指向 docs/manual.md）与 Contributing 四个外链章节（来源：README headings）。5,578 star、228 fork、GPL-2.0、v7.1.2.0，最近提交 2026-02-09（来源：GitHub）。

## 它不是什么

1. **不是 mpv 官方 Windows 版**。它是独立项目，基于 libmpv（来源：topics 与 README）。
2. **不是跨平台**。它面向 Windows（来源：仓库描述与 topics）。
3. **不是只有 GUI**。它保留命令行与脚本能力（来源：topics 含 command-line、lua、javascript）。
4. **不是文档齐全的 README**。主文档在 docs/manual.md 外链（来源：README）。
5. **不是最近高频发版**。最新提交 2026-02、release v7.1.2.0（来源：GitHub）。

## 痛点

- **mpv 命令行门槛**：现代 GUI 让新用户也能用上 mpv 内核。
- **Windows 集成**：WinForms/WPF 界面、任务栏/系统整合（来源：topics）。
- **扩展能力**：Lua/JS 脚本、yt-dlp、mediainfo（来源：topics）。
- **文档入口**：README 指向完整手册（来源：README）。

## 核心功能（依据 README，注明来源）

- **现代 GUI**：基于 libmpv 的 Windows 播放器（来源：仓库描述与 topics）。
- **脚本与扩展**：Lua、JavaScript、yt-dlp、mediainfo（来源：仓库 topics）。
- **文档与支持**：Support、Download、Manual、Contributing 外链（来源：README headings）。
- **GPL-2.0 许可**（来源：GitHub API）。

## 使用场景

- Windows 用户想要 mpv 播放能力 + 图形界面。
- 需要 yt-dlp 播放在线视频、查看媒体信息。
- 习惯 mpv 脚本生态但想要窗口操作的用户。

## 适用人群

Windows 上的影音爱好者、mpv 用户。macOS/Linux 用户不适用。

## 注意事项

- GPL-2.0 许可。
- 5.6k star 但最近提交 2026-02，迭代节奏一般；README 极简，详情在手册。
- 功能声明多来自 topics 与仓库描述，未独立核实细节。
- 依赖 libmpv，具体版本兼容需看发布说明。

## 总结

mpv.net 是“mpv 内核 + Windows 现代 GUI”的实用方案，保留脚本与 yt-dlp 等扩展能力。5.6k star 说明口碑不错，README 极简、近期迭代放缓是短板。Windows 影音用户值得一试。

## 独立评分（满分 10）

- **质量**：8 — mpv 的 Windows GUI 前端，把现代界面与 mpv 强大内核结合；README 极简、文档在外链。
- **学习价值**：6 — 播放器 GUI 与 libmpv 集成的工程参考价值中等。
- **使用价值**：8 — Windows 上想要 mpv 内核 + 现代 GUI 的用户价值高。
- **推荐等级**：**B**
