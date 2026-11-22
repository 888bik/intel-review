# FURSOY/fursoy-cpu

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/FURSOY/fursoy-cpu
- **分类**：developer-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：C++
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：66 / 8
- **许可证**：GitHub API 的 SPDX 标识为 `NOASSERTION`，需以仓库 LICENSE 文件为准（许可证细节未核实）
- **创建时间**：2026-09-06
- **最近更新**：2026-11-19（元数据 updated_at；最近提交 2026-11-16）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：一个用 SystemVerilog 写的 64 位乱序超标量 RISC-V 处理器（RV64IMAFDV + Zba/Zbb），能在 Sv39 虚存下启动 Linux 6.6.50，并宣称在 Nangate 45nm 达到 500MHz。

## 一句话理解

这是一个从零写的“真·高性能”开源 RISC-V CPU：乱序执行、超标量、向量扩展，还跑通了 Linux，属于教学/研究级别的处理器设计。

## 通俗解释

自己写 CPU 是计算机工程的“终极作业”。fursoy-cpu 的 Slice-1 是一个 2-wide 乱序超标量核心（来源：README “Microarchitecture Highlights”）：多级前端/后端（取指到提交）、128 项重排序缓冲（ROB）、16 检查点推测恢复、物理寄存器文件（160 整数/64 浮点/64×256 位向量）、解耦发射队列、分支预测（512 项 BTB + TAGE-Lite + IBP + RAS）、Sv39 虚存（DTLB/ITLB/PTW）、非阻塞缓存（16KiB L1I/L1D）、以及 SoC 外设（CLINT、PLIC、UART、自定义 SBI v2.0）（来源：README，细节为项目自述）。README 给出 Physical Synthesis & STA（Nangate 45nm @ 500MHz）、Sail-RISC-V 差分协同仿真、Linux 6.6.50 + BusyBox 启动与 CPI 分解等验证（来源：README headings，**这些数字为项目自述、未独立核实**）。66 star、C++（实际 SystemVerilog）、GPL 类许可（API 为 NOASSERTION，需核对 LICENSE）。

## 它不是什么

1. **不是商业 IP 核**。它是开源研究/教学处理器（来源：README 定位）。
2. **不是简单顺序核**。它明确 2-wide 乱序超标量 + 向量（来源：README）。
3. **不是只有仿真**。有综合、STA、Linux 启动等完整验证流程（来源：README headings）。
4. **不是标准许可标识清晰**。GitHub API 为 NOASSERTION，实际许可证需看仓库 LICENSE（未核实）。
5. **不是新近完成**。2026-09 创建，66 star、无 release，仍在开发（来源：GitHub）。

## 痛点

- **开源高性能 CPU 稀缺**：多数教学核是顺序/简单核，乱序超标量 + Linux 启动很少见。
- **验证链复杂**：RTL lint、差分仿真、综合 STA、Linux 启动需要完整流程；项目提供脚本（来源：README Quickstart）。
- **RVV 向量支持**：向量乱序执行与索引访存实现难度高。
- **可复现性**：README 给出 Ubuntu 24.04/WSL2 的前置条件与验证命令（来源：README）。

## 核心功能（依据 README，注明来源）

- **2-wide 乱序超标量核心**：多级流水线、128 项 ROB、16 检查点恢复、物理寄存器文件、解耦发射队列（来源：README Microarchitecture Highlights）。
- **分支预测**：BTB + TAGE-Lite + IBP + RAS（来源：README）。
- **Sv39 虚存与非阻塞缓存**：DTLB/ITLB/PTW、16KiB VIPT L1、MSHR/WBB（来源：README）。
- **SoC 与外设**：交叉开关、CLINT、PLIC、UART、SBI v2.0（来源：README）。
- **完整验证**：Verilator lint、Sail-RISC-V 差分协同仿真（8 套件）、Nangate 45nm @ 500MHz 综合与 OpenSTA、Linux 6.6.50 + BusyBox 启动（来源：README Quickstart，自述）。
- **CPI 分解**：cycle-exact 8 类 CPI 守恒分解（来源：README headings）。

## 使用场景

- 计算机体系结构教学与研究：学习乱序执行、分支预测、虚存。
- RISC-V 生态验证与工具链实验。
- 想复现“开源核启动 Linux”的工程师。

## 适用人群

体系结构研究者、CPU 设计爱好者、RISC-V 开发者。普通软件开发者用不上。

## 注意事项

- 许可证 API 为 NOASSERTION，商用前必须核对仓库 LICENSE（未核实）。
- “500MHz @ 45nm”“Linux 启动”“CPI 分解”等均为项目自述、未独立核实。
- 66 star、8 fork、0 issue、无 release，早期项目；完整启动验证需 2.5–3 小时（README 自述）。
- 语言标注 C++ 但实际核心是 SystemVerilog（topics 含 systemverilog）。

## 总结

fursoy-cpu 是一个雄心勃勃的开源 RISC-V 乱序超标量核：从微架构到综合、STA、Linux 启动的验证链相当完整，是体系结构与 CPU 设计的高质量一手资料。66 star 的小众项目，性能与启动声明需自行复现，许可证需核对。对做 CPU/体系结构的人非常值得一读。

## 独立评分（满分 10）

- **质量**：8 — 乱序超标量核 + 完整验证链，工程量大；极新、数字自述、许可待核实。
- **学习价值**：9 — 体系结构一手资料，学习价值极高。
- **使用价值**：6 — 普通软件开发者用不上，受众极窄。
- **推荐等级**：**B**
