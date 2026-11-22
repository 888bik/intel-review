# cerulion-inc/cerulion

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息

- **仓库地址**：https://github.com/cerulion-inc/cerulion
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Rust
- **评分**：原始评分未核实；本报告独立评分见文末
- **Star / Fork**：121 / 4
- **许可证**：AGPL-3.0
- **创建时间**：2024-07-09
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：Cerulion v2.0.0（tag `v2.0.0`，发布于 2026-11-12；README 提到 2.1.0 的迁移说明，说明已有更新版本但仍标未核实）
- **一句话简介**：机器人软件开发平台，把“运行、录制、回放验证”与“给 AI 模型操作仿真机器人的 harness/MCP”整合进一个开发闭环，可对接 ROS 2。

## 一句话理解

Cerulion 想做机器人界的“Git + CI”：机器人程序每次运行都可录成 MCAP，改了代码后拿同一份录制回放，行为变了就报警；同时给 AI 代理一套工具，让模型直接操控仿真机械臂做任务。

## 通俗解释

机器人软件难调试，因为物理世界不可重放：现场出了 bug，你很难把“当时的传感器输入”原样喂回去复现。Cerulion 的做法是把机器人节点图（graph）运行时的数据录成标准 MCAP 文件，之后可用 `cerulion bag play --resim all --verify` 把录制的输入重新喂给改过的代码，逐字节比对输出——README 演示了把控制器速度从 0.3 改成 0.25 后，同一份录制验证失败并报出 `differs at x: recorded 0.3, replayed 0.25`，退出码 1（来源：README “Run, record and verify”）。它还提供 `Flashback`——忘了录制时从内存滚动历史里捞出故障前后的片段。另一半是“harness”：一个跑在本机的仿真机械臂 + MCP 服务，AI 代理（如 Claude Code）可以接上它，做“把碗放到盘子里”这样的任务，并通过网页看每次 episode（来源：README “Run the harness on your own machine”）。这是把 AI 代理引入机器人开发闭环的一次认真尝试。

## 它不是什么

1. **不是单一 CLI 工具**。它是运行时 + 录制验证 + 仿真 harness + MCP 服务 + 云端的整套平台（来源：README Quickstart）。
2. **不是 ROS 2 的替代品**。它提供 `rmw_cerulion` 与 ROS 2 集成路径，能跑已有 ROS 2 栈、附着到运行中的栈，或把 ROS 2 进程和原生节点混在同一图里（来源：README “Why Cerulion”）。
3. **不是硬件本身**。仿真 harness 驱动的是仿真机械臂；真实机器人需要另行配对（账号登录后电脑自动发现机器人，来源：README）。
4. **不是完全离线可用**。首次安装/登录/下载仿真器需要联网与免费账号；登录后录制回放可离线（来源：README Quickstart 与 “Run the harness”）。
5. **不是“AI 直接控制你的真机器人”的成熟方案**。README 明说 harness 目前是实验性（experimental），驱动的是 simulated robot（来源：README “Run the harness on your own machine”）。

## 痛点

- **机器人 bug 难复现**：录制 + 逐字节回放验证让“改代码是否改变行为”可判定，可在 CI 中拦截行为变化（来源：README “Why Cerulion”、“Run, record and verify”）。
- **忘记录制**：Flashback 从滚动内存历史保存故障前后片段（来源：README）。
- **ROS 2 数据通路受限**：零拷贝共享内存、跨固定大小消息的变长字段、知道图结构的调度器、显式背压与虚拟时钟（来源：README “Why Cerulion”）。
- **AI 代理缺机器人训练场**：harness 提供仿真臂 + 三个任务场景 + MCP 工具，让代理在仿真里练手并看回放（来源：README）。

## 核心功能（依据 README，注明来源）

- **运行与多进程图**：`cerulion graph run`，可拆成每节点一进程；周期性、数据触发、同步与外部节点按依赖级别调度（来源：README “Why Cerulion” 与 “Run, record and verify”）。
- **录制与验证**：标准 MCAP 录制，`bag play --resim all --verify` 回放比对，退出码 0 通过 / 1 数据分歧 / 6 调度分歧 / 2–5 无法完成；支持 `--tolerance`、`--report`（来源：README details）。
- **Flashback**：从滚动内存历史保存故障前后时刻（来源：README 开头与 headings “Capture the failure with Flashback”）。
- **零拷贝与变长字段**：直接把相机帧、扫描、schema 定义的变长字段写入共享内存，本地进程读同一份字节（来源：README “Why Cerulion”）。
- **ROS 2 集成**：`cerulion ros2 run/launch`，rmw 库（Jazzy 用系统自带，Lyrical 用发布包），Linux x86_64/ARM64 与 macOS 支持（来源：README installation details）。
- **仿真 harness 与 MCP**：`cerulion rcp --sim --http 127.0.0.1:8765` 启动，附三个场景（bowl-on-plate、open-drawer、soup-in-basket），Claude Code 用 `claude mcp add --transport http` 接入，episode 可在网页查看；首次运行下载约 4GB、需 5GB 可用空间（来源：README）。
- **托管 MCP 与云实例**：`https://mcp.cerulion.com/mcp/cerulion`，提供 open_bag、get_debrief、find_anomalies、scrub、robot_list、sim_start 等工具（来源：README “Give your model Cerulion”）。
- **基准与验证**：`cerulion rcp bench --model ... --cell ...`、`--verify` 对照 provenance ledger（来源：README）。

## 使用场景

- 机器人开发团队需要可回放、可 CI 验证的行为回归测试。
- ROS 2 项目想获得零拷贝、录制验证和 AI 辅助能力。
- AI 研究者用仿真机械臂 + MCP 评测模型的机器人操作能力。
- 远程机器人运维：账号配对、跨网络读取主题、云端实例。

## 适用人群

机器人工程师、ROS 2 用户、研究 embodied AI / AI agent 操控机器人的团队。普通软件开发者用不上。

## 注意事项

- AGPL-3.0 许可，对外提供服务需注意源码开放义务。
- harness 为实验性；README 明说默认场景不是公布基准用的 start state，单次 episode 有随机性（温度 1.0 可能成败各异），公布结果基于每模型 100 个 start state（来源：README）。
- 托管 MCP endpoint 处于 early access，需申请；仿真消耗账户 sim credits（每秒 1 credit，来源：README）。
- 首次安装链路较长（installer、账号登录、系统依赖、~4GB 仿真器），对新手不友好。
- README 提到 2.1.0 的迁移说明，但最新 release API 返回 2.0.0，版本状态需以仓库页为准。

## 总结

Cerulion 是“机器人开发工程化 + AI 代理接入”的一次系统性尝试：录制回放验证直击机器人不可复现的痛点，零拷贝与图感知调度有技术深度，仿真 harness + MCP 又把 LLM 代理带进机器人任务评测。它功能面很宽、文档详尽，但新、复杂、部分能力实验性。对机器人/embodied AI 团队值得重点跟踪；普通开发者了解思路即可。

## 独立评分（满分 10）

- **质量**：8 — 平台化设计完整、验证机制严谨、跨平台与 ROS 2 集成扎实；复杂度高、harness 实验性、元数据不全扣分。
- **学习价值**：9 — 录制回放验证、图调度、零拷贝、AI+机器人 harness 都是高质量一手资料。
- **使用价值**：7 — 机器人团队价值高；门槛高、受众窄。
- **推荐等级**：**B**（方向与深度俱佳，但成熟度和适用面限制其评级）。
