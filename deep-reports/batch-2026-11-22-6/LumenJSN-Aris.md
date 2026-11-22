# LumenJSN/Aris

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/LumenJSN/Aris
- **分类**：ai-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Swift
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：25 / 1
- **许可证**：Apache-2.0
- **创建时间**：2026-11-12
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：Aris 1.3.1（tag `v1.3.1`，发布于 2026-11-22）
- **一句话简介**：住在 MacBook 刘海里的桌面 AI 代理，说话或打字即可操作你的 Mac，写操作前一律先确认

## 一句话理解
在 MacBook 刘海里放一个 AI 代理，按住键说话或打字，它就能读你的 Mac 并动手做事——但任何写入、发送、删除都要先给你一张确认卡。

## 通俗解释
Aris 是一个用 Swift 写的 macOS 原生应用（要求 Apple silicon、macOS 14+），把 AI 对话与执行入口塞进刘海区域。它本身不训练模型，而是「接线」到你已有的后端：Claude Code（推荐）、Codex、Antigravity、API Key（OpenAI/Anthropic/Gemini/OpenRouter/Groq/xAI/Mistral/DeepSeek）或本机 Ollama/LM Studio 模型。按住 ⌥ Space 说话，或点刘海打字；回答、确认卡与运行中的任务都显示在刘海，历史、用量、待办和日历放在侧刘海。README 约 1.8 万字符，安全模型、权限表、隐私边界、构建与测试说明都很完整，项目 25 star，已在 v1.3.1 提供自动更新。它强调「你的数据留在你的 Mac」：历史、记忆、密钥都在本地，helper 只监听 127.0.0.1 且带 token；外部文本（网页、邮件、消息、文件、skill）被视为污染数据，读过之后该轮不能再自主行动。

## 它不是什么
1. 不是聊天网页或云助手，它是本机 App，没有账号、没有自有服务器、没有分析。
2. 不是只做问答的对话框，它能操作消息、邮件、备忘录、日历、提醒、通讯录、Finder 文件与更多应用。
3. 不是 Anthropic/OpenAI/Google 官方产品，README 明确声明无关联，也不包含其他应用的代码与素材。
4. 不是 Mac App Store 应用：依赖运行时查找的私有 Apple API，且自签名、未公证。
5. 不是完全无人值守的自动代理：写、发、删、花钱、跨应用自动化都要确认卡，付款、抹盘、触碰系统保护区被明确禁止。

## 痛点
- 想在任意应用里用语音操作 AI，但不想在浏览器与终端之间来回切换
- AI 能读文件却缺乏统一的「先看清楚再执行」确认机制
- 多个 CLI 后端（Claude Code、Codex、本地模型）各有入口，难以统一调用
- 语音输入后想按自己的写作风格润色，通用听写做不到

## 核心功能（依据 README，注明来源）
- 刘海内对话与确认卡：按住 ⌥ Space 说话或点刘海打字，回答、确认卡与运行任务都在刘海显示，历史/用量/待办/日历在侧刘海（来源：README Features）
- 多后端可插拔：Claude Code、Codex、Antigravity、API Key 或本地 Ollama/LM Studio，所有功能在所有后端一致可用（来源：README Brains）
- 操作你的 Mac：消息、邮件、备忘录、日历、提醒、通讯录、Finder 文件（转换/合并/压缩/移动/撤销）、音乐、计时器、天气、股票、地图与汇率；其他应用由自带 computer-use 引擎在后台操作（来源：README Features）
- 看图指物：询问屏幕内容、用手势圈选或选择窗口作为提问对象（来源：README Features）
- 听写与免提对话：向任意应用听写、学习词典与写作风格；端上结束检测与耳机打断（来源：README Features）
- 后台队友与技能：Teammates 是后台 worker，可接任务回报；Skills 使用开放 Agent Skills 格式（SKILL.md 加可选 scripts/references/assets），启用前经 SkillSpector 静态扫描和 AI 审阅（来源：README Features）
- 本地记忆：关于你和任务执行情况的记忆留在 Mac 上，可在设置中查看、确认或删除（来源：README Features）
- 安全门与污染文本：读取无需询问，写入/发送/删除/消费/跨应用自动化均需确认卡；外部文本被加围栏与标签，读过它的轮次不能再自主行动（来源：README Safety model）
- 韩英双语：界面、语音与回答均支持韩语和英语（来源：README Features）

## 使用场景
- 在终端修 flaky test 时让 Claude Code 通过刘海接收指令并回传结果
- 出门前让 Aris 查航班与住宿、读攻略、整理成行程，发送前由你确认
- 对着任意应用按住右 ⌥ 听写长段文字，并按个人风格润色
- 用本地模型处理敏感内容，请求不出本机

## 适用人群
使用 Apple silicon Mac（macOS 14+）、已订阅或已安装 Claude Code / Codex 等 CLI、希望用一个统一且带确认门的桌面入口来驱动 AI 操作电脑的高级用户；能接受自签名应用与私有 API 带来的兼容性风险。

## 注意事项
- 25 star、1 fork，个人项目，README 自述「use at your own risk」，无担保
- 自签名、未公证、无 Team ID，首次启动需手动 Open Anyway 或清除 quarantine 标记
- 刘海玻璃质感需 macOS 26，旧系统为纯深色刘海；听写在 14/15 依赖 Apple 旧语音引擎与系统听写开关
- 使用私有 Apple API，系统更新可能使其失效，也因此无法上架 App Store
- 麦克风、辅助功能、屏幕录制、完全磁盘访问等权限需逐项授权
- 依赖 Peekaboo、SkillSpector、Whisper 等首次使用下载；更新检查会访问 GitHub
- 从源码构建的应用不自更新，权限需重新授予

## 总结
Aris 的看点不在模型能力，而在它把「AI 可以读你的 Mac，但不能擅自写」做成了一条可审计的流程：确认卡、污染文本隔离、可撤回的记忆与技能扫描，都在 README 里讲得很清楚。它是 25 star 的个人项目，功能覆盖面广、文档质量高，但私有 API、自签名未公证与 Apple 权限体系决定了它更适合愿意折腾的 macOS 高级用户，而非开箱即用的大众工具。

## 独立评分（满分 10）

- **质量**：8 — 把多个 AI 后端与 macOS 系统操作收进刘海交互，安全门设计细致；但私有 API、自签名与未公证限制大
- **学习价值**：7 — 可读到一套完整的桌面代理架构：确认卡、污染文本隔离、Agent Skills 与本地记忆
- **使用价值**：8 — 一个入口即可语音或文字驱动 Claude/Codex/本地模型操作消息、邮件、文件与日历
- **推荐等级**：**B**
