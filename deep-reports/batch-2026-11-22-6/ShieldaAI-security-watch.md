# ShieldaAI/security-watch

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/ShieldaAI/security-watch
- **分类**：self-hosted（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：JavaScript
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：4 / 2
- **许可证**：Apache-2.0
- **创建时间**：2026-10-23
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-19）
- **最新 release**：Security Watch 1.2.0（tag `v1.2.0`，发布于 2026-11-19）
- **一句话简介**：自托管的依赖与安全公告监控器：读你的 lockfile 建 SBOM，盯着公告和安全新闻，命中就发 Slack 或邮件并告诉你去哪查

## 一句话理解
这是一个跑在你自己服务器或笔记本上的安全哨兵：把你项目的依赖清单变成一份资产清单，持续比对公开漏洞公告和安全新闻，一旦和你跑的东西有关，就发 Slack 或邮件告诉你哪个系统、哪个库、去哪查。

## 通俗解释
README 说，安全新闻太多而与你相关的很少，等到 CVE 正式入库或每天查一次都太晚——漏洞常在几小时内就被利用，而 CVE 记录往往滞后于厂商公告和新闻。Security Watch 默认每 30 分钟检查一次，停机后会自动补上；它同时读公告数据库和安全新闻，新闻一提到你清单里的东西就先给一条“可能受影响，请核查”。它还能从现有 lockfile 和 manifest 读出依赖（覆盖 npm、Python、Go、Rust、PHP、Ruby、Java、.NET、Dart、Elixir、Swift、Haskell 等 12 类生态）写成 CycloneDX SBOM，也可以手写 YAML 清单，并为系统、库附上负责人和备注，告警里直接重复这些备注，让人知道该找谁、看哪里。README 非常长（约 4.8 万字符），覆盖安装、CI 上传 SBOM、22 种 LLM 提供商、Docker 与 Compose、内部决策规则、运维命令、隐私边界与限制，诚实度不错：它明确写了没有自动补丁、没有独立安全审计、不做详尽历史审计，当前是 1.2.0。它只有 4 star、2 fork，但内容远比星数扎实。

## 它不是什么
1. 不是漏洞扫描器或补丁工具——它不修补、不执行来自新闻的 shell，也不做详尽的历史审计。
2. 不是托管 SaaS——它跑在你自己的机器或容器里，资产清单留在你手里；README 说没有托管面板。
3. 不是装了就安全的保证——它明说 health 只报告监控健康，不等于系统安全，空 findings 也不是 all-clear。
4. 不是纯规则引擎的替代品——它用 LLM 对新闻做判定，但模型不能降低源严重级别、不能改收件人、也不能证明依赖是安全的。
5. 不是万能资产发现——它是从 lockfile、SBOM 或手写清单建立清单，产品、数据库和 SaaS 需要你自己描述。

## 痛点
- 安全信息过载，绝大多数与你无关，人工筛选跟不上。
- CVE 记录滞后，等它出现再动手已经晚了。
- 每天或每周一次的检查会留下大段无人值守的空窗。
- 光知道“lodash 有漏洞”没用，还得知道哪个服务、仓库、团队受影响。
- 清单会过期：库本身会变 vulnerable，需要 CI 持续送新 SBOM，一周没更新就该告警。

## 核心功能（依据 README，注明来源）
- 从代码生成清单：sbom 与 init --project 读取 12 类生态的 lockfile 和 manifest，写出 CycloneDX SBOM；提供 GitHub Action 和一行命令供其它 CI 上传，也接受 Trivy、Syft、cdxgen、GitHub 依赖图产出的 SBOM（来源：README Features / Keep it current from CI）。
- 持续比对公告与新闻：收集 cve.now 更新与 The Hacker News 文章，按配置频率与回溯范围抓取，停机后自行补追；新闻命中清单条目会先报“可能受影响”（来源：README The problem / Why a daily check is not enough / How decisions work）。
- 告警带证据与落点：每条 finding 有 Finding ID、影响判定、源严重级别、修复版本、Where to look、Affected item；这些备注来自你自己的 systems.yaml（来源：README What an alert looks like / Describe systems by hand）。
- 可配置的通知策略与摘要：严重、正在被利用、恶意包、尚无结构化数据的严重新闻线索即时通知，其它进每日摘要；目录为空就不发；可用 explain、acknowledge、suppress、review 管理（来源：README What gets sent / Operate）。
- 22 种 LLM 提供商：涵盖 OpenAI、Anthropic、Gemini 等 API，Groq、DeepSeek、Mistral 等托管端点，Ollama、LM Studio、vLLM、LiteLLM 等本地或网关，默认 minimal 披露档案；模型判定默认需要本地复核（来源：README LLM providers / How decisions work）。
- Docker 与 Compose 部署：给出只读根文件系统、cap-drop、非 root UID 与 GID、SQLite 放原生 volume、私有 .env 与只读挂载等具体做法（来源：README Run with Docker）。
- 运维与隐私边界：scan、start、status、findings、deliveries、health、backup、verify-backup、prune 等命令，说明哪些数据会发给 cve.now、OSV、模型提供商和 Slack 或 email，README 也列出未实现项（来源：README Operate / Boundaries and privacy）。

## 使用场景
- 工程团队想在本机或小服务器上持续盯住自己依赖栈的新公告。
- 需要把 SBOM 从 CI 推到监控端，并让告警带负责人和集群、命名空间等上下文。
- 希望在 CVE 正式入库前，从厂商公告和安全新闻里抢先得到提示。
- 需要把 AI 判定限制在可复核范围内，并保留证据、历史与本地复核队列。
- 用 Docker 或 Compose 在受控环境里跑一个只读、非 root 的监控服务。

## 适用人群
负责依赖安全、SRE 或平台工程的从业者；有自建 CI 并愿意维护 SBOM 的团队；希望数据留在自己环境里、同时接受配置与运维成本的中小组织。不适合完全没有运维能力、或期待开箱即托管服务的人。

## 注意事项
- 仅 4 star、2 fork（2026-10-23 创建），但 README 极详尽；有 v1.2.0 release（2026-11-19），最新提交合并了 news-signals 特性分支。
- README 明示没有独立安全审计，也没有自动补丁、托管面板、VEX 导入、操作系统包匹配、分布式 worker 等；要看 SECURITY.md 再决定是否依赖。
- 需要 Node.js 24.10 以上；首次扫描读最近一天公告，约每秒一次请求，预计 15 到 20 分钟，后续通常一两分钟（来源：README Get started）。
- 同一数据库只能有一个 scan 或 scheduler 实例写入；不要把数据库放在网络文件系统或在容器间共享。
- 默认披露档案会把匹配的包名、版本和部分证据发给所选模型提供商；监控私有依赖前要看 SECURITY.md。
- 通知的 accepted 只表示目标接收，不代表被阅读；不确定送达要先查目标再 retry。
- health 不发起网络请求，它报告的是监控健康，不是系统是否安全。

## 总结
Security Watch 是一个设计相当完整、文档极其细致的自托管安全监控工具：从 SBOM 生成、公告与新闻双通道、可复核的 LLM 判定，到通知策略、运维命令和隐私边界，都写得很清楚，并且坦率列出限制。4 star 与“无独立安全审计”提醒我们它仍需要谨慎评估；真正要用好它，得愿意维护清单、配置 CI 上传并理解它的判定边界。对有安全运维需求的团队，它是值得认真看一眼的候选。

## 独立评分（满分 10）

- **质量**：7 — 功能设计完整、文档极细，但 4 star 且无独立安全审计
- **学习价值**：7 — 把 CVE、新闻、SBOM、LLM 判定与通知策略串成一套可理解的流程
- **使用价值**：8 — 团队可以用它持续盯住自己栈里的新漏洞并定位到具体服务
- **推荐等级**：**B**
