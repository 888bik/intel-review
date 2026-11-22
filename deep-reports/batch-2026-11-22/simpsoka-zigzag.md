# simpsoka/zigzag

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息

- **仓库地址**：https://github.com/simpsoka/zigzag
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Rust
- **评分**：原始评分未核实；本报告独立评分见文末
- **Star / Fork**：1,013 / 70
- **许可证**：Apache-2.0
- **创建时间**：2026-10-30
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：ZigZag 0.1.9（tag `v0.1.9`，发布于 2026-11-22，当天发布）
- **一句话简介**：用 Rust 编写的免费开源绣花（电脑刺绣）设计软件，支持把 SVG 转成针迹、可视化编辑、导出主流绣花机格式，并通过 MCP 让 AI 代理参与创作。

## 一句话理解

ZigZag 是“刺绣界的 Figma + AI 助手”：把矢量图变成能上绣花机的针迹文件，能可视化微调每一针，还破天荒地内置 MCP，让 Claude 这类 AI 直接帮你画图、检查设计。

## 通俗解释

电脑刺绣有个专门的环节叫“打版/数字化”（digitizing）：把普通矢量图转换成绣花机认识的针迹指令（哪里走线、哪里剪线、什么针法、什么顺序）。传统软件（如 Wilcom、Ink/Stitch）要么昂贵要么难用。ZigZag 用 Rust 写引擎求性能，提供桌面可视化编辑器，支持把 SVG 转针迹、微调对象/针法、预览真实缝制效果，并导出 PES、DST、EXP、JEF、VP3 等主流格式（来源：README 开头）。最有特色的是它内置了 MCP（Model Context Protocol）服务——AI 代理能直接在你正在看的画布上增删对象、检查设计、生成变体、甚至发送到 Brother Skitch (PP1) 蓝牙绣花机（来源：README “Agents (MCP)” 与 “Skitch over Bluetooth”）。这是一个把 AI 代理真正接入专业创作工具的先锋案例。

## 它不是什么

1. **不是普通画图软件**。它面向刺绣针迹，导出的是 PES/DST 等机器格式，不是 PNG/SVG 插画工具（虽然也能导入 SVG 和导出预览 PNG）。
2. **不是只运行在浏览器里的 Web 应用**。它是 Mac/Windows/Linux 桌面应用（README 提供 DMG/Homebrew 与各平台下载），CLI 是独立的 `zigzag` 命令（来源：README）。
3. **不是必须联网/付费才能用的 SaaS**。README 称 free、open-source，可本地下载使用、自带更新检查；AI 代理功能是可选能力（来源：README 开头与下载章节）。
4. **不是“一键完美打版”**。README 有 `review_design`、`measure_stitching`、`check_design` 等工具专门检查针迹问题，说明自动转换后仍需人工/AI 校验（来源：README MCP 工具列表）。
5. **不是只支持 Brother 机器**。它导出多种通用格式；Skitch (PP1) 蓝牙直连是特定机型的加分项（来源：README 开头与 Skitch 章节）。

## 痛点

- **专业刺绣软件昂贵/难用**：商业打版软件价格高，开源替代品交互老旧。ZigZag 用现代编辑器体验 + Rust 性能切入。
- **AI 与创作工具脱节**：大部分 AI 助手只能在聊天框里生成代码/文字，无法直接操作专业软件画布。ZigZag 的 MCP 让代理“真的动手改设计”（来源：README “Agents (MCP)”）。
- **跨机器格式兼容**：不同绣花机格式各异；一款软件覆盖 PES/DST/EXP/JEF/VP3 减少来回转换（来源：README 开头）。
- **质量检查靠经验**：针迹密度、底线、剪线次数等新手难判断；`review_design`、`measure_stitching` 把专家检查流程工具化（来源：README MCP 工具列表）。

## 核心功能（依据 README，注明来源）

- **SVG → 针迹转换与多格式导出**：PES、DST、EXP、JEF、VP3；CLI `zigzag convert logo.svg -o logo.pes -o logo.jef ...`（来源：README 开头与 “CLI” 示例）。
- **桌面可视化编辑器**：Stitch types、Studio、Illustrator 工作流、Mac app 等章节（来源：README headings）。
- **MCP 代理集成**：Studio 在本机 `http://127.0.0.1:7878/mcp` 运行 MCP server（Streamable HTTP，仅 localhost、带 Origin 检查），提供 `get_design`、`add_svg`、`transform_objects`、`set_document`、`render_preview`、`review_design`、`measure_stitching`、`export`、`send_to_skitch`、`batch` 等大量工具，每次调用对应一次撤销（来源：README “Agents (MCP)”）。
- **AI 友好的设计审查**：`review_design` 以绣花师视角检查底衬、间距、填充方向、空轮廓、边框、字高、缝制成本；`measure_stitching` 量化针迹外溢、露布、拥挤针点等并可与之前结果对比（来源：README MCP 工具列表）。
- **Skitch 蓝牙直连**：可把设计直接发到 Brother Skitch (PP1)（来源：README 开头与 “Skitch over Bluetooth”）。
- **CLI 与 headless MCP**：`zigzag convert/info/fabrics/machines/skitch` 命令；`zigzag-mcp --headless` 可在无 GUI 下供代理使用（来源：README “CLI” 与 “Agents (MCP)”）。
- **与 Ink/Stitch、Hatch 对等能力**：README 专门有 “Ink/Stitch parity” 与 “Hatch parity” 章节（来源：README headings）。

## 使用场景

- 绣花打版师把客户 SVG logo 转成机器文件并检查质量。
- 设计师用 Claude Code/Claude Desktop 通过 MCP “对话式”修改刺绣图案并实时预览。
- 小型刺绣工作室用 Skitch (PP1) 直接蓝牙输出设计。
- 开发者研究 MCP 如何集成到专业桌面创作软件（本项目的 MCP 设计是很好的参考）。

## 适用人群

刺绣/打版从业者、DIY 刺绣爱好者、对“AI 代理操作专业软件”感兴趣的开发者。普通无刺绣需求者用不上。

## 注意事项

- **很新**：2026-10-30 创建，0.1.9 版本当天发布，功能在快速迭代，生产使用需评估稳定性。
- Skitch 蓝牙直连依赖特定硬件（Brother Skitch PP1），其他机器走通用格式导出。
- Apache-2.0 许可，商业友好。
- 项目自述的“性能”“对等能力”未独立核实，应以实际导出效果与机器测试为准。

## 总结

ZigZag 最值得关注的不是“又一个绣花软件”，而是它把 MCP 深度嵌入专业创作工具：代理能实时改你眼前的画布、每次调用可撤销、还能做专家级设计审查。这种“AI copilot 在原声工具里动手”的模式，对整个创作软件行业都有示范意义。刺绣垂类本身受众有限，但作为 AI 集成范例值得开发者细读。

## 独立评分（满分 10）

- **质量**：8 — Rust 引擎 + 现代编辑器 + 详尽 MCP 工具集，迭代极快；0.1.x 早期版本稳定性扣分。
- **学习价值**：9 — MCP 与桌面创作软件深度集成的绝佳案例，工具设计（撤销、terse、batch、审查）非常有参考价值。
- **使用价值**：7 — 刺绣垂类刚需；非刺绣用户用不到，早期版本风险扣分。
- **推荐等级**：**B**（垂类价值高、AI 集成范例价值更高，但受众窄且太新）。
