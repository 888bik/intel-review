# ericwang915/agent-hive

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/ericwang915/agent-hive
- **分类**：ai-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：JavaScript
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：22 / 0
- **许可证**：MIT
- **创建时间**：2026-11-17
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：Agent Hive 1.6.0（tag `v1.6.0`，发布于 2026-11-21）
- **一句话简介**：把 Claude Code、Codex、Gemini 等 CLI 变成一群可排班、可接力评审的 macOS 代理团队

## 一句话理解
把多个编码代理 CLI 变成一群有名字、有分工的「蜜蜂」，能定时上班、接力写代码并互相评审，你在一个蜂巢窗口里看着它们干活。

## 通俗解释
Agent Hive 是一个 macOS 桌面应用（macOS 12+，Apple 与 Intel 均支持），把 Claude Code、Codex、Gemini CLI、Antigravity、DSH、Cursor Agent 乃至任意接受 prompt 的命令行包装成「蜜蜂」。每只蜜蜂有自己的文件夹、会话、聊天窗口与模型选择；你可以跟它说话、给它排日程、把它串成接力队，或拉进群里让「蜂后 Queen」分派任务。它不自带 API Key、无账号、无服务器，直接复用你已在终端登录的订阅。README 约 2.2 万字符，功能、对比、安全、FAQ、文件结构一应俱全，22 star，v1.6.0。核心卖点是「常驻团队」而非一次性并行：定时与事件触发、跨引擎评审回路、夜间记忆与技能提炼、token 自动压缩，都围绕长期无人值守运行设计。

## 它不是什么
1. 不是 Claude Code 的内置 subagents/agent teams：那些只用 Claude；这里混用多厂商、长期存活、各自有文件夹与排程。
2. 不是云端编排平台：无自有服务器、无账号、不上传代码；唯一自发请求是检查 GitHub 新版本。
3. 不是终端 UI：它要求 macOS 桌面（Electron 加原生 HTML/CSS/JS），不支持 Windows/Linux。
4. 不是看板或纯 git worktree 工具：虽有 worktree 并行与 Files 页，主视图是蜂巢/卡片与群聊。
5. 不是 API 网关：不存密钥，依赖各 CLI 自己的登录与计费。

## 痛点
- 同时开十个终端跑代理，状态分散、难观察、难回收
- 代理跑完就没了，无法按天/周定时重复执行或在新提交时自动触发
- 单一模型自己写自己审，反馈盲区大
- 长会话上下文膨胀导致 token 成本上升，却没有统一的压缩策略
- 换一个代理工具就要换一套界面和操作习惯

## 核心功能（依据 README，注明来源）
- 混合团队：Claude Code 写、Codex 审、Gemini 测，任意接受 prompt 的 CLI 都能当蜜蜂（来源：README Highlights 与 FAQ）
- 定时与事件触发：一次性、每日、工作日、周末、每周、每月、每 N 分钟；新提交、GitHub PR/评论/CI 失败、其他蜜蜂完成均可触发；支持睡眠补跑与每日预算（来源：README Highlights、Schedules 与 Event triggers）
- 蜂后调度：群里 @某只蜜蜂直接开工，不指定时由小模型挑选收件人，其余保持休眠；蜂后选蜂、派活、汇总（来源：README Let Queen lead a group）
- 接力与评审回路：最多六只蜜蜂共享文件夹，评审者把问题退回写作者，最多三轮；同一步的蜜蜂各自 git worktree 并行，最后一蜂合并（来源：README Chain bees into a relay team）
- 拖拽建队与蜂巢视图：把一只蜜蜂拖到另一只后面即建立接力，拖到同一只上并行，再拖到第二只上合并；蜂巢按链、评审环、并行分叉展示（来源：README See the whole hive at a glance）
- 逐轮可审计：每只蜜蜂独立聊天窗口、行内 diff、嵌套子代理、每轮成本；Changes 页可提交、丢弃、推送、开 PR（来源：README Read every step, then review the diff）
- Token 节省：会话半满或闲置一小时后自动压缩、定时运行新开会话、按蜜蜂勾选 MCP 服务；README 自述三轮任务中每轮上下文比裸 claude -p 少约 30%（29k 对 44k tokens），属自述、未独立核实（来源：README Highlights 与 Token savers）
- 记忆与技能：夜间 Dreamer 维护长期记忆、Gardener 把当天对话提炼成可复用技能；记忆分层为事实→观察→页面，每条显示确认次数，可纠正或删除（来源：README Memory and skills）
- 本地与安全：MIT、无构建步骤、无遥测，监听 127.0.0.1 随机端口且每次启动带 token；应用经 Apple 签名与公证（来源：README Highlights 与 Safety）

## 使用场景
- 每天清晨让蜂后带一组蜜蜂拉取变更、跑测试、生成日报
- 新 PR 或 CI 失败时自动触发一只蜜蜂定位问题，另一只评审，第三只测试
- 把写作者与评审者换成不同厂商模型，跨引擎互查
- 夜间让 Dreamer/Gardener 整理当天对话，第二天新会话直接带上记忆与技能

## 适用人群
已持有 Claude / ChatGPT 等订阅、在 macOS 上同时管理多个编码代理、希望把代理当作「常驻团队」排班与监督的开发者与技术负责人。

## 注意事项
- 22 star、0 fork，项目 2026-11-17 才创建，仍早期
- 仅 macOS 12+，Windows/Linux 不支持
- 需要 PATH 上至少有一个可用 CLI 与 git；gh 可选
- Allow all 会跳过所有确认，只应在可信文件夹使用
- 30% token 节省为 README 自述、未独立核实，且只测了三轮任务
- 使用 Use my Chrome 需 Claude in Chrome 扩展；屏幕与文件权限仍由系统把控

## 总结
Agent Hive 把多代理编排从「开一堆终端」推进到「管一支可排班、可评审、可积累记忆的团队」：蜂后分派、接力评审、事件触发、分层记忆与 token 压缩构成了完整闭环，而且不要求 API Key。代价是只支持 macOS、项目很新（22 star），关键收益数字为自述。对于已经重度使用多个编码 CLI 的 macOS 开发者，它是目前少见的、把长期无人值守做进产品形态的尝试。

## 独立评分（满分 10）

- **质量**：8 — 把多 CLI 代理编排成可排程、可评审、可记忆的团队，功能密度高；但仅 macOS、22 star 仍早期
- **学习价值**：8 — 蜂后调度、接力评审、分层记忆与 token 压缩都是可研究的多代理编排实践
- **使用价值**：8 — 复用已有 CLI 登录、无需 API Key，即可让多个代理定时、并行、带审查地干活
- **推荐等级**：**B**
