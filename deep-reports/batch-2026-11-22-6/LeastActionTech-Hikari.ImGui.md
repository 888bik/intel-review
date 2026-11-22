# LeastActionTech/Hikari.ImGui

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/LeastActionTech/Hikari.ImGui
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：C#
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：0 / 0
- **许可证**：MIT
- **创建时间**：2026-11-03
- **最近更新**：2026-11-09（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：v1.93.2（tag `v1.93.2`，发布于 2026-11-22）
- **一句话简介**：零原生依赖的纯 C# 版 Dear ImGui，目标对齐 v1.93.0-docking

## 一句话理解
一个纯 C# 重写的 Dear ImGui，目标是对齐 v1.93.0-docking，不依赖任何原生库。

## 通俗解释
README 简短但信息密度高：给出仓库结构、NuGet 核心库、基于 Silk.NET 的 OpenGL 与 Vulkan demo、测试项目，以及从 C++ 官方头文件同步并审计符号一致性的两个 Python 工具。README 自述测试项目有 465 个测试并有 PARITY_COMPLIANCE.md 记录；0 star，刚入库，尚无社区验证。README 没有给出性能基准数字。

## 它不是什么
1. 不是 Dear ImGui 官方移植，原版设计与架构版权归 Omar Cornut 与贡献者。
2. 不提供渲染后端本身，只产出 ImDrawData，OpenGL、Vulkan 等由 demo 或用户自己的渲染器处理。
3. 不是对旧版本的兼容层，目标锁定 v1.93.0-docking。
4. 不是成熟生态，README 未列出广泛的第三方集成与长期维护记录。
5. 不是原生 C# 的全新设计，而是对 C++ 实现的逐符号重实现。

## 痛点
- .NET 项目想用 Dear ImGui 通常要引入原生 DLL 和跨平台绑定，部署复杂。
- 手工移植容易在符号、结构体与行为上和上游漂移。
- 没有统一的 parity 审计工具时，很难证明“这个端口跟上游一致”。
- 字体图集与渲染生命周期是常见坑；最近一次提交正是在修这个问题。

## 核心功能（依据 README，注明来源）
- 纯 C# 实现即时模式 GUI 核心：CreateContext、NewFrame、Begin/End、Render 与 GetDrawData 的调用流程与原版一致（来源：README Quick Start）
- 提供基于 Silk.NET 的 OpenGL 与 Vulkan 交互 demo，可用 dotnet run 切换后端（来源：README Running the Interactive Demo）
- 提供 Tools/sync_imgui_reference.py 拉取官方 C++ 头文件、verify_imgui_parity.py 审计 C# 符号与结构一致性（来源：README Canonical Parity Verification Tooling）
- 仓库结构包含 NuGet 核心库、demo、测试项目与 PARITY_COMPLIANCE.md 记录（来源：README Repository Structure）
- 以 MIT 许可发布，并明确区分 Dear ImGui 原设计与 C# 移植的版权归属（来源：README Authors & Attribution）

## 使用场景
- .NET/C# 应用需要内嵌调试面板或工具 UI，且不想打包原生动态库
- 想研究 C++ 库到 C# 的系统性移植与一致性验证方法
- 需要可离线审查的 parity 记录来评估移植可信度
- 用 Silk.NET 做图形项目，想直接复用 demo 的渲染对接方式

## 适用人群
熟悉 C# 与 .NET 工具链的图形或工具开发者；对 imGui 架构与移植工程感兴趣的人。不适合追求稳定生产级 GUI 框架或需要官方支持的团队。

## 注意事项
- README 自述测试项目有 465 个测试并提供 dotnet test 命令，但未给出性能基准数据。
- 最近一次提交为修复字体渲染与字体图集生命周期问题，说明这一块仍在修整期。
- 上游目标版本是 v1.93.0-docking，而 Release 标签是 v1.93.2，使用时应注意版本对应关系。
- 许可证：MIT（GitHub API 标注，README 指向 LICENSE.md）。
- 0 star、刚入库，尚无社区验证；已有 v1.93.2 Release。

## 总结
把 Dear ImGui 以零原生依赖的方式带进 C# 世界，并用同步与 parity 审计工具把“像不像上游”变成可检查的问题，这个思路比单纯翻译代码更值得看。但项目 0 star、字体处于修整期、也没有性能与生态验证，目前更适合评估与技术参考，而不是替代原版投入生产。

## 独立评分（满分 10）

- **质量**：6 — 目标对齐 v1.93.0-docking、有 parity 工具和测试，但 0 star、刚起步
- **学习价值**：6 — 展示了如何把 C++ immediate-mode GUI 逐符号移植到纯 C# 并做一致性审计
- **使用价值**：5 — 适合不想引入原生依赖的 .NET 项目，但生态与成熟度远不及原版
- **推荐等级**：**C**
