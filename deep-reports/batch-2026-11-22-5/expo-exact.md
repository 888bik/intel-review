# expo/exact

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/expo/exact
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Rust
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：4898 / 142
- **许可证**：MIT
- **创建时间**：2026-11-02
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：全平台应用框架：一份源码在 Web、iOS、macOS、Linux、Android 上以各平台原生方式渲染，并围绕 AI 代理的“写-跑-看-测”循环设计，追求 100ms 冷启动与极小运行时。

## 一句话理解

它不是“一套 UI 到处画一样”，而是“一份源码、每个平台都做到原生出色”：Web 用真 DOM、iOS 用 UIKit、macOS 用 AppKit，同时 AI 代理能用一组统一操作在每台设备上构建、运行、截图、点击、验证。

## 通俗解释

传统跨平台框架（React Native、Flutter）常用最小公分母或自绘引擎，牺牲各平台特性。Exact 自称“all-platform”：Web 用浏览器 DOM（约 20KB 运行时）、iOS 用 UIKit、macOS 用 AppKit、Android 用硬件加速 Canvas + Material 3 与原生控件、Linux/Windows 直接绘制（来源：README 平台表）。它围绕三件事设计（来源：README “Why Exact?”）：① Agent Native——一组 10 个操作（tree/screenshot/tap/type/state/layout/logs/clock/prefer/perf）驱动所有平台，时间可确定性推进（不靠 sleep），一条命令 verify 检查明暗模式、多宽度、无障碍树等（来源：README）；② Speed is King——100ms 冷启动、100ms 重载、首帧前零 JS、约 20KB Web 运行时等硬性预算持续检查（来源：README，**数字为项目自述、未独立核实**）；③ No Compromises——默认用平台原生控件，需要时给完整创意控制（CSS/SVG/Canvas/wgpu）与 60+ 原生能力包（相机、passkey、HealthKit 等）（来源：README）。但 README 明说 pre-1.0、每日变化、无打包发布、无 API 稳定性与兼容层（来源：README NOTE）。4,898 star、MIT、无 release（截至研究日期）。

## 它不是什么

1. **不是传统跨平台框架**。它主张 all-platform：每平台原生出色，而非统一绘制（来源：README）。
2. **不是稳定 1.0**。pre-1.0、每日变化、无 packaged release/API 保证（来源：README）。
3. **不是 JSX/React 层**。UI 用 Contract 语言声明，逻辑可用 TypeScript 或 Rust（来源：README）。
4. **不是所有平台都同等成熟**。Windows 早期、Android Material 3 打磨中（来源：README 平台表）。
5. **不是生态兼容 RN/Flutter 插件**。README 明确其插件不兼容（来源：README 诚实权衡）。

## 痛点

- **跨平台体验妥协**：all-platform 让每个平台都原生。
- **AI 代理难以可靠操作 UI**：统一驱动 + 可 seek 的确定性时间 + 结构化诊断（来源：README）。
- **启动与反馈慢**：硬性性能预算与 baked first frame（来源：README）。
- **既要原生又要定制**：precedence author→platform→default，需要时下探原生对象（来源：README）。

## 核心功能（依据 README，注明来源）

- **多平台原生渲染**：Web DOM、UIKit、AppKit、Android Canvas+Material 3、Linux/Windows 绘制，另有 tvOS/watchOS/终端 host（来源：README 平台表）。
- **Agent 驱动**：10 个操作统一驱动各平台、可推进的确定性时钟、一键 verify（来源：README Agent Native）。
- **性能预算**：100ms 冷启动/重载、首帧零 JS、约 20KB 运行时、持续门禁与回归 bisect（来源：README，自述）。
- **Contract UI 语言**：严格类型、无隐藏副作用、整个 app 可放进模型上下文（来源：README）。
- **原生能力与创意控制**：60+ 包、native module 与 hatch 下探原生对象、CSS/SVG/Canvas/wgpu（来源：README）。
- **数据栈可选**：TypeScript（Hermes/浏览器）、Rust（原生/Wasm）或零 JS 的 HTTP/GraphQL（来源：README）。
- **MIT 许可**（来源：GitHub API）。

## 使用场景

- 需要一套源码覆盖多端且强调原生体验的团队。
- 用 AI 代理自动构建/测试跨端应用的实验。
- 对启动性能有硬指标的应用。

## 适用人群

跨平台开发者、AI 编码代理研究者、性能敏感的客户端团队。生产项目需评估 pre-1.0 风险。

## 注意事项

- MIT 许可。
- **分类为本报告自行判断**（新入库项目，情报库无分类）。
- pre-1.0、每日变化、无发布/API 稳定性（来源：README），4.9k star 但风险高。
- 性能数字（100ms、20KB、vs React 81KB）均为项目自述、未独立核实。
- README 诚实列出与 RN/Flutter 生态的差距，选型时务必阅读。

## 总结

Exact 是一个雄心勃勃的“全平台原生 + AI 代理原生”框架：每平台原生渲染、统一 agent 驱动、严格性能预算，README 写得坦率有力。4.9k star、MIT，但 pre-1.0、每日变化、无稳定发布，性能数字自述。适合尝鲜与研究，生产使用需谨慎评估。

## 独立评分（满分 10）

- **质量**：8 — all-platform 原生渲染 + agent 原生工作流 + 严格性能预算，设计雄心大；pre-1.0、每日变化、无发布。
- **学习价值**：8 — 跨端原生渲染、agent 驱动循环与性能预算工程有很高参考价值。
- **使用价值**：7 — 愿意尝鲜的跨平台/AI 开发者价值高，生产使用风险大。
- **推荐等级**：**B**
