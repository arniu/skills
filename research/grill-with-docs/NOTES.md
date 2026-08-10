---
repo: https://github.com/mattpocock/skills
path: skills/engineering/grill-with-docs/
commit: 84fdeff (2026-08-06)
license: MIT
dependencies:
  - /grilling
  - /domain-modeling
---

# 研究笔记

极简路由壳（仅 7 行），把 grilling 会话升级为"边拷问边产出文档"——同时调用 `/grilling` 与 `/domain-modeling`。

`disable-model-invocation: true`，不经模型自我触发；由路由壳发起一个 `/grilling` 会话并把 `/domain-modeling` 挂进去，拷问途中顺带生成 ADR 与词汇表。

关键机制是路由壳模式——入口只做分发，真正的方法论住在被委托的技能里，这里零重复。

方法论升级只需改本体一处，所有路由壳同步受益；7 行的入口让"带文档的拷问"这一组合零成本可复用。

值得照搬的是路由壳的组合模式——把通用方法论包成可叠加、可组合的技能，再组装出更专的变体；wayfinder 的默认 ticket 类型就是 grilling。
