# Parcha-ai/agentrun

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息

- **仓库地址**：https://github.com/Parcha-ai/agentrun
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：JavaScript
- **评分**：原始评分未核实；本报告独立评分见文末
- **Star / Fork**：1,250 / 102
- **许可证**：Apache-2.0
- **创建时间**：2026-09-23
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：AgentRun observation CLI 0.1.10（tag `cli-v0.1.10`，发布于 2026-11-06）
- **一句话简介**：开源 Agent 运行平台，在 Claude Code、Codex、OpenCode、pi 等 agent harness 之上增加“自我改进、持久化执行与统一控制台”的能力。

## 一句话理解

AgentRun 给 AI 代理装了“记忆 + 续航 + 仪表盘”：代理跑完能沉淀经验下次更聪明，任务中断能续跑，所有代理的费用、审批和状态在一个控制台里管。

## 通俗解释

现在的编码代理（Claude Code、Codex 等）有个通病：每次任务都从头开始——重新查同样的 API、重读同样的文档、重建同样的计划，你为重复的 token 买单；而且任务跑一半挂了就丢了。AgentRun 在代理外面包了一层平台：它把每次运行存在统一存储里（进程重启、runner 宕机都能续），跑完后把“经验”写成 corrections.md 或编译成可复用 workflow，下次运行从这些经验出发（来源：README “Why AgentRun”、“Agents that learn the work”、“Runs that survive”）。它还提供审批、花费上限、统一控制台等“控制面”能力。支持本地、单容器或带 runner 的服务器部署，也有托管云（AgentRun Cloud）。口号是“Run smarter agents. Use fewer tokens.”（来源：README）。

## 它不是什么

1. **不是一个新的 AI 模型或 LLM**。它不训练、不提供模型；它编排已有 harness 上的代理（README 明确支持 Claude Code、Codex、OpenCode、pi）。
2. **不是替代 Claude Code 的编码工具**。它是“在代理周围做 plumbing”的平台，你仍在自己喜欢的 harness 里干活（来源：README “Why AgentRun”）。
3. **不是只有云版本**。可自托管（laptop/container/server），也有 AgentRun Cloud 托管选项（来源：README “Why AgentRun” 与 “Deploy it”）。
4. **不是“省钱承诺已独立验证”**。README 案例称 code review 成本从涨 36 倍降到每次 18 美分、学习后 run 从 10 秒降到 2 秒、成本从 $0.05 到 $0.0077（来源：README “Case studies” 与图片说明）——这些是**项目自述，未独立核实**。
5. **不是“代理完全自主无需管”**。它有审批、花费上限、问题询问机制，强调 “You stay in control”（来源：README）。

## 痛点

- **代理重复劳动、token 浪费**：同一类任务反复探索；AgentRun 用学习机制沉淀 corrections/workflow（来源：README）。
- **运行不可恢复**：长任务中断即丢失；统一存储 + runner 租约（30 秒）让任务可续跑，已完成步骤不重跑（来源：README “Runs that survive”）。
- **多代理管理混乱**：费用、审批、失败、运行状态分散；一个控制台集中管理（来源：README “You stay in control”）。
- **重复工作本可用 workflow 而非 agent**：AgentRun 提供类型化 workflow DSL（工具调用、模型步骤、快速类型决策），让确定性工作不走 LLM 循环（来源：README “Workflows”）。
- **Provider 集成、恢复、追踪的样板代码**：平台把这些 plumbing 做好，用户专注业务（来源：README “Why AgentRun”）。

## 核心功能（依据 README，注明来源）

- **代理学习**：开启 `"learning": true` 后，把用户纠正写入各代理自己的 git 文件夹的 `corrections.md`，每次运行整体注入 `# Learned`；累计 20 次完成后可把工作编译成 workflow（默认 `compile_after: 20`），经代码检查、dry-run、回放验证后才采用（来源：README “Agents that learn the work”）。有大小上限（corrections 2048 字节、workflow.json 16384、README 1024）并拒绝含 secret 的内容（来源：README details）。
- **Workflows**：类型化 JSON DSL（schema 定义输入输出），步骤可调用工具、跑模型、做快速类型决策、分支、并行 map、有限循环、调用子 workflow 或回退给代理；有 TypeScript builder 与 `npx @parcha/agentrun workflows validate` 校验（来源：README “Workflows”）。
- **持久化运行**：所有 agent/revision/run/step 存在统一 store；服务中断可续，多 runner 时 30 秒租约 failover，已完成步骤不重跑，中断的写入标记为 interrupted 而非重试（来源：README “Runs that survive”）。
- **控制面**：每个工具可 allow/ask/deny；花费上限到达时暂停而非失败，无法定价的费用标 “unknown” 而非 $0；代理可停下来问人；一个控制台含实时 Timeline、文件、浏览器屏幕接管（来源：README “You stay in control”）。
- **多 harness / 多计算机**：支持 Claude Code、Codex、OpenCode、pi；可为任务分配 sandbox、Linux desktop、Android phone 或浏览器（来源：README “Why AgentRun”、“Harnesses and computers”）。
- **模板、工具连接、CLI/代码接口**：有 Templates、Connect your tools、From the terminal and from code 等章节（来源：README headings）。

## 使用场景

- 大量使用编码代理、苦于 token 成本和重复探索的团队。
- 需要长任务可靠运行、断点续跑的自动化流程（如批量代码审查、批量证书处理）。
- 想让代理“越用越聪明”、把人工纠正沉淀为知识的场景。
- 需要统一审计多代理费用、审批与运行状态的管理员。

## 适用人群

AI 代理重度用户、Agent 平台工程师、需要生产级 agent 编排的团队。偶尔用代理的人暂时不需要。

## 注意事项

- **项目较新**（2026-09 创建），release 是 observation CLI 0.1.10，平台本身版本成熟度需评估。
- 成本/提速案例均为项目自述，未独立核实，不要直接当承诺。
- 学习机制的回放验证与 secret 过滤是亮点，但仍需评估沉淀内容的质量与偏差风险。
- 72 个 open issues，迭代活跃中；Apache-2.0 许可，商业使用友好。

## 总结

AgentRun 抓住了一个真实且高价值的痛点：代理“每次都从零开始”的浪费。它的三板斧——学习沉淀、持久化运行、统一控制面——设计扎实，workflow DSL 与回放验证尤其体现工程深度。作为 2026 年新项目，它还在早期，成本收益数据需自行验证，但方向是 agent 基础设施里最值得跟踪的一条线。

## 独立评分（满分 10）

- **质量**：8 — 架构完整、设计理念先进（学习/回放验证/failover），但新项目、release 仍在 CLI 阶段。
- **学习价值**：9 — 代理学习、workflow DSL、持久化执行都是 agent 工程的优质研究对象。
- **使用价值**：8 — 代理重度用户价值很高；效果数据自述需核实，普通用户用不到。
- **推荐等级**：**B**（方向和实现都优秀，但成熟度与数据验证不足，暂不给 A）。
