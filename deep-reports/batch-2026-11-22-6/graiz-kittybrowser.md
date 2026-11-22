# graiz/kittybrowser

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/graiz/KittyBrowser
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Rust
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：47 / 5
- **许可证**：MIT
- **创建时间**：2026-11-13
- **最近更新**：2026-11-21（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：v0.2.0（tag `v0.2.0`，发布于 2026-11-21）
- **一句话简介**：用 kitty 图形协议在终端窗格里画出的真浏览器，可与编码代理共享页面并通过命令行驱动

## 一句话理解
在终端窗格里开一个真正能点、能滚、能看图的浏览器，让编码代理和你盯着同一个页面干活。

## 通俗解释
KittyBrowser（命令行工具 kb）用无头 Chrome 加载网页，再用 kitty 图形协议把每一帧按像素画进终端窗格，所以你看到的是真实排版、字体和图片，而不是文本近似。README 约 1.7 万字符，从安装、按键、代理集成、配置到排障非常完整，47 star、5 fork，v0.2.0。它的目标场景非常具体：编码代理在旁边窗格打开 kb，你俩看同一页面；代理通过 kb ctl 加载 dev server、查控制台错误、点流程、切手机宽度、读文档，你在旁边实时看着。GitHub 实际名称是 graiz/KittyBrowser（旧地址会重定向），本报告按任务给定保留 graiz/kittybrowser 作为标题与文件名。README 自述「early but usable」，日常在 macOS 的 Rex 与 Ghostty 使用；kitty、WezTerm 应该可用但未测，Linux 能编译但未测。

## 它不是什么
1. 不是终端文本浏览器（如 w3m）：它画的是 Chrome 渲染后的像素，不是文字近似。
2. 不是独立桌面浏览器：依赖支持 kitty 图形协议的终端与本机 Chrome/Chromium/Edge/Brave。
3. 不是 tmux 友好工具：README 明确说在 tmux / screen / Zellij 中不可用，Terminal.app 与 iTerm2 也不支持。
4. 不是 Windows 原生应用：需要 Unix 终端与 Unix socket，只能在 WSL 2 里跑（未测）。
5. 不是默认开放的代理后门：MCP server 默认关闭，socket 仅当前用户可访问，敏感文件阻止代理上传。

## 痛点
- 编码代理说「页面没问题」时，你看不见它到底看了什么
- 终端里想看真实页面布局，却只能用 curl 或文本浏览器猜
- 让代理操作浏览器时，担心它读到 SSH key、.env 或浏览器 cookie
- 多个代理与多个页面难以分别保持登录状态

## 核心功能（依据 README，注明来源）
- 像素级浏览器：无头 Chrome 加 kitty 图形协议，真实布局、字体、图片，支持点击、滚动、输入、查找、缩放、复制（来源：README 开头介绍）
- 代理共视：kb ctl open/check/click/screenshot/viewport/outline/text 等命令，让代理在旁侧窗格加载页面、报告控制台错误与弹窗、读大纲或某节文本（来源：README For coding agents）
- 技能与 MCP：kb skill install 把用法教给 Claude Code 等读技能的代理；kb mcp on 提供同一套命令的 MCP server，kb mcp serve -i NAME 可限定只驱动指定命名实例（来源：README For coding agents）
- 无头模式：kb --headless 后台运行并以 id 标识，默认 10 分钟无命令退出；--name 保留独立 profile 与登录态（来源：README For coding agents）
- 完整浏览交互：地址补全来自本地历史、窗口/弹窗切换、OSC 52 复制到系统剪贴板（含 SSH）、按终端背景切换深浅色、鼠标在链接/文本/控件上变形（来源：README Using kb）
- 多实例与命名：kb list 编号，命名实例始终使用 ~/.kittybrowser/profiles/NAME，避免 Chrome 不共享 profile 的问题（来源：README Using kb）
- 调试命令：console、network、eval、style --why、box、listeners、storage，以及选择器语法（CSS、text=、@N、:focus）（来源：README For coding agents）
- 安全边界：socket 位于 ~/.kittybrowser 且仅属主可访问；upload 阻止 ~/.ssh、~/.aws、.env、私钥、cookie 类文件，需用户在页面文件选择器中亲自选（来源：README Security）
- 自检与排障：kb doctor 检查终端、Chrome、tmux/Zellij 与数据目录；README 提供 SSH、缩放、Apple Silicon 架构等常见问题解法（来源：README Troubleshooting）

## 使用场景
- 让代理打开本地 dev server，你在旁边窗格实时看它点击与报错
- CI/脚本中无头运行，kb ctl check 遇控制台错误或失败请求即退出 1
- 用 kb ctl viewport 375x800 快速看手机宽度布局
- 在有 kitty 图形协议的终端里远程 SSH 浏览网页，无需图形转发

## 适用人群
在 Ghostty/kitty/WezTerm/Rex 里工作、重度使用编码代理且希望「看得见」代理浏览过程的开发者；也适合需要脚本化浏览器操作的 CI 场景。

## 注意事项
- 47 star、5 fork，v0.2.0，README 自述 early but usable
- 需 kitty 图形协议终端与 Chrome/Chromium/Edge/Brave；tmux/screen/Zellij、Terminal.app、iTerm2 不支持
- Linux 能编译但未测试；Windows 仅 WSL 2 且未测
- 任何以你身份运行的程序都能通过 socket 在 kb 已登录页面执行 JS；开启 MCP 前请理解这一点
- 地址补全只用本地历史，不发往建议服务；cookie 与 profile 独立于日常 Chrome
- README 中「a scroll step is a few KB」等为自述，未独立核实
- GitHub 实际名称为 graiz/KittyBrowser，标题与文件名按任务给定保留 graiz/kittybrowser

## 总结
KittyBrowser 把「浏览器」和「编码代理」放进同一个终端窗格：像素级渲染保证你看到的就是代理看到的，kb ctl 与 MCP 又把操作变成可脚本化、可审计的命令流，安全边界也写得比同类工具细致。47 star、v0.2.0、文档完整度很高，但 macOS 为主、Linux/Windows 未测，且 socket 权限模型要求用户理解本机威胁。对符合终端条件、又常和代理并肩调页面的人来说，这是很值得试的一环。

## 独立评分（满分 10）

- **质量**：8 — 终端里的真浏览器加 kb ctl 代理接口，设计巧妙且文档完备；但 v0.2 早期、macOS 为主、Linux 未测
- **学习价值**：7 — kitty 图形协议渲染、无头 Chrome 与 MCP 控制通道的组合有很高参考价值
- **使用价值**：8 — 和编码代理共用同一屏页面，边看边让它检查控制台、点击流程、调手机视口
- **推荐等级**：**B**
