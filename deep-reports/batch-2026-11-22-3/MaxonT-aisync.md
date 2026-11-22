# MaxonT/aisync

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/MaxonT/aisync
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Python
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：49 / 6
- **许可证**：MIT
- **创建时间**：2026-10-21
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-18）
- **最新 release**：aisync 0.2.11（tag `v0.2.11`，发布于 2026-11-16）
- **一句话简介**：给多代理开发用的“本地中枢”：共享规则、哈希链日志与文件占用声明，协调 Claude Code、Codex、Cursor 等代理的并发工作并提供可观测性。

## 一句话理解

它像多个 AI 代理共用的“共享大脑和记事本”：大家都从这里读同一份规则、声明“我在改这个文件”、并把做过的事写进不可篡改的日志，避免互相踩踏。

## 通俗解释

同时让多个编码代理在一个项目里干活，最怕三件事：上下文不一致（各读各的 AGENTS.md）、并发冲突（两个代理改同一文件）、事后无法审计（谁在什么时候做了什么）。aisync 的定位是“One local hub for developers working with multiple AI coding agents: share the right context, coordinate concurrent work, and verify what happened”（来源：README 开头）。它核心是 agent-agnostic：为 Claude Code、Codex、Cursor 提供 native hooks，同时通过 CLI、MCP server 或共享指令文件把中枢开放给其它代理（来源：README）。它用“hash-chained journal”（哈希链日志）记录事件，用“file claims”（文件占用声明）协调并发，还有“rules as interfaces, loaded in layers”（0.2.0 新特性，来源：README headings）。README 有 “Why not just AGENTS.md?” 专章，说明它比静态指令文件多了动态协调与可验证性。

## 它不是什么

1. **不是单一代理的提示词文件**。它强调多代理协调与可验证日志（来源：README）。
2. **不是只支持某一家代理**。core 是 agent-agnostic，三家 native hooks 不是上限（来源：README）。
3. **不是 AGENTS.md 的简单替代**。README 专章解释差异（来源：README headings）。
4. **不是云端服务**。它是本地 hub（来源：README “One local hub”）。
5. **不是大项目**。49 star、v0.2.11、2026-10 创建，早期（来源：GitHub）。

## 痛点

- **上下文漂移**：多代理各读各的指令，规则不一致。
- **并发写冲突**：不同代理改同一文件互相覆盖。
- **缺乏审计**：无法可靠回放“谁做了什么”。
- **native hooks 边界**：代理种类多，不能只靠某家的 hook（来源：README “Integrations”）。

## 核心功能（依据 README，注明来源）

- **本地中枢 + agent-agnostic**：CLI、MCP server、共享指令文件接入其它代理（来源：README）。
- **native hooks**：Claude Code、Codex、Cursor（来源：README）。
- **哈希链日志**：hash-chained journal 保证事件不可篡改（来源：仓库描述与 README）。
- **文件占用声明**：file claims 协调并发写（来源：仓库描述）。
- **分层规则**：0.2.0 “rules as interfaces, loaded in layers”（来源：README headings）。
- **安装与文档**：Install（含 Two agents, one file）、Use、Configuration、How it works（来源：README headings）。

## 使用场景

- 多个编码代理并行开发同一代码库。
- 需要共享规则且可审计代理行为的团队。
- 想用 MCP/CLI 把代理能力加入统一中枢的高级用户。

## 适用人群

多代理工作流开发者、AI 编码重度用户。单代理或普通用户用不上。

## 注意事项

- MIT 许可。
- 49 star、6 fork、8 issue、v0.2.11，早期小项目。
- Python 3.11+，README 标注 dependencies 0（来源：README 徽章）。
- 哈希链与文件声明的可靠性需自行验证。

## 总结

aisync 抓住了多代理并发的核心痛点：上下文、冲突与审计。agent-agnostic 设计 + native hooks + 哈希链日志 + 文件占用声明，思路完整且务实。49 star 的小项目，但对“同时开多个代理干活”的团队很有针对性价值。值得跟踪试验。

## 独立评分（满分 10）

- **质量**：7 — 多代理痛点抓得准、设计完整；极新、可靠性待验证。
- **学习价值**：7 — 哈希链日志 + 文件占用声明的多代理协调思路有参考价值。
- **使用价值**：7 — 多代理并行开发者价值高。
- **推荐等级**：**B**
