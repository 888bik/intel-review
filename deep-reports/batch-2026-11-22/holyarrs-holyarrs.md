# HolyArrs/HolyArrs

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息

- **仓库地址**：https://github.com/HolyArrs/HolyArrs
- **分类**：self-hosted（据 zhao 情报库分类；原始库未核实）
- **语言**：Go
- **评分**：原始评分未核实；本报告独立评分见文末
- **Star / Fork**：2,670 / 176
- **许可证**：AGPL-3.0
- **创建时间**：2026-11-09
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-19）
- **最新 release**：1.2.0（发布于 2026-11-17）
- **一句话简介**：一个自托管应用，把剧集、电影、音乐的“请求—搜索—下载—入库—通知”整条媒体自动化链路整合到单一 Web 界面中。

## 一句话理解

HolyArrs 想用一个 App 取代你 NAS 上那一整套 Sonarr + Radarr + Prowlarr + Bazarr + Overseerr + qBittorrent 的“全家桶”：你点想要什么，它负责找、下、改名、归库并通知媒体服务器。

## 通俗解释

玩过家庭影院（homelab）的人都知道，搭建自动追剧系统要装一堆工具：Radarr 管电影、Sonarr 管剧集、Prowlarr/Jackett 管索引源、qBittorrent 下载、Bazarr 抓字幕、Overseerr 做请求审批……每个都要单独配置、互不打通。HolyArrs 的野心是把这些能力合并进一个统一的自托管应用：在一个像流媒体一样好看的界面里浏览、点“想要”，它按你定的 Pick rules（质量/大小/语言）挑最佳种子，交给下载器，字幕一并抓取并跟文件放一起，完成后让 Plex/Jellyfin/Emby 刷新，还能通过 Discord/Telegram/邮件等发通知（以上均来源：README “Ask for a title…” 表格）。它还能从旧工具导入配置，让你平滑迁移。注意这是 2026-11 才创建的新项目，仍在快速迭代。

## 它不是什么

1. **不是下载器本身**。README 明确“Doesn't replace downloaders”——它连接 qBittorrent、SABnzbd 等，自己不下载（来源：README “What it doesn't do yet”）。
2. **不是媒体服务器**。它是 Plex/Jellyfin/Emby 的“上游自动化”，不替代它们播放内容（来源：README “works with what you already run”）。
3. **不是有手机 App 和 TV App 的成熟产品**。README 明确“Phone and TV apps. Not built.”，只有 Web 界面（来源：README）。
4. **不是音乐中心**。README 说“Music in Explore is still being built”，音乐探索尚在开发（来源：README）。
5. **不是需要你自己配一堆 API key 才能显示海报**。README 称标题、封面、图表来自 HolyArrs 自己的元数据服务器，当然也支持你用自己的 key/服务器（来源：README 表格）——但这条是项目自述，未独立核实。

## 痛点

- **“Arr 全家桶”配置繁琐**：多个应用、多个端口、多个 API key 互相填来填去；HolyArrs 用单应用降低运维复杂度。
- **迁移成本高**：已有 Sonarr/Radarr 等大量规则，重新配置痛苦。README 提供从 Sonarr/Radarr/Lidarr/Prowlarr/Bazarr/Seerr/Overseerr/qBittorrent/SABnzbd 的导入，且“只读取不改旧应用、可撤销”（来源：README “Bring your old setup with you”）。
- **请求审批分散**：朋友/家人请求内容要跨平台管理；HolyArrs 内置 Requests 和审批、信任机制（来源：README）。
- **字幕/通知割裂**：字幕与媒体文件分离、通知靠额外脚本；HolyArrs 把字幕随文件保留并内置多通道通知（来源：README 表格，项目自述）。

## 核心功能（依据 README，注明来源）

- **统一媒体请求与自动化**：搜索 Sources、按 Pick rules 选版本、发到下载器、归库、通知媒体服务器（来源：README “Ask for a title…” 段落）。
- **Explore / Requests / Pick rules / Calendar / Alerts**：类流媒体浏览（悬停播预告）、整剧或单季请求与审批、按大小/质量/语言定规则、追剧日历、Discord/email/Telegram/ntfy/Gotify/Pushover/webhook 通知（来源：README 表格）。
- **兼容现有生态**：下载器 qBittorrent/Transmission/Deluge/rTorrent/aria2/SABnzbd/NZBGet；媒体服务器 Plex/Jellyfin/Emby；来源 Torznab/Newznab/Prowlarr/Jackett（来源：README “works with what you already run”）。
- **旧配置导入**：从 Sonarr/Radarr/Lidarr/Prowlarr/Bazarr/Seerr/Overseerr/qBittorrent/SABnzbd 导入库目录、标题、质量配置、排除项、索引器、下载器、字幕语言、请求等，且可撤销、不改旧应用（来源：README 导入表格）。
- **安全与轻量**：密钥/密码用配置目录中的 master key 加密；镜像约 16.1 MB（来源：README 表格；镜像大小为项目自述，未独立核实）。
- **旧应用 API 兼容**：开启后可继续用 Seerr、Prowlarr、Bazarr、Unpackerr、Homepage 的 API（来源：README 表格）。
- **Docker 部署**：提供 docker compose（推荐）与 docker CLI 两种运行方式，支持多架构（来源：README “Usage”）。

## 使用场景

- 已有 Plex/Jellyfin + 下载器、想简化“Arr 全家桶”的 homelab 玩家。
- 想为家人/朋友提供统一“点播请求”入口的私人媒体管理员。
- 希望从旧 Arr 工具迁移、但不想推倒重来的人。

## 适用人群

自托管/NAS 玩家、家庭影院管理员。不适合只想要“一个能播放的 App”的普通用户。

## 注意事项

- **极新项目**：2026-11-09 创建，274 个 open issues，说明功能多但不稳定，生产环境需谨慎备份。
- AGPL-3.0 许可证：若做公开网络服务对外提供，需注意 AGPL 的源码开放义务。
- README 强调 “Keep your master key safe”——加密密钥丢失可能无法恢复已存密码（来源：README 小节标题）。
- 多个功能指标（16.1MB 镜像、元数据服务器免 key、字幕抓取效果）为项目自述，未独立核实。

## 总结

HolyArrs 是一个“整合型挑战者”：用单应用替代一整排 Arr 工具，设计和导入迁移思路都很讨巧。但它太新、issue 太多，适合愿意尝鲜和测试的 homelab 用户，不建议作为稳定生产环境唯一依赖。等它再打磨几个月，值得重新评估。

## 独立评分（满分 10）

- **质量**：7 — 功能设计完整、README 精良，但项目仅两周龄、274 个未解决 issue，稳定性存疑。
- **学习价值**：7 — 自托管媒体自动化整合架构、迁移导入设计有参考价值。
- **使用价值**：8 — 对 Arr 全家桶用户痛点明确；新项目风险扣分。
- **推荐等级**：**B**（方向很好、值得关注，但当前阶段建议观望或仅试验使用）。
