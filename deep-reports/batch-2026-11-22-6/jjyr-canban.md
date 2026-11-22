# jjyr/canban

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/jjyr/canban
- **分类**：ai-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Rust
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：40 / 1
- **许可证**：Apache-2.0
- **创建时间**：2026-11-05
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：20261122-1（tag `20261122-1`，发布于 2026-11-21）
- **一句话简介**：基于 Rust 与 libghostty 的看板工作台，用持久终端、每卡 worktree 和本地工作流统一管理十个以上编码代理

## 一句话理解
Canban 是一个桌面看板应用，把 Claude Code、Codex、Cursor CLI 等编码代理跑在持久终端里，用一张卡片代表一个任务，配上独立的 Git worktree 和内联 diff 审阅，再用全局注意力收件箱告诉你哪个代理正在等你批准或审查。

## 通俗解释
同时开十个编码代理，最常见的失控方式是终端标签页遍地、某个代理卡在确认提示上几小时没人看见。Canban 的答案是把终端嵌进看板：每张卡片对应一个可重启后仍存活的 PTY，跑的是你原本的真实 CLI，不是被简化过的聊天封装；libghostty-vt 负责终端仿真，Rust 与 NSH 负责原生界面。当你需要干预时，按 ⌘I 打开跨项目、跨 worktree、跨远程主机的注意力收件箱，一键跳到对应卡片和实时终端。审阅不必切到外部编辑器，卡片自带隔离 worktree，可在 diff 的具体行上留言并回传给代理。更进一步，还能用类 GitHub Actions 的 YAML 把 plan、develop、merge 串成工作流，配 cron 和串行合并队列。README 还提到可经 SSH 或 Tailscale 连到无头 Linux 主机，把重负载丢到远程机器，并提供 120Hz 远程终端体验——该指标为自述、未独立核实。项目 40 星、有发布与频繁提交，但只支持 macOS 13+（Apple Silicon）与 Linux 桌面。

## 它不是什么
1. 它不是代理本身：跑的仍是你安装的 claude、codex、agy、cursor-agent 等原生 CLI。
2. 它不是网页看板：是 Rust 原生桌面应用，依赖 GTK4 与 GStreamer 等系统库。
3. 它不是云端托管服务：工作流在本地运行，远程能力靠 SSH/Tailscale 连你自己的机器。
4. 它不是完整的 Git 托管平台：分支与合并仍由你掌控，它提供的是隔离 worktree 和审阅界面。
5. 它不是跨平台通用工具：Windows 不在支持列表内，macOS 仅限 Apple Silicon。

## 痛点
- 多个代理并行时，卡在权限确认或等待输入的那个很难被及时发现。
- tmux 窗格一多就找不到目标，切换上下文成本高。
- 代理改完代码后，审查需要跳出终端开编辑器，行级反馈再手工粘回去。
- 重复的 plan、开发、合并流程缺少自动化，合并冲突与顺序难以管理。
- 本地机器算力有限，希望把重活交给远程 Linux 主机但保持终端体验。

## 核心功能（依据 README，注明来源）
- 注意力收件箱：跨项目、worktree 与远程主机显示哪些代理在等待输入、工具批准或审查，一键跳转（来源：README Why Canban?、Attention Inbox）
- 原生代理终端：在可重启恢复的持久 PTY 中运行真实 CLI，保留原生 TUI、交互权限、MCP 与会话续接（来源：README Native Agent CLI & Persistent Terminals）
- 每卡片 worktree 与内联 diff 审阅：在改动行上直接留言并向代理发起定向修订请求（来源：README Per-Card Worktrees & Inline Diff Review）
- 可编程工作流：类 GitHub Actions 的 YAML 组合 shell 与 agent 步骤，支持 plan 到 develop 到 merge、cron 与串行合并队列（来源：README Programmable Workflows）
- 多机协同：本地桌面经 SSH 或 Tailscale 连接无头 canband 主机，控制远程看板、工作流与实时终端（来源：README Multi-Machine over SSH & Tailscale）
- 安装与依赖说明：单行安装脚本，列明 Linux 的 GTK4/GStreamer 运行时依赖与各发行版从源码构建所需包（来源：README Installation）
- 开放组件构成：基于 Herdr、Sheprd、NSH、Ghostty 的 libghostty-vt 等开源项目并给出致谢（来源：README Related Software and Acknowledgements）

## 使用场景
- 同时管理十个以上编码代理任务，用看板和收件箱替代一堆终端标签。
- 对每个代理产出做隔离 worktree 的行级审查，再把意见直接回传让它修订。
- 把重复性的修复、测试、合并流程写成 YAML，实现无人值守的 plan 到 merge 流水线。
- 在本地 macOS 上操作远程 Linux 构建机，把重编译和代理运行卸载到远程。
- 团队内共享一套代理 CLI 配置与工作流模板，统一多项目的自动化方式。

## 适用人群
适合重度使用 Claude Code、Codex 等编码代理、同时推进多个任务的开发者和技术负责人。需要 macOS 13+ Apple Silicon 或 Linux 桌面，能接受通过脚本安装并处理 GTK4/GStreamer 系统依赖；只用单个代理或不愿管理原生依赖的用户收益有限。

## 注意事项
- 平台限定明确：macOS 13+ 仅 Apple Silicon，Linux 桌面需 GTK4 与 GStreamer；Windows 不支持。
- 120Hz 远程终端等体验描述为自述、未独立核实；项目仍处早期，40 星、1 fork。
- 工作流与合并队列是自动化工具，合并前仍应人工审查，避免批量合并未经检查的代理输出。
- 远程主机通过 SSH/Tailscale 连接，需自行保障网络与凭据安全。
- 许可证为 Apache-2.0，基于多个开源组件，分发时应保留第三方声明。

## 总结
Canban 抓住了多代理并行开发的真实痛点，并且给出的不是概念演示而是一套完整工程：持久 PTY 保住原生 CLI 体验，每卡 worktree 隔离状态，注意力收件箱解决谁在等我的问题，类 Actions 工作流把重复流程自动化，SSH/Tailscale 再把算力延伸到远程。README 结构清晰、安装路径明确，40 星与活跃提交说明有一定早期共识。它的边界同样清楚——平台窄、依赖重、生态新，远程终端流畅度等说法尚未核实。对已经深度使用编码代理的团队，它值得认真试用；对轻度用户，现有终端加 tmux 可能已经够用。

## 独立评分（满分 10）

- **质量**：8 — Rust 原生多代理工作台功能完整、迭代活跃，但平台限定且生态较新
- **学习价值**：7 — 持久 PTY、每卡片 worktree、类 Actions 工作流与远程 SSH 的组合有架构参考价值
- **使用价值**：8 — 同时跑十个以上编码代理并集中审阅时非常实用，但系统门槛与资源占用需考虑
- **推荐等级**：**B**
