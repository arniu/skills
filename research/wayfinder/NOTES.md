---
repo: https://github.com/mattpocock/skills
path: skills/engineering/wayfinder/
commit: 84fdeff (2026-08-06)
license: MIT
dependencies:
  - /grilling
  - /domain-modeling
  - /research
  - /prototype
---

# 研究笔记

把"规划"重新定义为一张共享的决策 ticket 地图——大块工作超过一次会话容量时，在 issue tracker 上建图，逐个解析 ticket，直到路线清晰。

先制图（命名 destination → 广度优先 grilling 摸清迷雾 → 建地图 issue → 建 ticket 并二次连阻塞边 → 并行派出 research 子代理），再逐张解析（claim 认领 → 解析 → 记录决议并关闭 → 追加进 Decisions-so-far → 升格新迷雾、剔除越界）；一次会话只解析一张 ticket（research 类除外）。

关键机制是迷雾 vs ticket 判定（能否现在就精确陈述问题，而非能否回答）、HITL / AFK 二分（research = AFK 子代理，grilling = HITL 必须真人作答）、"Plan, don't do"（只规划、不执行）、按名称引用（用 issue 标题，绝不用裸 id）、先认领再做。

超过一次会话的工作无法靠单次推理看清，地图把"看不见的迷雾"显式化，让每次会话只消化一小块决策，且能并行、可追踪——规划从"写 spec"变成"维护一张决策地图"。

值得带走的是"以 ticket 地图做规划"的整套形态——把大块工作组织成一张可协作、可并行的决策地图。
