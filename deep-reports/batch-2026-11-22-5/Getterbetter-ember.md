# Getterbetter/ember

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/Getterbetter/ember
- **分类**：productivity（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Swift
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：16 / 0
- **许可证**：MIT
- **创建时间**：2026-11-12
- **最近更新**：2026-11-18（元数据 updated_at；最近提交 2026-11-18）
- **最新 release**：Ember 1.0.3（tag `v1.0.3`，发布于 2026-11-18）
- **一句话简介**：一款 macOS 小工具：让 MagSafe 充电指示灯跟随专注模式、定时与低电量状态自动开关或变色，并在接入/唤醒后自动恢复。

## 一句话理解

MacBook 的 MagSafe 充电灯在开会/勿扰时还亮着很烦人？Ember 让它跟着专注模式自动熄灭（或变绿/琥珀），专注结束再恢复，还会自我修复系统重置的灯状态。

## 通俗解释

macOS 的专注模式（睡眠、勿扰、工作等）能过滤通知，但 MagSafe 充电指示灯不受它管。Ember 通过 Focus Filter 把灯纳入专注模式：专注开启时变暗（或绿/琥珀），结束恢复；支持 Auto/常开/常关三种模式、小睡一小时或到早晨、可选熄灯时段（如 22:00–07:00）、低电量保护（插电且低于阈值时保持亮以便看到充电）、自愈（macOS 在插电/唤醒时重置灯，Ember 一两秒内恢复）、安全退出（退出/崩溃/卸载后灯恢复正常），还有四个快捷指令动作（来源：README What it does）。它需要 Apple Silicon 且带 MagSafe 3 的 MacBook、macOS 26+，通过小 helper 获得权限并签名公证（来源：README Install/Set up）。16 star、MIT、v1.0.3。

## 它不是什么

1. **不是通用灯控工具**。它只管 MagSafe 充电指示灯（来源：README）。
2. **不是支持所有 Mac**。需 Apple Silicon + MagSafe 3 + macOS 26+（来源：README Install）。
3. **不是完全无障碍**。需要管理员授权装 helper（来源：README Set up）。
4. **不是 Mac 睡眠时还能工作**。README FAQ 明确睡眠时无法自愈（来源：README）。
5. **不是大项目**。16 star、0 fork、v1.0.3、2026-11 创建（来源：GitHub）。

## 痛点

- **专注模式不管充电灯**：Ember 把灯纳入 Focus Filter。
- **系统重置灯状态**：自愈机制恢复（来源：README）。
- **充电可见性**：低电量保护避免看不到充电指示（来源：README）。
- **自动化**：快捷指令动作接入任意自动化（来源：README）。

## 核心功能（依据 README，注明来源）

- **Focus 联动**：把 Ember 的滤镜加到各专注模式，灯随模式变化（来源：README What it does/Set up）。
- **模式与小睡**：Auto/Always off/Always on、小睡一小时/到早晨、可选熄灯时段（来源：README）。
- **低电量保护与自愈**（来源：README）。
- **Stay awake on charger**：可选，插电亮灯关闭时防睡眠（屏幕仍关）（来源：README）。
- **安全退出与快捷指令**：退出/崩溃/卸载恢复、四个 Shortcut 动作（来源：README）。
- **签名公证 + 权限 helper**（来源：README Install/Set up）。
- **MIT 许可**（来源：GitHub API）。

## 使用场景

- 希望 MagSafe 灯在会议/勿扰时熄灭的 MacBook 用户。
- 夜间不想被充电灯打扰的人。
- 用快捷指令自动化设备状态的用户。

## 适用人群

符合条件的 Apple Silicon MacBook（MagSafe 3）用户。其他平台用不上。

## 注意事项

- MIT 许可。
- **分类为本报告自行判断**（新入库项目，情报库无分类）。
- 16 star、v1.0.3、极新；平台要求严格（来源：README）。
- 需管理员授权 helper；睡眠期间无法自愈（来源：README）。
- 功能为项目自述，未独立核实；实际兼容以 macOS 版本为准。

## 总结

ember 是一个把“小细节”做到位 macOS 工具：MagSafe 灯与专注模式联动、自愈、低电量保护、快捷指令，考虑得很周到。16 star、平台极窄、极新，但对符合条件的 MacBook 用户是提升体验的小而美工具。

## 独立评分（满分 10）

- **质量**：8 — 把 MagSafe 灯与专注模式联动的小工具，细节考虑周到、有签名公证；平台极窄。
- **学习价值**：6 — macOS 系统服务与 Focus 过滤器的集成有参考意义。
- **使用价值**：8 — 符合条件的 MacBook 用户价值高，其他人用不上。
- **推荐等级**：**B**
