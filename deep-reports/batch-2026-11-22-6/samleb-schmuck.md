# samleb/schmuck

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 C

## 基本信息
- **仓库地址**：https://github.com/samleb/schmuck
- **分类**：developer-tools（**分类为本报告自行判断**；该项目为新入库，情报库尚无分类）
- **语言**：Ruby
- **评分**：本报告独立评分见文末（该项目为新入库，情报库尚无评分）
- **Star / Fork**：3 / 0
- **许可证**：MIT
- **创建时间**：2019-02-03
- **最近更新**：2026-11-17（元数据 updated_at；最近提交 2026-11-16）
- **最新 release**：无。GitHub `/releases/latest` 返回 404，表示未使用 GitHub Releases 发布
- **一句话简介**：给 Ruby 的 String 加上 to_proc，用一行路径字符串当 block 读取嵌套哈希与数组

## 一句话理解
一个 Ruby 小库，把「取嵌套数据」写成字符串路径并当作 block 传给 map、then 等方法。

## 通俗解释
解析 JSON 后常常是哈希套数组套哈希，普通 Ruby 要为每层写一个 map 和变量名。Schmuck 给 String 加 to_proc，于是可以写 response.then(&'[data]*([customer][address][city])') 这样的路径：方括号取键，星号把后续访问映射到每个元素，& 表示 receiver 为 nil 时跳过。它本质是普通 Ruby 代码的组合，路径里还能接 .flatten、.uniq、.sum 等任意方法。README 用 REST、GraphQL 等例子对比了 jq、JSONPath 等查询语言。

## 它不是什么
1. 不是 RubyGems 上可直接安装的成熟 gem，目前需要从 GitHub 装
2. 不是独立的查询语言或命令行工具，只是一段可当 block 用的字符串路径
3. 不是 JSON 专用库，它操作的是你已有的 Ruby 对象
4. 不是安全的静态检查方案，路径在运行时解析，编辑器无法补全

## 痛点
- 多层嵌套数据用 Ruby 原生写法要堆很多 map 和中间变量
- jq 等语言需要序列化再反序列化，还有原生库依赖
- 想要查询语法与 Ruby 方法无缝混用

## 核心功能（依据 README，注明来源）
- String#to_proc，把路径字符串当作 block 使用（来源：README Usage）
- 支持键、索引、方法调用，以及星号映射与 & 的安全跳过（来源：README Usage 语法表）
- 路径可接任意 Ruby 方法，如 flatten、uniq、sum、any?（来源：README What sets schmuck apart）
- 嵌套数组默认保留结构，直到显式调用 flatten（来源：README What sets schmuck apart）
- 提供 REST、顶层数组与深层 GraphQL 三类对照示例（来源：README Usage）

## 使用场景
- 在 Ruby 应用里从 API 响应中抽取嵌套字段
- 想用一行可读路径替代多层 map 与 dig
- 需要查询逻辑直接复用 Ruby 方法链

## 适用人群
写 Ruby、经常处理嵌套 JSON 的开发者；对轻量 DSL 设计感兴趣的人。项目 3 star、无 release，实际采用很少。

## 注意事项
- 尚未发布到 RubyGems.org，需通过 Git 在 Gemfile 中引入
- GitHub /releases/latest 返回 404，没有正式发布版本
- 路径是运行时解析并缓存的字符串，写错只在运行到那一行时才暴露
- 默认严格，访问 nil 上的属性会抛错，需用 & 显式跳过
- 3 star、刚入库，社区验证有限

## 总结
一个想法漂亮、文档也用心的 Ruby 小工具，但采用度和发布状态都还早期，评 C。

## 独立评分（满分 10）

- **质量**：6 — 示例丰富、语法设计有巧思，但仅 3 star、未上 RubyGems 且无 release
- **学习价值**：6 — 把路径字符串扩展成 block 的 DSL 思路值得一看
- **使用价值**：5 — 处理嵌套 JSON 时比多层 map 紧凑，但生态与采用度很低
- **推荐等级**：**C**
