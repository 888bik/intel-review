# AmSach/maple

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/AmSach/maple
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Python
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：65 / 5
- **许可证**：Apache License 2.0（LICENSE 文件确认，GitHub API 误标 NOASSERTION）
- **创建时间**：2026-11-07
- **最近更新**：2026-11-20（元数据 updated_at；最近提交 2026-11-19）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：MAPLE：对 MoE 大模型做“流形激活的潜在专家剪枝”，把 DeepSeek-Coder-V2-Lite/Gemma 4 26B 等压进 5GB 以下 GGUF，目标是在 6GB 消费级显卡上全显存运行。

## 一句话理解

它把 MoE 大模型里“几乎不会被路由到的专家”剪掉，显著缩小体积，让原本 9–10GB 的模型塞进 6GB 显卡的显存里跑，避免 PCIe 来回搬数据。

## 通俗解释

MoE（混合专家）模型参数巨大但每次只激活少数专家。MAPLE 的思路是：对代码/数学等目标领域，绝大多数专家几乎不被调用，把它们整组剪掉，并把 gate logits 设为 -∞（FP16 用 -1e9）以保持剩余专家的 softmax 路由概率不变（来源：README “The Forensic Proof”，**以下方法与数字均为项目自述、未独立核实**）。README 宣称：DeepSeek-Coder-V2-Lite（16B MoE）从 8.57GB(Q4)/5.64GB(IQ2) 压到 4.50GB GGUF（保留 54/64 路由专家 + 2 共享专家），Gemma 4 26B-A4B 压到 4.75GB（保留 48/128 路由专家），在 RTX 4050 6GB 笔记本上 100% 显存驻留，生成吞吐分别为 65.6–76.3 和 45.4 tokens/s（来源：README）。它还给了 HumanEval/MBPP/GSM8K/MMLU 等基准表，声称保留 96.7–98.2% 质量，同时坦承多语言困惑度从 98.9 升到 572.4（剪掉非英语专家的权衡，来源：README）。65 star、Apache-2.0（GitHub API 标 NOASSERTION，LICENSE 文件确认为 Apache 2.0）、无 release、2026-11 创建。

## 它不是什么

1. **不是通用量化工具**。它做的是结构化的专家剪枝 + 量化容器（来源：README）。
2. **不是无损压缩**。README 明确多语言困惑度大幅上升是“intended trade-off”（来源：README）。
3. **不是官方模型**。DeepSeek/Gemma 是第三方模型，项目做的是剪枝衍生版（来源：README）。
4. **不是已被独立验证的性能结论**。所有速度/保留率数字为项目自述、未独立核实。
5. **不是有 release 的稳定项目**。65 star、无 release、5 fork、0 issue（来源：GitHub）。

## 痛点

- **6GB 显卡的 PCIe 内存墙**：未剪模型有 4.5GB 要放在系统内存，每 token 107.5ms PCIe 惩罚（来源：README，自述）。
- **MoE 专家冗余**：技术 token 上 top 48 专家承载 94.7% softmax 概率质量（来源：README，自述）。
- **剪枝后路由不变**：softmax masking 保持剩余专家相对概率（来源：README）。
- **领域权衡**：牺牲多语言能力换代码/数学场景的显存驻留（来源：README）。

## 核心功能（依据 README，注明来源）

- **MAPLE 剪枝方法**：流形激活的潜在专家剪枝 + logit masking 保持路由不变（来源：README “Forensic Proof”）。
- **旗舰发布**：DeepSeek-Coder-V2-Lite MAPLE 4.50GB GGUF、Gemma 4 26B-A4B MAPLE 4.75GB GGUF（来源：README）。
- **基准表**：HumanEval、MBPP、GSM8K、MATH、MMLU、ARC、HellaSwag 保留率对比（来源：README，自述）。
- **硬件遥测与负载基准**：RTX 4050 6GB 上的显存驻留与 TPS（来源：README，自述）。
- **直接下载与运行指南**：Hugging Face GGUF 与提示模板/生产负载示例（来源：README）。
- **Apache License 2.0**（LICENSE 文件确认；GitHub API 曾误标 NOASSERTION）。

## 使用场景

- 6GB 消费级显卡上跑代码/数学 MoE 模型。
- MoE 剪枝与路由不变性研究。
- 显存受限的本地推理实验。

## 适用人群

本地 LLM 玩家、模型压缩研究者。需要多语言能力的用户不适合。

## 注意事项

- 许可：Apache-2.0（LICENSE 文件确认；GitHub API 标 NOASSERTION，本仓库已读 LICENSE 澄清）。
- 所有性能/保留率为项目自述、未独立核实；争议性结论需自行复现。
- 多语言能力明显退化（README 自述困惑度 572.4）。
- 65 star、无 release、极早期；依赖 GGUF/llama.cpp 生态。

## 总结

maple 用“剪掉沉睡专家 + 保持路由 softmax 不变”把大 MoE 压进 6GB 显卡，思路完整、基准详尽，还坦诚列出多语言退化的权衡。所有数字为自述、项目极新、争议大，适合本地推理玩家与模型压缩研究者试验，不宜无条件相信其结论。

## 独立评分（满分 10）

- **质量**：7 — MoE 专家剪枝 + 严格<5GB 容器的思路完整、基准详尽；数字全为自述、争议大。
- **学习价值**：8 — MoE 结构化剪枝、softmax 不变性与 PCIe 内存墙的分析很有研究价值。
- **使用价值**：7 — 6GB 显卡跑大 MoE 模型有吸引力，但需自行验证。
- **推荐等级**：**B**
