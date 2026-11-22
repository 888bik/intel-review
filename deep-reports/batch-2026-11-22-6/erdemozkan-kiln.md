# erdemozkan/kiln

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/erdemozkan/kiln
- **分类**：ai-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：TypeScript
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：6 / 1
- **许可证**：MIT
- **创建时间**：2026-11-03
- **最近更新**：2026-11-19（元数据 updated_at；最近提交 2026-11-20）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：在浏览器里用 WebGPU 直接加载 Hugging Face 原始权重跑语言/视觉/语音模型的 TypeScript 库

## 一句话理解
一个浏览器里的推理库：直接读取模型的原始权重（safetensors / pytorch_model.bin / GGUF），用 WebGPU 跑起来，不需要 ONNX 转换、编译步骤或推理服务器。

## 通俗解释
Kiln 是一个 TypeScript 写的 JavaScript 库，目标是把闲置设备的算力用起来：作者从「世界 DRAM 被闲置」的讨论和抽屉里那台 2017 MacBook Pro 出发，做了这个只需打开网页就能跑模型的方案。它实现了自己的 WebGPU kernel，而不是执行导出的 ONNX 图，因此能加载 Hugging Face 或自己服务器上的原始权重，加载时再量化到 4/8 bit；API 名称沿用 transformers.js（pipeline / AutoTokenizer / AutoModel / TextStreamer），多数代码只改 import 和模型 id 就能迁移。README 约 1.55 万字符，任务表、支持模型、基准、已知短板都写得很实在。项目 6 star、1 fork，无 GitHub release，代码最新提交为 0.3.0，同时提供 npm 与 Python 包。

## 它不是什么
1. 不是模型托管或 API 服务：模型文件由 Hugging Face Hub 或你自己的静态服务器提供，推理全在本地。
2. 不是 transformers.js 的封装：它自己实现模型族的 WebGPU kernel，代价是支持架构比 transformers.js 少。
3. 不是万能模型运行时：SmolLM3、Gemma 1/2、Llama 4、MoE、ModernBERT 等明确未支持。
4. 不是性能已被独立验证的方案：README 的基准是在特定机器上自测。
5. 不是零依赖的理想化玩具：它没有运行时依赖与 WASM runtime，但需要支持 WebGPU 的浏览器；无 WebGPU 时回退到 CPU（WASM SIMD）更慢。

## 痛点
- 浏览器里跑模型通常要先把权重转成 ONNX，流程繁琐
- 推理依赖服务器，数据要上传，隐私与成本都成问题
- transformers.js 在老设备上速度不理想，而高端设备又不是人人都有
- 同一个仓库的 4bit/8bit 变体要分别准备文件

## 核心功能（依据 README，注明来源）
- 原始权重直载：支持 safetensors、pytorch_model.bin 与 GGUF，加载时量化到 4bit/8bit，一个仓库服务所有精度（来源：README Features）
- 不绑死 Hub：endpoint 指向镜像或自有静态服务器，单个 GGUF 可从任意 URL 加载，本机文件/文件夹可就地读取不上传；Node.js 支持路径加载（来源：README Features）
- 门控与私有模型：setAccessToken 仅向 huggingface.co 发送 token，不发往镜像或单文件 URL（来源：README Features）
- 熟悉 API 与多任务：pipeline / AutoTokenizer / AutoModel / TextStreamer；覆盖聊天、图像/音视频问答、embedding、分类、NER、问答、翻译、摘要、ASR、TTS、检测、分割、深度、抠图、超分、OCR、文生图等（来源：README Features 与 Supported models）
- 结构化输出与 LLM 增强：把回答约束到 JSON schema、正则、语法或选项列表；工具调用、内置技能、LoRA、检索助手与跨调用/刷新的 prompt 缓存（来源：README Features 与 Tools and skills）
- 下载与执行体验：浏览器缓存、断点续传、离线加载；模型在 worker 中运行并流式返回，页面不阻塞（来源：README Features）
- 双语言入口：npm 包 kiln-webgpu 与 Python 包 kiln_webgpu（后者无 Python 依赖，启动 Node 进程驱动），Node 22.12+（来源：README Quickstart 与 From Python）
- 小体积：单个 320 KB（gzipped）ES module，可直接从 CDN 引入；大小为自述、未独立核实（来源：README Features）
- 基准：README 自述在 31 组任务/模型对上端到端快于 transformers.js，中位 2.2×，M3 上 SmolLM2-135M 解码约 307 tokens/s；均属自述、未独立核实（来源：README Benchmarks）

## 使用场景
- 在网页里做一个完全本地的聊天/翻译/OCR 小工具，数据不出浏览器
- 用自建静态服务器托管自有模型，绕开 Hub 与转换流程
- 在老笔记本上跑小模型，验证「闲置算力再利用」
- 通过 Python 包在无 PyTorch/CUDA 的环境里调用同一套 pipeline

## 适用人群
熟悉 ES module 与 WebGPU 的前端/AI 工程师，希望在浏览器或 Node/Python 中做本地多模态推理、且愿意跟进一个 6 star 早期库的人。

## 注意事项
- 6 star、1 fork，无 GitHub release（/releases/latest 为 404），最新为 0.3.0
- 所有速度与体积数字为 README 自述，未独立核实；README 自己也列出落后项（4bit 下载体积、MaskedLM logits）
- WebGPU 需较新 Chrome/Edge/Safari 26+/Firefox 141+；Node 需 22.12+
- 架构支持有限，新增模型族需有人实现 kernel
- 模型权重不随包分发；默认抠图模型 RMBG-1.4 仅限非商用
- 模型输出需与参考实现比对后再用于严肃场景

## 总结
Kiln 的定位很清楚：不是再做一层封装，而是自己写 WebGPU kernel 以直接吃原始权重，换来「零转换、零服务器、隐私本地」和 transformers.js 风格 API，并覆盖从文本到图像、音频的广泛任务。README 难得地把慢的地方也写出来，工程气质不错。但 6 star、无 release、基准为自述、架构覆盖有限，意味着它目前更适合评估和原型，而不是生产依赖。

## 独立评分（满分 10）

- **质量**：8 — 直接吃原始权重、transformers.js 风格 API 与多任务覆盖很扎实；但 6 star、无 release、性能为自述
- **学习价值**：8 — WebGPU kernel 自实现、GGUF/4bit 加载、结构化输出与下载缓存都是可学的浏览器推理实践
- **使用价值**：7 — 无需服务器与转换，一个 ES module 或 Python 包即可在本地跑多模态模型
- **推荐等级**：**B**
