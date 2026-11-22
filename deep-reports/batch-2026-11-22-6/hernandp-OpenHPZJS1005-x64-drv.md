# hernandp/OpenHPZJS1005-x64-drv

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/hernandp/OpenHPZJS1005-x64-drv
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：C++
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：0 / 0
- **许可证**：GPL-2.0
- **创建时间**：2026-09-17
- **最近更新**：2026-11-20（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：Driver 0.2.0（tag `0.2.0`，发布于 2026-11-22）
- **一句话简介**：借助 Codex 写的、给初代 HP LaserJet 1005 用的 Windows 10/11 x64 开源打印驱动

## 一句话理解
一个开源 Windows x64 打印驱动，让 2000 年代的原版 HP LaserJet 1005 能在 Windows 10/11 上继续打印。

## 通俗解释
README 讲了一个真实故事：作者从父母家角落里翻出旧打印机，为做热转印艺术作品需要一个 Win11 驱动，于是借助 Codex、参照网上规范和 CUPS 的 ZjStream 开源实现写了一个。README 很完整，含安装、签名证书、构建、驱动架构与限制；项目 0 star，刚入库，尚无社区验证。驱动能稳定打印文本、位图和多页文档属作者自述、未独立核实。

## 它不是什么
1. 不支持 LaserJet P1005 / M1005，那两个型号用不同驱动。
2. 不是通用打印机驱动，只认 USB ID 03f0:1317 的原版 LaserJet 1005 系列。
3. 不是开箱即用的正式签名驱动，使用测试证书自签名，需要用户手动导入到受信任根与受信任发布者。
4. 不包含 HP 固件，用户要从原版 HP 安装包里取 sihp1005.img。
5. 没有验证 Letter 纸张下的物理打印；README 说 A4、600dpi、单面是已验证路径。

## 痛点
- HP 在 XP 时代就停止了对这台机器的软件支持，现代 Windows 没有可用驱动。
- 老硬件扔掉可惜，但自己写打印驱动门槛极高。
- ZjStream 这类协议在 CUPS 有开源实现，Windows 侧却没有现成方案。
- 自签名驱动在 Windows 上要手工导入证书，普通用户很难完成。

## 核心功能（依据 README，注明来源）
- 实现 IPrintOemUni 的渲染 DLL 与监控 DLL，管线为 GDI 加 Unidrv 光栅化，经 lj1005render.dll 与 JBIG 加 ZJS 到后台打印，再由 lj1005monitor.dll 送到 USB 或并口（来源：README Driver architecture）
- 支持文本、位图与多页文档打印，使用 A4、600dpi、单色、单面作为已验证配置（来源：README Purpose of this project）
- 提供 Have Disk 手动安装流程与 get-hp-firmware.ps1 脚本，从 HP 官方包提取并校验 sihp1005.img（来源：README Install with Have Disk）
- 提供基于 Visual Studio C++ 与 Windows SDK 的构建脚本，Release 构建会运行七项驱动测试（来源：README Build and test）
- 提供打包 mini ZIP 与测试证书生成脚本，输出自签名的 tester 安装包与 SHA256SUMS.txt（来源：README Build the mini ZIP）

## 使用场景
- 手里还有原版 HP LaserJet 1005、想让它在新电脑上继续工作
- 做 toner transfer 等艺术或手工打印，需要老激光打印机
- 想研究 Windows 用户态打印驱动与 ZJS 协议如何落地
- 想看一个由 AI 辅助完成、但带测试与文档的硬件逆向项目

## 适用人群
有旧 LaserJet 1005 且愿意折腾测试证书与手动安装的个人用户；对 Windows 打印体系、驱动逆向感兴趣的开发者。不适合需要企业签名驱动或多型号支持的环境。

## 注意事项
- 支持范围被明确限定为 Windows 10 22H2 x64 或 Windows 11 x64、一台 USB 打印机、A4 纸。
- 驱动用测试证书签名，未做正式代码签名；作者说未来希望用 SignPath 之类的方案。
- README 自述 Letter 纸张在 A4 纸下会重复出空白页，Letter 的物理打印仍未验证。
- 作者自述借助 Codex 编写，依赖 CUPS 的 ZjStream 开源实现与网上规范；实际稳定性需自行验证。
- 许可证：GPL-2.0（GitHub API 标注，README 表述为 GPL-2.0-or-later）。
- 0 star、刚入库，尚无社区验证；已有 Release 0.2.0。

## 总结
一个目标极小但工程做得相当完整的驱动项目：从固件提取、测试签名、构建测试到驱动架构都有文档，也让老硬件延寿有了可复现的路径。但它只支持一台具体型号和 A4 配置，0 star、无广泛验证，且需要用户自己处理证书与固件，仍属爱好者级作品。

## 独立评分（满分 10）

- **质量**：6 — 真做了驱动架构、构建脚本和测试文档，但支持范围极窄且只有 0 star
- **学习价值**：6 — 能看到 C++ Unidrv 插件、JBIG 加 ZJS 管线和自签名安装流程的完整工程细节
- **使用价值**：6 — 只对持有原版 LaserJet 1005 且用 Win10 或 Win11 x64 的人有用
- **推荐等级**：**C**
