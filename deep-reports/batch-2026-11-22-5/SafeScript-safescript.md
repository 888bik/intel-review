# SafeScript/safescript

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/SafeScript/safescript
- **分类**：developer-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：JavaScript
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：28 / 0
- **许可证**：MIT
- **创建时间**：2026-10-23
- **最近更新**：2026-11-16（元数据 updated_at；最近提交 2026-11-18）
- **最新 release**：v2.1.0 – Debugging（tag `v2.1.0`，发布于 2026-11-17）
- **一句话简介**：JavaScript 的安全计算子集（“代码版 JSON”）：无 eval、无全局、无原型访问，编译成普通 JS/PHP，并带事务运行时、时间旅行调试与编辑后继续。

## 一句话理解

JSON 让数据交换变安全，SafeScript 想让“运行别人写的代码”也变安全：它是 JavaScript 的严格子集，编译成普通 JS/PHP，打开 .safe 文件就能看出代码安全，还能逐步调试、回退、边改边跑。

## 通俗解释

很多应用需要跑用户提交的脚本（webhook 处理、插件、公式），但直接 eval JavaScript 极度危险。SafeScript 的定位是“JSON for code”：严格 JS 子集，没有 eval、全局变量、原型访问，编译成普通 JS 或 PHP，编译器压缩后约 48KB（gzip 约 12KB），可本地验证、无需信任（来源：README 开头，**体积为项目自述、未独立核实**）。v2.1 加了三项安全加固：ReDoS 防护（拦截危险正则与超大输入）、Error.stack 脱敏（防服务器路径泄露）、scope 函数 toString() 包装（隐藏宿主源码）（来源：README）。v2.0 加了事务运行时：所有变更走 journal（预写日志 + 读覆盖），commit 前不改状态；调试模式下每条语句插检查点，可单步、看局部变量、回退、暂停时改代码继续（来源：README）。适用场景包括 webserver handler、onclick 事件、插件脚本、公式表达式（来源：README Use Cases）。28 star、MIT、v2.1.0。

## 它不是什么

1. **不是完整 JavaScript**。它是严格子集，禁用 eval/全局/原型（来源：README）。
2. **不是新的虚拟机语言**。它编译成普通 JS 和 PHP（来源：README）。
3. **不是浏览器 DevTools 调试**。调试跑在宿主应用自己的 UI 里（来源：README）。
4. **不是成熟生态**。28 star、0 fork、v2.1.0、2026-10 创建（来源：GitHub）。
5. **不是“绝对安全”的证明**。它是受控子集 + 加固，仍需在宿主侧正确集成（README 语境）。

## 痛点

- **运行用户脚本不安全**：严格子集从语言层面收窄能力。
- **副作用难回滚**：journal 事务让变更可 commit/回退。
- **调试体验差**：检查点 + time travel + 编辑继续内置在宿主 UI。
- **宿主代码泄露**：v2.1 的 stack/toString 加固（来源：README）。

## 核心功能（依据 README，注明来源）

- **安全 JS 子集**：无 eval/全局/原型，编译成 JS/PHP（来源：README）。
- **.safe 文件格式**：类型声明、信任单元、自包含设计、宿主工作流（来源：README headings）。
- **事务运行时**：journal（WAL + 读覆盖），commit 前不改状态（来源：README）。
- **调试**：checkpoint 单步、看局部变量、回退、暂停改代码继续（来源：README）。
- **v2.1 安全加固**：ReDoS 防护、Error.stack 脱敏、toString 包装（来源：README）。
- **执行模型**：Auto-Async、Execution Guards、Hooks、Host-Level Composition（来源：README headings）。
- **MIT 许可**（来源：GitHub API）。

## 使用场景

- 让用户提交 webhook/事件处理脚本的 SaaS。
- 插件系统、公式/表达式引擎。
- 需要可回滚、可调试的嵌入式脚本。

## 适用人群

需要安全嵌入用户代码的平台开发者。普通应用开发者按需使用。

## 注意事项

- MIT 许可。
- 28 star、0 fork、v2.1.0、早期；README 约 5 万字符，细节繁多。
- 编译器体积、安全加固效果为项目自述、未独立核实。
- 安全性依赖宿主正确使用，需自行审计集成方式。

## 总结

SafeScript 把“安全运行用户脚本”做成语言级方案：严格子集 + 事务运行时 + 内建 time-travel 调试 + 近期三项安全加固，设计完整且务实。28 star、早期、数字自述，但对口场景（插件/webhook/公式）很有价值。

## 独立评分（满分 10）

- **质量**：8 — “代码版 JSON”的安全子集设计完整，事务运行时与调试能力突出；早期、受众特定。
- **学习价值**：8 — 安全脚本子集、journal 事务与 time-travel 调试的实现有较高参考价值。
- **使用价值**：7 — 需要嵌入用户脚本的场景价值高，通用性一般。
- **推荐等级**：**B**
