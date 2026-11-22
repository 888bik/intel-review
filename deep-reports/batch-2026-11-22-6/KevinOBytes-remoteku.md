# KevinOBytes/remoteku

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/KevinOBytes/remoteku
- **分类**：productivity（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：JavaScript
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：6 / 0
- **许可证**：未核实（仓库无 LICENSE 文件，GitHub API 未标注许可证）
- **创建时间**：2025-12-20
- **最近更新**：2026-11-15（元数据 updated_at；最近提交 2026-11-19）
- **最新 release**：v2.1.0（tag `v2.1.0`，发布于 2026-11-19）
- **一句话简介**：用 Electron 把 macOS / Windows / Linux 电脑变成带实时指令面板的 Roku 遥控器

## 一句话理解
把电脑当成一只功能齐全的 Roku 遥控器，并且能在电视上打字、搜片、定时关机。

## 通俗解释
RemoteKu 是一个 Electron 桌面应用，通过 Roku 的 ECP 协议在网络里找到电视并发送按键。它提供模拟实体遥控器外观的操作面板，也配了一个 Companion Dock：实时电视键盘、频道启动器、输入与电源控制。README 写着 v2.0 起加入悬浮画中画迷你遥控器、macOS 菜单栏集成、Touch Bar 支持，v2.1 又补上全平台内容搜索、剪贴板直接投屏、睡眠定时等。README 本身较长（约 8 千字符），信息量够，但功能与性能指标均为作者自述、未独立核实。项目 6 star、0 fork，属于个人小工具，社区验证有限。

## 它不是什么
1. 不是 Roku 官方应用，README 未声明与 Roku 公司有官方合作或背书。
2. 不是通用红外/蓝牙万能遥控器，只针对支持 ECP 的 Roku 设备。
3. 不是投屏服务器或媒体中心，不解码、不托管视频，只发控制指令与文字。
4. 不是移动端 App，README 只给出 macOS（双架构 dmg、Homebrew Cask）、Windows（exe）、Linux（AppImage）安装包。
5. 不是授权清晰的项目：文末写 ISC，但仓库无 LICENSE 文件、GitHub API 未标注许可证，按仓库解析结果记为未核实。

## 痛点
- 电视遥控器想打搜索词或密码时逐个字母移动非常慢
- 多设备切换要重新走 SSDP 扫描等待，打断操作节奏
- 睡前想让电视自动关机，普通遥控器通常没有可编程定时
- 想在电脑上用物理键盘直接控制电视，苦于没有趁手的桌面客户端

## 核心功能（依据 README，注明来源）
- 硬件拟态遥控器面板与 Companion Dock：桌面端把遥控器与实时电视键盘、频道启动器并排展示（来源：README What's New in v2.0）
- 全局内容搜索：走 Roku 的 /search/browse 协议，从桌面深搜所有流媒体频道（来源：README What's New in v2.1）
- 剪贴板直投：把密码、链接或搜索词流式写入电视屏幕，并自动清理换行（来源：README What's New in v2.1）
- 睡眠定时与自动关机：15/30/45/60/90 分钟或关闭，到期发送 PowerOff，带倒计时徽标（来源：README What's New in v2.1）
- 触控板/滚轮调音量：双指滑动以约 130ms 节流步进音量并带机械点击反馈；该节流数字为自述、未独立核实（来源：README What's New in v2.1）
- 悬浮画中画迷你遥控器：可折叠为悬浮小面板，不占屏幕空间（来源：README What's New in v2.0）
- Live TV Keyboard：用电脑键盘逐字符流式输入（含退格与回车）或整串发送（来源：README What's New in v2.0）
- 多设备与快捷键：README 自述 <150ms 重连上次设备、顶部设备切换、WASD/方向键映射、全局唤起热键、macOS 菜单栏与 Touch Bar；重连速度属自述、未独立核实（来源：README What's New in v2.0）
- 架构拆分：main.js 管理 BrowserWindow/Tray/TouchBar/globalShortcut，roku-client.js 负责 SSDP 发现、子网回退、图标缓存与实时媒体遥测（来源：README Architecture & Security）

## 使用场景
- 客厅电视搜片时从电脑键盘输入长搜索词或 Wi-Fi 密码
- 睡前给电视设定 45 分钟后自动关机
- 一台电脑管理客厅、卧室等多台 Roku，靠快捷键即时切换
- 在 Mac 上用菜单栏或 Touch Bar 快速静音、返回主页

## 适用人群
家里有 Roku 设备、希望用电脑键盘高效输入与批量控制的 macOS / Windows / Linux 用户，尤其是嫌电视端输入慢、又需要自定义快捷操作的人。

## 注意事项
- 6 star、0 fork，刚入库，社区验证很少
- 无 LICENSE 文件、GitHub API 未标注许可证；README 文末写 ISC，许可证按仓库解析结果记为：未核实（仓库无 LICENSE 文件，GitHub API 未标注许可证），商用前请向作者确认
- macOS 与 Windows 安装包未签名或触发 SmartScreen，首次运行需手动放行
- 所有延迟、重连速度等数字均来自 README 自述，未独立核实
- 依赖 Roku 的 ECP 且与电视同局域网，网络隔离环境下不可用

## 总结
RemoteKu 把 Roku 遥控器做成了功能密集的桌面小工具：搜索、剪贴投屏、睡眠定时、悬浮迷你遥控、菜单栏与 Touch Bar 集成都安排到位，README 信息也足够详实。但它是 6 star 的个人项目，性能数字均为自述，授权状态 unresolved，适合愿意自行放行安装包并接受小众项目维护风险的 Roku 用户试用。

## 独立评分（满分 10）

- **质量**：6 — 功能齐全的 Electron 遥控器，但代码与性能细节主要靠 README 自述，6 star 缺乏社区验证
- **学习价值**：6 — 展示 Electron 主进程、preload 隔离、Roku ECP 发现与 macOS 菜单栏/Touch Bar 集成的完整结构
- **使用价值**：7 — 跨平台把电脑变成 Roku 遥控器，含剪贴投屏、睡眠定时与全局快捷键，开箱即用
- **推荐等级**：**C**
