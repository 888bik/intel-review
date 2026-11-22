# neoma-pcam/pcam-live

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/neoma-pcam/pcam-live
- **分类**：ai-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Rust
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：1 / 0
- **许可证**：MIT
- **创建时间**：2026-10-06
- **最近更新**：2026-11-20（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：NEOMA Physical AI Brain-Spine Co-Processor Suite (v1.0)（tag `v1.0.0-brain-spine`，发布于 2026-11-04）
- **一句话简介**：自述的室温模拟物理协处理器：提供 12+5 域 API、64 自旋 Ising 求解与 PoPW 硬件签名

## 一句话理解
pcam-live 声称把“物理平衡求解”做成云端协处理器：客户端只传四个标量，远端用模拟硅硬件算出阻尼系数与控制信号，再返回带硬件签名的结果，覆盖量化交易、电池、机器人甚至 AI 治理等十几个领域。

## 通俗解释
这个仓库的 README 极长（约 4.6 万字符），几乎全部由项目方叙述构成。它描述了一套“双轨”架构：Track A 面向工业与物理控制，含通用平衡、量化、电池、制药、机器人、出行、化学、Ising、安全、治理、数据中心散热和电网稳定共 12 个域；Track B 面向非临床生命科学研究，含康复、生理数字孪生、预后合成等 5 个域。调用方式包括 REST API、dimod 兼容的 Python SDK、Hosted MCP Server 和本地 Rust stdio MCP。README 里塞满了极为惊人的数字：低于 0.03 毫秒的反射延迟、0.011 毫秒的 64 自旋 Ising、83.08% 的 GPU 功耗下降、NASA B0005 电池 168 个循环全量验证、SEAL-PoPW 单用途签名等。这些全部是项目自述、未独立核实，仓库只有 1 star，也找不到第三方复现报告。更关键的是，README 自己写明“enclave core 与神经硬件逻辑为专有保密”，也就是核心并不在仓库里。

## 它不是什么
1. 不是一个可以本地复现的开源硬件项目，核心物理引擎被作者标为专有与保密。
2. 不是一个纯软件库，主要能力通过云端 API、RapidAPI 或 Apify 端点交付。
3. 不是经过同行评审的基准工具，所有性能表格由项目方自己提供。
4. 不是可直接用于交易、医疗或安全关键系统的组件，README 免责声明明确要求外部硬件互锁。
5. 不是简单的 Ising 求解器，它把物理、生物、金融、治理等叙事打包在同一套四标量接口下。

## 痛点
- 非线性系统的求解在软件里耗时，项目声称用模拟硬件微秒级收敛。
- 高频执行、机器人控制等场景希望有可审计的确定性信号。
- 云端推理集群的功耗与散热成本，项目声称可大幅降低。
- AI 治理需要“硬件锚定”的审计凭据，项目声称能生成防篡改证书。
- 真实物理失败数据稀缺，项目声称能生成三年期预测与故障模拟数据。

## 核心功能（依据 README，注明来源）
- 提供 12 个 Track A 生产端点，包括通用平衡、quant、battery、pharma、robotics、mobility、chemistry、ising、security、governance、datacenter-thermal 与 grid-stability（来源：README Part 1 与 10-Domain Production Endpoints）。
- 提供 5 个 Track B 生命科学端点，响应带 regulatory_scope 与 phi_retention 合规头，强调仅用于非临床研究与数字孪生（来源：README Part 2）。
- 64 自旋 Ising 基态求解器，dimod BQM 兼容，可通过 pip 安装 pcam-sampler 直接 offload（来源：README Ising BQM Ecosystem Integration）。
- Hosted MCP Server 通过 Apify 提供 Streamable HTTP 端点，也提供本地 Rust stdio MCP，暴露五个硬件工具（来源：README Hosted MCP Server 与 Local Self-Hosted MCP Server）。
- SEAL-PoPW 单用途 SHA-256 凭据与动态材料 PUF，用于“零信任”硬件来源审计（来源：README Silicon Root-of-Trust & Dynamic Material PUF）。
- 客户端验证脚本 pcam_objective_audit.py，声称可检验 RTT、zeta 区间与 SEAL 签名（来源：README Zero-Trust Client-Side Hardware Attestation Protocol）。
- n8n 社区节点，把表格或数据库行映射到 10 个域并返回 zeta、control_intensity 与审计凭据（来源：README No-Code Enterprise & Operations Workflow）。
- 给出 NASA PCoE B0005 电池 168 循环、GPU 功耗 PoC 与 64 自旋 Ising 三组基准及 CSV，但均由项目方提供（来源：README Public Ground-Truth Benchmark 与 Empirical Benchmark）。

## 使用场景
- 想试验“模拟物理协处理器”概念的研究者，通过免费额度做小规模调用。
- 需要把 Ising 或 QUBO 问题 offload 到外部求解器的 dimod 用户。
- 希望给 AI 代理接入硬件锚定审计网关的团队。
- 关注 GPU 集群功耗、电池老化预测的非关键路径 PoC。
- 想用 MCP 工具把物理求解能力接进 Claude、Cursor 等客户端。

## 适用人群
适合愿意把项目叙事当作实验素材、自行做严格验证的研究者；不适合需要稳定 SLA、可复现基准或可审计内核的生产团队。

## 注意事项
- 只有 1 star、2026 年 10 月 6 日创建，全部性能数字均为自述、未独立核实，应视为项目方主张而非事实。
- README 自身承认核心 enclave 与硬件逻辑是 Proprietary & Confidential，开源部分主要是客户端示例、SDK 和文档，无法从代码验证“物理硅”是否存在。
- 亚毫秒延迟指 enclave 边界上的模拟平衡时间，端到端 RTT 受公网路由影响，这一点 README 已注明。
- 服务依赖 api.neoma-pcam.com、RapidAPI、Apify 等第三方网关及 API key，可用性与数据路由需自行评估。
- 金融、医疗、安全等关键用途被作者自己排除在保修之外，并明确要求独立硬件互锁与断路器。
- README 中版本标识混乱：标题写 v5.7.0、仓库 release 名为 v1.0.0-brain-spine、SDK 又提 v5.4.0，说明文档维护并不严谨。
- 大量术语（PoPW、PUF、零 PRNG、EU AI Act 合规门）堆叠，缺少可验证的第三方材料，判断时务必保守。

## 总结
就“能不能据此判断技术真实性”而言，pcam-live 给出的证据严重不足：1 star、核心闭源、所有基准由项目方提供、叙事跨度从量子金融到欧盟合规无所不包。它更像一份面向合作方与 API 用户的营销型技术白皮书，而不是可复现的开源工程。因此这里给 C：概念叙述可以一读，数字不该引用，生产采用前必须自己做独立的端到端验证。

## 独立评分（满分 10）

- **质量**：3 — README 堆砌大量超常性能数字与硬件叙事，但仅 1 star、无第三方复核，可信度无法确认
- **学习价值**：4 — 可作为“如何包装硬件加速叙事”的样本，具体技术细节多为自述、难以复用
- **使用价值**：3 — 服务依赖云端 API 与第三方网关，缺少可独立验证的性能依据，实际采用风险高
- **推荐等级**：**C**
