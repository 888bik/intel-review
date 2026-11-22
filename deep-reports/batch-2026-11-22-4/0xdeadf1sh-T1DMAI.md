# 0xdeadf1sh/T1DMAI

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/0xdeadf1sh/T1DMAI
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Python
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：88 / 4
- **许可证**：MIT
- **创建时间**：2026-07-29
- **最近更新**：2026-11-20（元数据 updated_at；最近提交 2026-11-15）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：一个 1 型糖尿病血糖预测模型库：作者用自己的连续血糖数据训练/测试多种模型（DILATE+LoRA、Timestral、XGBoost、MiniRocket 等）并给出详尽对照实验。

## 一句话理解

这是“用我自己的数据做血糖预测”的开源实验场：把十几种时间序列模型放在两个数据集上横评，连“哪个模型其实不怎么样”都如实写出来。

## 通俗解释

1 型糖尿病患者需要预测未来血糖以调整胰岛素。T1DMAI 不是产品，而是一个模型库+基准：作者在自有数据（my data）与公开数据集 MetaboNet 上训练/测试多种方法，包括自己的 D16 DILATE + LoRA、Timestral（zero-shot/微调）、XGBoost/LightGBM、MiniRocket、Chronos/TimesFM 等（来源：README “Models/Results”）。README 的结论非常坦诚：“All models suck”——在作者数据上 XGBoost 最好（RMSE 22.4），自己的模型紧随其后（22.8，第 3/35）；在 MetaboNet 上微调 Timestral 209M 各时域最低（60 分钟 32.6）；3771 参数的 LoRA 适配器在多数情况改善结果，但在 MetaboNet 120 分钟反而变差；低血糖检测几乎所有模型都漏掉很多（来源：README “Main results”，**以上数字为项目自述、未独立核实**）。还有 Timestral、MiniRocket 专章、局限、条款、完整表格与复现说明（来源：README headings）。88 star、MIT。

## 它不是什么

1. **不是医疗设备或临床建议工具**。README 有 Limitations/Terms，且结论自称“all models suck”（来源：README）。
2. **不是单一模型**。它是“model zoo”，含十多种模型与对照（来源：仓库描述）。
3. **不是通用时间序列库**。专注血糖预测（来源：仓库 topics）。
4. **不是结果已被独立验证**。所有数字为作者自述、未独立核实。
5. **不是有 release 的产品**。88 star、无 release、2026-07 创建（来源：GitHub）。

## 痛点

- **血糖预测模型繁多难选型**：统一在同一数据上横评。
- **指标会骗人**：低 RMSE 不等于能抓低血糖；README 专门报告低血糖检出率（来源：README）。
- **LoRA 是否真有用**：用 3771 参数适配器做消融，诚实地报告反例（来源：README）。
- **复现**：README 有 Reproduce 章节（来源：README headings）。

## 核心功能（依据 README，注明来源）

- **模型库**：D16 DILATE + LoRA、Timestral（5M–209M，zero-shot/微调）、XGBoost/LightGBM、MiniRocket、Chronos-2/TimesFM 2.5 等（来源：README Models/Results）。
- **双数据基准**：my data 与 MetaboNet，多时域 RMSE 与 Wilcoxon 检验（来源：README Main results，自述）。
- **低血糖检出与置信区间**：报告 catches lows 与 90% band 覆盖率（来源：README）。
- **方法/结果/局限/条款/完整表格/复现**章节（来源：README headings）。
- **MIT 许可**（来源：GitHub API）。

## 使用场景

- 血糖预测研究者参考基准与实验设计。
- 时间序列（含 DILATE 损失、LoRA 适配器）方法对比。
- “如何诚实报告模型局限”的写作范本。

## 适用人群

医疗 ML/时间序列研究者、糖尿病患者中的技术玩家。不能作为治疗依据。

## 注意事项

- MIT 许可。
- 所有结果为项目自述、未独立核实；作者自称“all models suck”。
- 涉及个人健康数据，使用需谨慎；README 有 Terms/Limitations。
- 88 star、4 fork、0 issue、无 release；README 约 5.7 万字符，细节繁多。

## 总结

T1DMAI 的价值在“诚实”：它不只是秀模型，而是把十几种方法放在两个数据集上横评，并直说多数模型不够好、低血糖几乎都漏。88 star 的研究型项目，数字未独立核实，但对血糖预测/时间序列研究是很有参考价值的实验记录。

## 独立评分（满分 10）

- **质量**：7 — 横评十几种模型且诚实报告局限，实验设计扎实；数字自述、非临床。
- **学习价值**：8 — 血糖预测基准与“如何诚实报告模型”有很高参考价值。
- **使用价值**：6 — 研究者有价值，不能作为临床依据。
- **推荐等级**：**B**
