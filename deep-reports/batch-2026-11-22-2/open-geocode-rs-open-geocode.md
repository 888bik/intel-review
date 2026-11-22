# open-geocode-rs/open-geocode

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 A

## 基本信息

- **仓库地址**：https://github.com/open-geocode-rs/open-geocode
- **分类**：developer-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Rust
- **评分**：原始评分未核实；本报告独立评分见文末
- **Star / Fork**：78 / 17
- **许可证**：Apache-2.0
- **创建时间**：2026-05-14
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：v0.1.0（发布于 2026-06-05）
- **一句话简介**：纯 Rust 编写、基于开放数据（OSM 与 OpenAddresses）的自托管地理编码引擎，单文件提供正向/反向/自动补全/批量地理编码，主打快速、轻量与隐私。

## 一句话理解

这是“自己掌控的地址→坐标引擎”：不依赖商业地理编码 API，把全世界地址索引压进一个大文件，用内存映射提供查询，速度宣称远超 Photon/Pelias，且地址数据不出你的网络。

## 通俗解释

把“北京市海淀区××路 1 号”变成经纬度（地理编码），通常要么调 Google/Mapbox 的付费 API（有配额、按次收费、数据出境），要么自建 Nominatim/Pelias（要 PostgreSQL/PostGIS、导入全球数据要几天）。open-geocode（og）走第三条路：用纯 Rust 从 OSM 和 OpenAddresses 构建一个索引文件，查询时内存映射这个文件，提供正向、反向、自动补全和批量地理编码，无需 PostgreSQL 或 Elasticsearch（来源：README “Why open-geocode?”）。README 自述：国家尺度的索引几分钟建好，正向 QPS 是 Photon/Pelias 的 6 倍、反向超过 20 倍，内存只需四分之一；全球索引是一个 26.3GB 文件，在 24 核/32GB 台式机上 5 小时 20 分建好（来源：README Benchmarks，**这些性能与构建数字为项目自述、未独立核实**）。它还在 MESSY STREETS 学术基准上给出与 Google/Mapbox 等的对比表（来源：README 表格，项目自述）。

## 它不是什么

1. **不是商业地理编码 API 的客户端**。它自己构建索引、自己服务查询，不调用第三方地理编码服务（来源：README）。
2. **不是数据库+服务的重型栈**。不需要 PostgreSQL/PostGIS 或 Elasticsearch，更新就是换一个文件（来源：README “Why open-geocode?”）。
3. **不是全球数据的“实时更新”服务**。索引按构建时的快照（Planet Pack 为 2026-10 的 OSM + OpenAddresses）生成，更新需重新构建/换文件（来源：README 脚注）。
4. **不是100% 准确**。README 的基准表里 og 在 GH4/GH6 精度档低于 Google/Mapbox，且有 52 个输入回退到 area 级别；全答案 CRR 为 94.4%（来源：README 表格与脚注，自述）。
5. **不是成熟大版本**。最新 release 仅 v0.1.0（2026-06），78 star，项目仍早期（来源：GitHub release 与 star 数）。

## 痛点

- **商业 API 贵且泄露数据**：按次收费、限额、把地址发给第三方；og 完全自托管、数据不出网（来源：README）。
- **开源方案太重**：Nominatim 要 Postgres+PostGIS、planet 导入数天、64GB 内存；Pelias 难自托管且无 batch 端点（来源：README “Why open-geocode?”）。
- **批量地理编码不便**：og 提供 batch，把 CSV 丢进去带坐标返回（来源：README）。
- **部署成本**：内存映射文件让它能在 Cloud Run/Fly Machines 这类 scale-to-zero 平台跑，闲时几乎不花钱（来源：README，自述）。

## 核心功能（依据 README，注明来源）

- **四种地理编码**：正向、自动补全、反向、批量，统一由一个文件提供服务（来源：README 开头）。
- **解析器**：semi-Markov CRF，每次构建都用所服务的地址训练（来源：README “Features”）。
- **搜索**：Tantivy 索引 + FST 字典，Levenshtein 自动机容错拼写错误，跨文字 Unicode 音译，支持自由格式与结构化输入（来源：README “Features”）。
- **反向编码**：Hilbert 打包的 R-tree + 真实行政边界的点在多边形判断，返回最近地址及完整 locality/region/country（来源：README “Features”）。
- **结果质量**：校准置信度 + rooftop/interpolated/street 三级精度（来源：README “Features”）。
- **Docker 快速开始**：README 有 Quickstart (Docker)、Use Cases、Docs/API/Benchmarks 链接（来源：README headings）。
- **基准数据**：MESSY STREETS 对比表（Google、Mapbox、Pelias、Photon、Nominatim 等），Planet Pack 26.3GB/630.9M 记录（来源：README Benchmarks，**自述、未独立核实**）。

## 使用场景

- 需要批量把地址数据地理编码、又不想把数据发给第三方的团队。
- 自建地图/位置服务，替代 Nominatim/Pelias 的重型部署。
- 在边缘/小内存/scale-to-zero 环境提供地理编码（内存映射优势）。
- 多语言/跨文字地址（音译与容错）场景。

## 适用人群

后端/地理信息工程师、重视数据隐私的团队、自建地图服务的人。只需偶尔查几个地址的用户用商业 API 更省事。

## 注意事项

- **性能与准确度数字均为项目自述、未独立核实**；选用前应在实际数据上测。
- Apache-2.0 许可（代码）；索引数据来自 OSM/OpenAddresses，需遵守其数据许可（ODbL 等），细节未核实。
- v0.1.0 早期版本、78 star，社区很小，API/索引格式可能变化。
- 全球索引文件 26.3GB（自述），存储和首次构建门槛不低。

## 总结

open-geocode 是“用工程化思路重做地理编码”的代表：纯 Rust、单文件、内存映射、无外部数据库，把开源地理编码的部署门槛从“几天 + 64GB”压到“一个文件 + 一条 Docker 命令”。它的性能与精度声明很亮眼但均为自述，需自行验证；78 star、v0.1.0 也说明它很新。对有隐私/批量/自建需求的人，这是值得认真评估的新选项；对通用用户，商业 API 仍更省事。

## 独立评分（满分 10）

- **质量**：8 — 架构设计精良（单文件 + mmap + CRF/Tantivy/R-tree）、文档与基准详尽；版本早期、数据自述未核实，扣分。
- **学习价值**：9 — 地理编码引擎的解析、索引、反向、精度分级实现是一手高质量资料。
- **使用价值**：8 — 自托管/批量/隐私场景价值很高；数据许可与早期版本风险扣分。
- **推荐等级**：**A**（技术方向与工程质量突出，值得该领域重点关注）。
