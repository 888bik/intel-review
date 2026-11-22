# dooly6276/claude-bulkhead

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/dooly6276/claude-bulkhead
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：JavaScript
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：35 / 6
- **许可证**：MIT
- **创建时间**：2026-10-13
- **最近更新**：2026-11-15（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：v0.5.2（tag `v0.5.2`，发布于 2026-11-20）
- **一句话简介**：一组 Claude Code（及其它编码代理）的 hooks 安全护栏：知道哪个会话拥有哪个进程、端口和 git 树，防止代理误删、误杀服务、抢占端口或 git add -A 到别的会话改动。

## 一句话理解

它像给 AI 代理套的“安全围栏”：多个代理并行干活时，防止一个代理 rm -rf、pkill 掉别人的服务、占用别人的端口，或把别人会话的改动一起 git add -A。

## 通俗解释

同时开多个编码代理时，它们共享机器资源，容易互相伤害：某个代理执行 rm -rf 时变量为空导致删错目录、pkill 杀掉别的开发服务器、启动服务占了别人端口、git add -A 把另一个会话正在改的文件也提交了。claude-bulkhead 是一组 hooks（钩子），核心是“知道哪个会话拥有哪个进程、端口和 git tree”（来源：仓库描述），从而在危险操作前拦截。README 很长（约 5.3 万字符），包含 Install、What it stops、Why this exists、What it does、Worktrees in one step、Run heavy jobs on another machine、How it works、Guards、Codex/Gemini 和其它代理、Configuration、When it blocks something you meant（误拦截作弊表）等章节（来源：README headings）。它强调零依赖（Zero dependencies），并支持 Claude Code、Codex、Cursor、Gemini CLI、opencode、Qwen Code、pi 等多种代理（来源：仓库描述与 topics）。

## 它不是什么

1. **不是独立安全软件**。它是 hooks 集合，装进编码代理工作流生效（来源：README Install）。
2. **不是替代代码审查或权限控制**。它是操作层的护栏，不是完整安全方案。
3. **不是只支持 Claude Code**。README 有 “Codex, Gemini and other agents” 章节（来源：README headings）。
4. **不是“永不误拦”**。README 专章 “When it blocks something you meant” 与 cheat sheet 说明会误拦（来源：README headings）。
5. **不是新项目但仍在活跃**。2026-10 创建、v0.5.2（2026-11-20）、最近提交 2026-11-22（来源：GitHub）。

## 痛点

- **空变量误删**：rm -rf $VAR 变量为空导致灾难。
- **误杀/抢端口**：pkill 或启动服务影响别的代理/开发服务器。
- **git 误提交**：git add -A 把别的会话改动一起提交。
- **多代理隔离**：需要知道会话与进程/端口/git 树的归属关系。
- **误拦截体验**：护栏太严会挡正常操作，需 cheat sheet 与配置。

## 核心功能（依据 README，注明来源）

- **进程/端口/git 树归属感知**：仓库描述明确“knows which session owns which process, port and git tree”。
- **危险操作拦截**：What it stops / Guards 章节列出拦截项（来源：README headings）。
- **Worktree 一键**：Worktrees in one step（来源：README headings）。
- **远程重任务**：Run heavy jobs on another machine (optional)（来源：README headings）。
- **多代理支持**：Claude Code、Codex、Cursor、Gemini CLI、opencode、Qwen Code、pi（来源：README headings 与 topics）。
- **零依赖**：仓库描述强调 Zero dependencies。
- **误拦截处理**：When it blocks something you meant + cheat sheet、Configuration（来源：README headings）。

## 使用场景

- 同时运行多个编码代理、共享一台机器的开发者。
- 担心代理误操作破坏工作区的团队。
- 需要可配置护栏与远程任务隔离的工作流。

## 适用人群

多代理并行开发者、关注 agent safety 的工程团队。单代理用户价值较低。

## 注意事项

- MIT 许可。
- 35 star、6 fork、3 issue、v0.5.2，仍在活跃迭代（最近提交 2026-11-22）。
- 护栏可能误拦正常操作，需读 cheat sheet 与配置。
- 它降低风险但不等于绝对安全，仍需备份与审查。

## 总结

claude-bulkhead 抓住了多代理并行的真实安全隐患：代理会互相“误伤”。以“会话归属”为核心设计 hooks 护栏，覆盖删文件、杀进程、抢端口、误提交等场景，还支持多代理与零依赖。35 star 的早期项目，但对同时开多个代理的人非常实用。值得安装试验，注意误拦与配置。

## 独立评分（满分 10）

- **质量**：8 — 会话归属护栏设计切中要害、多代理支持、零依赖；会误拦。
- **学习价值**：7 — 多代理安全护栏的实现思路有参考价值。
- **使用价值**：8 — 同时开多个代理的开发者非常实用。
- **推荐等级**：**B**
