# ZigaoWang/zigzag

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 A

## 基本信息
- **仓库地址**：https://github.com/ZigaoWang/zigzag
- **分类**：developer-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Swift
- **评分**：76（情报库评分）；本报告独立评分见文末
- **Star / Fork**：349 / 43（研究日期实查；库中旧数为 153 star，截至 2026-11-15）
- **许可证**：Apache-2.0（LICENSE 文件核实确认，与情报库记录一致；API 亦标注 Apache-2.0）
- **创建时间**：2026-10-25
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22，消息 docs: clear palette glass and finishes in notes）
- **最新 release**：Zigzag 0.9.3（tag `v0.9.3`，发布于 2026-11-21）
- **一句话简介**：一款用 Swift 与 WebKit 编写的 Mac 轻量浏览器，主打侧边栏标签与空间、Arc 风格体验、Chrome 扩展支持，无账号、无遥测、安装约 35 MB。

## 一句话理解

一个高中生作者为“Arc 关停后不想回到顶部标签”而做的 Mac 浏览器：用系统自带的 WebKit 内核做成原生 Swift 应用，把侧边栏、空间、分屏这些 Arc 的灵魂功能搬过来，还能直接装 Chrome 扩展，体积只有约 35 MB。

## 通俗解释

The Browser Company 宣布在 2026-12-22 关闭 Arc，很多用户不愿回到传统顶部标签。（来源：README "Why I built it"。）作者 Zigao 是上海的一名高中生，他试过的替代品要么是 Chromium 大分支（fork Helium 源码约 100GB、单次构建 3–4 小时），要么是围绕 AI 打造的浏览器，于是自己用 Swift + WebKit 写了 Zigzag 并一直当主力浏览器用（来源：README "Why I built it"，体积与构建时间为项目自述、未独立核实）。选择 WebKit 而非 Chromium 的理由是：WebKit 随 macOS 自带，省电省内存，安全修复随系统更新分发（来源：README）。它把标签放在侧边栏并按“空间（Spaces）”分组，支持分屏、命令栏、从 Arc 一键导入空间与固定标签、直接安装 Chrome Web Store 扩展（来源：README "Coming from Arc" 与 "What it does"）。作者明确宣称无账号、无统计、无遥测，并列出应用自身会发起的每一类网络连接（来源：README "Privacy"）。研究日期实查：349 star、43 fork、28 issue、最新 v0.9.3、仍在活跃开发。

## 它不是什么

1. **不是 Chromium 分支或 Electron 应用**。它基于 macOS 自带的 WebKit（WKWebView），作者明确说不是 Chromium、不是 Electron（来源：README "How it's built"）。
2. **不是 Arc 的官方续作或分支**。它是独立重写的 Arc 风格替代品，不是 The Browser Company 的产品（来源：README "Why I built it"）。
3. **不是功能完整的成熟浏览器**。README 列出诸多缺失：Mac 间/iPhone 无同步、无 Easels/Notes、部分 Chrome 扩展 API 不支持、仅支持 macOS 15.4+（来源：README "What's missing"）。
4. **不是内置 AI 的浏览器**。AI 功能默认关闭，只有用户主动开启后，Mac 上的 AI 应用才能通过 MCP 使用标签页，且不含隐私窗口与密码（来源：README "What it does" / "AI apps"）。
5. **不是有跨设备生态**。密码经 iCloud Keychain 同步，但空间与标签页不会同步到其他 Mac 或 iPhone（来源：README "What's missing"）。

## 痛点

- **Arc 关停后的迁移痛点**：File › Import from Another Browser 选 Arc，可把空间、固定标签、文件夹、收藏、历史、密码、登录态、扩展一起导入，打开的标签（含分屏）也能按需惰性加载（来源：README "Coming from Arc"）。
- **Chromium 浏览器沉重**：WebKit 随系统自带，安装约 35 MB、省电省内存，安全修复随 macOS 更新（来源：README，体积为项目自述、未独立核实）。
- **Chrome 扩展可用性**：可从 Chrome Web Store 直接安装，1Password、Bitwarden、Dark Reader、Tampermonkey 等主流扩展可用（来源：README "What it does" / "Extensions"，具体兼容性为项目自述）。
- **隐私顾虑**：无账号、无遥测，README 逐项列出更新、搜索建议、网站图标、过滤列表等自发连接（来源：README "Privacy"，为项目自述、未独立核实）。
- **恶意内容隔离**：图片与 PDF 在无网络、无文件访问权限的沙箱 XPC helper 中解码渲染（来源：README "How it's built"）。

## 核心功能（依据 README，注明来源）

- **Spaces 与 Favorites**：每个空间有独立颜色、图标、固定标签与文件夹；常用站点以磁贴呈现，标签/固定标签/收藏可自定义 emoji（来源：README "What it does"）。
- **Sites in spaces（类似 Arc 的 Air Traffic Control）**：可为站点指定打开的空间，甚至按路径（如 `github.com/org1` 与 `github.com/org2`）分流；独立 profile 的空间类似 Firefox 容器（来源：README "What it does"）。
- **分屏、标签组与窗口**：分屏最多四块（拖到边缘半屏、角落四分之一屏），标签可成组为文件夹，标签拖到另一个窗口免重新加载，多窗口可共享空间标签（来源：README "What it does"）。
- **命令栏与 Zigzag Mini**：⌘T/⌘L 打开命令栏，可搜标签、历史、站点并配置搜索引擎；⌃⌘N 开小窗快速浏览，⌘O 并入主窗口（来源：README "What it does"）。
- **扩展系统**：基于 WebKit 的 `WKWebExtensionController`，下载并校验 Chrome Web Store 的 CRX3 包，对 WebKit 缺失的 Chrome API 由 Zigzag 通过桥接补齐并检查扩展权限（来源：README "How it's built"）。
- **内置拦截与隐私**：约 13 万条 EasyList/EasyPrivacy/EasyList China 规则编译为 WebKit 内容规则，另有 uBlock Origin 列表、DuckDuckGo Autoconsent 拒绝 Cookie 弹窗、WebRTC IP 保护（来源：README "What it does" / "How it's built"，规则数量为项目自述）。
- **密码与安全**：密码存于系统钥匙串，经 iCloud Keychain 同步，支持 Touch ID、密码生成器、弱密码/重复密码检查与 Passkey；证书警告、HSTS 预加载列表（约 9.5 万条）处理同 Safari/Chrome（来源：README "What it does" / "How it's built"）。
- **开发者与自动化**：Developer Mode 提供控制台、Inspect Element、设备尺寸、明暗/打印样式、UA 切换与手机二维码；支持 AppleScript、Shortcuts、`osascript`，AI 应用可经 MCP 使用标签（默认关）（来源：README "What it does"）。
- **导入与 Web Apps**：可从 Arc、Chrome、Safari、Firefox、Edge、Orion 等十余种浏览器导入；网站可“Install as App”成为独立应用并出现在应用程序文件夹、Dock 与 ⌘Tab（来源：README "What it does" / "How it's built"）。
- **工程实现**：约 12 万行 Swift 6 + AppKit，按功能分模块，2000+ 测试覆盖导入、密码、扩展、安全修复；Sparkle 2 签名更新、Developer ID 签名与公证（来源：README "How it's built"，行数为项目自述、未独立核实）。
- **Apache-2.0 许可**（来源：LICENSE 文件，已核实）。

## 使用场景

- Arc 用户在 2026-12-22 关服前的迁移落点，尽量保留原有空间与工作流。
- 希望浏览器原生、轻巧、省电且能装 Chrome 扩展的 Mac 用户。
- 重视隐私、想要无账号无遥测并可自行核查网络行为的用户。
- 需要分屏、命令栏、按站点分配空间等重度标签管理的工作流。

## 适用人群

Mac（macOS 15.4+）上追求原生体验与 Arc 风格标签管理的用户，尤其是从 Arc 迁移的人。需要跨设备同步、Easels/Notes 或完整 Chrome 扩展 API 的人可能不满足；非 Mac 平台无法使用。

## 注意事项

- 研究日期实查 349 star / 43 fork，热度高于库中旧数（153 star，2026-11-15）。
- 项目很年轻（2026-10 创建），README 自承“you'll find bugs”，仍在高频迭代（最近提交 2026-11-22）。
- 缺失功能需在选型前确认：跨设备同步、Easels/Notes、部分 Chrome 扩展 API（tab groups、debugger、webRequest 等）、PAC 代理脚本（来源：README "What's missing"）。
- 仅支持 macOS 15.4 或更高版本，最低版本源于 WebKit 的扩展支持（来源：README）。
- 体积、性能、规则数量、代码行数等均为项目自述、未独立核实；隐私承诺亦为作者自述，可通过其列出的连接清单自行抓包验证。
- 内置过滤列表（除 uBlock Origin 外）随浏览器版本更新，不会单独自动刷新（来源：README "What's missing"）。

## 总结

zigzag 是一个完成度与野心都不小的 Arc 替代品：原生 Swift/WebKit 带来轻量与省电，侧边栏空间、分屏、命令栏、从 Arc 导入、Chrome 扩展与详尽的隐私说明，恰好命中 Arc 关停后的迁移窗口。349 star、v0.9.3、活跃开发，Apache-2.0 许可已核实。它仍年轻、功能有缺口、平台单一，但思路清晰、工程扎实（沙箱解码、签名更新、2000+ 测试），是 Mac 浏览器领域值得关注的新选项，故给 A 级。

## 独立评分（满分 10）

- **质量**：9 — 原生 Swift/WebKit 实现扎实，沙箱解码、签名更新、2000+ 测试，架构与安全细节考虑周到。
- **学习价值**：8 — 扩展桥接、WebKit 内容规则、按站点分流、XPC 沙箱等实现有较高工程参考意义。
- **使用价值**：9 — 精准命中 Arc 关停迁移需求，Chrome 扩展兼容、隐私友好，Mac 用户开箱即用。
- **推荐等级**：**A**