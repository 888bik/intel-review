# hyphatech/spindle

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/hyphatech/spindle
- **分类**：developer-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：OCaml
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：56 / 1
- **许可证**：MIT
- **创建时间**：2026-10-07
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：0.15.0（tag `0.15.0`，发布于 2026-11-10）
- **一句话简介**：面向现代 OCaml 的 Web 框架：自研 RFC 严格 HTTP/1.1 引擎、OCaml 5 多核原生、端到端类型安全与从路由生成的 OpenAPI 文档。

## 一句话理解

它让 OCaml 开发者写 Web 服务时“类型一路打通”：请求体验证、JSON 派生、API 文档、前端 schema 都从 OCaml 类型生成，底层还是自己写的、逐条 RFC 测试过的 HTTP 引擎。

## 通俗解释

OCaml 生态长期缺现代 Web 框架。spindle 的定位是“The web framework for modern OCaml”（来源：仓库描述），特点很硬核：自研 HTTP/1.1 引擎，逐条满足 RFC 9112/9110，并有 WebSocket（6455）、表单（7578）、Structured Fields（RFC 9651）的测试套件；基于 OCaml 5 的 Eio，直线式 handler、多核设计；类型端到端（JSON 从类型派生、输入全验证）；OpenAPI 文档用 Scalar 从路由生成；前端可用 Zod/Melange schema 从 OCaml 类型派生；内置 SQLite/Postgres、WebSocket、SSE、session、日志、traces、metrics（来源：README Features）。README 给了 split-bill 的完整示例：deriving wiretype 定义类型、post 路由、几行 curl 调用，文档自动出现在 /docs（来源：README Example）。它还支持把文档装到 opam 目录并在 AGENTS.md 里让代理读 llms.txt（来源：README With an agent）。56 star、MIT、0.15.0。

## 它不是什么

1. **不是通用语言或运行时**。它是 OCaml 的 Web 框架，依赖 Eio/OCaml 5（来源：README）。
2. **不是用现成 HTTP 库**。它自己实现 HTTP/1.1 引擎并逐条 RFC 测试（来源：README Features）。
3. **不是只做后端**。它还生成前端 schema 与 OpenAPI 文档（来源：README）。
4. **不是成熟大生态**。56 star、1 fork、0 issue、0.15.0，早期（来源：GitHub）。
5. **不是没有学习曲线**。需要 OCaml、Eio、ppx_wiretype 等知识（来源：README Install 示例）。

## 痛点

- **OCaml Web 工具链缺口**：spindle 提供端到端框架。
- **HTTP 实现正确性**：RFC 严格 + 测试套件避免协议细节踩坑。
- **前后端类型漂移**：从 OCaml 类型派生 JSON/前端 schema（来源：README）。
- **代理友好的文档**：llms.txt + AGENTS.md 一段式接入（来源：README）。

## 核心功能（依据 README，注明来源）

- **RFC 严格 HTTP/1.1 引擎**：RFC 9112/9110、WebSocket 6455、表单 7578、Structured Fields 9651 测试套件（来源：README Features）。
- **Eio-native / OCaml 5 多核**：直线 handler（来源：README）。
- **端到端类型**：JSON 从类型派生、输入验证（来源：README）。
- **OpenAPI + Scalar 文档**：从路由生成（来源：README）。
- **前端同步**：Zod/Melange schema 从 OCaml 类型派生（来源：README）。
- **内置能力**：SQLite/Postgres、WebSocket、SSE、session、日志、traces、metrics（来源：README）。
- **opam 安装 + 代理文档**：opam install spindle、llms.txt（来源：README Install/With an agent）。
- **MIT 许可**（来源：GitHub API）。

## 使用场景

- 用 OCaml 写类型安全的 Web API/服务。
- 需要 RFC 严格 HTTP 实现与自动生成文档的项目。
- 让 AI 代理通过 llms.txt 学习框架的项目。

## 适用人群

OCaml 开发者、函数式编程与类型系统爱好者。不用 OCaml 的人用不上。

## 注意事项

- MIT 许可。
- 56 star、1 fork、0 issue、0.15.0（2026-11-10），早期小项目。
- RFC 严格等为项目自述，未独立核实；生态与库支持有限。
- 安装需要 opam 与 Eio/ppx 工具链。

## 总结

spindle 是 OCaml Web 开发的一次有力补强：自研 RFC 严格 HTTP 引擎 + 端到端类型 + 自动文档 + 电池齐全，还有对代理友好的文档设计。56 star、早期、受众窄，但对 OCaml 开发者很值得一试。

## 独立评分（满分 10）

- **质量**：8 — RFC 严格 HTTP 引擎 + 端到端类型 + 电池齐全，设计扎实；OCaml 受众窄、早期。
- **学习价值**：8 — OCaml 5 多核 Web 框架的类型驱动与 RFC 测试方法有很高学习价值。
- **使用价值**：7 — OCaml 开发者价值高，其他语言用不上。
- **推荐等级**：**B**
