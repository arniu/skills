---
repo: https://github.com/mattpocock/skills
path: skills/productivity/grilling/
commit: 84fdeff (2026-08-06)
license: MIT
---

# 研究笔记

`grill-me` / `grill-with-docs` 两个路由壳背后的拷问方法论本体——把"不留情面"的苏格拉底式访谈变成可重复的 agent 流程。

把设计画成设计树（design tree，每个决策长出挂靠其上的子决策）；按轮次推进，每轮只问前沿（frontier，前提已定的决策），编号提问并给出推荐答案，等用户答完再推进下一轮。

关键机制是前沿驱动——每轮重算可问的问题集，依赖未决答案的绝不提前问；会话结束于前沿为空，未获确认绝不动手。

访谈者的节奏本是隐性的，编码成"设计树 + 前沿 + 轮次"后，拷问才能被稳定复现、成规模地跑。

值得带走的是这套编码——把隐性的对话技艺写成显式流程；"查证归 agent、决策归用户"的分工线可直接搬。
