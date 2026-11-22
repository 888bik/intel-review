# humanethq/airgap-check

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/humanethq/airgap-check
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Go
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：21 / 0
- **许可证**：MIT
- **创建时间**：2026-11-08
- **最近更新**：2026-11-14（元数据 updated_at；最近提交 2026-11-11）
- **最新 release**：v0.2.3（tag `v0.2.3`，发布于 2026-11-11）
- **一句话简介**：审计 AI 仓库在断网环境中会崩溃或偷偷联网的行为，检查清单依赖、遥测与 .env 密钥路由

## 一句话理解
一个用 Go 写的命令行审计工具：在你的 AI 应用还没进隔离网络之前，扫出它会在运行时下载模型权重、语料、价格表或偷偷发遥测的地方，并可在真正断网的容器里实测一遍。

## 通俗解释
很多人以为把大模型接口指向本地 vLLM 或 Ollama 就万事大吉，但库往往在首次使用时才下载 tokenizer 排名、ONNX 嵌入权重、语料或价格表；离线时这些调用要么直接失败，要么在退避重试里卡上几十秒。airgap-check 想在部署前把这些行为暴露出来。

它分两层工作。静态层扫描依赖清单和锁文件，包括 requirements、pyproject、Pipfile、package.json、Cargo.toml、go.mod、Gemfile 及 uv.lock、poetry.lock、package-lock、pnpm-lock、yarn.lock、Cargo.lock，并用内置的最小 TOML/YAML 读取器解析，不依赖外部解析器；同时用大量源码模式匹配识别 nltk.download、spacy.load、from_pretrained、tiktoken.get_encoding、whisper.load_model、模型权重 URL、运行时价格检查、pip install 或 git clone 之类的安装命令，以及直接的遥测端点。动态层用 docker run 加 network none 启动一次性容器，注入 loopback DNS sinkhole、TLS SNI 与 HTTP 监听以及 Python/Node 的 socket 钩子，把外联请求捕获下来。README 还给出 --fix 建议的 Dockerfile 环境变量与预热命令，以及对 .env 文件只分类键名和 git 跟踪状态、绝不读取值的隐私原则。

README 包含非常详尽的基准章节，列出多个阶段的精确率与召回率，例如 Phase 2 的 89.7% 精确率、87.5% 召回率，Phase 3 留出集的 93.0% 与 88.3%，以及 Benchmark v2 更低的结果。这些都是项目自述、未独立核实的数字，作者自己也强调样本小、部分阶段不是留出集。工具仅用 Go 标准库，动态运行部分需要 Docker。

## 它不是什么
1. 不是安全认证，README 明确说干净的结果并不能证明应用在隔离网络里一定安全。
2. 不是通用的依赖漏洞扫描器，它关心的是运行时是否联网，而非 CVE 或许可证合规。
3. 不是能覆盖所有语言的万能探测器：动态钩子主要验证过 Python 与 Node 容器，其他运行时回退到错误文本提取。
4. 不是修改代码的自动修复器，--fix 只打印 Dockerfile 片段与预热建议，需要人工判断后再应用。
5. 不是实时监控工具，它报告观察到的行为，且只测到你实际运行的代码路径。

## 痛点
- 气隙或受限网络中的 AI 应用常在首次使用时才暴露隐式下载，上线即崩。
- 遥测线程会静默外联，且命令可能仍以退出码 0 结束，传统测试难以发现。
- 依赖清单里看不出传递依赖带来的运行时下载，比如某个库间接拉入 huggingface-hub。
- 手动用 strace 排查成本高，且不易把具体调用映射回源码位置。
-  Teams 或医院、国防网络对审计证据有需求，但商业方案昂贵或不透明。

## 核心功能（依据 README，注明来源）
- 依赖清单与锁文件扫描，支持 scope 标签区分 prod、transitive_lockfile 与 optional_extra（来源：README What it does 中 Manifest & lockfile scan）
- 源码模式与启发式规则，识别常见运行时下载调用、权重 URL、价格检查、安装型 subprocess 与遥测端点（来源：README Source patterns & heuristics）
- 在 docker run --network none 中运行应用或探针，配合 loopback sinkhole 与 Python/Node socket 钩子（来源：README Python and Node --run）
- --fix 输出 Dockerfile 环境变量与模型预热命令，并支持 .airgap-check.json 或 --rules 自定义规则（来源：README --fix & custom rules 与 Custom rules）
- .env 键名与 git 跟踪状态分类，不读取也不记录密钥值（来源：README .env keys 与 Design principles）
- JSON 机器可读输出与多种安装方式，包括 curl 脚本、go install 与源码构建（来源：README Install 与 Usage）

## 使用场景
- 把 AI 应用部署到气隙 VPC、医院或国防网络前做预检。
- 排查某个容器为何在无网环境下启动慢或崩溃。
- 在 CI 中静态扫描依赖变更，防止新引入会联网的库。
- 审计第三方 AI 仓库是否携带静默遥测与硬编码缓存写入。

## 适用人群
负责自托管或离线部署的运维与安全工程师、需要交付气隙方案的集成商，以及想研究静态分析与容器网络观测实现细节的 Go 开发者。

## 注意事项
- 自述的精确率与召回率数字未独立核实，且作者说明部分阶段用探针而非真实启动应用，阶段划分也存在留出集与非留出集差异。
- 动态 --run 依赖 Docker；其他运行时兜底为 shell_only，未单独验证。
- 硬编码外部 IP 的流量在内核层就会 ENETUNREACH，不一定命中 loopback 监听器；DoH 流量也可能绕过系统解析器。
- 非 root 容器中忽略 HTTP_PROXY 且无法绑定特权端口时，会记录 ENETUNREACH 而非 SNI。
- sinkhole 立即应答，真实隔离环境可能直接丢包，因此实际挂起时间可能比 README 表格更长。
- 21 star、0 fork，项目较新；v0.2.3 发布于 2026-11-11，仍在快速修规则误报与漏报。

## 总结
airgap-check 抓住了一个真实而具体的痛点：AI 应用的隐式联网行为往往在断网那一刻才被发现。它用 Go 标准库把清单扫描、源码启发式与无网容器实测拼成一套可操作的流程，并为误报漏报提供了大量诚实的基准和自我批评。缺点同样明显：基准数字为自述、未独立核实，动态探测覆盖有限，且它明确声明不是安全认证。对于要向隔离网络交付 AI 系统的团队，它是值得纳入流水线的预检工具；对普通在线应用用户，价值有限。

## 独立评分（满分 10）

- **质量**：8 — Go 标准库实现、规则细致，附有可复核的基准章节
- **学习价值**：8 — 离线依赖探测、sinkhole 与 socket hook 设计很有学习价值
- **使用价值**：8 — 部署气隙 AI 应用前能提前发现隐式下载与遥测
- **推荐等级**：**B**
