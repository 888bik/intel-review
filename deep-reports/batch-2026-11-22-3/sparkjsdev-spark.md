# sparkjsdev/spark

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/sparkjsdev/spark
- **分类**：developer-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：TypeScript
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：3938 / 595
- **许可证**：MIT
- **创建时间**：2025-05-23
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-19）
- **最新 release**：v2.4.0（tag `v2.4.0`，发布于 2026-10-20）
- **一句话简介**：面向 THREE.js 的高级 3D Gaussian Splatting（高斯泼溅）渲染器，可在 WebGL2 上融合 splat 与网格、实时编辑并高性能渲染大规模 3D 场景。

## 一句话理解

它让网页浏览器高效显示“高斯泼溅”这种新型 3D 表示——照片级真实感的点云场景——并和 THREE.js 的普通 3D 物体混在一起渲染、实时编辑。

## 通俗解释

3D Gaussian Splatting 是近两年火起来的新技术：用大量带颜色/透明度/协方差的“小椭球”表示场景，能 photorealistic 地重建真实世界，但数据量巨大、浏览器里难渲染。spark 是给 THREE.js 用的 splat 渲染器：它集成进 THREE.js 渲染管线，把 splat 对象和网格对象融合渲染；瞄准 98%+ 的 WebGL2 支持，声称在低功耗移动设备也能快（来源：README Features，**性能为项目自述、未独立核实**）；支持 .PLY、.SPZ、.SPLAT、.KSPLAT、.SOG 等主流 splat 格式；多个 splat 对象正确排序；多视角同时渲染；可对单个 splat 变换编辑做动画；实时改色、位移、骨骼动画；还有 shader graph 系统在 GPU 上动态创建/编辑 splat（来源：README Features）。3,938 star，v2.4.0，是 Web 端 splat 渲染的活跃方案。

## 它不是什么

1. **不是独立的 3D 引擎**。它是 THREE.js 的渲染器扩展，需配合 THREE.js 使用（来源：README 开头）。
2. **不是 splat 训练工具**。它负责“渲染”，不是从照片训练出 splat 模型（README 讲的是渲染与格式支持）。
3. **不是只能用某一种格式**。支持多种主流 splat 格式（来源：README Features）。
4. **不是静态展示**。强调 fully dynamic、可编辑、shader graph（来源：README Features）。
5. **不是 100% 设备覆盖**。README 说“98%+ WebGL2 support”，仍依赖 WebGL2（来源：README Features，自述）。

## 痛点

- **Web 端 splat 渲染难**：数据量大、排序复杂、移动端性能差。
- **与常规 3D 融合困难**：splat 与普通 mesh 混合渲染需要统一管线。
- **编辑能力缺失**：很多渲染器只显示不能改；spark 提供实时编辑与 shader graph。
- **格式碎片化**：多种 splat 格式需要统一支持。

## 核心功能（依据 README，注明来源）

- **THREE.js 管线集成**，splat 与 mesh 对象融合渲染（来源：README Features）。
- **可移植性**：98%+ WebGL2 支持、低功耗移动设备也能快（来源：README Features，自述）。
- **多格式**：.PLY（含压缩）、.SPZ、.SPLAT、.KSPLAT、.SOG（来源：README Features）。
- **多 splat 正确排序、多视角同时渲染**（来源：README Features）。
- **动态编辑**：变换/动画、实时改色、位移、骨骼动画（来源：README Features）。
- **Shader graph 系统**：GPU 上动态创建/编辑 splat（来源：README Features）。
- **安装**：NPM、CDN、Copy Code 多种方式；有 examples 与本地开发/文档构建说明（来源：README headings）。

## 使用场景

- 网页中展示照片级 3D 重建场景（数字孪生、虚拟看房、文物展示）。
- 需要把 splat 与常规 3D UI/角色混合的应用。
- 对 splat 做实时编辑、动画、特效的创意开发。

## 适用人群

WebGL/THREE.js 开发者、3D 可视化与 XR 开发者。不做 3D 的开发者用不上。

## 注意事项

- MIT 许可。
- “98%+ WebGL2”“移动端快”等为项目自述、未独立核实，实际性能需按场景测。
- 115 个 open issues，活跃开发中；v2.4.0（2026-10），近期仍在提交（2026-11-19）。
- splat 数据本身可能很大，加载与内存需自行优化。

## 总结

spark 是 Web 端 3DGS 渲染的实力派：把 splat 无缝接入 THREE.js、支持主流格式、强调动态编辑与 shader graph，瞄准真实落地场景。3.9k star、活跃迭代说明社区认可。性能声明需自行验证，但对要在浏览器里玩高斯泼溅的团队，它是值得优先评估的渲染器。

## 独立评分（满分 10）

- **质量**：8 — THREE.js 集成、格式与编辑能力丰富、活跃迭代；性能数字自述。
- **学习价值**：8 — Web 端 3DGS 渲染的一手资料。
- **使用价值**：8 — 浏览器里做高斯泼溅的团队价值高。
- **推荐等级**：**B**
