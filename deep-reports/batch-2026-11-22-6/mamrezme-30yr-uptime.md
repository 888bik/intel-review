# mamrezme/30yr-uptime

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/mamrezme/30yr-uptime
- **分类**：productivity（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：TypeScript
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：2 / 0
- **许可证**：MIT
- **创建时间**：2026-11-08
- **最近更新**：2026-11-14（元数据 updated_at；最近提交 2026-11-09）
- **最新 release**：v0.1.0（tag `v0.1.0`，发布于 2026-11-09）
- **一句话简介**：把等 Claude 的时间变成 16 种工位微运动的 Claude Code 插件，像素小人演示，纯本地不耗 token

## 一句话理解
一个 Claude Code 插件：每当你等它跑长任务，就用像素小人演示一段颈肩腰腕眼的小运动。

## 通俗解释
30yr-uptime 是一个 Claude Code mod。默认每 45 分钟，若下一次主回合超过 15 秒，就在提示符上方弹出一个休息动作，由像素角色演示颈部、肩背、髋、腕、眼或呼吸的练习，按钮是 Done / In 5 min / Skip。全部在终端里完成：不发网络请求、不调模型、不用 token，也不读你的 prompt、文件与代码。README 约 7.5 千字符，把 16 个动作、命令、设置、hooks 与隐私边界写得非常细。项目 2 star、v0.1.0，属于很新的小插件；角色是 Clawd 的非官方粉丝演绎，作者声明与 Anthropic 无关。

## 它不是什么
1. 不是独立健身 App，只是终端里的一层显示与计时。
2. 不是 Claude 官方插件，README 明确写明非 Anthropic 制作或背书。
3. 不是健康建议来源，动作说明是作者自述，未独立核实医学有效性。
4. 不是 AI 功能：不调用模型、不生成文本，所有文案写在插件里。
5. 不是跨编辑器工具：依赖 Claude Code 2.1.287+ 的 mod 机制，测试于 2.1.319。

## 痛点
- 等长回合时久坐不动，颈肩和眼睛疲劳
- 通用提醒器不了解「代理正在跑长任务」这一时机
- 担心健康插件上传数据或消耗模型额度

## 核心功能（依据 README，注明来源）
- 16 种工位动作：站起、喝水、走路、20-20-20 眼休、盒式呼吸、收下巴、肩胛夹紧、椅背伸展、坐姿扭转、腕部拉伸、深蹲、弓步、臀踢腿、腘绳肌拉伸、小腿拉伸、髋屈肌拉伸，均可站或坐完成（来源：README The moves）
- 像素动画与进度：角色按部位特写演示，计次、倒计时与进度条；随机抽取已开启动作，不连续重复同一个（来源：README The moves）
- 三种响应：Done / In 5 min / Skip；终端窄于 54 列退化为文字与按钮，并尊重 NO_COLOR（来源：README 开头说明与 The moves）
- /break 命令：立即开始、开关提醒、查看设置与动作、/break set 改键值、/break reset 重置（来源：README The /break command）
- 可调默认：waitSeconds=15、intervalMinutes=45，每个动作可 on/off；全部关闭即不再提醒（来源：README Settings）
- hooks 只监听六类事件（session.start、command.run for /break、turn.start、turn.complete、ui.render on Spinner、ui.render on AbovePrompt），只绘制、不改变 Claude 看到或发送的内容（来源：README What the hooks do）
- 隐私：无网络、无模型调用、不读 prompt/回复/文件/代码；只存 paused 与 settings 两项（来源：README Privacy: what data it sends）

## 使用场景
- 长时间让 Claude 跑测试、重构或批量改动时被动做微运动
- 想手动插入一次休息时运行 /break
- 关闭深蹲等不适合工位的动作，只保留眼休与肩颈

## 适用人群
长时间在终端等待编码代理、又在意工位人体工学的开发者；适合愿意装小插件、且能接受 2 star 早期项目维护不确定性的用户。

## 注意事项
- 2 star、0 fork，v0.1.0，刚入库，社区验证几乎没有
- 需 Claude Code 2.1.287+，mod 默认开启；动画需终端至少 54 列
- 非官方、非 Anthropic 背书，角色为粉丝演绎
- 动作有效性为 README 自述，未独立核实，身体不适请自行判断
- 0.1.0 早期版本，设置与 hooks 行为可能变动

## 总结
30yr-uptime 把「等代理」这段时间做成了一个不打扰、不联网、不烧 token 的健康小钩子；文档把动作、设置、hooks 与隐私边界写得比很多大项目还清楚。但它的能力边界非常窄，2 star、0.1.0，角色还是非官方二创。适合把它当作一个轻量实验性插件试用，不宜期待长期维护或医学背书。

## 独立评分（满分 10）

- **质量**：6 — 思路轻巧、零网络零 token，但只是 2 star 的 Claude Code 小插件，功能边界很窄
- **学习价值**：6 — 可看到 Claude Code 插件 hooks 如何只监听回合事件、在 spinner 上叠加动画而不干预模型
- **使用价值**：6 — 长时间等待时强制做 16 种工位微运动，纯本地、不消耗 token
- **推荐等级**：**C**
