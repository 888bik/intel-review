# memrohq/memro

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/memrohq/memro
- **分类**：ai-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Go
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：24 / 7
- **许可证**：PolyForm Noncommercial License 1.0.0（LICENSE 文件确认；GitHub API 标 NOASSERTION，仅限非商业使用）
- **创建时间**：2026-10-18
- **最近更新**：2026-11-21（元数据 updated_at；最近提交 2026-11-21）
- **最新 release**：Memro v0.1.2（tag `v0.1.2`，发布于 2026-11-21）
- **一句话简介**：给所有 AI 编码工具（Claude Code、Cursor、Codex、Copilot 等）用的共享长期记忆层：一次写规则、所有代理遵守，编辑违反规则时告警，并提供 MCP server、SDK 与托管/自托管方案。

## 一句话理解

它想解决“跟每个 AI 代理重复解释项目规则”的痛点：把规则写进一个文件、每个代理每次对话都能看到，谁改代码违反规则就告警，还能让多个代理共享长期记忆。

## 通俗解释

同时用 Claude Code、Cursor、Codex 等工具时，项目规则往往只对某个工具生效，换个工具就得重讲。Memro 的做法（来源：README 开头）：你说“never use Intl in cart code”，它被存到 .memro/rules.md 并注入各代理的 hooks；每个 agent、每次 prompt 都能看到，不需要靠搜索碰运气；.edit/command 用到被反引号标注的禁用项时会告警（Claude Code/Codex，warn only，不阻断）。README 给了一个 benchmark：规则埋在 3,000 条事实里，注入规则的代理 150/150 避开陷阱，只读 notes 文件 139/150（差异不显著），只用搜索 58/150（来源：README，**项目自述、未独立核实**）。它还有长期记忆、文件 claim（防两代理同时改）、连接 Slack/GitHub/Linear/Notion 等 40+ 集成、托管或 Docker 自托管（来源：README）。24 star、7 fork、v0.1.2、Go 服务 + SDK + MCP server。许可证是 PolyForm Noncommercial 1.0.0（LICENSE 文件确认，GitHub API 标 NOASSERTION）——仅限个人/研究等非商业用途，商用需 Memro Cloud 或商业许可（来源：README 与 LICENSE）。

## 它不是什么

1. **不是通用知识库**。它专注 AI 编码代理的规则/记忆/协调（来源：README）。
2. **不是商业开源**。source-available，PolyForm 非商业许可（来源：README 与 LICENSE 文件）。
3. **不是强阻断护栏**。规则违反只告警、不阻断（来源：README）。
4. **不是必须联网**。可 npx memro-cli init 本地用规则，也可 Docker 自托管（来源：README Quickstart Option B）。
5. **不是成熟大项目**。24 star、7 fork、v0.1.2、2026-10 创建（来源：GitHub）。

## 痛点

- **规则重复解释**：一次写规则、所有代理遵守。
- **搜索漏规则**：每 prompt 注入而非依赖检索（来源：README）。
- **编辑违反难发现**：编辑时告警（来源：README）。
- **多代理记忆与冲突**：共享记忆 + 文件 claim（来源：README）。

## 核心功能（依据 README，注明来源）

- **规则文件与 hooks**：.memro/rules.md，注入各代理每次 prompt（来源：README）。
- **编辑校验**：反引号标注禁用项，命中告警（来源：README）。
- **长期记忆与协调**：跨工具/机器共享记忆、文件 claim（来源：README）。
- **benchmark**：150/150 vs 139/150 vs 58/150（来源：README，自述）。
- **连接与部署**：40+ connectors、MCP server、Go 服务、SDK（Python/JS/Go）、Docker 自托管（来源：README）。
- **Claude Code 插件**：/plugin install memro（来源：README）。
- **PolyForm Noncommercial 1.0.0**（LICENSE 文件确认；GitHub API 标 NOASSERTION，仅限非商业）。

## 使用场景

- 同时使用多个 AI 编码工具、想统一项目规则与记忆的开发者。
- 需要跨机器/团队共享代理上下文的团队（非商业）。
- 自托管 MCP 记忆服务的实验。

## 适用人群

多代理开发者、个人/研究用户。商业团队需购买许可或用 Memro Cloud。

## 注意事项

- **许可为 PolyForm Noncommercial 1.0.0**（LICENSE 文件确认，非 API 标注），商业使用受限。
- **分类为本报告自行判断**（新入库项目，情报库无分类）。
- 24 star、7 fork、v0.1.2，早期；benchmark 数字为项目自述、未独立核实。
- 规则违反仅告警不阻断；效果依赖代理集成正确性。

## 总结

memro 把“跨工具的规则与长期记忆”做成统一层：规则注入、编辑告警、文件 claim、MCP 与自托管，设计贴合多代理痛点。24 star、v0.1.2、benchmark 自述，尤其要注意 PolyForm 非商业许可——个人/研究可用，商业需付费。对口用户值得试用。

## 独立评分（满分 10）

- **质量**：8 — 跨工具共享规则与长期记忆、编辑时校验，benchmark 有数据；PolyForm 非商业许可限制大。
- **学习价值**：7 — 规则即接口、跨代理记忆与编辑校验的设计有参考价值。
- **使用价值**：8 — 多代理开发者价值高，但商业使用受限。
- **推荐等级**：**B**
