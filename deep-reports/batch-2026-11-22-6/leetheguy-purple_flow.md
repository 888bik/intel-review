# leetheguy/purple_flow

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/leetheguy/purple_flow
- **分类**：ai-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Elixir
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：14 / 1
- **许可证**：MIT
- **创建时间**：2026-09-25
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-13）
- **最新 release**：PurpleFlow 0.1.0（tag `v0.1.0`，发布于 2026-09-30）
- **一句话简介**：用 Elixir/OTP 从零重写的极简 n8n，工作流以 TOML 文件存放，主打让 AI 代理自己编排自动化与智能体集群

## 一句话理解
一个用 Elixir 写的自托管工作流自动化引擎：把 n8n 的核心模型简化成纯文本 TOML 工作流，让 AI 代理通过 HTTP、MCP 或令牌直接创建和运行流程，甚至编排会自我复制的智能体集群。

## 通俗解释
GitHub 描述字段为空，以下内容均依据 README，未核实的细节不作推断。作者的定位很清楚：n8n 对人好用，但对 AI 不够友好，例如 Agent 输出被裁剪、流式能力弱、延迟和许可问题、没有企业版就没有 Git 版本控制。于是他用 Elixir/OTP 做了一个第一性原理重写的极简版本，工作流与节点都是普通 TOML 文件，天然适合放进 Git，也适合让模型读写。

README 里最醒目的是性能对比：自述可在 512MB 内存上跑一万多个并发工作流，内存效率约为 n8n 的 200 倍；压力测试中一万个一秒任务 3.2 秒跑完，而 n8n 用 2GB 内存只能跑 100 个；100 条失败分支半秒完成，n8n 则到 208 秒栈溢出。这些都是项目自述、未独立核实的数字，读者应把它们当作作者在其测试环境下的报告，而不是第三方基准。另外作者坦言基础版本是用 Opus 5.5 在 4 天内、靠 80 美元免费额度辅助生成的，这既解释了迭代速度，也提醒我们代码成熟度需要自己判断。

设计上它刻意不做大量便利连接器节点，而是让 AI 查 API、自己生成 HTTP 请求；节点类型只保留 webhook、cron、manual 三种触发器，以及 http、ssh、postgres、code、batch、wait、respond、noop、workflow 等少数几种，流程控制直接写在工作流里。凭据用 Postgres 加密存储，工作流文件夹独立 git 管理，代码节点跑在隔离 sidecar。README 也诚实指出一个未解决的安全隐患：同一 sidecar 内不同代码节点的代码可能互相接触。

## 它不是什么
1. 不是 n8n 的官方分支或完整兼容替代品，节点生态远少于 n8n，没有大量现成连接器。
2. 不是零运维服务，官方支持方式是 Docker Compose，还要自己管 Postgres、环境变量与凭据。
3. 不是已经形成社区规模的成熟产品：14 star、1 fork、只有一个 v0.1.0 发布，README 自己也说 still early。
4. 不是无风险的智能体平台：示例 purple-goo 的 webhook 默认无鉴权，README 明确提醒别直接暴露到公网。
5. 不是完全隔离的沙箱：本地开发模式没有 runner 容器，代码节点在应用自身 VM 内运行，且作者自述已知代码节点间存在隔离缺口。

## 痛点
- n8n 的 Agent 节点输出被裁剪、流式支持有限，在 AI 场景下束手束脚。
- 企业版才有 Git 版本控制，个人和自托管用户难以把工作流纳入代码库管理。
- SaaS 方案在一个人用和每月数百美元的商业套餐之间缺少中间档。
- 让多个 AI 代理协作时，JavaScript/Python 方案在高并发和内存占用上成本偏高。
- MCP 工具过载导致模型推理质量下降，需要更轻的技能加载方式。

## 核心功能（依据 README，注明来源）
- 工作流与节点均为纯文本 TOML，可独立 git init 管理并在约两秒内自动热加载（来源：README How it works 与 Workflow files）
- 内置 webhook、cron、manual 三种触发器，以及 http、ssh、postgres、code、batch、wait、respond、noop、workflow 等节点（来源：README How it works）
- 支持代理通过 /agents 页面、令牌或 MCP 桥接读写工作流，并按名字懒加载技能（来源：README Getting started 与 Lazy load your skills）
- 凭据在 Postgres 中以 PURPLEFLOW_SECRET_KEY 加密，并支持 OAuth 登录类凭据（来源：README Where are the connectors 与 Credentials）
- 代码节点在独立 sidecar 运行，无网络、无数据库、无凭据、只读文件系统；文件服务与 MCP 也做隔离（来源：README Security）
- Docker Compose 一键起应用、Postgres、runner 与文件服务，含健康检查、看门狗与自动迁移（来源：README Running it）
- 示例包含会生成子智能体集群的 purple-goo 递归 swarm 工作流（来源：README Samples 与 Purple Goo）

## 使用场景
- 让编码代理根据自然语言需求自己生成、调试并版本化自动化流程。
- 需要高并发、低内存占用的自托管自动化底座，例如大量 webhook 触发的轻量任务。
- 想在团队内把工作流当代码来 review、回滚和分发，而不是锁在 SaaS 后台。
- 研究代理集群构建代理集群这类递归编排实验。

## 适用人群
熟悉 Docker、Postgres 与 AI 代理框架的开发者和自动化工程师；愿意自己承担运维与安全配置的小团队。只想开箱即用、不想碰环境变量和 Worker 配置的用户会失望。

## 注意事项
- 所有性能与对比数字均为自述、未独立核实，引用时请标注其测试条件（两个任务、单一模型、每组 20 次运行等）。
- 授权令牌权限很大，README 提醒要约束代理；goo webhook 无鉴权，切勿直接暴露公网。
- 三次登录失败会锁定该地址四小时；反向代理后必须设置 PURPLEFLOW_CLIENT_IP_HEADER，否则可能把所有人锁在外面。
- 工作流文件夹必须在首次 docker compose up 前自行建立并 git init，否则 Docker 会以 root 创建空的 git 目录。
- 14 star、1 fork，社区很小；作者自述存在代码节点间相互接触的未解决安全缺口。

## 总结
PurpleFlow 的野心是把 n8n 的核心体验用 Elixir/OTP 重做一遍，并直接面向 AI 代理设计：TOML 工作流、懒加载技能、MCP 桥接、递归智能体 swarm，都用相当克制的组件数量实现。README 信息量很大，也难得地写清了安全边界与已知缺口。但它仍是 v0.1.0 的早期项目，性能优势主要是自述、未独立核实，且运维门槛不低。适合愿意自己掌控工作流与数据、并接受早期软件不确定性的技术用户。

## 独立评分（满分 10）

- **质量**：7 — 设计完整但仍是 v0.1.0 早期项目，性能数字为自述
- **学习价值**：8 — 用 Elixir/OTP 重写 n8n 的架构与 AI 代理编排思路有启发
- **使用价值**：7 — 自托管自动化底座可用，但需要自己运维 Docker 与 Postgres
- **推荐等级**：**B**
