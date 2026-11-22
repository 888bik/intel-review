# Amirjon06/iphone-linux

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/Amirjon06/iphone-linux
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Python
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：4 / 0
- **许可证**：未核实（仓库无 LICENSE 文件，GitHub API 未标注许可证）
- **创建时间**：2026-11-22
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-22）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：通过 WiFi 把 iPhone 挂载为 Linux 文件夹，支持整盘与单个 App 沙盒的读写

## 一句话理解
iphone-linux 用 WiFi 把 iPhone 变成 Linux 下的一个普通文件夹：cd 进去就能 cp 照片、echo 一个文本文件，甚至直接读写某个 App 的私有 Documents 目录。

## 通俗解释
在 Linux 上从 iPhone 拷文件，传统做法是插线加 ifuse（在现代 iOS 17 以上经常失效），或者走 iCloud 网页（慢、需要联网、只给部分目录）。这个项目借用 Xcode 无线调试用的 mobdev2 协议，通过 pymobiledevice3 连接手机，再用 FUSE 把 iPhone 文件系统暴露成一个挂载点。首次在手机上点一次“信任”后，配对记录会被保存，之后不再交互。它用 mDNS/Bonjour 自动发现同一 WiFi 里的 iPhone，也可以在路由器屏蔽 mDNS 时直接给 IP。除了整盘读写，还能用 HouseArrest 协议只挂载某一个 App 的沙盒；watch 模式会在手机休眠或离网时自动卸载、回来后自动重挂；读操作有 3 MB 预读缓存。README 写得很完整，有命令表、排错、架构说明和 13 项真机测试，但项目在 2026 年 11 月 22 日当天才创建，只有 4 star，而且没有 LICENSE 文件、没有 release。

## 它不是什么
1. 不是 iTunes 或 iCloud 的替代品，它不管理同步，只把手机存储挂成文件夹。
2. 不是无需设置的即插即用工具，iPhone 必须开启开发者模式并信任这台 Linux 机器。
3. 不是 USB 方案，它走 WiFi，要求两台设备在同一网络。
4. 不是只针对照片的导出器，它提供通用读写、单 App 沙盒和只读模式。
5. 不是成熟稳定的发行软件，没有 release，也没有明确许可证声明。

## 痛点
- iOS 17 以后 ifuse 等 USB 方案经常失效，Linux 用户缺少可靠的拷贝路径。
- iCloud 网页既慢又要求联网，还只暴露有限目录。
- 想读写某个 App 的 Documents 目录，通常要越狱或借助专用工具。
- 手机休眠或离开 WiFi 后，挂载会断掉，需要手动重连。
- 大量照片顺序读取慢，重复的小文件操作体验差。

## 核心功能（依据 README，注明来源）
- 通过 WiFi 把 iPhone 挂载为 /mnt/iPhone 的 FUSE 文件系统，支持复制照片和写文本文件（来源：README 开头与 Who This Is For）。
- 用 pymobiledevice3 的 mobdev2 协议连接，mDNS/Bonjour 自动发现 IP，AFC 暴露文件系统，配对记录一次保存（来源：README How It Works）。
- mount-app 用 Apple 的 HouseArrest 协议挂载单个 App 沙盒，可列出 bundle id 并读写其 Documents 与 Library（来源：README Per-App Sandbox Access）。
- 提供 mount、mount-ro、mount-app、watch、apps、unmount 六个命令，只读模式在文件系统层拒绝写入（来源：README Commands）。
- watch 模式用指数退避轮询，手机休眠时干净卸载、回来时自动重挂，也可装成 systemd 用户服务（来源：README Auto-Mount (Watch Mode) 与 Auto-Mount at Login）。
- 连接中断后在下次访问时自动重连并重试操作，调用方看不到报错（来源：README Reliability）。
- 3 MB 预读缓存在后台线程预取，针对大照片库和视频文件的顺序读取优化（来源：README Performance 与 How It Works）。
- 提供安装脚本创建 venv 与依赖，并说明如何配置 /etc/fuse.conf 允许非 root 访问（来源：README Install 与 Allow non-root access）。
- README 列出 13 项针对真机的测试，覆盖读、偏移读、JPEG 头校验、写入、二进制与 1 MB 写入、mkdir、rmdir、unlink、rename（来源：README Tests）。

## 使用场景
- 无数据线从 iPhone 批量拷照片和视频到 Linux。
- 用脚本、编辑器或 shell 直接读写手机上的文件。
- 查看或修改某个 App 的 Documents 与 Caches 内容。
- 手机经常休眠，希望挂载能自动恢复而不中断工作流。
- 不想依赖 Apple 账号、iCloud 或 iTunes 的环境。

## 适用人群
适合已开启开发者模式、能与手机同处一个 WiFi 的 Linux 用户，尤其是需要从手机批量取照片或调试 App 沙盒的人；普通用户若只想偶尔导几张照片，配置成本可能偏高。

## 注意事项
- 许可证按仓库解析结果照录：未核实（仓库无 LICENSE 文件，GitHub API 未标注许可证）。README 末尾自称 MIT，但仓库里没有 LICENSE 文件，两者并不一致。
- 只有 4 star，2026 年 11 月 22 日才创建，尚无社区验证；GitHub release 返回 404，只能从源码安装。
- 需要 Ubuntu 24.04 以上（作者只说该版本测过）、Python 3.10 以上、开启开发者模式的 iPhone，并与电脑处于同一 WiFi。
- 首次配对要在手机上点“信任”；若误点 dismissal，需要重置位置与隐私设置才会再次弹出。
- 部分路由器会阻断设备间 mDNS，此时必须用 iphone-linux mount 加 IP 地址。
- 13 项测试“全部在真机通过”是作者自述、未独立核实；仓库很新，异常场景覆盖有限。
- 写入操作直接作用于手机文件系统，误删或覆盖没有回收站，建议先试 mount-ro。

## 总结
iphone-linux 用现成协议拼出了一条很实用的路径：pymobiledevice3 负责 WiFi 连接，FUSE 负责把它变成文件夹，HouseArrest 负责单 App 沙盒，再补上自动重挂和预读缓存，README 也把排错和限制讲得清楚。它的问题在“工程之外的成熟度”——4 star、当天新建、没有 release，也没有可核实的 LICENSE，README 自称 MIT 却拿不出许可证文件。作为小工具值得一试，尤其适合临时导照片；真要长期依赖，建议先解决许可证与更广泛的设备验证。

## 独立评分（满分 10）

- **质量**：5 — WiFi 挂载 iPhone 的方案实用且 README 完整，但 4 star、当天新建、无 LICENSE 与 release
- **学习价值**：6 — mobdev2、AFC/FUSE 与 HouseArrest 的组合，加上断线重连和预读缓存，实现思路清晰
- **使用价值**：7 — 无数据线读写 iPhone 文件是真需求，但要求开发者模式、同网段与一次性信任配对
- **推荐等级**：**C**
