# publicarray/pstview

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/publicarray/pstview
- **分类**：productivity（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Zig
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：0 / 0
- **许可证**：未核实（仓库无 LICENSE 文件，GitHub API 未标注许可证）
- **创建时间**：2026-11-15
- **最近更新**：2026-11-20（元数据 updated_at；最近提交 2026-11-21）
- **最新 release**：pstview v0.7.0（tag `v0.7.0`，发布于 2026-11-20）
- **一句话简介**：用 Zig 写的只读 PST/OST/MBOX 查看与导出工具，带离线邮件头分析和已删邮件恢复

## 一句话理解
一个用 Zig 写的只读邮件取证工具：打开 Outlook PST/OST 和 MBOX，安全查看、分析邮件头、恢复已删条目，并导出成 PST、MSG 或 EML。

## 通俗解释
README 结构清楚，把能做什么、不能做什么、怎么构建都列了出来。核心卖点是源邮箱只读、不写源文件也不在其旁边创建文件；查看时脚本、网络和远程图片全被拦掉，链接不跳转，附件只保存不打开。项目已有 v0.7.0 Release 与跨平台 CI，但 0 star、刚入库，尚无社区验证。

## 它不是什么
1. 不是 Outlook 的替代品，也不修改源 PST/OST；README 提醒用 Outlook 打开就会改动数据文件。
2. 不解析 ANSI（97-2002）格式与高加密 PST。
3. 不打开附件，除文本和图片外只能保存。
4. pstx 命令行工具不负责导出，导出只能在 pstview 里做。
5. 导出的 PST 虽按 MS-PST 规范并用自带读取器检查，但尚未用 Outlook 或 scanpst.exe 验证。

## 痛点
- 用 Outlook 打开证据文件会改变它，取证场景需要绝对只读。
- 邮件里的远程图片和脚本会泄露查看者信息或执行代码。
- 被删除或隐藏的邮件往往还在文件里，普通查看器看不到。
- 钓鱼邮件需要离线查看 SPF、DKIM、DMARC、投递路径与相似的仿冒域名。
- 跨 Windows、macOS、Linux 的统一工具很少，且很多依赖第三方库。

## 核心功能（依据 README，注明来源）
- 以只读方式打开多个 PST/OST 分片并合并成一个邮箱视图，重复项会被标记（来源：README Several PST files as one）
- 离线邮件头分析，覆盖仿冒域名、SPF/DKIM/DMARC、投递路径延迟、垃圾邮件判定，可按文件夹按风险排序（来源：README Header analysis）
- 恢复仍留在文件里且通过校验的已删条目和旧版本，方法沿用 libpff 的 pffrecover；隐藏项自动显示（来源：README Hidden and deleted items）
- pstx 提供 tree、list、show、search、triage、headers、props、hash、verify、hidden、recover、analyse 等子命令，退出码 0 或 1（来源：README Download）
- 可把 OST、MBOX 或多个 PST 分片写出为单个 Unicode PST，并导出 msg、eml 与带源 SHA-256 的 HTML/CSV 报告（来源：README Formats）

## 使用场景
- 邮件取证：只读检查 PST/OST 证据副本并恢复已删条目
- 批量分析可疑邮件的头部，给整个文件夹做风险排序
- 把 Google Takeout、Thunderbird 或 Apple Mail 的 MBOX 转成 Outlook 可读的 PST
- 在无网络的浏览器版本里本地查看邮箱，文件不上传

## 适用人群
数字取证人员、安全研究者，以及需要跨平台读旧邮箱或做格式迁移的个人。不适合需要修改邮箱、处理 ANSI 老格式或高加密 PST 的场景。

## 注意事项
- Windows 需要 WebView2；macOS 11 以上且未公证，需手动去掉 quarantine 属性；Linux 需要 GTK 4.12 以上与 WebKitGTK 6.0。
- 作者明确说 PST 写出器虽遵循 MS-PST 并用自带读取器验证，但没在 Outlook 里测过，遇到问题应报告。
- 仓库没有 LICENSE 文件，使用前需确认授权：许可证：未核实（仓库无 LICENSE 文件，GitHub API 未标注许可证）。
- 0 star、刚入库，尚无社区验证；已有 v0.7.0 Release。
- 构建需要 Zig 0.17.1 以上，CI 会在三平台构建并比对 pstx 输出与导出文件的一致性。

## 总结
从只读保证、安全渲染、隐藏与已删条目恢复到离线头分析和跨格式导出，这个项目把取证查看器该有的边界想得很细，还配了 fuzz 与 CI。但 0 star、许可证未核实，加上 PST 写出未经过 Outlook 实测，目前更适合当作技术参考或自行验证后使用。

## 独立评分（满分 10）

- **质量**：7 — 跨三平台、无第三方依赖且带 fuzz 与 CI，但 0 star、PST 写出未对接 Outlook 验证
- **学习价值**：7 — 能学到只读取证查看器如何做到不碰源文件、安全渲染与恢复已删条目
- **使用价值**：6 — 对要做邮件取证或格式转换的人很实用，但许可证未核实
- **推荐等级**：**C**
