# tristanbuckmaster/navier-stokes

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/tristanbuckmaster/navier-stokes
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Lean
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：338 / 25
- **许可证**：未核实（仓库无 LICENSE 文件，GitHub API 未标注许可证）
- **创建时间**：2026-11-20
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-20）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：L. Alpöge 与 T. Buckmaster 的论文《Finite-time blow-up for the Navier–Stokes equations》及配套的 Lean 4 形式化，包含 79 页论文与 Mathlib 形式证明。

## 一句话理解

这是数学前沿成果的“可机器检查版本”：一篇关于纳维-斯托克斯方程有限时间爆破的 79 页论文，配上 Lean 4 + Mathlib 的形式化证明，让关键定理能被计算机逐行核验。

## 通俗解释

纳维-斯托克斯方程描述流体运动，其解是否会在有限时间内“爆破”（某些量趋于无穷）是著名的数学难题。这个仓库收录了 L. Alpöge 与 T. Buckmaster 的论文（paper/finite-time-blow-up-navier-stokes.pdf，2026-11-20 版本，79 页）及配套 Lean 4 形式化（lean/ 目录，Lean v4.33.1 + Mathlib，版本在 lean/lakefile.toml 固定）（来源：README Contents）。README 还说明 lean/README.md 解释形式化陈述如何对应论文的 Theorem 1.1、Theorem 7.1、Corollary 7.2 与 Appendix B，以及形式证明与论文证明的差异；lean/NOTICE 用文字说明每个陈述文件与已证内容（来源：README）。仓库有 “What is and is not here” 与 “State of the Lean text” 章节，明确范围与状态（来源：README headings）。338 star、25 fork、2026-11-20 创建，无 GitHub release；**仓库无 LICENSE 文件，GitHub API 也未标注许可证（未核实）**。

## 它不是什么

1. **不是 CFD 软件或数值模拟工具**。它是数学论文 + 形式化证明（来源：README）。
2. **不是应用开发库**。它服务数学研究（来源：README）。
3. **不是完整“所有结果已形式化”**。README 有 What is and is not here 与状态说明（来源：README headings）。
4. **不是有明确许可证**。无 LICENSE 文件、API 未标注（未核实）。
5. **不是入门教程**。面向数学/形式化专家（README 语境）。

## 痛点

- **顶级数学证明难验证**：Lean 形式化让定理可被机器检查。
- **论文与形式化对应关系**：README 专门解释映射与差异（来源：README）。
- **可复现**：固定 Mathlib 版本与 NOTICE 说明（来源：README）。

## 核心功能（依据 README，注明来源）

- **论文**：79 页 PDF（2026-11-20 版本）（来源：README Contents）。
- **Lean 4 形式化包**：Lean v4.33.1 + Mathlib，版本固定（来源：README）。
- **对应关系文档**：lean/README.md、lean/NOTICE（来源：README）。
- **范围与状态说明**：What is and is not here、State of the Lean text（来源：README headings）。
- **许可证**：未核实（仓库无 LICENSE 文件，API 未标注）。

## 使用场景

- 数学研究者核验/研究该爆破结果。
- 形式化验证社区研究大型数学证明的工程化。
- Lean/Mathlib 学习者阅读真实前沿形式化。

## 适用人群

偏微分方程/数学研究者、Lean 形式化专家。普通开发者用不上。

## 注意事项

- **分类为本报告自行判断**（新入库项目，情报库无分类）。
- **许可未核实**：无 LICENSE 文件、API 未标注，使用前需联系作者（来源：GitHub contents 与 API）。
- 338 star 但创建仅两天（2026-11-20），极新。
- 数学内容未独立核实（本报告的“论文存在与形式化”基于 README）；不给具体数学结论背书。

## 总结

navier-stokes 仓库把顶级数学成果与 Lean 形式化放在一起，学术与形式化价值极高。338 star、极新、无许可证、受众极窄，普通开发者基本用不上，但对数学与形式化社区是重要资料。

## 独立评分（满分 10）

- **质量**：8 — 纳维-斯托克斯有限时间爆破的论文 + Lean 4 形式化，学术价值极高；无许可证、受众极窄。
- **学习价值**：9 — 顶级数学结果的机器证明是一手研究资料，学习门槛极高。
- **使用价值**：5 — 普通开发者几乎用不上，价值在数学/形式化研究。
- **推荐等级**：**B**
