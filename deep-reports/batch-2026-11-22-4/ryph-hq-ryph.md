# ryph-hq/ryph

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/ryph-hq/ryph
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：TypeScript
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：17 / 1
- **许可证**：MIT
- **创建时间**：2026-11-04
- **最近更新**：2026-11-09（元数据 updated_at；最近提交 2026-11-09）
- **最新 release**：v0.1.4（tag `v0.1.4`，发布于 2026-11-09）
- **一句话简介**：自托管的团队编码代理平台：把“告警触发、聊天里 @ 一下、PR 上留 /ryph、定时任务”都变成在安全沙箱里跑的代理，最终以 PR 形式交付，全程留痕。

## 一句话理解

它像给团队养了一群“值班机器人”：出故障时自动被告警叫醒，读遥测、查代码、提热修复 PR；平时还能在 Slack/Telegram 里被 @ 出来回答问题，所有动作都跑在你自己的基础设施里并留痕。

## 通俗解释

让 AI 代理在团队里干活，难点不在“会不会写代码”，而在“怎么安全地接进真实系统”：给它什么权限、怎么隔离、怎么防止它乱推代码、怎么知道它做了什么。ryph 把这些基础设施做成一套自托管平台（来源：README 开头）。典型流程：告警触发 → ryph 在你的 infra 里起一个带预热缓存和预装依赖的安全沙箱 → 代理通过 Tool Proxy 读可观测性数据、结合语义记忆诊断 → 加入事故聊天群汇报、被人 @ 追问 → 构建修复、跑 repo 的检查、提交一个 CI 全绿的热修复 PR → 工程师合并部署 → 代理继续盯着遥测直到告警恢复（来源：README “Deploy an incident response bot”）。它还能从 UI 任务、PR/issue 上的 /ryph 评论、聊天 @、cron 定时触发；有 skills/MCP、按 agent 的策略、浏览器预览、永久运行记录等（来源：README “What else it does”）。架构是 workspace 多包（orchestrator、runner、model-proxy、mcp-gateway、forge、chat、historian 等，来源：README “How it works” 表格）。

## 它不是什么

1. **不是单个 AI 编码工具**。它是平台，由二十多个包组成，含控制面、数据面、沙箱、UI、各类集成（来源：README “How it works”）。
2. **不是 SaaS**。它自托管，沙箱跑在你自己的 infra，模型密钥不进容器（来源：README 与 model-proxy 包说明）。
3. **不是只做事故响应**。还能做团队助手 bot、UI 任务、PR 评论触发、定时、预览、skills/MCP（来源：README）。
4. **不是让代理绕过审查直接合并**。README 明确 forge safeguards 防止代理错误推送或绕过 code review（来源：README）。
5. **不是成熟大项目**。17 star、v0.1.4、2026-11 创建，很早期（来源：GitHub）。

## 痛点

- **代理接入真实系统的安全边界**：沙箱隔离、工具代理、密钥隔离需要成套方案。
- **事故响应耗时长**：从告警到定位到热修复，人工切换多系统；代理自动跑通。
- **代理行为不可审计**：historian 包提供每次运行的永久记录（来源：README 包表）。
- **触发方式分散**：告警/聊天/PR/定时统一成同一套 recipe（来源：README）。

## 核心功能（依据 README，注明来源）

- **事故响应 bot**：起沙箱、读遥测、语义记忆、聊天协作、提热修复 PR、盯部署（来源：README “Deploy an incident response bot”）。
- **团队助手 bot**：在 Slack/Discord/Telegram/Zulip 里 @，可读代码/指定数据库表、生成 Artifact、用沙箱浏览器验证前端修复（来源：README）。
- **多触发方式**：UI 任务、/ryph 评论、聊天 @、cron、webhook（来源：README “What else it does”）。
- **Checks / Skills / MCP**：.ryph/checks.yml 定义的检查、按 agent 的策略、ryph mcp 把部署暴露成 MCP 工具（来源：README）。
- **多包架构**：orchestrator（控制面）、runner、model-proxy（密钥不进容器）、mcp-gateway、forge（GitHub/GitLab/Radicle）、chat、historian、memory、browser、web、previews 等（来源：README 包表）。
- **部署**：Compose 与 Helm chart（来源：README 结尾）。

## 使用场景

- SRE/DevOps 团队想要自动化的事故响应代理。
- 团队想在聊天里安全地用编码代理读代码/查数据/提修复。
- 需要代理行为可审计、密钥隔离的自托管场景。

## 适用人群

平台/SRE 工程师、重视自托管与安全的团队。个人或小项目用不上。

## 注意事项

- MIT 许可。
- 17 star、1 issue、v0.1.4、2026-11 创建，极早期；1.6 万字符 README 多为文档链接，实际成熟度需验证。
- 平台复杂（多包 + Docker/K8s/Helm），部署运维门槛高。
- 文档中大量能力为项目自述，未独立核实。

## 总结

ryph 把“团队里安全地使用编码代理”做成完整平台：沙箱隔离、密钥隔离、多触发、可审计、以 PR 交付，思路成熟且切中企业痛点。17 star 的极早期项目，复杂度高、能力待验证。对 SRE/平台团队值得重点跟踪；个人用户了解思路即可。

## 独立评分（满分 10）

- **质量**：7 — 思路成熟、平台化完整、安全边界考虑周全；极早期、复杂度高、能力待验证。
- **学习价值**：7 — 自托管代理平台的沙箱/密钥隔离/多触发/可审计设计有很高参考价值。
- **使用价值**：7 — SRE/平台团队价值高，个人与小项目用不上。
- **推荐等级**：**B**
