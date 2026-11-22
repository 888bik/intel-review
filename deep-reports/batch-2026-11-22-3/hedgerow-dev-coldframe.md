# hedgerow-dev/coldframe

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/hedgerow-dev/coldframe
- **分类**：developer-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Python
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：78 / 1
- **许可证**：MIT
- **创建时间**：2026-11-05
- **最近更新**：2026-11-22（元数据 updated_at；最近提交 2026-11-15）
- **最新 release**：Carel pCOWeb persona（tag `v0.1.0`，发布于 2026-11-13）
- **一句话简介**：一个低交互蜜罐，伪装成超市制冷控制器（Danfoss AK-SM 800A、Dixell XWEB 500），专门吸引并识别针对工控设备的攻击。

## 一句话理解

它像一个“假制冷机组”：放在网上装成超市冷柜控制器，攻击者来打时，它记录攻击手法并识别已知 CVE，帮你了解工控威胁而不碰真实设备。

## 通俗解释

工控系统（ICS）一旦被攻击后果严重，但真实设备不能拿来当靶子。蜜罐是“假装成真实系统的诱饵”。coldframe 伪装成超市制冷控制器——Danfoss AK-SM 800A 与 Dixell XWEB 500（来源：仓库描述），用 Python 写的低交互蜜罐。它定义“personas”（角色）：模拟真实设备外观，Danfoss/Dixell 角色还能选配 Modbus TCP 服务（伪造case温度、设定值、除霜寄存器），默认关闭（因为真实 AK-SM 800A 只用 Modbus 跟自己的case控制器通信），打开可捕获通用工业扫描器（来源：README “Looking like a real unit”）。它能识别一批公开漏洞利用并在日志里打 exploit 字段：CVE-2023-25914（路径穿越）、CVE-2023-25915（命令注入）、CVE-2025-41452（nginx header 覆盖）、CVE-2025-41450（code-of-the-day 登录绕过）、CVE-2021-45420（XWEB 500 上传/命令路径）、CVE-2026-25085（XWEB Pro 登录绕过）等（来源：README “Exploits it recognises”）。启动只需一行环境变量命令，请求以 JSON 行输出到 stdout 与 events.jsonl（来源：README Quick start）。

## 它不是什么

1. **不是真实工控设备或模拟器**。它是低交互蜜罐，只伪装外观与少量协议（来源：README）。
2. **不是通用 Web 蜜罐**。它专攻超市制冷控制器这类 ICS 设备（来源：仓库描述）。
3. **不是高交互**。README 明说 low-interaction；Modbus 默认关闭（来源：README）。
4. **不是生产防护工具**。它用于威胁情报/研究，不保护真实系统（来源：README 定位）。
5. **不是大项目**。78 star、v0.1.0、2026-11 创建（来源：GitHub）。

## 痛点

- **ICS 威胁情报稀缺**：针对制冷控制器的攻击缺乏低成本观测手段。
- **真实设备不能当靶子**：蜜罐提供安全的诱捕环境。
- **已知 CVE 识别**：自动给攻击打标签，省去人工分析。
- **部署简单**：一行命令启动，JSON 日志便于接入分析管线（来源：README Quick start）。

## 核心功能（依据 README，注明来源）

- **多设备 persona**：Danfoss AK-SM 800A、Dixell XWEB 500，另有 Carel pCOWeb（release 名为 “Carel pCOWeb persona”）（来源：README 与 release）。
- **可选 Modbus TCP**：伪造温度/设定值/除霜寄存器，默认关闭（来源：README）。
- **CVE 识别**：匹配公开 exploit 时日志加 exploit 字段，列出多个 CVE 与绕过手法（来源：README “Exploits it recognises”）。
- **JSON 日志**：请求以 JSON 行写到 stdout 与 events.jsonl（来源：README Quick start）。
- **自测**：test_honeypot.py 在本地环回启动并攻击自己验证（来源：README Quick start）。
- **文档**：Personas、Exploits、Deploying、Configuration、Safety、Limitations、Sources 等章节（来源：README headings）。

## 使用场景

- ICS/OT 安全研究者收集针对制冷控制器的攻击情报。
- 企业 SOC 做威胁诱捕与攻击手法分析。
- 学术研究与蜜罐技术实验。

## 适用人群

ICS/OT 安全研究员、威胁情报团队、蜜罐爱好者。普通开发者用不上。

## 注意事项

- MIT 许可。
- 蜜罐部署需注意自身安全与法律合规（README 有 Safety/Limitations）。
- 78 star、v0.1.0、1 fork，极新；0 issue 但功能仍在早期。
- “识别 CVE”基于已知特征，不能发现未知攻击。

## 总结

coldframe 是一个聚焦明确的 ICS 蜜罐：伪装超市制冷控制器、识别一批真实 CVE、一行命令即可部署、JSON 日志易接入。78 star 的小项目，但对 OT 安全研究很有针对性价值。它不是防护工具而是情报工具，适合研究者与 SOC 试验。

## 独立评分（满分 10）

- **质量**：7 — 聚焦明确、CVE 识别实用、部署简单；极新、低交互。
- **学习价值**：7 — ICS 蜜罐与攻击识别有研究价值。
- **使用价值**：7 — OT 安全研究针对性强，受众窄。
- **推荐等级**：**B**
