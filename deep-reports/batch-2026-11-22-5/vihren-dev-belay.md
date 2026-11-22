# vihren-dev/belay

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/vihren-dev/belay
- **分类**：developer-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Lean
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：14 / 0
- **许可证**：MIT
- **创建时间**：2026-09-24
- **最近更新**：2026-11-19（元数据 updated_at；最近提交 2026-11-20）
- **最新 release**：SQLite verifier v0.1.2（tag `v0.1.2`，发布于 2026-10-07）
- **一句话简介**：一个用 Lean 证明 SQLite 数据库、语句与迁移性质的库：你写 SQL、声明要成立的性质，Lean 要么找出破坏它的数据库反例，要么给出内核验证的证明。

## 一句话理解

它把“这个数据库迁移会不会丢数据”从靠测试碰运气，变成可以形式化证明或自动找出反例：Lean 会构造出最小的、会出问题的数据库给你看，也能证明修好的迁移在所有数据库上都安全。

## 通俗解释

数据库迁移出 bug 很难发现，因为出错往往取决于特定的数据，而测试库里恰好没有那种数据。Belay 用 Lean（依赖类型证明助手）解决：你用 SQL 写迁移、用查询声明“什么必须保持成立”，然后 #sql_check 要么返回一个反例数据库，要么让 Lean 内核检查一份证明（来源：README 开头）。README 给了一个真实案例：把 customers 表改成 STRICT 时，迁移在事务内执行 PRAGMA foreign_keys=OFF（在事务内无效），DROP TABLE 触发 ON DELETE CASCADE 连带删掉 sessions——代码看不出错。Belay 构造出“一行 customer + 一行 session”的最小数据库，模拟 SQLite 语义重现数据丢失，并把 replay 脚本打印出来可粘进 sqlite3 复现（来源：README “An example” 与 “The counterexample”）。修复后（把 PRAGMA 移到事务外），可以写一个定理声明“在所有符合 schema 的数据库上，迁移前后 allSessions 查询结果一致”，由 Lean 内核验证（来源：README “The fix and the proof”）。14 star、MIT、v0.1.2。

## 它不是什么

1. **不是数据库或迁移工具**。它不执行你的线上迁移，而是验证迁移的性质（来源：README）。
2. **不是测试框架**。它追求的是内核验证的证明与反例，不是采样测试（来源：README）。
3. **不是支持全部 SQLite**。README 有 “Supported SQL” 章节，支持范围是有限的（来源：README headings）。
4. **不是即插即用**。需要 Lean 4 与 lakefile 配置，还要学会写性质声明（来源：README Install）。
5. **不是成熟产品**。14 star、0 fork、v0.1.2、2026-09 创建（来源：GitHub）。

## 痛点

- **迁移 bug 依赖数据**：测试库覆盖不到；反例生成直接构造出最小触发数据。
- **因果关系隐蔽**：PRAGMA 在事务内无效的坑，靠阅读代码难发现。
- **证明可信度**：Lean 内核验证，不只是启发式检查（来源：README “Why trust it”）。
- **代理可参与**：README 说编码代理可以帮忙证明这类性质（来源：README）。

## 核心功能（依据 README，注明来源）

- **反例搜索**：#sql_check 在符合 schema 的数据库上搜索违背性质的例子并打印 replay（来源：README “The counterexample”）。
- **内核证明**：decide +kernel 让 Lean 内核运行 SQLite 语义并认证事实（来源：README “The fact, in the kernel”）。
- **性质声明**：preserves 语句表达“迁移保持查询结果”（来源：README “The fix and the proof”）。
- **可声明的性质范围**：README 有 “What you can state” 章节（来源：README headings）。
- **安装与代理支持**：lakefile.toml 配置与 “With a coding agent” 章节（来源：README）。
- **MIT 许可**（来源：GitHub API）。

## 使用场景

- 验证关键数据库迁移不会丢数据/破坏不变量。
- 形式化方法教学：反例生成 + 内核证明的实例。
- 让编码代理协助写迁移正确性证明。

## 适用人群

Lean/形式化验证研究者、重视数据迁移安全的工程师。普通应用开发者门槛较高。

## 注意事项

- MIT 许可。
- 14 star、v0.1.2、0 fork，极早期；只支持部分 SQL（来源：README Supported SQL）。
- 反例与证明是项目自述的能力，未独立核实；需按文档实测。
- 学习成本高：需要 Lean 与 SQLite 语义知识。

## 总结

Belay 把数据库迁移正确性从“测试覆盖”提升到“形式化证明”，还能自动生成最小反例，示例真实、思路严谨。14 star 的极早期项目、受众窄，但对形式化与数据库安全方向是很有价值的一手材料。

## 独立评分（满分 10）

- **质量**：8 — 用 Lean 内核证明 SQLite 迁移性质的思路严谨、示例完整；极早期、受众窄。
- **学习价值**：9 — 数据库迁移的形式化验证与反例生成有很高学习价值。
- **使用价值**：6 — 对形式化/数据库安全研究者有价值，普通开发者门槛高。
- **推荐等级**：**B**
