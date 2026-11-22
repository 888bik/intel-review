# andrewboldi/tmux-restore

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/andrewboldi/tmux-restore
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Shell
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：83 / 6
- **许可证**：MIT
- **创建时间**：2026-10-31
- **最近更新**：2026-11-15（元数据 updated_at；最近提交 2026-11-16）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：一个 tmux 插件，让电脑重启后不仅恢复会话/窗口/分屏/滚动记录，连 Claude Code、Codex、Gemini CLI、opencode、pi 等编码代理的对话也能原地续上。

## 一句话理解

这是给 tmux 加的“断电记忆”：重启机器后，终端布局回来，正在跑的编辑器/日志/服务重新出现，AI 代理的会话也接着上次的位置继续，而不是丢给你一个空壳。

## 通俗解释

tmux 用户常装 tmux-resurrect（保存会话）和 tmux-continuum（定时自动保存）来防重启丢工作。但有个痛点没解决：AI 编码代理的会话断了就是断了，重开只得到一个全新对话；普通程序恢复后也可能只剩空 shell。tmux-restore 在这两个插件之上补了几件事：每个窗格重新打开时，用一个小适配器找到代理会话文件里的对话 id，生成“续接该对话”的命令；保存频率从 continuum 默认的 15 分钟提到 1 分钟，并在机器关机时再存一次（崩溃最多丢最后一分钟）；编辑器、监控、日志跟踪（tail -f、journalctl -f、kubectl logs -f）以及你在 @resurrect-processes 里列的程序会自动重跑（来源：README “What it adds to tmux-resurrect”）。它用 systemd（Linux）/launchd（macOS）后台服务做关机保存，用一条安装脚本装齐依赖。

## 它不是什么

1. **不是 tmux-resurrect/continuum 的替代品**，而是跑在它们之上的增强层，官方明确“runs on top of”（来源：README）。
2. **不是只保存布局**。它额外恢复代理会话、常驻进程与 Python venv（来源：README）。
3. **不是所有代理都开箱即用**。适配器是“one short file”，新代理需提供或自己写放至 ~/.config/tmux-restore/agents/（来源：README “How it works”）。
4. **不是默认保存所有滚动内容到任何地方**。滚动记录存到本机仅本人可读的目录；若不想保存可用 @resurrect-capture-pane-contents off 关闭——README 提醒密码也可能被存进去（来源：README）。
5. **不是无依赖脚本**。需要 tmux、TPM 与相关插件，安装脚本会补齐；不是纯配置即可用（来源：README Install）。

## 痛点

- **重启丢代理对话**：AI 代理做到一半重启就没了，续接成本高。
- **保存粒度太粗**：15 分钟一次，崩溃可能丢一大段工作。
- **恢复后是空壳**：tmux-resurrect 恢复窗格却不恢复里面跑的程序（README 说 zsh 下其它程序停在提示符等回车）。
- **虚拟环境丢失**：Python venv 不会自动回来，需额外配置。

## 核心功能（依据 README，注明来源）

- **代理会话续接**：每个窗格重开自己的对话并带相同 flags，支持 Claude Code、Codex、Gemini CLI、opencode、pi（来源：README 开头与 “What it adds”）。
- **高频 + 关机保存**：每分钟一次（continuum 默认 15 分钟），关机时再存一次（来源：README）。
- **进程恢复**：编辑器、监控、日志跟踪及 @resurrect-processes 列表中的程序重跑；zsh 下其它程序停在提示符（来源：README）。
- **Python venv 恢复**：在 ~/.zshrc 或 ~/.bashrc 加一行即可（来源：README）。
- **实现机制**：适配器读代理会话文件找到对话 id 并保存恢复命令；后台服务（systemd/launchd）负责关机保存；安装脚本 curl 一行，可手动安装（来源：README “How it works”、“Install”）。
- **并发恢复修复**：安装器把 tmux-resurrect 切到一个 fork，保证同一时刻只有一个恢复在跑（来源：README）。

## 使用场景

- 用 tmux + AI 编码代理、经常重启机器或遭遇崩溃的开发者。
- 需要长期运行的编辑器/日志/监控在重启后自动回来的场景。
- 多代理并行工作、每个窗格一个会话的重度用户。

## 适用人群

tmux 用户、编码代理重度使用者、重视“工作不丢”的开发者。不用 tmux 的人用不上。

## 注意事项

- MIT 许可。
- 滚动记录含敏感信息（如密码）会被存盘，介意者请关闭 @resurrect-capture-pane-contents（来源：README）。
- 83 star、2 issue，项目很新（2026-10 创建）、无 release，仍早期。
- 依赖 tmux/tpm/插件与系统后台服务；支持平台以 README 所述 Linux/macOS 为准。

## 总结

tmux-restore 抓住了一个非常具体的痛点：重启后 AI 代理会话“断片”。它不重造 tmux 生态，而是在 resurrect/continuum 之上补齐会话续接、高频保存与进程恢复，设计克制且实用。83 star 说明它还小众，但对“机器一重启就得重开一堆代理会话”的人来说，这是能显著省事的工具。

## 独立评分（满分 10）

- **质量**：8 — 思路克制、实用，恢复代理会话的适配器设计清晰；新项目、无 release、仅支持少数代理。
- **学习价值**：8 — 代理会话续接 + 关机保存 + 进程恢复的组合有参考价值。
- **使用价值**：8 — 对 tmux + 编码代理用户省事明显，但受众有限、含敏感滚动记录风险。
- **推荐等级**：**B**
