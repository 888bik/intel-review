# jjba23/sift

> 深度解读 · 研究日期 2026-11-22 · 推荐等级 B

## 基本信息
- **仓库地址**：https://github.com/jjba23/sift
- **分类**：developer-tools（据 zhao 情报库分类；原始库未核实）
- **语言**：Scheme
- **评分**：原始评分未核实（原始 249 项目库不在本地）；本报告独立评分见文末
- **Star / Fork**：87 / 2
- **许可证**：MIT
- **创建时间**：2026-10-31
- **最近更新**：2026-11-20（元数据 updated_at；最近提交 2026-11-21）
- **最新 release**：Sift 1.39.4（tag `v1.39.4`，发布于 2026-11-21）
- **一句话简介**：Lisp 家族（Scheme、Racket、Emacs Lisp、Common Lisp）的格式化器、规则式 linter 与语言服务器（LSP），格式化是保留注释的空操作幂等固定点。

## 一句话理解

它给 Lisp 各方言补上现代工具链：一键格式化且不丢任何注释/空行、按规则静态检查、还能作为 LSP 接进编辑器，而且格式化两次结果完全一致。

## 通俗解释

Lisp 代码以括号多著称，格式化与静态分析工具相对稀缺。sift 定位是“an idempotent formatter, a rule-based linter and a language server for Scheme, Racket, Emacs Lisp and Common Lisp”（来源：README 开头）。它最有意思的保证是：格式化是文本上的全函数且是固定点——格式化后的文件再格式化不变；注释、空行、#; datum comment、#. read-time form 都原位保留；测试套件里每个 fixture 都断言这两条性质（来源：README）。四种方言各按自己的规则处理，不做妥协（来源：README）。README 是 Org 格式，徽章显示 95 条规则、Guile 3.0+、LSP server、Nix/Guix 包（来源：README 头部徽章）。87 star、v1.39.4、发布频繁（来源：GitHub）。

## 它不是什么

1. **不是只支持一种 Lisp**。支持 Scheme、Racket、Emacs Lisp、Common Lisp 四种方言（来源：README）。
2. **不是会“善意重排”注释的格式化器**。它严格保留注释/空行/reader macro 的位置（来源：README）。
3. **不是编译器或解释器**。它是格式化、linter、LSP（来源：README）。
4. **不是新项目**。2026-10 创建，但已迭代到 v1.39.4、95 条规则（来源：GitHub 与 README 徽章）。
5. **不是没有测试保证**。每条性质都在测试 fixture 上断言（来源：README）。

## 痛点

- **Lisp 格式化难**：注释与 reader macro 位置敏感，普通格式化器易破坏。
- **幂等性缺失**：格式化两次结果不同会污染 diff；sift 保证固定点。
- **方言差异**：四种方言各有规则，避免“折中语法”误伤。
- **编辑器集成**：LSP 让格式化/检查接入日常编辑器。

## 核心功能（依据 README，注明来源）

- **幂等格式化**：固定点 + 保留注释/空行/#;/#.（来源：README）。
- **多方言**：Scheme、Racket、Emacs Lisp、Common Lisp，各按自身规则（来源：README）。
- **规则式 linter**：README 徽章显示 95 条规则（来源：README 徽章）。
- **LSP 语言服务器**（来源：README 与仓库 topics）。
- **打包**：Guile 3.0+、Guix channel、Nix flake（来源：README 徽章）。
- **MIT 许可**（来源：README 头部 SPDX 与 GitHub API）。

## 使用场景

- Lisp/Scheme 项目的代码格式化与静态检查。
- 在 Emacs/VS Code 等编辑器中通过 LSP 获得统一体验。
- 需要幂等、保注释格式化的团队。

## 适用人群

Lisp 各方言开发者、工具链爱好者。不写 Lisp 的人用不上。

## 注意事项

- MIT 许可。
- 87 star、2 fork、7 issue，v1.39.4（2026-11-21）迭代活跃。
- README 为 Org 格式，细节在 doc/rules.org 等文档，需自行阅读。
- 格式化保证为项目自述但有测试断言，仍建议实测复杂 reader macro。

## 总结

sift 是 Lisp 工具链的一次认真补齐：幂等、保注释的格式化 + 95 条规则 linter + LSP，覆盖四种方言，且把关键性质写进测试。87 star 的小众项目，但对 Lisp 开发者价值很高。

## 独立评分（满分 10）

- **质量**：7 — 幂等保注释格式化 + 95 条规则 + LSP，关键性质写进测试；小众。
- **学习价值**：8 — Lisp 方言格式化固定点与保注释设计很有参考价值。
- **使用价值**：7 — Lisp 开发者价值很高，其他语言用不上。
- **推荐等级**：**B**
