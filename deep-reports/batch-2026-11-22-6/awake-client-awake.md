# awake-client/awake

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/awake-client/awake
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Go
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：10 / 1
- **许可证**：GPL-3.0
- **创建时间**：2026-11-04
- **最近更新**：2026-11-20（元数据 updated_at；最近提交 2026-11-20）
- **最新 release**：Awake 0.9.2（tag `v0.9.2`，发布于 2026-11-20）
- **一句话简介**：把 API 请求存成 git 仓库里的 YAML 文件的跨平台客户端，团队用 git 协作、密钥留在本机钥匙串

## 一句话理解
一个把请求、集合和环境都写成普通 YAML 文件放进 git 仓库的 API 客户端，团队协作靠 pull request，而不是云端账号。

## 通俗解释
Awake 是 Bruno / Insomnia / Postman 的替代品，用 Go 与 Wails v3 写成，支持 macOS、Linux、Windows。它的核心主张是「文件即真相」：每个请求一个可读 YAML 文件，diff 可审、历史就是 git 历史；环境变量可共享并本地覆盖，密钥存在操作系统钥匙串里、不进仓库。它还带一个本地 MCP server，让 Claude Code 或 OpenCode 直接列出、发送、修改请求。README 约 3.7 千字符，功能与安装写得很紧凑，10 star，版本 v0.9.2，仍在 1.0 之前。它明确写着「无账号、无遥测、无付费计划」，许可为 GPL-3.0-or-later。

## 它不是什么
1. 不是云端协作平台：没有账号、没有同步服务，协作完全通过 git。
2. 不是 Postman 兼容层：能导入导出 Bruno/OpenCollection/OpenAPI/Postman，但内部格式是自己的 YAML。
3. 不是命令行工具：虽然提供 awake 二进制与 MCP server，主体是桌面 GUI。
4. 不是 OpenAPI 设计器：它执行请求并查看响应，不负责生成规范文件（README 只提导入导出）。
5. 不是纯 Linux 轻量应用：Linux 端需要 GTK4 与 WebKitGTK 6.0（Ubuntu 24.04+、Debian 13+ 或同等版本）。

## 痛点
- API 集合存在私有云账号里，离职交接与审计困难
- 请求定义改动无法像代码一样 review，团队协作靠手动同步
- 环境变量与密钥混在配置文件里，容易误提交
- 想让编码代理帮忙改请求，却缺少安全的本地接口

## 核心功能（依据 README，注明来源）
- 文件即真相：每个请求一个可读 YAML 文件，带可审查 diff 与普通 git 历史（来源：README Features）
- 协议与认证：HTTP、SSE、WebSocket，支持参数、头、basic/bearer/API key/OAuth 2、请求体与请求脚本（来源：README Features）
- 环境与密钥：共享变量加本地覆盖，密钥留在操作系统钥匙串、永不进仓库（来源：README Features）
- 编码代理集成：与 Claude Code 或 OpenCode 对话以重命名请求、抽取 base URL，或通过本地 MCP server 让代理列出、发送、写入请求（来源：README Features）
- 导入导出：Bruno、OpenCollection、OpenAPI、Postman（来源：README Features）
- 性能向交互：虚拟化树、SQLite 索引、搜索、命令面板、请求历史、明暗主题（来源：README Features）
- 安装校验：安装脚本下载后校验 SHA-256，macOS 清 quarantine 标记，Linux 添加应用菜单项（来源：README Install）

## 使用场景
- 后端团队把 API 集合当代码管理，用 PR 审查请求变更
- 在多个环境（dev/staging/prod）间共享变量，同时把 token 留在各自机器钥匙串
- 让 Claude Code 通过 MCP 批量重命名或抽取 base URL
- 从 Postman 或 Bruno 迁移到 git 原生工作流

## 适用人群
重视可审计性与 git 工作流、希望 API 定义与代码同库管理的开发团队；也适合需要在本机安全保存密钥、并愿意使用编码代理操作请求的重度 API 用户。

## 注意事项
- 10 star、1 fork，v0.9.2，仍预 1.0，社区验证有限
- GPL-3.0-or-later，若打算二次分发需遵守 copyleft
- Linux 依赖 GTK4/WebKitGTK 6.0，旧发行版可能装不上
- macOS 构建为 ad-hoc 签名、未公证，首次启动需手动放行
- 开发需要 Go 1.26+、Node.js 22+ 与 Wails v3 CLI
- README 自述 built to stay fast，但无独立基准数据

## 总结
Awake 的价值在取舍：把 API 客户端最容易被忽视的协作与密钥问题，用「YAML 进 git + 密钥进钥匙串」两个朴素决定解决掉，再补上 MCP 让代理也能参与。它功能覆盖 HTTP/SSE/WebSocket、多种认证与主流格式导入，方向清晰，但 10 star、v0.9.2 与 GPL 许可意味着它仍是需要评估维护风险的早期工具，适合愿意跟随其成长的 git 中心团队。

## 独立评分（满分 10）

- **质量**：7 — Git 原生 API 客户端思路清晰，YAML 文件即真相；但 v0.9.2 仍预 1.0、10 star
- **学习价值**：6 — 展示如何把集合、请求、环境与密钥拆成 YAML 加 keychain，并用本地 MCP 接编码代理
- **使用价值**：7 — 团队用 git 审查 API 定义，无需账号或付费计划，支持 HTTP/SSE/WebSocket 与多格式导入
- **推荐等级**：**B**
