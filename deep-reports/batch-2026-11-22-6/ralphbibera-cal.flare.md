# ralphbibera/cal.flare

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/ralphbibera/cal.flare
- **分类**：self-hosted（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：TypeScript
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：11 / 2
- **许可证**：MIT
- **创建时间**：2026-11-03
- **最近更新**：2026-11-21（元数据 updated_at；最近提交 2026-11-18）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：把开源排期应用 Cal.diy 一比一移植到 Cloudflare Workers 与 D1 的自托管版本

## 一句话理解
把 Cal.com 的开源社区版 Cal.diy 搬到 Cloudflare Workers 与 D1 上运行：去掉企业版功能、保持全 MIT 许可，让个人可以自己掌控排期服务，但需要自己搞定 Cloudflare 的账号、绑定和数据库。

## 通俗解释
Cal.flare 的定位在 README 里写得非常直白：它是 Cal.com 社区版的再次移植，企业内部功能如团队、组织、洞察、工作流、SSO/SAML 都被移除，不需要许可证密钥，也没有官方托管版本，完全由用户自己部署。技术栈上是 vinext、Cloudflare Workers 加 D1、Bun、tRPC、React、Tailwind、Prisma 与 Daily.co；Prisma 仍作为数据访问层，通过官方 D1 适配器工作，原有的多行原子操作改用 D1 的受保护批处理。

README 的篇幅主要花在部署与配置上：需要 Bun 1.4.4、Node 24.22.0、Cloudflare Workers 付费计划以及本地 workerd/D1；安装依赖、准备 .dev.vars、生成 NEXTAUTH_SECRET 与 CALENDSO_ENCRYPTION_KEY、绑定并迁移 D1、创建首个用户或跑开发种子数据，最后 bun run dev。文档还给出开发测试账号、日志级别配置、Playwright 端到端测试与 MailHog 邮件接收器的用法，以及从 Google Calendar、Microsoft Graph 到 Zoom、Daily 的凭据申请步骤。

值得注意的是开头的警告：README 明确要求仅用于个人、非生产用途，并称自托管需要 Cloudflare 管理、D1 管理与敏感数据保护的高级知识；商业与 enterprise 场景应使用 Cal.com。换句话说，这是一个把大型应用搬到边缘运行时的技术验证与自托管选项，不是零风险的开箱即用产品。项目 11 star、2 fork，尚无 release，最近提交是在整理邮件绑定与移植记录。

## 它不是什么
1. 不是 Cal.com 官方产品，作者只是社区贡献者，README 引导商业用户回到 Cal.com。
2. 不是功能完整的 Cal.com 替代品，团队、组织、SSO、工作流等企业功能被刻意移除。
3. 不是托管服务，没有官方云实例，必须自己在 Cloudflare 上部署。
4. 不是生产就绪方案，README 明说严格建议个人非生产使用。
5. 不是简单的配置模板，部署涉及 Workers 付费计划、D1 迁移、密钥管理和多个 OAuth 应用注册。

## 痛点
- 想把排期服务完全掌控在自己手里，但不想依赖商业 SaaS 或企业许可证。
- 传统 Node 或 PostgreSQL 自托管方案需要长期运行服务器，运维成本高。
- 上游 Cal.diy 的 Docker/PostgreSQL 工作流无法直接用于 Cloudflare Workers 环境。
- 边缘运行时下，数据库迁移、邮件发送与环境变量注入都需要重新设计映射关系。
- 缺少一份把 Workers 绑定、D1、Prisma 和 OAuth 回调串起来的完整操作手册。

## 核心功能（依据 README，注明来源）
- 将 Cal.diy 移植到 Cloudflare Workers 与 D1，保留原有排期、事件类型与账号体系（来源：README About Cal.flare）
- 移除全部企业版功能，代码库保持 100% MIT、无 Open Core 拆分（来源：README What is different from Cal.com 与 License）
- 使用 vinext、Bun、tRPC、React、Tailwind、Prisma 与 Daily.co 技术栈，并附移植计划、架构与清单文档（来源：README Built With）
- Prisma 通过官方 D1 适配器访问数据库，多行原子操作使用原生受保护 D1 批处理（来源：README Built With 段落）
- 本地开发流程与种子数据：.dev.vars、D1 迁移、首个用户创建或 db-seed:d1 脚本（来源：README Development 与 Setting up your first user）
- 部署与运行时绑定说明：DB、ASSETS、EMAIL、缓存绑定、可选 EDGE_CONFIG 与 Trigger.dev 任务器（来源：README Deployment 与 Configuration）
- 集成配置指南，覆盖 Google Calendar、Microsoft Graph、Zoom 与 Daily 的凭据申请（来源：README Integrations）
- Playwright 端到端测试与 MailHog 邮件接收器配置（来源：README E2E-Testing 与 Development mail receiver）

## 使用场景
- 个人在 Cloudflare 免费或付费套餐上搭建自己的预约页面。
- 想研究如何把一个大型 Next.js/Prisma 应用迁到 Workers 与 D1 的工程师。
- 需要把邮件、日历集成与 OAuth 回调全部跑在边缘的轻量部署实验。
- 在没有长期在线主机的情况下提供一个公开的排期入口。

## 适用人群
熟悉 Cloudflare Workers、D1 与 Bun 工具链，且有耐心处理迁移与凭据配置的自托管爱好者；以及对边缘运行时移植感兴趣的开发者。不想碰 Workers 付费计划或 OAuth 注册的人不适合。

## 注意事项
- README 开头即警告：仅限个人、非生产使用，自托管需要高级 Cloudflare 与 D1 知识。
- 部署需要 Cloudflare Workers 付费计划，且邮件默认走 Cloudflare Email Sending，要求已验证的发送域名。
- 开发种子账号仅用于本地，切勿带进生产数据；仓库无 release，11 star、2 fork，社区验证有限。
- 环境变量分运行时绑定与构建时变量，改动公开构建值后需要重建客户端资源。
- Workers 没有本地 sendmail，外部 SMTP 中继必须使用公网 TLS 或 STARTTLS。

## 总结
Cal.flare 做的是一件工程量不小的搬运工作：把 Cal.com 的开源版本改成能跑在 Workers 与 D1 上的自托管服务，同时用 MIT 许可移除所有企业功能。README 详尽到近乎操作手册，覆盖开发、迁移、部署、排错与各种日历集成，学习价值高于直接使用价值。但作者自己把适用范围限定在个人非生产，加上无 release、11 star 的早期体量，它更适合作为边缘移植案例来读，或者在小范围自用场景中尝试。需要稳定商业排期基础设施的人应按 README 建议回到 Cal.com。

## 独立评分（满分 10）

- **质量**：7 — 迁移工程量大，但 README 明确只建议个人非生产使用
- **学习价值**：7 — Prisma 加 D1、Worker 绑定与迁移流程有参考价值
- **使用价值**：7 — 个人自托管排期服务的可行方案，部署门槛偏高
- **推荐等级**：**B**
