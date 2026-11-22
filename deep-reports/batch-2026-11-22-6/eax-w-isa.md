# eax/w-isa

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/eax/w-isa
- **分类**：ai-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Python
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：2 / 0
- **许可证**：MIT
- **创建时间**：2026-11-03
- **最近更新**：2026-11-14（元数据 updated_at；最近提交 2026-11-11）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：给本地大模型做可逆、无梯度的权重编辑：JSON 指令编译加运行时事务与回滚，再配激活触发；v0.1.0 研究原型，2 star

## 一句话理解
W-ISA 是一套给本地语言模型改权重的小语言：你写一段 JSON 指令，它编译成针对具体 PyTorch 参数的操作，在冻结模型、无梯度的前提下用线性代数改权重，并且能精确撤销；再配一层基于激活的触发器，让某些编辑只在匹配到输入时生效。

## 通俗解释
README（约 2.4 万字符）开篇就写明这是 v0.1.0 研究软件：提供 JSON 指令格式、编译器、Python 运行时，支持具名权重操作、请求级补丁和快照恢复。默认加载 Qwen2.5-0.5B-Instruct，也接受本地 checkpoint 或其他 Transformers causal LM（如 SmolLM-135M）。指令操作码包括 ADD、SCALE、RANK1、PROJECT_OUT、WRITE_KEY、PRUNE，还能用 slice 只改某个维度的一段。编译时会检查参数名、操作数形状、有限值、字段与投影方向；执行前备份全部目标参数，异常或取消就回滚，undo 精确恢复，temporary 提供带锁的请求作用域。两篇 arXiv 论文（W-ISA 与 NCP，作者 Ekrem Aksoy）分别讲执行运行时与候选合成、验证、规则准入，软件用 MIT，论文用 CC BY-NC-ND 4.0。README 诚实区分已测环境：2026-11-04 在 ARM64 macOS、Python 3.12.9、PyTorch 2.15.0 等环境下报告 66 个测试通过，以及 Qwen 和 SmolLM 的往返与基准实验数据——这些是自述、未独立核实。2 star、2026-11-03 创建，无 release，最新提交只是补论文引用。

## 它不是什么
1. 不是训练或微调框架——它不改梯度、不训练，只做确定性的线性权重变换。
2. 不是生产级模型服务——CLI 的编辑只在当前进程的内存权重上生效，README 说常驻应用要直接用 WeightRuntime。
3. 不是通用模型编辑平台——默认只针对少数小模型与具名参数，换架构要先 inspect 参数名。
4. 不是免费提升能力的魔法——它是研究工具，README 用困惑度、logit 变化等客观指标报告影响，也承认编辑会改变输出。
5. 不是单篇论文的附带代码——它把运行时（W-ISA）与控制策略（NCP）拆成两篇，以引用互补。

## 痛点
- 想研究“改某个权重会发生什么”时，缺少可复现、可回滚、带校验的实验脚手架。
- 直接手写 inplace 修改容易破坏参数、留下脏状态，忘了恢复就污染后续实验。
- 多个请求或线程共享一个可变模型时，补丁可能相互覆盖。
- 缺少一种把“检测到某种输入再应用某种编辑”形式化的机制。
- 论文思路与可运行代码往往脱节，很难核对数字。

## 核心功能（依据 README，注明来源）
- JSON 指令与编译器：版本化指令数组，指定 op、target（来自 named_parameters 的全名）、操作数与可选 slice；编译期校验参数名、形状、有限值、投影方向非零，NPZ 用 allow_pickle=False（来源：README Instructions / Embedding the runtime）。
- 六种权重操作：ADD、SCALE、RANK1、PROJECT_OUT、WRITE_KEY、PRUNE，分别对应加法、缩放、秩一更新、投影消除某输入方向、单键最小范数修正、按阈值剪枝（来源：README Instructions 表格）。
- 事务式运行时：执行前备份全部目标参数，异常或取消回滚，undo 精确恢复；保留 Parameter 身份与 ties，按顺序执行并在每条后回转原 dtype；temporary 提供带可重入锁、finally 清理的请求作用域（来源：README Embedding the runtime）。
- 设备与 dtype 支持：--device 可选 auto、CUDA、MPS、CPU，权重默认 FP32，另支持 float16 与 bfloat16，编辑在 CPU 上用 FP64 计算，同步后写回原参数视图（来源：README Run it / Numerical behavior and cost）。
- 激活分析与触发：ActivationProfiler 在指定模块抓取输入或输出激活，contrast 给出正负类均值差，Rule 加 MatchActionController 按 cosine_score 大于阈值命中第一条规则应用补丁，阈值校准独立进行（来源：README Evaluation, profiles, and match-action）。
- 可复现实验：附带 SmolLM 往返、Qwen2.5-1.5B rank-one 基准、WikiText-2 困惑度与画像与匹配动作实验脚本，记录 logits、token 序列、权重哈希、设备与原始 JSON（来源：README Validation / Pretrained SmolLM roundtrip / Qwen2.5-1.5B execution comparison）。

## 使用场景
- 解释性或可控性研究：验证“把某个方向从权重里投影掉”会怎样影响行为。
- 需要严格可回滚的在线实验：请求级补丁结束后自动恢复，不污染下一个请求。
- 想用激活触发实现“输入像某类内容时才应用某种编辑”的控制策略。
- 复现论文中的局部实验，并核对 logits、token 与权重哈希。

## 适用人群
做 LLM 可解释性、可控性研究的研究者与研究生；想快速验证线性代数式权重干预思路、且能自己读 PyTorch 代码的人。不适合把模型当黑盒 API 调用、或需要生产 SLA 的团队。

## 注意事项
- 仅 2 star、0 fork、2026-11-03 创建，无 release（releases/latest 返回 404），最新提交是文档更新；尚无社区验证。
- README 中的性能与测试结果（66 个测试通过、方向响应从 6.4653542 降到约 1.43e-7、编辑事务 9.2 到 14.6 毫秒、Qwen 请求耗时表、RSS 增加等）均为自述、未独立核实，且明确绑定特定硬件与软件版本（2026-11-04 的 ARM64 macOS、PyTorch 2.15.0 等）。
- 声明 Python 3.10 以上，实测解释器是 3.12.9；依赖 torch、transformers、numpy；首次运行会下载模型权重与数据。
- 所有矩阵指令都是 O(mn) 且会物化稠密矩阵，峰值内存还要算上 undo 快照、事务备份、FP64 操作数和缓冲；FP32 Qwen 0.5B 权重本身约 2GB。
- 编辑后必须以全新 KV cache 开始生成；共享模型的其它推理要自己拿 runtime.lock 包住。
- 软件是 MIT，但两篇论文是 CC BY-NC-ND 4.0，引用时按各自许可处理；模型权重、依赖与数据集各有自己的许可证。
- 默认加载 Qwen2.5-0.5B-Instruct；示例的剪枝针对第一层 MLP down_projection，换模型前先 inspect 参数名。

## 总结
W-ISA 把可逆、无梯度的权重编辑做成了一个有格式、有编译器、有事务运行时、还有触发控制的完整小系统，并且配了论文与大量可复现实验，研究价值高于其 2 star 的体量。但它是明确的 v0.1.0 原型：进程内编辑、性能与结果均为自述、未独立核实，生产环境不应直接依赖。适合做解释性与可控性研究的人读代码、跑实验；想要稳定推理增强的人应把它当作思路来源而非成品。

## 独立评分（满分 10）

- **质量**：7 — 研究原型文档齐全且带论文与实验，但仅 2 star、无 release，实用性有限
- **学习价值**：8 — 把权重空间干预做成带校验、事务、回滚与触发的指令集，学术思路清晰
- **使用价值**：5 — 适合做可复现实验与解释性研究，不适合生产推理服务直接依赖
- **推荐等级**：**C**
