# konarkmodi/pi-doubleword

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/konarkmodi/pi-doubleword
- **分类**：ai-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：JavaScript
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：0 / 0
- **许可证**：MIT License（LICENSE 文件确认；GitHub API 标 NOASSERTION）
- **创建时间**：2026-11-13
- **最近更新**：2026-11-13（元数据 updated_at；最近提交 2026-11-13）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：把 pi 编码代理接到 Doubleword 推理 API 的扩展与配套基准指南

## 一句话理解
一份给 pi 编码代理接 Doubleword 推理 API 的配置指南，附带一个单文件扩展和几个可复现的基准脚本。

## 通俗解释
README 实质是一篇长教程：把 doubleword.ts 放进 pi 的扩展目录，启动时拉取带价格的模型目录，默认用更便宜的异步 flex 档，并修复一个会让系统提示词静默丢失的 developer 角色问题。README 较长但结构清楚；项目只有 0 star，刚入库，尚无社区验证。文中所有价格与延迟数字均为自述、未独立核实。

## 它不是什么
1. 不是通用 LLM 网关或代理框架，只服务 pi 与 Doubleword 这一对组合。
2. 不是一个开箱即用的客户端，核心只是一个需手动放置的 .ts 扩展文件。
3. 不是 Doubleword 官方项目，只是第三方使用说明。
4. 不是经过同行评审的性能研究，基准脚本样本很小。
5. 不是稳定发布物，没有 GitHub Release（返回 404）。

## 痛点
- pi 默认模型列表写死，价格与可用模型需要手动维护。
- 异步 flex 档更便宜但默认没有开启。
- 某些 OpenAI 兼容端点对 developer 角色返回 HTTP 200 却不执行指令，导致技能与规则静默消失，极难发现。
- 缓存命中和档位价格都会影响实际账单，缺少可复现的测量手段。

## 核心功能（依据 README，注明来源）
- 启动时从 /v1/models?include=pricing 拉取全部 18 个聊天模型并把实时单价映射到 pi 的费用字段（来源：README What you get at the end）
- 默认给每个模型打上 service_tier: "flex"，可用 /doubleword-mode 或环境变量切回 realtime，切换即时生效（来源：README Step 3 — Pick a default model and flex path）
- 用 compat: { supportsDeveloperRole: false } 把指令改走 system 角色，并提供技能检查与 PINEAPPLE-42 暗号检查确认系统提示词真的送达（来源：README Step 4 — Verify instructions and skills actually arrive）
- 附带 flex-bench.mjs、dev-role-probe.mjs、agentx-cache-bench.mjs 三个零依赖脚本，分别测档位成本延迟、指令送达与缓存和档位的关系（来源：README Reproduce the numbers）
- 密钥兼容环境变量、models.json 与 pi 的 /login 三种存放方式，按固定顺序解析（来源：README Step 1 — Get a key and drop it where pi keeps keys）

## 使用场景
- 已经用 pi 编码代理并打算把 Doubleword 当日常后端的人
- 想对比 realtime 与 flex 档在真实 agent 任务上的成本与首 token 延迟
- 怀疑某个 OpenAI 兼容端点吞掉了 system 或 developer 指令，需要快速判定
- 想观察 prompt 缓存命中是否跨档位共享

## 适用人群
熟悉命令行、自己管理 API 密钥的 pi 用户；对 prompt 缓存与计费细节感兴趣的个人开发者。不适合想要一键安装图形界面或跨多家供应商通用方案的人。

## 注意事项
- 所有 flex 折扣、TTFT、缓存节省比例均为自述、未独立核实，README 也明确写了单模型、单时段、小样本。
- README 自述所测端点上 GLM 不遵循 developer 角色而 Qwen 直接返回 HTTP 400；“OpenAI 兼容”只描述请求形状，不保证语义一致。
- 自定义 DOUBLEWORD_BASE_URL 意味着把密钥发给该端点，只应填可信主机。
- 许可证：MIT License（LICENSE 文件确认；GitHub API 标 NOASSERTION）。
- 0 star、刚入库，尚无社区验证；无 Release（/releases/latest 返回 404）。

## 总结
一篇质量高于项目规模的配置指南：真正有价值的不是那个扩展文件，而是把 developer 角色静默失效这件事用可复现的暗号检查钉死。但它绑定 pi 与 Doubleword，0 star、无 Release、无社区验证，只适合恰好踩在这个组合里的用户按图索骥，数字务必自己重跑。

## 独立评分（满分 10）

- **质量**：6 — 单个 TypeScript 扩展加几份基准脚本，思路扎实但无社区验证（0 star）
- **学习价值**：6 — 展示了模型目录热加载、flex 计费与 developer 角色静默失效的排查，值得读
- **使用价值**：5 — 只适合用 pi 加 Doubleword 的人，通用性很窄
- **推荐等级**：**C**
