# liulewis/supabase-free-backup-lite

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/liulewis/supabase-free-backup-lite
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Shell
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：3 / 0
- **许可证**：未核实（仓库无 LICENSE 文件，GitHub API 未标注许可证）
- **创建时间**：2026-11-13
- **最近更新**：2026-11-19（元数据 updated_at；最近提交 2026-11-19）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：用 GitHub Actions 每周把 Supabase 免费库 pg_dump 到 Cloudflare R2 的极简备份工作流

## 一句话理解
一个约 30 行的 GitHub Actions 工作流：每周把 Supabase 免费层数据库 pg_dump 后传到 Cloudflare R2。

## 通俗解释
README 很短且直白：Supabase Free 没有自动备份，Pro 要每月 25 美元，这个 lite 版只做一件事——定时导出数据库并上传对象存储，明确声明不做加密、不做恢复演练、不做告警、不做校验。它同时是一份付费完整版的引流页。3 star，刚入库。

## 它不是什么
1. 不是完整备份方案，README 明说没有加密、没有恢复演练、没有校验和告警。
2. 不包含 Storage 文件，那些在 S3 而非 Postgres 里。
3. 不是一个可安装的库或 CLI，只是需要复制到自己仓库的 workflow 文件。
4. 不是官方 Supabase 项目，也不是免费版的长期维护承诺。
5. 没有 Release（返回 404）。

## 痛点
- Supabase Free 层不提供自动备份，升级 Pro 每月 25 美元。
- 连接串选错端口就会失败：5432 的 Session pooler 可用，6543 的 transaction pooler 跑不了 pg_dump，直连主机在免费层是纯 IPv6 而 GitHub runner 是 IPv4。
- 出网有每月 5GB 上限，dump 频率一高就超额。
- 公开仓库的定时任务会在 60 天不活动后被静默禁用。

## 核心功能（依据 README，注明来源）
- 每周定时执行 pg_dump 并把产物上传到 Cloudflare R2 的 supabase-backups 日期路径（来源：README Backup Supabase Free Tier — Free Lite Version）
- 包含 auth.users 在内的整库导出（来源：README Scope）
- 给出 10 分钟配置步骤：建私有仓库、建 R2 桶与令牌、填五个 Secrets、用 Session pooler 连接串（来源：README Setup (10 minutes)）
- 提供出网估算提醒与外部 Supabase Backup Planner 链接（来源：README Egress warning）

## 使用场景
- 个人项目或副业在用 Supabase Free，想要最低成本的灾难兜底
- 每周一次全库导出到自己的 R2 桶即可满足的轻量需求
- 想验证自己的出网量与保留策略是否落在 5GB 免费额度内

## 适用人群
预算敏感、能自己配 GitHub Secrets 和 R2 的个人开发者。需要合规、加密、可验证恢复流程的团队应直接看别的方案。

## 注意事项
- README 自述的出网估算（500MB 库每周约 2GB 每月、每天约 15GB 每月）是推算而非实测，需按自己库大小核算。
- dump 未加密上传，R2 桶默认私有但任何持有凭据的人都能读取。
- README 明确把加密、预检、校验、恢复演练、告警、cron 看门狗放到付费完整版（29 美元首发、39 美元标准），本项目本身是引流入口。
- 许可证：未核实（仓库无 LICENSE 文件，GitHub API 未标注许可证）。
- 3 star、刚入库，尚无社区验证；无 Release（/releases/latest 返回 404）。

## 总结
把“免费层没有备份”这件事用一个最小工作流补上，读它主要能学到 pooler 端口和 IPv6 这两个坑。但它刻意不做加密与恢复验证，还明确把关键能力放在付费版里，适合当临时兜底而不是正式备份制度。

## 独立评分（满分 10）

- **质量**：5 — 约 30 行的 GitHub Actions 工作流，能跑但刻意不做加密与校验
- **学习价值**：5 — 把 Supabase Free 的备份盲区、pooler 端口陷阱和 5GB 出网限制讲得很清楚
- **使用价值**：7 — 对免费层用户很实用，但只是最小兜底，且明确引导购买付费完整版
- **推荐等级**：**C**
