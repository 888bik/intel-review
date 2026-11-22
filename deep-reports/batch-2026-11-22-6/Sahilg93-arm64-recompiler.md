# Sahilg93/arm64-recompiler

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/Sahilg93/arm64-recompiler
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：C
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：4 / 0
- **许可证**：MIT
- **创建时间**：2026-11-15
- **最近更新**：2026-11-16（元数据 updated_at；最近提交 2026-11-15）
- **最新 release**：v0.1.0: Plink natively, PuTTY GUI and SSH in hosted mode（tag `v0.1.0`，发布于 2026-11-15）
- **一句话简介**：把 Windows x86-64 PE 程序静态提升为 LLVM IR，再编译成原生 Apple Silicon ARM64 Mach-O 的再编译器

## 一句话理解
一个静态再编译器：读取真实的 Windows x86-64 可执行文件，提前把机器码提升到 LLVM IR，再编译成 macOS ARM64 原生程序，被提升的代码在运行时不做逐指令模拟。

## 通俗解释
传统做法是靠 Wine 加 Rosetta 动态翻译，或者逐条解释执行；这个项目选择另一条路：在构建时把整个 PE 程序反汇编成 LLVM IR，补齐一个最小的 Win32/CRT 运行时，然后交给原生编译器生成 Mach-O。README 列出的已验证目标包括 PuTTY Plink 0.83 原生运行 -V、PuTTY 0.83 图形界面与 SSH 会话、Plink 通过 SSH 执行远程命令、jq 1.8.2 与 ripgrep 14.1.1 的真实负载，以及运行时 DLL 加载、命名导出、回调与线程本地存储。

README 的性能表给出具体数字：Plink -V 在重新编译后中位耗时 42.31 毫秒，相比 Wine 加 Rosetta 的 262.23 毫秒快 6.20 倍，峰值内存 5.51MB 对 17.74MB；托管模式下 PuTTY 图形界面冷启动快约 1.11 倍，热运行的 SSH 命令快约 1.54 倍，但内存高出 1.6 到 2 倍。这些都是项目自述、未独立核实的测量，作者也注明测试条件与机器状态影响很大。

项目并没有把自己包装成万能工具。README 用一整节列出限制：图形与网络程序仍需 hosted 模式，依赖 Wine 11.0、Wine prefix 与在 Rosetta 下运行的 wineserver，没有独立应用包；指令覆盖由已经试过的程序驱动，AES-NI、SHA、无进位乘法和 AVX 都未提升；部分运行时生成代码仍退回 x86 解释器；ripgrep 只能单线程；Direct3D 尚未开始。另有 WAR_STORIES.md 记录十一个具体坑，从 .pdata 函数发现、lock cmpxchg 标志位错误到 TLS 回调与 iCloud 删除源文件，可读性很强。

## 它不是什么
1. 不是通用的运行任意 Windows 程序工具，README 明确否认这一点。
2. 不是纯静态翻译的完美实现，图形与网络场景仍依赖 Wine 的动态层与 Rosetta。
3. 不是跨平台方案，目标平台限定在 Apple Silicon macOS，需要 Clang、Python 与 LLVM。
4. 不是生产级兼容层：指令覆盖有限，硬件加速加密路径走 --trace 规避，Direct3D 未实现。
5. 不是全部代码都 MIT：Wine 的 host-mode 补丁遵循 LGPL 2.1 或更高版本，corpus 中的第三方二进制各有原许可。

## 痛点
- 想在 Apple Silicon 上原生运行手头几个 Windows 命令行工具，但不接受逐指令模拟的性能损耗。
- 动态二进制翻译在启动速度与内存占用上不理想，尤其对短命的命令行进程。
- Windows 运行时有大量未文档化行为，例如 TLS 回调、API Set 前缀、UCRT 的 FILE 结构，缺少可复用的实现参考。
- 社区里缺少把真实 PE 程序完整提升到 LLVM IR 并公开的工程案例。

## 核心功能（依据 README，注明来源）
- 静态把 Windows x86-64 PE 提升为 LLVM IR 并编译为原生 ARM64 Mach-O，运行时不做指令级模拟（来源：README 首段）
- 支持 Plink 0.83 原生 -V、PuTTY 0.83 图形与 SSH、jq 1.8.2、ripgrep 14.1.1 等真实二进制（来源：README What works today 与 Corpus status）
- 自动解析 .pdata 异常目录发现函数入口，精确模拟 lock cmpxchg 的 eax 标志位（来源：README How it works 中 Whole-program lifter）
- 最小 Win32/CRT 运行时，涵盖 TLS 回调、FLS、堆与内存保护、临界区、API Set 前缀路由与 UCRT stdio（来源：README How it works 中 Runtime environment）
- hosted 模式把程序与 Wine 的 Windows 侧 DLL 一起提升，仅系统调用层使用原生代码（来源：README How it works 中 Hosted mode）
- 共享提升模块缓存，DLL 编译一次即可被多个程序复用，显著降低二进制体积（来源：README Shared lifted modules）
- 包含回归测试、多个端到端 fixture 与 WAR_STORIES.md 故障复盘（来源：README Quickstart 与 War stories）

## 使用场景
- 在 Apple Silicon Mac 上原生跑 Plink、jq 这类已被验证的 Windows 命令行程序。
- 研究二进制翻译、LLVM IR 提升与 Windows 运行时仿真的工程实现。
- 复现或扩展作者已验证的二进制列表，做移植边界实验。
- 阅读 WAR_STORIES.md 学习静态重编译中的边缘案例排查方法。

## 适用人群
熟悉 LLVM、Capstone、C 与 Windows PE 内部结构的系统程序员和逆向工程研究者；只是想在 Mac 上偶尔用一下 Windows 软件的用户更适合 Wine 或虚拟机。

## 注意事项
- 所有性能数字均为自述、未独立核实，且作者强调冷启动受 wineserver 影响、机器上有其他负载。
- 图形与网络程序仍需 Wine 安装、Wine prefix 与 Rosetta，无法脱离 Wine 生态独立运行。
- AES-NI、SHA、无进位乘法、AVX 与 Direct3D 未实现；PuTTY 依赖 --trace 探测到硬件加速路径时会陷阱。
- 托管程序内存占用是原始的 1.6 到 2 倍，作者称尚未调查原因。
- 项目仅 4 star、0 fork、11 个 open issues，只有 v0.1.0 一个发布，高度实验性。

## 总结
arm64-recompiler 是这批项目里技术野心最突出的一个：用静态提升加原生编译的方式，把真实 Windows 二进制搬到 Apple Silicon，并在 Plink、jq、PuTTY 等目标上给出了可复现的验证路径。README 把已实现与未实现的部分分得很清楚，WAR_STORIES.md 更是难得的一线排错记录，学习价值极高。但它的实际可用面很窄，4 star 的体量也意味着几乎没有社区验证，性能数字为自述、未独立核实。把它当作二进制翻译研究样本或技术读物最合适，而非日常兼容层。

## 独立评分（满分 10）

- **质量**：8 — 技术雄心与完成度远超其 4 star 体量，但高度实验性
- **学习价值**：8 — 从 PE 到 LLVM IR 的静态提升与 Win32 运行时仿真极具学习价值
- **使用价值**：6 — 目前只覆盖少数二进制，实际可用面很窄
- **推荐等级**：**B**
