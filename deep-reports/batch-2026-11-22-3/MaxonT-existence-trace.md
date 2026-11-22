# MaxonT/existence-trace

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/MaxonT/existence-trace
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Python
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：23 / 1
- **许可证**：MIT
- **创建时间**：2026-10-07
- **最近更新**：2026-11-16（元数据 updated_at；最近提交 2026-11-19）
- **最新 release**：existence-trace 0.4.6（tag `v0.4.6`，发布于 2026-11-12）
- **一句话简介**：一个“自我改进的个人状态系统”：AI 通过 PAD 情绪、希望/安全感（G）与成长（XP）视角阅读你的日记，向你反馈并沉淀成个人规则，仅用 Python 标准库。

## 一句话理解

它把日记 + AI 变成一个“认识自己”的循环：你写每天的记录，AI 从情绪、安全感、成长几个角度读，问你反馈，再把你的偏好提炼成规则，帮你看见几个月后的自己。

## 通俗解释

很多人写日记但很少回看，更难以从中发现自己的情绪模式与成长轨迹。existence-trace 提供一个本地、纯 Python 标准库的系统：AI 读取你的日记，通过 PAD 情绪模型（Pleasure-Arousal-Dominance）、security/hope（G）和 growth（XP）等视角分析，向你提问、收集反馈，并把反馈转化为规则（来源：仓库描述与 README）。README 开篇发人深省：“Six months from now, what will you remember about today?”（来源：README）。它内置 chat memory 对比、导入日记、连接 AI、命令集、自我改进偏好循环、9 条原则、隐私说明等章节（来源：README headings）。它明确说“Not a clinical tool”（不是临床工具）（来源：README headings），强调隐私（来源：README “Privacy”）。23 star、v0.4.6，极早期的个人项目。

## 它不是什么

1. **不是心理治疗或临床工具**。README 明确 “Not a clinical tool”（来源：README）。
2. **不是通用日记 App**。它是“日记 + AI 分析 + 偏好学习”的个性化系统（来源：仓库描述）。
3. **不是需要大量依赖**。纯 Python 标准库（来源：仓库描述）。
4. **不是云端上传**。强调隐私，本地运行（来源：README Privacy）。
5. **不是成熟产品**。23 star、v0.4.6、2026-10 创建（来源：GitHub）。

## 痛点

- **日记写了不回看**：缺乏工具把记录转化为洞察。
- **情绪模式不可见**：PAD 等模型提供结构化视角。
- **偏好难沉淀**：通过反馈循环把“你喜欢什么/讨厌什么”变成规则。
- **隐私顾虑**：个人日记敏感，项目强调本地与隐私。

## 核心功能（依据 README，注明来源）

- **PAD + G + XP 视角分析**：AI 按情绪、安全感/希望、成长角度读日记（来源：仓库描述）。
- **反馈循环与规则沉淀**：AI 提问、收集你的反馈、转化为规则；“self-improving preference loop”（来源：README headings）。
- **日记导入与 AI 连接**：Bring in your diary、Connect an AI（来源：README headings）。
- **内置 chat memory 对比**：Built-in chat memory vs. existence-trace（来源：README headings）。
- **命令集与 9 原则**：Commands、The 9 principles（来源：README headings）。
- **隐私与免责声明**：Privacy、Not a clinical tool（来源：README headings）。

## 使用场景

- 想用 AI 辅助自我反思、发现情绪与成长模式的人。
- Obsidian/日记用户想把笔记接入 AI 分析（topics 含 obsidian、daily-notes）。
- 量化自我（quantified-self）爱好者。

## 适用人群

喜欢日记/自我追踪、且愿意折腾本地 AI 工具的开发者。需要专业心理帮助者不适用（项目也声明非临床）。

## 注意事项

- MIT 许可。
- 23 star、1 fork、3 issue，极早期个人项目。
- “Not a clinical tool”——不能替代专业心理健康服务。
- 情绪分析基于模型，结果主观，需理性看待；隐私取决于本地配置与 AI 连接方式。

## 总结

existence-trace 是一个很有个人色彩的项目：把日记、情绪模型与偏好学习组合成“认识自己”的循环，纯标准库、强调隐私、明确非临床。23 star 的极早期作品，理念大于工程成熟度。对自我追踪爱好者值得一试，但别把它当心理工具。

## 独立评分（满分 10）

- **质量**：7 — 理念清晰、纯标准库、强调隐私；极早期、个人项目。
- **学习价值**：7 — 日记 + 情绪模型 + 偏好循环的思路有启发。
- **使用价值**：6 — 自我追踪爱好者有价值，非临床定位需牢记。
- **推荐等级**：**B**
