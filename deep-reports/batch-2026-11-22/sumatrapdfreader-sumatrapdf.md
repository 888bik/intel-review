# sumatrapdfreader/sumatrapdf

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 A

## 基本信息

- **仓库地址**：https://github.com/sumatrapdfreader/sumatrapdf
- **分类**：productivity（据 zhao 情报库分类；原始库未核实）
- **语言**：C（topics 另有 `c-plus-plus`，即 C++ 代码亦存在，未逐文件核实）
- **评分**：原始评分未核实；本报告独立评分见文末
- **Star / Fork**：18,054 / 2,104
- **许可证**：GPL-3.0（README：主代码 (A)GPLv3，部分代码 BSD，见 AUTHORS —— 已核实）
- **创建时间**：2012-10-02
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-21）
- **最新 release**：3.6.1 release（tag `3.6.1rel`，发布于 2026-04-06）
- **一句话简介**：Windows 平台上的多格式、轻量、快速开源文档阅读器，主打 PDF 但也支持 EPUB、MOBI、漫画与多种电子书格式。

## 一句话理解

SumatraPDF 是 Windows 上“反 Adobe Reader”的阅读器：启动快、体积小、不常驻后台、不塞广告，打开 PDF/漫画/电子书就是纯粹的阅读体验。

## 通俗解释

很多人对 PDF 阅读器的印象是 Adobe Reader——功能多但启动慢、更新频繁、动不动几十上百 MB 安装包。SumatraPDF 走了相反路线：用 Win32/C 写成，单文件、秒开、几乎不占资源，虽然界面朴素但该有的都有（搜索、目录、书签、缩放、双页、快捷键）。它不只支持 PDF，还支持 EPUB、MOBI、CBZ/CBR（漫画）、FB2、CHM、XPS、DjVu 等（来源：README），一个程序顶掉好几个阅读器。它是开源社区“小而美”软件的代表作，在全球有大量忠实用户，尤其受漫画读者和老机器用户欢迎。

## 它不是什么

1. **不是 PDF 编辑/制作工具**。它是阅读器，不提供表单填写、注释编辑、OCR、转换等重功能（这些需其他工具；README 仅描述 reader）。
2. **不是跨平台软件**。README 明确“for Windows”，macOS/Linux 不在支持范围（来源：README）。
3. **不是基于 Electron/Chromium 的“现代外壳”**。它是原生 Win32 程序（topics：`win32`），不以 Web 技术包装（来源：仓库 topics）。
4. **不是只支持 PDF**。README 列了 9 种格式（PDF、EPUB、MOBI、CBZ、CBR、FB2、CHM、XPS、DjVu）（来源：README 原文）。
5. **不是停更的老软件**。最近提交 2026-11-21，仍在活跃维护（来源：GitHub commits）。

## 痛点

- **臃肿阅读器体验差**：主流 PDF 工具启动慢、常驻进程、弹窗广告；SumatraPDF 以“轻量快速”直击此痛点。
- **多格式需要多个软件**：漫画、EPUB、PDF 各装一个阅读器很麻烦；SumatraPDF 用单程序覆盖多格式。
- **老机器/低配设备资源紧张**：原生 Win32 实现让它能在老旧 Windows 上流畅运行。
- **缺乏现代界面并非缺点而是取舍**：它把开发资源放在阅读体验与兼容性上，而非 UI 动画。

## 核心功能（依据 README，注明来源）

- **多格式阅读**：PDF、EPUB、MOBI、CBZ、CBR、FB2、CHM、XPS、DjVu（来源：README 原文）。
- **Windows 原生实现**：README 原文“a multi-format ... reader for Windows”，topics 含 `win32`（来源：README 与 topics）。
- **开源许可**：主代码 (A)GPLv3，部分代码 BSD（见 AUTHORS）（来源：README）。
- **官方文档与网站**：Website、Manual、Developer Information（如何贡献）三个入口（来源：README “More Information”）。
- **CI 构建**：README 顶部有 GitHub Actions build badge（来源：README 徽章）。

## 使用场景

- 日常打开/阅读 PDF 文档、论文、说明书，替代臃肿的 Adobe Reader。
- 漫画爱好者阅读 CBZ/CBR 格式的本地漫画。
- 老电脑、低配 Windows、不想装“全家桶”的用户。
- 开发者做 PDF/电子书阅读器的参考实现（Win32 原生代码）。

## 适用人群

Windows 用户、PDF/漫画/电子书读者、追求轻量软件的人。跨平台用户无法使用。

## 注意事项

- GPL-3.0（部分 BSD），二次分发需遵守对应许可证。
- 最晚 release 在 2026-04，虽日常提交活跃，但“正式版本”间隔较长，想用最新修复可能需要自编译或使用预发布版（预发布细节未核实）。
- 不支持 macOS/Linux，不要误以为开源就等于全平台。

## 总结

SumatraPDF 证明了“软件可以小而快且好用”。18k star 是用户用脚投票的结果。它在 PDF 阅读这个红海品类里靠“克制”脱颖而出：不追求功能全面，而把阅读体验做到极致。对 Windows 用户，它几乎是无脑推荐；唯一遗憾是没有跨平台版本。

## 独立评分（满分 10）

- **质量**：9 — 长期稳定维护、轻量高效、口碑极佳；功能聚焦也意味着不做复杂编辑。
- **学习价值**：7 — Win32 原生多格式阅读器实现有参考价值，但 README 极简，深入需读源码。
- **使用价值**：9 — Windows 上 PDF/漫画阅读的顶级选择，免费无广告。
- **推荐等级**：**A**（品类内标杆，Windows 用户强烈推荐）。
