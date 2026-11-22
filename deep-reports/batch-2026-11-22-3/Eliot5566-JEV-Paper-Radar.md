# Eliot5566/JEV-Paper-Radar

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/Eliot5566/JEV-Paper-Radar
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Python
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：45 / 14
- **许可证**：MIT
- **创建时间**：2026-09-23
- **最近更新**：2026-11-19（元数据 updated_at；最近提交 2026-11-20）
- **最新 release**：v0.2.0 — Usage auditing and safer decisions（tag `v0.2.0`，发布于 2026-11-13）
- **一句话简介**：一个可上传的 AI 技能/CLI：订阅 arXiv、bioRxiv 等 RSS，按你的研究兴趣每天筛选、排序出一份带理由和来源的论文阅读清单，并推送到 Slack/Discord/邮件等。

## 一句话理解

它像一位每天早上的“论文助理”：按你关心的主题扫一遍最新预印本，挑出最相关的几篇、说明为什么值得读，再发到你指定的地方。

## 通俗解释

科研者每天面对海量 arXiv/bioRxiv 新论文，靠人工刷 RSS 效率低。Paper Radar 的设计很巧：它把每个“研究兴趣”写成一个原子问题（Noul），用 Jev/Claude 对每篇论文做判断，相关性默认取 max(weight × p)（或用 noisy-OR 让匹配多个兴趣的论文排名更高）；所有问题在“一次调用”里并行问完（speculative fan-out），只看标题/摘要/类别，故意不含作者与机构以免偏见；判断通过后，可选的生成模型只为进前 10 的论文写一句 TL;DR（来源：README “How it works”）。结果按你设定的阈值分成 must-read/maybe/near-miss，可导出 HTML/CSV/JSON/RSS，也可推 Slack、Discord、Telegram、邮件（来源：README）。它支持作为 Claude skill 上传、GitHub Action 定时运行，且“no separate model API key”（用 Claude 订阅）（来源：仓库描述与 README）。

## 它不是什么

1. **不是论文全文数据库**。它读公开摘要（标题/摘要/类别），不存全文（来源：README “Retrieve, then judge”）。
2. **不是必须自己有模型 API key**。README 强调可复用 Claude 订阅/要用 Claude 而非 Jev（来源：仓库描述与 “Use Claude instead of Jev”）。
3. **不是纯生成式摘要工具**。主流程是确定性筛选 + 置信度门控，生成模型只写前 10 的一句话（来源：README “Cascade”）。
4. **不是只支持 arXiv**。支持 bioRxiv/medRxiv/任意 RSS（来源：README “How it works” 流程图）。
5. **不是大项目**。45 star、v0.2.0、2026-09 创建，社区很小（来源：GitHub）。

## 痛点

- **信息过载**：每天新论文太多，人工筛选耗时。
- **推荐理由缺失**：普通 RSS 只列标题，不知为何相关。
- **作者偏见**：看作者/机构会影响判断；项目故意去掉这些字段（来源：README）。
- **成本与可解释性**：大量调用生成模型贵；用级联 + 阈值 + 审计轨迹控制（来源：README）。

## 核心功能（依据 README，注明来源）

- **兴趣即原子问题**：每个兴趣一个 Noul，相关性 max(weight×p) 或 noisy-OR（来源：README “How it works”）。
- **推测式并行 + 先看后判**：一次调用问完所有问题；只给模型标题/摘要/类别（来源：README）。
- **级联 + 置信度门控**：Jev 筛掉海量论文，生成模型只写进前 10 的 TL;DR；阈值可用 calibrate 校准（来源：README）。
- **去重与来源**：对最近 14 天去重，保留来源与审计轨迹 data/（来源：README 流程图）。
- **多输出渠道**：站点 + RSS、Slack/Discord/Telegram/email、HTML/CSV/JSON/RSS 导出（来源：README）。
- **部署形态**：可上传的 skill、CLI、GitHub Action 定时工作流（来源：仓库描述与 README headings）。

## 使用场景

- 科研者/学生每天跟踪某领域最新论文。
- 团队共享论文雷达，推送到 Slack/邮件。
- 用 Claude 订阅做零额外 key 的文献筛选。

## 适用人群

科研人员、研究生、需要文献跟踪的团队。非科研用户用不上。

## 注意事项

- MIT 许可。
- v0.2.0、45 star、12 issue，早期小项目。
- 依赖 Claude/Jev 订阅与 RSS 源可用性；筛选质量取决于兴趣描述写法（README 有专章 “Write interests Jev can answer well”）。
- 成本/使用可通过 “Inspect usage and cost” 查看（来源：README headings）。

## 总结

Paper Radar 是一个设计极为克制又聪明的文献工具：用原子兴趣 + 级联判断 + 置信度门控，把“每天读什么论文”变成可解释、可校准、低成本的流程。它不追求炫技，而把工程判断（去掉作者偏见、只给前 10 写摘要）想得很清楚。45 star 的小项目，值得科研者一试。

## 独立评分（满分 10）

- **质量**：8 — 设计克制、可解释可校准、输出渠道多；项目小、早期。
- **学习价值**：8 — 原子兴趣 + 级联判断 + 置信度门控的设计很聪明。
- **使用价值**：8 — 科研者文献跟踪价值高。
- **推荐等级**：**B**
