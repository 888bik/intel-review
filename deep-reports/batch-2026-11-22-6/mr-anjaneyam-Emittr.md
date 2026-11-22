# mr-anjaneyam/Emittr

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/mr-anjaneyam/Emittr
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Python
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：47 / 1
- **许可证**：MIT
- **创建时间**：2026-09-27
- **最近更新**：2026-11-20（元数据 updated_at；最近提交 2026-10-10）
- **最新 release**：Emittr v2.0.0（tag `v2.0.0`，发布于 2026-10-09）
- **一句话简介**：把已 root 的安卓手机变成硬件级 USB 键盘与触控板，可直连目标机或经局域网中继输入

## 一句话理解
Emittr 把一部已 root 的安卓手机变成真正的 USB HID 键盘与鼠标：插上 OTG 线后，目标电脑把它识别为普通键盘和触控板；也可以让手机留在目标机上当硬件桥，自己从同一 Wi-Fi 下的笔记本或另一部手机浏览器里打字，按键以物理电信号注入，目标机不需要装任何软件。

## 通俗解释
它解决的是一个很具体的尴尬：面对被锁死的工作站、气隙隔离的服务器、还没进系统的 BIOS，或者干脆没有可用键盘的机器，普通远程控制软件需要装客户端、开网络、改剪贴板，往往全被禁用。Emittr 换了个思路——没有人会禁用键盘。手机通过内核 configfs 把自己配置成标准 USB 设备，对主机来说就是一把键盘加一只五键鼠标，所以 Windows、macOS、Linux、ChromeOS、游戏主机乃至主板固件都原生认得。README 非常长（四万余字符），把原理、场景、命令、API、FAQ 都写全了，信息量充足。不过需要 root、内核开启 USB Gadget ConfigFS、以及 OTG 数据线，门槛并不低；README 里多个演示动画标注为占位，所谓 1000 WPM 打字、Sub-5ms 延迟均为自述、未独立核实。

## 它不是什么
1. 它不是远程桌面或 VPN：不传输屏幕画面，也不在目标机上建立网络连接，只单向注入 HID 报告。
2. 它不是 BadUSB 橡皮鸭那类一次性载荷工具：强调实时双向交互与可视化控制，而不是盲发脚本。
3. 它不是剪贴板同步工具：README 明确说 0 次主机剪贴板写入，秘密不进入目标机内存。
4. 它不是即插即用的普通外设：必须先 root 手机、确认内核支持 ConfigFS，并运行安装脚本，不是买来就能用的产品。
5. 它不是已完成形态：DuckyScript、多媒体键、陀螺仪空中鼠标、多国键盘布局等仍在路线图中。

## 痛点
- 受限工作站禁 U 盘、禁剪贴板、禁远程软件，但没人禁键盘，传统工具撞墙。
- 气隙或隔离机器没有网络，装 agent 有审计要求，需要零落地痕迹的输入方式。
- 密码、BitLocker 密钥等长凭证在客户机上输入，怕被剪贴板抓取或 EDR 监控。
- 无头服务器、树莓派、笔记本键盘坏了卡在 BIOS/GRUB，操作系统和网络栈还没起来。
- Wi-Fi 鼠标类 App 要装 exe、过防火墙、延迟高，还可能把按键泄露到云。

## 核心功能（依据 README，注明来源）
- 双工作模式：硬件 USB HID 直连（手机自身即键盘鼠标），或本地网络中继（手机留在目标机上，浏览器经 WebSocket 转发按键）（来源：README The Two Superpowers）
- 批量文字输入：可调每键 5ms 至 60ms 速度、1 至 5 秒倒计时、进度条与中止按钮，并自动规范化弯引号、破折号、不间断空格与 CRLF/LF 换行（来源：README Bulk Text Typer）
- 实时键盘镜像：复用 Gboard/SwiftKey 等输入法，提供 Tab、Esc、退格与四向方向键辅助键，可折叠击键记录（来源：README Live Keyboard Mirror）
- 精确触控板：低延迟光标、双指滚动、轻触点击、左右中键与 0.5 倍至 2.0 倍灵敏度（来源：README Precision Trackpad & Gestures）
- 双 Xbox 摇杆与原生横向滚动：发送 USB-IF AC Pan（0x0238）报告，由系统原生转为 WM_MOUSEHWHEEL（来源：README Dual Xbox Thumbsticks & Native AC Pan Scrolling）
- 卡键防护：一键 Unstick 发出全零释放报告，后台监视 UDC 状态并在重连时自动清理修饰键（来源：README Failsafe Modifier Guard）
- 命令行与 API：提供 emittr 守护进程控制命令、usbtype 注入命令，以及 8088 端口上的 REST 与 WebSocket 接口（来源：README Command Line Control、REST & WebSocket API Reference）
- 安装与跨机兼容：apt 装 .deb、git clone 后跑 install.sh、Magisk 开机服务；gadget 描述符按自供电 2mA 配置以兼容手机和电视的 OTG 供电限制（来源：README Quick Start、Low-Power OTG Host Compliance）

## 使用场景
- 在禁用 U 盘与远程工具的受管工作站上，用手机作为键盘输入脚本或命令。
- 把密码、2FA 令牌从个人手机的密码库经硬件桥注入客户机，避免落进剪贴板。
- 对隔离或隔离取证环境做清洁诊断，只喂入命令、不挂载存储，留下干净审计轨迹。
- 在 BIOS、UEFI、GRUB、LUKS 解锁界面等操作系统启动前的阶段操作无头机器。
- 给挂在墙上或玻璃后的数字标牌、自助终端接一根维护线即可获得完整外设。

## 适用人群
适合运维、渗透测试与安全从业者，以及经常面对锁定环境、无头设备和预启动固件的工程师。它需要一台已 root、内核支持 USB Gadget ConfigFS 的安卓手机，熟悉 Termux、NetHunter 或 chroot 环境的人才能真正用起来；只想找普通无线键鼠替代品的普通用户会觉得门槛过高。

## 注意事项
- 前置条件硬：必须 root，内核需开启 CONFIG_USB_CONFIGFS 与 CONFIG_USB_CONFIGFS_F_HID，并准备 OTG 数据线。
- 性能数字如 1000 WPM、Sub-5ms 为自述、未独立核实；README 中三个演示位仍标为占位。
- 本地网络中继模式下手机连着 Wi-Fi，目标机虽隔离但桥本身在无线网内；README 建议保持私有 Wi-Fi，需要绝对离线时改用屏幕直控模式。
- 安全能力仍在路线图：会话配对 PIN、令牌授权、仅 localhost 模式均为计划项，当前 API/WebSocket 认证尚未落地。
- 许可证为 MIT；项目 47 星、1 fork，社区验证仍然有限。

## 总结
Emittr 的切入点清晰而讨巧：不去和终端管控策略正面对抗，而是把自己伪装成最不可能被禁的设备——键盘。它把 configfs gadget、AC Pan 报告描述符、修饰键释放保护这些底层细节包装成 Material 3 的 PWA 操作台，还给了完整的 CLI 与 API，README 的工程完成度在同类小项目里相当突出。代价是硬件与 root 门槛高，演示素材未齐、安全认证未完成、社区规模小。把它当作一个思路鲜明、可玩的硬件 HID 工具看待最合适，离生产级通用方案还有距离。

## 独立评分（满分 10）

- **质量**：8 — README 结构完整、安装与 API 说明详尽，但社区规模很小、部分能力仍属路线图
- **学习价值**：7 — USB HID gadget、configfs、AC Pan 报告描述符等实现思路有学习价值
- **使用价值**：7 — 适合有 root 安卓设备的运维与渗透场景，但门槛高、受众窄
- **推荐等级**：**B**
