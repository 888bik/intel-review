# GoogleChrome/modern-web-guidance

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 A

## 基本信息

- **仓库地址**：https://github.com/GoogleChrome/modern-web-guidance
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：未核实（GitHub API 的 `language` 字段为 `null`；仓库主要是 Markdown 指南与 CLI 工具，具体语言构成未逐文件核实）
- **评分**：原始评分未核实；本报告独立评分见文末
- **Star / Fork**：24,785 / 1,036
- **许可证**：Apache-2.0
- **创建时间**：2026-03-23
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-16）
- **最新 release**：v1.0.1（发布于 2026-11-16）
- **一句话简介**：由 Chrome、Edge、Firefox 与 MDN 团队共同维护的一套“技能包”，把现代 Web 平台最佳实践与浏览器兼容数据直接塞进编码代理的上下文，引导代理少写过时代码。

## 一句话理解

这是一本给 AI 编码代理看的“现代 Web 开发规范手册”：代理写代码前先查它，就能少用老掉牙的 JS hack，多用浏览器原生、高性能、无障碍的新 API。

## 通俗解释

LLM 的训练数据里有大量老代码，所以编码代理写前端时常常“习惯性写旧模式”：明明有原生 `Popover API`、`View Transitions`、CSS `:has()`，它却写一堆 JavaScript 和 polyfill。Modern Web Guidance 解决这个问题的方式很聪明——它不是给人看的文档站，而是给代理用的“技能（skills）”：一个 CLI（`npx modern-web-guidance@latest`）能搜索指南、按 ID 取指南、扫描代码库里的过时模式，把精炼过的、专家整理的规则按 token 效率注入代理上下文（来源：README “Quickstart”、“Why?”）。内容覆盖 262 个现代 Web 特性，其中 CSS & Layout 140 项、HTML & DOM 37 项、JavaScript & APIs 85 项（来源：README “The full list” 与 “What's Included?”），由 Google Chrome、Microsoft Edge、Mozilla Firefox 和 MDN 团队共同维护（来源：README 开头）。对任何用 AI 写前端的团队，这是“给代理立正确规矩”的基础设施。

## 它不是什么

1. **不是给人读的普通文档站**。它是给编码代理注入上下文的 skills + CLI；虽然有文档站链接，但核心价值在 `install/search/retrieve/legacy-scan` 命令（来源：README “Quickstart”）。
2. **不是浏览器或 Web 框架**。它不改运行时、不提供组件库，只提供“指南数据 + 工具”。
3. **不是只有 Chrome 视角**。README 明确由 Chrome、Edge、Firefox、MDN 团队共同维护（来源：README 开头），不是单一厂商的宣传材料。
4. **不是“环境兼容清单”本身**。它内置浏览器兼容数据用于指导采用（README 开头提到 browser compatibility data），但重点在“怎么写现代代码与稳妥 fallback”，而非单纯特性表。
5. **不是收费服务**。README 提供 npx 安装、更新、技能包列表，许可证 Apache-2.0；是否有其他商业组件未核实。

## 痛点

- **代理默认写旧代码**：LLM 训练数据里 legacy 代码占比大，生成的方案往往“能跑但臃肿”（README “Why?” 原文）。
- **知道 API 存在≠会用**：模型可能知道某 API，但缺乏生产级的现代实现模式密度（来源：README “Why?”）。
- **Polyfill 依赖过重**：老做法常引入大库；指南引导用轻量 fallback（来源：README “Why?” 第三条）。
- **多个浏览器厂商标准不一**：四大团队共同维护保证指南的兼容性视角更全面。

## 核心功能（依据 README，注明来源）

- **CLI 工具**：`npx modern-web-guidance@latest install`（交互式安装向导）、`search "<query>"`（搜索指南）、`retrieve "<id>"`（按 ID 取指南）、`legacy-scan .`（扫描代码库里的过时模式）（来源：README “Quickstart”）。
- **262 个现代 Web 特性指南**：CSS & Layout 140、HTML & DOM 37、JavaScript & APIs 85（来源：README “The full list” 小节标题与 “What's Included?”）。
- **核心领域**：用户体验（View Transitions、scroll-driven animations）、CSS 布局（container queries、subgrid、oklch）、性能（预加载、INP 诊断、scheduler.yield）、表单与 UI（Popover、dialog、Anchor Positioning、`:user-invalid`）、无障碍（读屏与键盘可操作性）、内置 AI（本地翻译/摘要/语言检测 API）（来源：README “Core Disciplines” 表格）。
- **Token 效率设计**：指南经过 eval 剪除模型已知的低价值内容（来源：README “What's Included?” 开头）。
- **Eval 与凭证**：README 有 “Evals to prove this works well” 与 “Recent eval results snapshot” 章节（结果细节未独立核实）。
- **项目配置**：支持 `.mwgrc` 配置文件（来源：README heading “Project Configuration (.mwgrc)”）。
- **技能包与更新**：Available Skill Packs、Updating、Telemetry & Privacy 等章节（来源：README headings）。

## 使用场景

- 用 Claude Code / Cursor / Copilot 等代理写前端，想让生成代码更现代、更规范。
- 老项目现代化：用 `legacy-scan` 找出可替换为原生 API 的旧模式。
- 团队统一前端代码风格与浏览器兼容基线，把专家经验固化进代理工作流。
- 前端性能/无障碍优化，直接把相关指南喂给代理。

## 适用人群

前端开发者、使用 AI 编码代理的团队、关注 Web 平台现代化的人。后端/纯移动端开发者用不到。

## 注意事项

- GitHub `language` 字段为 null（未归类），不影响使用；主要内容是 Markdown + CLI。
- 有遥测（Telemetry & Privacy 章节），团队使用前应阅读其数据政策（来源：README heading）。
- Eval 结果、性能收益为项目自述，未独立核实。
- 24,785 star 说明社区认可度高，但项目 2026-03 才创建，很新；兼容数据依赖浏览器厂商更新。
- Apache-2.0 许可，商业友好。

## 总结

Modern Web Guidance 代表了一个重要方向：**把领域专家知识做成“代理可读的技能”**。它不做框架、不做编辑器，只在代理写代码的那一刻注入正确的现代 Web 实践——四厂背书 + 262 项指南 + eval 驱动的内容剪裁，让它成为 AI 前端开发的事实规范之一。对用代理写 Web 的团队，它几乎是“必装调料”；对想做“给代理用的知识产品”的人，它也是极佳的参考样本。

## 独立评分（满分 10）

- **质量**：9 — 四大厂背书、内容系统、eval 驱动、token 效率设计、更新机制完善；新项目扣分不多。
- **学习价值**：9 — 既是现代 Web 特性速查库，又是“给代理做技能包”的范本。
- **使用价值**：9 — AI 前端开发场景立刻可用，能直接改善生成代码质量；非前端场景用不到。
- **推荐等级**：**A**（AI 前端开发强烈推荐；领域权威度高）。
