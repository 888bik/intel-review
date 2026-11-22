# huynhsontung/Screenbox

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息

- **仓库地址**：https://github.com/huynhsontung/Screenbox
- **分类**：productivity（据 zhao 情报库分类；原始库未核实）
- **语言**：C#
- **评分**：原始评分未核实；本报告独立评分见文末
- **Star / Fork**：4,730 / 224
- **许可证**：GPL-3.0
- **创建时间**：2022-01-12
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-21）
- **最新 release**：v0.24.0（发布于 2026-10-26）
- **一句话简介**：基于 LibVLCSharp 与 UWP 构建的 Windows 现代媒体播放器，主打流畅性能、易用界面与跨设备（PC / Xbox）。

## 一句话理解

Screenbox 是“UWP 原生版的现代播放器”：用 Fluent Design 界面包住 LibVLC 强大的解码能力，在 Windows 10/11 和 Xbox 上提供轻快、好看、手势友好的播放体验。

## 通俗解释

Windows 上播放器很多，但“既好看又原生又轻量”的不多：VLC 功能强但界面老派，UWP 商店里的很多播放器是套壳且卡顿。Screenbox 走的是正统路线——用微软 UWP（Universal Windows Platform）框架 + LibVLCSharp（VLC 引擎的 .NET 封装）做底层解码，界面采用 Fluent Design，支持手势快进/调音量、画中画、投屏 Chromecast、网络媒体浏览等（来源：README）。它能在 Windows 10 1903+、Windows 11 和 Xbox 上运行（来源：README）。通过 Microsoft Store 安装可自动更新且不需要微软账号（来源：README “Install”）。一句话：它把 VLC 的“能播一切”和 UWP 的“现代体验”结合起来，是 Windows 商店里口碑很好的开源播放器。

## 它不是什么

1. **不是 VLC 官方客户端**。它基于 LibVLCSharp/LibVLC（VLC 的引擎库），是独立项目，不是 VideoLAN 官方的 VLC 应用（来源：README 原文 “built on top of LibVLCSharp and the Universal Windows Platform”）。
2. **不是跨平台播放器**。它面向 Windows 10 1903+/11 与 Xbox，不支持 macOS/Linux（来源：README）。
3. **不是 mpv 那类命令行播放器**。它是带完整图形界面的现代应用（Fluent Design、手势、快捷键）（来源：README）。
4. **不是“只有基础播放”的极简工具**。README 列出画中画、Chromecast、网络浏览、截帧、手势等一批功能（来源：README “Some notable features”）。
5. **不是只能从商店装**。推荐 Microsoft Store，但 README 也提到其他安装方式（来源：README “Install” 段落；具体方式如 sideload 细节未核实）。

## 痛点

- **Windows 商店播放器体验参差**：很多 UWP 播放器解码弱或界面粗糙；Screenbox 用 LibVLC 引擎 + 原生 Fluent 界面补齐。
- **VLC 桌面版“不现代”**：功能强但 UI 老；Screenbox 提供手势、Fluent 视觉、Xbox 体验。
- **跨设备体验**：想在 PC 和 Xbox 上都用同一套播放器；UWP 天然覆盖（来源：README）。
- **网络媒体/投屏需求**：内置 Chromecast 与网络媒体浏览，不用另装工具（来源：README）。

## 核心功能（依据 README，注明来源）

- **现代 Fluent Design UI**：README 原文 “Fluent design user interface”，并配深色/浅色截图（来源：README）。
- **手势与快捷键**：手势快进与调音量、窗口缩放快捷键（数字行 1–4）、类 YouTube 的快捷键布局（来源：README “Some notable features”）。
- **画中画与截帧**：Picture-in-picture mode、Save the video frame as an image（来源：README 原文）。
- **Chromecast 与网络播放**：Chromecast support、Browse and play media over the network（来源：README 原文）。
- **底层技术栈**：基于 LibVLCSharp 与 UWP（来源：README 原文）。
- **分发方式**：推荐 Microsoft Store（自动更新、无需微软账号），支持 PC 与 Xbox（来源：README “Install” 与开头）。

## 使用场景

- Windows 10/11 用户想要一个好看、轻快、功能够用的日常视频播放器。
- Xbox 上播放本地/网络媒体。
- 需要 Chromecast 投屏、画中画、手势操作的场景。
- 想通过 Microsoft Store 获得自动更新的开源软件的用户。

## 适用人群

Windows / Xbox 用户、看重界面体验和易用性的普通用户。命令行党和跨平台用户更适合 mpv。

## 注意事项

- GPL-3.0 许可，商业再分发需注意合规。
- 196 个 open issues，活跃开发中，功能“and many more on the way”（来源：README），成熟度中等。
- 最新版 v0.24.0 发布于 2026-10，仍在持续迭代（最近提交 2026-11-21）。
- UWP 架构决定了它不兼容旧版 Windows（<10 1903）与非 Windows 平台。

## 总结

Screenbox 是 Windows 现代播放器里的优等生：LibVLC 提供解码底气，UWP + Fluent 提供体验上限，PC 与 Xbox 双端覆盖是差异化优势。它不像 mpv 那样极客，也不像 VLC 那样全能老牌，而是精准卡在“好看好用够强”的甜点位。对 Windows 用户，它是值得装来替代商店里那些套壳播放器的选择。

## 独立评分（满分 10）

- **质量**：8 — 技术选型扎实、界面现代、持续维护；issue 较多、功能仍在补齐。
- **学习价值**：6 — UWP + LibVLCSharp 集成有参考价值，但 README 信息量有限，深入需读源码。
- **使用价值**：8 — Windows/Xbox 日常播放体验优秀，免费开源。
- **推荐等级**：**B**（Windows 平台值得一装，但受众受平台限制，未达普适 A 级）。
