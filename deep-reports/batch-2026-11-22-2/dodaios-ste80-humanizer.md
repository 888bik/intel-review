# Dodaios/ste80-humanizer

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息

- **仓库地址**：https://github.com/Dodaios/ste80-humanizer
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Python
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：1,064 / 72
- **许可证**：MIT
- **创建时间**：2026-11-09
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-18）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：一个 agent skill（智能体技能），把 AI 写的文字两遍加工——先去掉 26 种“AI 腔”，再改写成约 80% 接近 ASD-STE100 受控英语的清晰文体。

## 一句话理解

这是一个“去 AI 味 + 说人话”的写作技能包：它不改你的原意，只把那些一眼假的句子翻新成谨慎、清楚、像人写的文字，可以直接装进 Claude Code、Codex、Cursor 等编码代理里用。

## 通俗解释

你让 AI 写段说明，它总爱来几句“Great question!”、“这不是 X，而是 Y”、“此外，值得注意的是……”之类的套话， sentences 又长又绕，读起来一股“机器味”。这个技能用两遍处理解决：第一遍（Pass 1）对照维基百科整理的 26 种 AI 写作痕迹，把套话和堆砌的从句清掉；第二遍（Pass 2）按 Andrej Karpathy 2026-10-02 的建议，把句子往航空维修手册用的 ASD-STE100 受控英语方向改到“80%”——一句话一个意思、主动语态、短句、简单时态，但不像 100% STE 那样僵硬（来源：README “Why two passes” 与 “The method: what Karpathy said”，其中引用了 Karpathy 的 X 帖子）。它做成一个 agent skill，可以根据编号报告改了哪些规则（如 §1、§8、S2），方便你回查。

## 它不是什么

1. **不是通用写作 App 或网站**。它是一个 skill（技能）包，装进编码代理或 Claude 里使用，README 给出 skills.sh、Claude Code 插件、Claude.ai/Desktop 三种安装方式（来源：README “Installation”）。
2. **不是改作文器或润色器**。它明确“without changing what it says”（不改变原意），目标是可读性和去 AI 痕迹，不是重写观点（来源：README 开头）。
3. **不是 100% ASD-STE100**。默认只到 80%，保留“may/should”、常用缩写等自然表达；还有 Strict 模式才是接近 100%（来源：README “The method” 对照表）。
4. **不是一次处理完**。README 强调很多 humanizer 只删短语、句子结构还是 AI 味，所以要“两遍”处理（来源：README “Why two passes”）。
5. **不是它自己发明的方法**。26 种 AI 痕迹来自维基百科与 blader/humanizer，STE80 来自 Karpathy 的帖子，本项目是把两者工程化为 skill（来源：README 开头与 Sources and credits）。

## 痛点

- **AI 文本一眼假**：套话、破折号、“not just X”、尾随分词、戏剧性收尾等 26 种痕迹让内容失去可信度。
- **去套话不等于清楚**：只删词不改句子结构，长句和被动语态仍在，读起来还是累（README 用对照表展示三阶段效果）。
- **人工重写成本高**：逐句按受控英语规范改写很费时；把规则固化成 skill 让代理自动做。
- **改动不透明**：很多改写工具不告诉你改了什么；本技能按规则编号报告，可追溯。

## 核心功能（依据 README，注明来源）

- **两遍处理**：Pass 1 用 26 种 AI 痕迹清单（§1–§26），Pass 2 用 15 条 STE80 结构规则（S1–S15），编号报告改动（来源：README “How to read the skill”）。
- **80% 与 Strict 两种强度**：80% 默认保留自然词表和缩写、允许少量超长；Strict 严格限制词表、句长、情态动词（来源：README 对照表）。
- **安装方式**：skills.sh（任意 agent）、Claude Code 插件、Claude.ai 与 Claude Desktop（来源：README “Installation” 小节）。
- **Checker 与测试**：README 有 Checker、Tests 小节，用于验证改写效果（来源：README headings）。
- **Limits 与 Disclaimer**：README 明确列出局限与免责声明（来源：README headings）。
- **来源透明**：README 引用 Karpathy 原帖、维基百科 Signs of AI writing、blader/humanizer，并按 ASD-STE100 Issue 9 校正了原帖配图的一处错误（来源：README “The method” 中 fact check 说明）。

## 使用场景

- 把 AI 生成的技术文档、README、邮件、说明文字改得更自然清楚后再发布。
- 装进编码代理，让代理写注释、提交信息、文档时自动遵循清晰写作规范。
- 为英文非母语读者写材料时，用受控英语降低理解门槛。
- 团队统一写作风格：把 26+15 条规则作为可执行的 checklist。

## 适用人群

用 AI 大量生成英文文字的人、技术写作者、关注可读性的人。中文写作场景不适用（STE100 是英文规范）。

## 注意事项

- MIT 许可，使用自由。
- README 有 Disclaimer 和 Limits，说明它并非万能，改写效果依赖原文。
- 项目 2026-11 才创建、无 release，仍处于早期；规则清单来自第三方（维基百科/Karpathy），不是本项目原创。
- 0 个 open issues，社区反馈尚少。

## 总结

ste80-humanizer 的聪明之处在于“组合式创新”：把维基百科的 AI 痕迹清单和 Karpathy 的 STE80 写法拼成一套可执行的两遍流程，并做成 agent skill 让代理直接用。它不产生新理论，但把一个清晰写作的经验法则工程化、可复用，这正是 skill 生态该有的样子。对英文写作和代理输出质量有要求的人，值得一装。

## 独立评分（满分 10）

- **质量**：8 — 方法来源清晰、两遍设计合理、有 checker 与测试；新项目、无 release，扣分。
- **学习价值**：8 — AI 写作痕迹 + 受控英语 + skill 工程化的组合很有教学意义。
- **使用价值**：8 — 英文写作/代理输出场景立竿见影；中文与口语场景用不上。
- **推荐等级**：**B**（实用且思路好，但受众与语言受限，暂不给 A）。
