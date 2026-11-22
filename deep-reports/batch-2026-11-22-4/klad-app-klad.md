# klad-app/klad

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/klad-app/klad
- **分类**：ai-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Rust
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：330 / 23
- **许可证**：PolyForm Internal Use License 1.0.0（LICENSE 文件确认；含附加许可条款与大公司营收上限 <50 万欧元限制；docs/format.md、docs/cli.md、docs/skill/、crates/client/ 为 MIT，见 LICENSE-MIT）
- **创建时间**：2026-11-04
- **最近更新**：2026-11-21（元数据 updated_at；最近提交 2026-11-19）
- **最新 release**：v0.4.0 tools （tag `v0.4.0`，发布于 2026-11-19）
- **一句话简介**：一款与 AI 代理共享的 2D 绘画与动画应用：你用笔画、代理通过命令行用同样的笔刷在同一张画上作画，每一笔都作为可编辑数据保留。

## 一句话理解

它让 AI 代理拿到“真正的画笔”：不再吐一张不可改的图片，而是一笔一笔在有路径、压力、颜色的图层上画，你能随时选中任何一笔改色、弯曲、移动，双方实时看到对方笔触。

## 通俗解释

图像生成模型给你像素，想改一处只能再抽一次卡。klad 的理念是“每一笔都是数据”（来源：README “Why not just generate the picture?”）：笔画保留路径、压力、笔刷、颜色，填充保留区域，几小时后仍能挑出来改。它是一个完整的 2D 绘画+动画应用（二十多种笔刷、图层/蒙版/效果、时间轴与洋葱皮、关键帧补间、IK 骨骼、导出 PNG/sprite sheet/GIF/Godot），还有真正的像素艺术模式（网格、调色板、Aseprite 双向、palette variants）；同时代理通过 klad 命令行在同一张画上作画，每次操作都是共享历史里的一步，可撤销（来源：README）。有三个 MIT 许可的 agent skill（工具/绘画技艺/像素），支持 Claude Code、Codex 等能跑命令的代理（来源：README）。文件格式按字节文档化（docs/format.md，MIT），画作是本地文件夹，无需账号，适合 git（来源：README）。

## 它不是什么

1. **不含图像模型**：klad 明确“There is no image model in klad”，画什么由你的代理决定（来源：README）。
2. **不是开源软件**：源码可见但采用 PolyForm Internal Use License 1.0（GitHub API 标 NOASSERTION，LICENSE 文件确认），个人/营收<50万欧元公司可免费自建自用（来源：LICENSE 文件与 README）。
3. **不是纯矢量绘图**：笔画像矢量一样保路径压力，但落在画布上仍是“paint”（笔触质感）（来源：README）。
4. **不是只在浏览器**：当前面向 macOS（支持压感/倾斜），Linux/Windows 也能构建通过测试（来源：README）。
5. **不是成熟产品**：330 star、v0.4.0、README 标 “early, you build it yourself”（来源：README 与 GitHub）。

## 痛点

- **生成图片不可编辑**：klad 让每笔可改，避免 reroll。
- **人与代理协作**：共享画布与历史，双方笔触即时可见、可撤销。
- **游戏美术工作流**：关键帧补间、arcs 检查、导出 Godot SpriteFrames/TileSet（来源：README Made for games）。
- **像素画规范**：网格、调色板绑定、Aseprite 往返（来源：README Pixel art）。

## 核心功能（依据 README，注明来源）

- **笔画即数据**：笔刷/压力/路径/颜色保留，可读 JSON、按地址改、批量 JSON ops（来源：README For developers）。
- **完整绘画动画**：20+ 笔刷、图层/蒙版/裁切、效果、时间轴/洋葱皮、关键帧补间、IK（来源：README）。
- **像素艺术**：真网格、调色板、Aseprite 双向、klad check 检查杂点/锯齿、导出 GIF/TileSet（来源：README Pixel art）。
- **命令行与代理**：stroke/set/fill/pixel/inbetween/export/undo 等命令与三个 MIT skill（来源：README）。
- **游戏导出**：sprite sheet、GIF、Godot SpriteFrames/TileSet（来源：README）。
- **文件格式文档化**：docs/format.md 按字节描述（MIT）（来源：README）。

## 使用场景

- 与编码代理协作绘制插画、游戏道具/角色/背景。
- 需要可编辑笔画而非一次性图片的创作。
- 像素画、逐帧动画、Godot 美术管线。

## 适用人群

游戏美术、插画师、想让代理参与绘画的开发者。只想“生成一张图”的用户不需要。

## 注意事项

- **许可证特殊**：PolyForm Internal Use 1.0，营收>50万欧元公司使用需购买许可证；部分目录 MIT（来源：LICENSE 文件）。
- 330 star、v0.4.0、3 issue，早期、需自行构建；当前主要 macOS。
- README 功能为项目自述，未独立核实。
- 画作依赖代理能力，效果因代理而异。

## 总结

klad 的立意非常独特：把 AI 从“图片生成器”变成“拿着画笔的协作者”，每笔可编辑、人与代理共享历史、还能服务游戏美术管线。330 star、早期、需自建，且许可对大公司有营收限制。对想认真做可编辑 AI 绘画的团队，这是值得关注的方向。

## 独立评分（满分 10）

- **质量**：8 — 笔画即数据 + 人机共享画布的设计独特、功能完整；早期需自建、许可对大公司有限制。
- **学习价值**：8 — 可编辑 AI 绘画与笔画数据模型很有启发性。
- **使用价值**：7 — 游戏美术/插画协作价值高，许可与成熟度需留意。
- **推荐等级**：**B**
