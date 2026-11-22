# akkio-inc/Igloo

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/akkio-inc/Igloo
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Go
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：20 / 0
- **许可证**：Apache-2.0
- **创建时间**：2026-10-29
- **最近更新**：2026-11-20（元数据 updated_at；最近提交 2026-11-20）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：开源 SQL 数据仓库：PostgreSQL 线路协议、内嵌 DuckDB、Apache Iceberg 表与 Arrow Flight SQL，跑在你自己的云或机器上（20 star）

## 一句话理解
Igloo 是一个开源 SQL 数据仓库，跑你自己的云账号或机器上：客户端用 PostgreSQL 协议连进来，查询在内嵌的 DuckDB 上执行，表以 Apache Iceberg 格式放在对象存储里，仓库的治理、共享和调度能力都用 SQL 管理。

## 通俗解释
README 说 Igloo 用 Postgres 线路协议，所以 psql、JDBC、psycopg、pgx、dbt、BI 工具和笔记本都能直连，不需要专用驱动；结果还能通过 Arrow Flight SQL 以 Arrow 记录批的形式高速传给 pandas、polars、Spark 和 BI。表是对象存储里的 Iceberg（S3、GCS、ADLS 或任何 S3 兼容存储），没有专有格式要迁移，Spark、Trino、PyIceberg 可通过 Igloo 的 Iceberg REST catalog 读写。查询跑在内嵌 DuckDB 上，多数分析负载一台机器就够。它还有 SQL 内的预测（Amazon Chronos、Google TimesFM）和 AI 函数、迁移报告（回放 Snowflake 和 Databricks 查询历史看哪些能跑、答案是否一致、成本多少）、Docker Compose、Helm 与 OpenTofu 部署。README 极长（约 2.6 万字符），功能清单从零拷贝克隆、RBAC、SSO、行访问与脱敏策略、Delta Sharing、联邦查询、个人数据擦除、干净室、数据目录、加载调度、告警、媒体分析、工作负载管理到分布式执行、缩容到零，几乎覆盖现代数仓全部话题。开头“比 Snowflake 和 Databricks 快 3 倍、成本低 25 倍”是自述、未独立核实：README 自己说明这是 TPC-H 100GB、按公开价、Igloo 单机 32 或 64 核，对 X-Small 到 X-Large 的中位数，方法和全部结果在 bench/README.md，读者应自行审阅 caveat。20 star、2026-10-29 创建，暂无 release（/releases/latest 返回 404），最新提交是修一条 MySQL 连接的读操作。

## 它不是什么
1. 不是托管数仓服务——它跑在你自己的云或硬件上，你自己付机器和存储的钱。
2. 不是数据库本身——查询引擎是内嵌 DuckDB，Igloo 是外面的协议、治理、编排与兼容层。
3. 不是闭源专有存储——表是开放的 Iceberg，官方强调没有专有格式要迁出。
4. 不是装好就快 3 倍的魔法——性能对比是自述、未独立核实，条件写在 benchmark 里。
5. 不是单二进制——架构图显示还要配 Lakekeeper 与 PostgreSQL，数据落在对象存储。

## 痛点
- 商业数仓按 credit 计费，成本随用量涨；企业想用自有机器控制成本。
- 迁移前不知道现有查询有多少能跑、答案是否一致、会花多少钱。
- BI 和 ELT 工具依赖 Postgres 协议与 dbt 适配器，换引擎常要改工具链。
- 数据治理（行级权限、脱敏、擦除、干净室）在自建方案里往往缺失。
- 表格式专有会锁定用户，迁移成本高。

## 核心功能（依据 README，注明来源）
- 兼容接入：Postgres 线路协议让 psql、JDBC、dbt、BI 直连，另提供 Arrow Flight SQL（grpc 端口 31337）把结果作为 Arrow 记录批给 ADBC、pandas、polars 与 JDBC（来源：README Postgres-compatible / Arrow-native extracts）。
- 开放表格式与多引擎：Iceberg 表支持 CREATE、INSERT、UPDATE、DELETE、MERGE、time travel、零拷贝克隆、branches 与 tags；通过 8181 端口的 Iceberg REST catalog 让 Spark、Trino、PyIceberg 以 Igloo 用户身份读写（来源：README Open table format / What you get / Other engines）。
- 安全与治理：用户、角色、grants、SCRAM-SHA-256、SSO（Okta、Entra、Google Workspace、OIDC）与 SCIM、访问令牌与服务账号、行访问策略、脱敏策略与标签，web console 亦受同样策略约束（来源：README Users roles and grants / Single sign-on / Row access and masking policies）。
- 数据协作与合规：Delta Sharing 双向共享、个人数据擦除（ERASE SUBJECT，跨所有打标表删除并改写文件、过期快照、清缓存并校验）、干净室（模板审批、结果阈值、双方看不到对方行）（来源：README Delta Sharing / Erasure of personal data / Clean rooms）。
- 目录、加载与调度：数据目录（按名、描述、列、标签、所有者搜索，血缘发送 OpenLineage）、COPY INTO、pipes、cron 或 interval 任务、物化视图；缩容到零网关按需启停计算（来源：README Data catalog / Loading and scheduling / Scale to zero）。
- 免运维能力：自动小文件合并、快照过期、孤儿文件清理；多副本读写同表并自动重试并发写；本地读缓存；按查询 tag 计费与限额、计算池（来源：README Automatic table maintenance / Several replicas / Read cache / Workload management）。
- 迁移与部署：迁移报告回放查询历史；Docker Compose（make up）、Helm chart、AWS 与 GCP 与 Azure 的 OpenTofu 模块；内置 web console（来源：README Quick start / Deploy / Documentation）。

## 使用场景
- 用自有 AWS、GCP、Azure 或机房机器搭建分析型数据仓库，控制成本与数据驻留。
- 从 Snowflake 或 Databricks 迁移前的兼容性、成本评估。
- 需要让 dbt、Metabase、Superset、Power BI、Tableau、Jupyter 不改造工具链直接接入。
- 有严格治理需求：行级权限、PII 脱敏、GDPR 与 CCPA 擦除、跨组织干净室与 Delta Sharing。
- 媒体和广告场景做去重触达、受众重叠、地理与 H3 分析。

## 适用人群
数据平台工程师、分析基础设施团队、希望摆脱商业数仓锁定与计价的组织；对 Iceberg、DuckDB、Arrow 技术栈熟悉并愿意自运维的人。不适合没有运维资源、或把厂商基准当既成事实就采购的人。

## 注意事项
- 20 star、2026-10-29 创建，暂无 release（releases/latest 返回 404），仍很新，社区验证有限。
- “快 3 倍、成本低 25 倍”及“每个尺寸都更快更便宜”均为自述、未独立核实；方法是 TPC-H 100GB、S3 按公开价、Igloo 单机 32 或 64 核，细节和 caveat 在 bench/README.md。
- Quick start 首次 make up 要下载约 2GB 镜像、快网络下约两分钟；TPC-H SF1 约 600 万行 lineitem，笔记本加载约 20 秒、22 个查询暖缓存合计约 4 秒——这些也是 README 自述、未独立核实。
- 密码不能设占位符（如 change-me）或少于 16 字符，否则 Igloo 拒绝建账号。
- README 架构图显示除 Igloo 外还需 Lakekeeper（Iceberg REST catalog）和 PostgreSQL（存元数据与用户），对象存储放 Parquet；不是单二进制。
- 组件多为 Apache-2.0、MIT 或 PostgreSQL 许可，但 DuckDB 扩展里有 GEOS 与 MariaDB Connector C 两个 LGPL 2.1 库，商用分发需留意。
- 截图用的是合成数据，文中公司均为虚构。

## 总结
Igloo 是一个野心很大、README 写得很完整的数据仓库项目：开放 Iceberg 表加内嵌 DuckDB，配 Postgres 与 Arrow 双协议入口，再把治理、共享、调度、可观测和迁移评估做成 SQL 能力，几乎覆盖自建数仓的全部要点。20 star、无 release、性能数字全部自述且未独立核实，这些是评估时必须打折的地方；它的真正价值在于给出一套可部署、可审阅的参考架构。适合有运维能力和数据工程经验的团队试水；期待开箱即超越商业数仓的人应先读 benchmark 的 caveat。

## 独立评分（满分 10）

- **质量**：8 — 架构与功能面非常完整，但性能对比均为自述且无 release、星数仍少
- **学习价值**：8 — 一套把 Iceberg、DuckDB、Postgres 协议和治理能力串起来的现代数仓参考实现
- **使用价值**：8 — 想摆脱 Snowflake 和 Databricks 计费、用自有机器跑分析的人可直接部署
- **推荐等级**：**B**
