# jdefrancesco/SrcWranglr

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/jdefrancesco/SrcWranglr
- **分类**：developer-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Rust
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：28 / 7
- **许可证**：MIT
- **创建时间**：2026-11-03
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-17）
- **最新 release**：v1.1.0 — use-after-free detection and lifetime matrix（tag `v1.1.0`，发布于 2026-11-17）
- **一句话简介**：图形化 C/C++ 源码导航与审计工具：代码关系图、跨引用、指针生命周期缺陷检测，并可接入 Semgrep/cppcheck/flawfinder 和 AI 代理。

## 一句话理解

它像给 C/C++ 代码做“X 光机”：把调用关系画成图、按 X 看交叉引用、跟踪指针生命周期找 use-after-free/double-free，还能把外部扫描器和 AI 代理的发现汇到同一处。

## 通俗解释

理解大型 C/C++ 代码库很难：谁调谁、某个指针在哪里被释放后又用了、哪次提交引入了安全修复。SrcWranglr（作者在学习 Rust 时从 Python 原型重写而来，来源：README About）用图形界面解决：Trail 视图把选中符号放中间、调用者放左、被调/使用的放右；按 X 看 IDA 式交叉引用；Call hierarchy 看调用树（来源：README Features）。分析方面：约 300 个 C/POSIX/Windows/内核/弱加密危险函数的参数感知检查、过程内指针生命周期跟踪（use-after-free CWE-416、double-free CWE-415，分支感知分级）、导入 CodeQL/SARIF、值追踪、fuzz 目标排序与 harness 草稿（来源：README）。它还集成 Semgrep/cppcheck/flawfinder（可选项，且忽略被审代码里的抑制注释）、提供 MCP server 给代理、有 prompt-injection tripwire（来源：README）。28 star、v1.1.0。

## 它不是什么

1. **不是 IDE**。它是独立的图形化源码导航/审计工具（来源：README Quick start）。
2. **不是自己编译代码**。索引直接读源码，尊重 .gitignore，不需要编译数据库；CodeQL 也由用户自己跑（来源：README）。
3. **不是所有外部扫描器都自带**。Semgrep/cppcheck/flawfinder 是可选需另装（来源：README）。
4. **不是跨平台已验证**。README 说开发测试在 macOS，Linux/Windows 尚未测试（来源：README）。
5. **不是小而简单**。28 star 但 README 约 1.9 万字符、功能面很宽（来源：GitHub 与 README）。

## 痛点

- **C/C++ 代码关系难可视化**：图 + 交叉引用帮助理解。
- **指针生命周期 bug 难查**：过程内跟踪释放后再用/二次释放。
- **安全审计工具分散**：内置检查 + 外部扫描器 + SARIF 汇总到 Findings，可记录 verdict。
- **代理需要结构化代码上下文**：MCP server 让代理查符号/调用图/交叉引用而非 grep 全文件（来源：README）。

## 核心功能（依据 README，注明来源）

- **Trail 调用关系图 + 交叉引用 + 调用层次**（来源：README Features）。
- **危险函数检查与指针生命周期分析**：约 300 个函数、use-after-free/double-free 分支感知（来源：README）。
- **外部工具集成**：Semgrep、cppcheck、flawfinder、CodeQL/SARIF（来源：README）。
- **值与 fuzz 辅助**：值追踪、fuzz 目标排序、libFuzzer/AFL++ harness 草稿（来源：README）。
- **AI 代理**：MCP server、AAVR 漏洞研究技能、内置 Agent 面板、prompt-injection tripwire（来源：README）。
- **审计组织**：Notes、Journal、Sessions、Audit bundles（来源：README）。
- **MIT 许可**（来源：GitHub API）。

## 使用场景

- 审计/理解大型 C/C++ 代码库。
- 安全研究与漏洞挖掘（含 fuzz 目标选择）。
- 让 AI 代理以结构化方式读代码。

## 适用人群

安全研究员、C/C++ 工程师、代码审计人员。非 C/C++ 项目用不上。

## 注意事项

- MIT 许可。
- 28 star、7 fork、1 issue、v1.1.0（2026-11-17），早期。
- 仅 macOS 测试；需要 Rust 1.95+（来源：README）。
- 分析基于语法与启发式，README 明确需人工复核；代理功能需 API key。

## 总结

SrcWranglr 把“代码导航 + 安全审计 + 代理协作”整合进图形工具，指针生命周期分析与外部扫描器汇总很实用。28 star、仅 macOS、早期，但对 C/C++ 安全审计方向很有价值。

## 独立评分（满分 10）

- **质量**：8 — 代码导航+指针生命周期分析+外部工具+代理 integration 功能面很全；仅 macOS、早期。
- **学习价值**：8 — C/C++ 源码图分析与 use-after-free 检测实现有很高参考价值。
- **使用价值**：7 — C/C++ 安全审计价值高，平台受限。
- **推荐等级**：**B**
