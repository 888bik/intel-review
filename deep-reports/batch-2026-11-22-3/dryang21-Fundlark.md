# dryang21/Fundlark

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/dryang21/Fundlark
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Python
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：36 / 1
- **许可证**：MIT
- **创建时间**：2026-11-08
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-15）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：一个 AI 技能/CLI：用你自己的 AI 助手查找科研资助、核对官方征募信息并跟踪申请，附一份可直接用的资助目录。

## 一句话理解

它像科研基金的“信息助理”：告诉 AI 你的背景，它从目录里找出你能申请的奖学金/基金，去官网核对截止时间，并帮你管理申请进度。

## 通俗解释

找科研经费（fellowship/grant）很费时：要翻大量网站、判断自己是否合格、核对截止时间，还容易信息过期。Fundlark 提供一份资助目录与 AI 技能/CLI：你用自然语言描述自己（如“美国某大学癌症免疫学博士后”），它返回一份精简、已核对的清单，每条链接到资助方官网、注明“本人可申请/需导师或机构”、并把“与你的研究相关”和“你是否符合资格”分开（来源：README “What you get” 示例）。示例输出里还能看到“官网无法访问、截止日期未确认”的诚实标注（来源：README 示例中的 CRI Irvington 条目）。它提供三种用法，可配合 AI 助手（Claude Code、Codex）使用；有 catalog、Privacy、FAQ 等章节（来源：README headings）。36 star、无 release、2026-11 创建。

## 它不是什么

1. **不是资助机构或申请平台**。它只做信息发现与跟踪，不代你申请（来源：README）。
2. **不是保证信息实时准确**。README 示例明确标注“官网无法访问、日期未确认”的条目（来源：README）。
3. **不是只给一种用法**。README 有 “Three ways to use it”（来源：README headings）。
4. **不是需要上传个人敏感数据到第三方**。有 Privacy 章节（来源：README headings）。
5. **不是大项目**。36 star、1 fork、0 issue、2026-11 创建（来源：GitHub）。

## 痛点

- **资助信息分散**：大量网站、格式不一，人工检索耗时。
- **资格判断复杂**：能否本人申请、是否需机构、专业/年限限制等。
- **信息过期**：截止时间常变，需核对官网。
- **申请跟踪缺失**：需要记录进度。

## 核心功能（依据 README，注明来源）

- **资助目录（catalog）**：What's in the catalog 章节（来源：README headings）。
- **三种使用方式**：Three ways to use it（来源：README headings）。
- **AI 助手集成**：Use it with your AI assistant（For you / For AI agents），topics 含 agent-skills、claude-code、codex（来源：README headings 与 topics）。
- **官网核对与诚实标注**：示例中对无法访问的官网标“unconfirmed”（来源：README 示例）。
- **资格与相关性分离**：每条注明 apply yourself 或需 advisor/institution，并分开“relevant to research”与“eligible”（来源：README）。
- **浏览器端跟踪**：仓库描述提到“track applications in your browser”（来源：仓库描述）。

## 使用场景

- 博士生/博士后寻找科研资助与奖学金。
- 用 AI 助手批量核对资助官网与截止时间。
- 管理多个申请的进度。

## 适用人群

科研人员、研究生、博后。非科研用户用不上。

## 注意事项

- MIT 许可。
- 36 star、1 fork、0 issue、无 release，极新项目。
- 资助信息可能过期，务必以官网为准（README 自己也这么做）。
- “核对官网”依赖网络与页面可访问性，失败会明确标注未确认。

## 总结

Fundlark 是一个务实的小工具：把“找科研资助 + 核对官网 + 跟踪申请”交给 AI 助手，并且难得地保留了“官网打不开就标未确认”的诚实。36 star 的早期项目，目录与数据质量需自行核对。对找经费的科研人值得一试。

## 独立评分（满分 10）

- **质量**：7 — 务实、官网核对诚实标注难得；极新、数据需自行核对。
- **学习价值**：7 — 资助发现 + 核对 + 跟踪的 AI 工作流有参考意义。
- **使用价值**：8 — 找经费的科研人价值高。
- **推荐等级**：**B**
