# osigurdson/mib

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/osigurdson/mib
- **分类**：productivity（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：C
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：5 / 0
- **许可证**：MIT
- **创建时间**：2026-11-08
- **最近更新**：2026-11-14（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：mib v0.1.5（tag `v0.1.5`，发布于 2026-11-22）
- **一句话简介**：约 300KiB、Vim 键位、支持内嵌图片与 OCR 搜索的单文件 Markdown 草稿本，面向 Linux Wayland

## 一句话理解
一个只有约 300KiB 的本地 Markdown 草稿本，不依赖 Electron 或 WebView，用 Vim 键位操作，还能在粘贴的截图里搜索文字。

## 通俗解释
mib 刻意不做文件管理：只有九个草稿页，用 Alt+1 到 Alt+9 切换，适合临时笔记、会议记录和截图配注，长期内容还是交给 git 或 wiki。它用 C 写成、单二进制、跑在 Wayland 上，带明暗主题和多套配色，编辑体验尽量贴近 Vim（normal/insert/visual 模式、motion、operator、寄存器、搜索与替换）。如果系统装了 Tesseract，还能对粘贴的图片做 OCR，让搜索命中截图里的文字。

## 它不是什么
1. 不是 Vim 或 Neovim 的替代品，没有 vimrc、插件、宏和块可视模式
2. 不是笔记知识库，官方定位是工作草稿、不是归档
3. 不是跨平台应用，没有 X11 支持，Windows 版仍是实验性
4. 不是 Electron 套壳，是原生单二进制、体积约 300KiB

## 痛点
- 记几条临时笔记却要启动笨重的编辑器或笔记应用
- 截图里的文字搜不到，事后找不到信息
- 喜欢 Vim 键位但不愿为随手记录配置整套环境

## 核心功能（依据 README，注明来源）
- 九个草稿页，快捷键切换与列表，无需管理文件（来源：README Nine pages）
- Vim 风格编辑：模式、motion、operator、文本对象、寄存器与替换（来源：README Using mib）
- 粘贴截图或拖入图片即插入页面，可选 Tesseract OCR 并高亮命中（来源：README Optional OCR）
- 明暗主题与多套内置配色，可切换源码、普通与阅读视图（来源：README Themes 与 Using mib）
- 笔记以普通 Markdown 存于本地目录，可用 grep、外部编辑器或 git 管理（来源：README File Organization）

## 使用场景
- 开会或调试时快速记要点、贴截图
- 在 Wayland 桌面上想要一个启动极快的轻量编辑器
- 想把截图里的错误信息纳入全文搜索

## 适用人群
偏好 Vim 键位、使用 Wayland 的 Linux 开发者；对体积与依赖敏感、想要本地纯文本笔记的人。

## 注意事项
- 仅支持 Wayland，明确无 X11 支持
- Windows 版只经 Wine 验证，未在真机试过
- 与 Vim 仍有差异：无宏、无块可视模式、替换只支持小型正则子集
- OCR 需要另装 Tesseract 与英文数据包，可用 MIB_OCR=0 关闭
- 5 star、作者更新积极，但仍属小众项目

## 总结
小而完整：单二进制、无重型依赖、文档与测试齐备，发布流程也规范。考虑到体量与受众，评 B。

## 独立评分（满分 10）

- **质量**：7 — 单二进制、约 300KiB、文档与 CI 齐全，完成度高于体量
- **学习价值**：6 — C 加 Wayland 手写编辑器、Vim 键位与 OCR 搜索，实现细节可读
- **使用价值**：7 — 轻量本地 Markdown 草稿本，适合 Wayland 桌面随手记录
- **推荐等级**：**B**
