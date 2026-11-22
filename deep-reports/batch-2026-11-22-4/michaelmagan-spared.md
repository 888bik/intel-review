# michaelmagan/spared

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/michaelmagan/spared
- **分类**：self-hosted（据 zhao 情报库分类；原始库未核实）
- **语言**：Rust
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：45 / 2
- **许可证**：Apache-2.0
- **创建时间**：2026-11-10
- **最近更新**：2026-11-16（元数据 updated_at；最近提交 2026-11-13）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：一个自托管的 Rust Web 应用宿主：每个应用连同它的 SQLite 数据库跑在独立 Wasm 沙箱里，像一个单二进制、可托管多应用的 Cloudflare Workers + Durable Objects。

## 一句话理解

它让“一个服务器上跑很多隔离的小应用”变得简单：每个 Rust Web 应用和它自己的 SQLite 都在独立 Wasm 沙箱里，一个二进制就能托管，数据天然隔离。

## 通俗解释

Cloudflare Workers + Durable Objects 很好用，但数据是托管的、不能自建。spared 想做自托管版本：一个二进制、一台服务器，托管多个 Rust 写的 Wasm 应用，每个应用有自己的 SQLite（跑在沙箱内部，不是宿主），实现数据隔离（来源：README 开头）。README 给了对标 workerd 的基准图：热请求 p50 148µs vs 388µs（快 2.6 倍）、16 连接 p99 5.45ms vs 16.53ms（低 3 倍）、200 应用下不可回收内存 108MB vs 516MB（少 4.8 倍）等（来源：README，**项目自述、未独立核实**）。用法是 curl 安装脚本、spared setup（编译 SQLite 到 Wasm，约一分钟）、spared serve、spared new/deploy，应用就是一个 Rust crate（来源：README Try it）。它还给出与 celld、Spin、workerd、wasmCloud 的对比表（来源：README “How it compares”）。

## 它不是什么

1. **不是云服务商**。它是自托管的单个二进制（来源：README）。
2. **不是跑 JavaScript**。应用是 Rust 编译成 Wasm（来源：README）。
3. **不是共享数据库**。每个应用自己的 SQLite 在沙箱内，数据隔离（来源：README）。
4. **不是成熟产品**。README 标 “Early preview”，45 star、无 release、2026-11 创建（来源：README 与 GitHub）。
5. **不是“全面领先”**。README 自述 Rust 在 14 项后端任务中 8 项胜过 workerd 的 JS，但加密/gzip/JSON 等 JS 调原生代码更快；200 应用下 spared 总内存反而更多（来源：README，自述）。

## 痛点

- **自建 Workers 难**：workerd 自托管复杂，数据隔离难做。
- **多应用数据隔离**：把 SQLite 放进每个沙箱，从架构上隔离。
- **性能与唤醒延迟**：热请求与空闲唤醒优化（来源：README 基准，自述）。
- **开发体验**：应用即 Rust crate，spared new/deploy 脚手架，甚至可让编码代理按 AGENTS.md 直接建应用（来源：README）。

## 核心功能（依据 README，注明来源）

- **Wasm 沙箱 + 内置 SQLite**：每个应用独立沙箱，SQLite 在沙箱内（来源：README）。
- **单二进制托管多应用**：一个二进制、一台服务器（来源：README）。
- **CLI 流程**：install.sh、spared setup（wasip2 target + wasi-sdk + 编译 SQLite）、serve（127.0.0.1:8080 应用、8081 管理）、new/deploy（来源：README Try it）。
- **基准对比**：热请求、p99、唤醒、内存与 workerd 对比（来源：README，**自述、未独立核实**）。
- **与同类对比表**：celld、Spin、workerd、wasmCloud（来源：README “How it compares”）。
- **Apache-2.0 许可**（来源：GitHub API）。

## 使用场景

- 想自托管函数/小应用平台的开发者。
- 需要多租户/多应用数据隔离的场景。
- Rust + Wasm + SQLite 技术栈的实验与生产评估。

## 适用人群

Rust 开发者、自托管/无服务器爱好者、平台工程师。只想部署普通 Web 应用的人门槛较高。

## 注意事项

- Apache-2.0 许可。
- Early preview、45 star、无 release，极早期；性能数字自述、未独立核实。
- 仅测试于 Linux x86_64 glibc 2.35+（来源：README）。
- 需要 Rust 工具链（应用是 Rust crate）；setup 首次编译 SQLite 约一分钟（来源：README，自述）。

## 总结

spared 把“自托管的 Workers + 持久化对象”做成单二进制 + Wasm 沙箱 + 应用内 SQLite，架构清晰、基准亮眼。Early preview、45 star、自述数字决定了它仍需验证。对 Rust/Wasm 与自托管平台感兴趣者，这是很有前途的实验。

## 独立评分（满分 10）

- **质量**：8 — Wasm 沙箱 + 应用内 SQLite 的架构清晰、基准与对比详尽；early preview、数字自述。
- **学习价值**：8 — 自托管 Workers、Wasm 沙箱隔离与应用内持久化的设计有很高参考价值。
- **使用价值**：7 — Rust/Wasm 与自托管平台场景价值高，普通部署门槛较高。
- **推荐等级**：**B**
