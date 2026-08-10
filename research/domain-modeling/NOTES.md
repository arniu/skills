---
repo: https://github.com/mattpocock/skills
path: skills/engineering/domain-modeling/
commit: 84fdeff (2026-08-06)
license: MIT
---

# 研究笔记

主动构建并打磨仓库领域模型的技能——敲定统一词汇（ubiquitous language）、按需记录架构决策（ADR）。

会话中挑战既有术语、点破模糊词、用具体场景压测概念边界、与代码交叉核对；术语一敲定就用 `CONTEXT-FORMAT.md` 的格式写进 `CONTEXT.md`（纯词汇表，零实现细节），需要记录的决策用 `ADR-FORMAT.md` 的格式写 ADR。

关键机制是单一事实来源——`CONTEXT.md` 只放词汇表；ADR 只在"难反悔、无上下文会困惑、有真实取舍"三者齐备时才写。

协作型技能（`grill-with-docs` 顺带产出的术语表、wayfinder 的制图阶段）靠共享词汇才能对齐；词汇与实现分离，词汇表才不会被代码细节污染。

词汇表与实现分离（`CONTEXT.md` 只放词汇）的纪律、ADR 的三条门槛（难反悔 / 无上下文会困惑 / 有真实取舍），都可以直接借用。
