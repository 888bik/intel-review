# garritfra/llmtee

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/garritfra/llmtee
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Go
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：21 / 1
- **许可证**：Apache-2.0
- **创建时间**：2026-11-08
- **最近更新**：2026-11-21（元数据 updated_at；最近提交 2026-11-11）
- **最新 release**：v0.1.1（tag `v0.1.1`，发布于 2026-11-11）
- **一句话简介**：自托管的 LLM 代理：像 Unix tee 一样透传 OpenAI/Anthropic 及兼容 API 的响应，同时记录每次调用的 token、成本、延迟与首 token 时间，并支持按 key 设预算。

## 一句话理解

它像给 LLM API 装了一个“水表+限流器”：不改客户端，把 base URL 指过来，每次调用的 token、花费、延迟都记进数据库，key 超预算就返回 429，而响应原样透传。

## 通俗解释

用 LLM 的团队常不知道钱花在哪、哪个代理在烧钱。llmtee 是一个 Go 写的单二进制代理，兼容 OpenAI、Anthropic 及 OpenRouter/Ollama/vLLM/Groq 等 OpenAI-compatible 端点；客户端只要把 base URL 指到它（如 OPENAI_BASE_URL=http://localhost:8080/openai/v1），它就把响应原样透传并留一份副本（来源：README 开头与 Quickstart）。记录内容包括 token、成本、延迟、首 token 时间，存 SQLite（备份就是拷文件）或 PostgreSQL（来源：README）。两种 key：provider key（只存 SHA-256 哈希）与 llmtee key（代理替你换入真实 provider key，队友/代理看不到），可按日/月设预算，超限 429（来源：README Keys and budgets）。数据可通过 Web UI、CSV、Prometheus metrics、MCP server 读取（来源：README）。README 也很坦诚：它不在做路由/fallback/多租户，0.x 配置可能变，且在请求路径上——挂了 LLM 调用就失败（来源：README）。21 star、Apache-2.0。

## 它不是什么

1. **不是 LLM 网关**。README 明说不是带路由、fallback、多租户的 gateway（来源：README）。
2. **不是会改响应**。Golden tests 要求字节级一致的透传（来源：README How it works）。
3. **不是估算 token**。没有 usage 就标 “usage unknown”，从不估算（来源：README Token counts and cost）。
4. **不是无风险旁路**。它在请求路径上，宕机则调用失败（来源：README）。
5. **不是成熟大版本**。21 star、v0.1.1、0.x，配置可能变（来源：GitHub 与 README）。

## 痛点

- **LLM 花费不透明**：逐请求记录 token/成本/延迟。
- **预算失控**：key 级日/月预算，超限 429（来源：README）。
- **真实 key 泄露**：llmtee key 让队友/代理不接触 provider key（来源：README）。
- **多来源统一视图**：本地模型与托管 API 进同一 dashboard（来源：README）。
- **性能顾虑**：README 自述 Apple M4 Pro 200 RPS 下 p50 增加 0.14ms、内存约 33MB（来源：README，**自述、未独立核实**）。

## 核心功能（依据 README，注明来源）

- **透明代理与 tee**：按端点转发 OpenAI/Anthropic/OpenRouter/自定义上游，流式逐块透传（来源：README Endpoints/How it works）。
- **成本与用量**：token、成本、延迟、TTFT；价格取自 LiteLLM 表（构建时嵌入、每周刷新）（来源：README Token counts and cost）。
- **两种 key + 预算**：provider key 哈希存储；llmtee key 代换真实 key；日/月预算 429（来源：README）。
- **多存储与导出**：SQLite/PostgreSQL、Web UI、CSV、Prometheus /metrics、MCP server（来源：README）。
- **部署**：Docker、二进制（Linux/macOS/Windows amd64+arm64）、compose/systemd/Caddy 示例（来源：README Quickstart）。
- **Apache-2.0 许可**（来源：GitHub API）。

## 使用场景

- 个人/小团队想看清并封顶 LLM 花费。
- 给多个代理/同事分发不暴露真实 key 的调用凭据。
- 本地模型与云端 API 的统一观测。

## 适用人群

AI 应用开发者、小团队技术负责人。不用 LLM API 的人用不上。

## 注意事项

- Apache-2.0 许可。
- 21 star、v0.1.1、1 issue，0.x 早期，配置可能变。
- 在请求路径上，需监控 /healthz 并保留直连 base URL 作为回退（来源：README）。
- 跨预算边界的那一个请求会完成后才超限（README 明确，来源：README）。

## 总结

llmtee 把“LLM 调用的可观测性 + 预算封顶 + key 隔离”做成一个克制的单二进制，设计务实、文档诚实。21 star、0.x、在请求路径上有风险，但对想控成本的团队非常实用。对口用户值得部署试用。

## 独立评分（满分 10）

- **质量**：8 — 单二进制代理、成本/预算/多种数据源齐全、设计克制；0.x、在请求路径上有风险。
- **学习价值**：7 — LLM 代理的可观测性、预算控制与流式 tee 实现有参考价值。
- **使用价值**：8 — 个人/小团队追踪与封顶 LLM 花费非常实用。
- **推荐等级**：**B**
