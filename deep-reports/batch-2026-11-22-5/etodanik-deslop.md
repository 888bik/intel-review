# etodanik/deslop

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/etodanik/deslop
- **分类**：ai-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：HTML
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：52 / 4
- **许可证**：MIT
- **创建时间**：2026-11-16
- **最近更新**：2026-11-21（元数据 updated_at；最近提交 2026-11-17）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：一个 Claude skill：基于心理语言学与语料研究总结的规则，修改草稿以去除 LLM 写作痕迹，并在三个公开人/机同题语料上校验过。

## 一句话理解

它像一位“文风编辑”：不是套几句“不要用显然、此外”，而是用比较人类与模型文本的语言学研究发现痕迹，按规则改稿，并声称在人/机同题回答的公开语料上验证过。

## 通俗解释

“去 AI 味”工具很多，但常靠拍脑袋的列清单。deslop 的规则来自比较人类与模型文本的语料语言学、心理语言学研究，并在三个公开语料（人与模型回答相同提示）上校验（来源：README 开头，研究细节在 references/research.md）。README 给了一个例子：Claude 默认风格的客户邮件（228 词、3 个 Claude 痕迹）经过一遍改写变成 132 词、0 痕迹且不丢事实（来源：README Example，**效果为项目自述、未独立核实**）。它是一个 Claude skill，有 Example、Install、Compared with other approaches、The pass、Tools、Files、Evidence、Licensing 等章节（来源：README headings）。52 star、MIT、HTML 语言字段、2026-11-16 创建。

## 它不是什么

1. **不是通用写作 App**。它是 Claude skill（来源：README 开头）。
2. **不是只删固定短语**。它基于语言学研究与语料校验（来源：README）。
3. **不是保证“查不出 AI”**。目标是去写作痕迹，非对抗检测（README 语境）。
4. **不是成熟大项目**。52 star、无 release、1 issue、2026-11 创建（来源：GitHub）。
5. **不是中文工具**。规则面向英文 LLM 文本（README 示例为英文）。

## 痛点

- **AI 文本有固定指纹**：词汇、句式、节奏上的痕迹。
- **去痕迹缺乏依据**：deslop 用研究与语料库支撑规则。
- **改写丢事实**：示例强调“不丢事实”（来源：README，自述）。
- **与其他方法对比**：README 有 Compared with other approaches 章节（来源：README headings）。

## 核心功能（依据 README，注明来源）

- **基于研究的规则**：语料语言学/心理语言学，references/research.md（来源：README）。
- **一遍改写流程**：The pass 章节（来源：README headings）。
- **语料校验**：三个公开人/机同题语料（来源：README）。
- **对比与证据**：Compared with other approaches、Evidence（来源：README headings）。
- **Tools / Files 说明**（来源：README headings）。
- **MIT 许可**（来源：GitHub API）。

## 使用场景

- 把 AI 起草的邮件、文档、稿件改得更自然。
- 研究 LLM 写作痕迹与去痕迹方法。
- 与其他 humanizer 工具对比效果。

## 适用人群

英文写作者、使用 Claude 的知识工作者、文本风格研究者。中文写作场景不适用。

## 注意事项

- MIT 许可。
- **分类为本报告自行判断**（新入库项目，情报库无分类）。
- 52 star、无 release、极新；示例效果为项目自述、未独立核实。
- 规则面向英文，中文用户参考价值有限。

## 总结

deslop 用“研究 + 语料校验”给去 AI 腔工具增加了依据，比纯清单式 humanizer 更有说服力。52 star、极新、效果自述，但英文写作与文本风格研究者值得一试。

## 独立评分（满分 10）

- **质量**：8 — 用心理语言学规则去 AI 腔、且有语料库对照，思路有依据；早期、规则效果需实测。
- **学习价值**：8 — 语料语言学驱动的文本去痕迹方法有较高参考价值。
- **使用价值**：8 — 需要改善 AI 文本质量的写作者价值高。
- **推荐等级**：**B**
