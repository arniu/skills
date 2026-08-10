---
repo: https://github.com/mattpocock/skills
path: skills/engineering/research/
commit: 84fdeff (2026-08-06)
license: MIT
---

# 研究笔记

后台子代理（background agent）技能，面向高可信一手资料的调研，把发现写成仓库里的 Markdown 文件。

启动一个后台 agent（background agent）：只认一手资料（官方文档、源码、spec、第一方 API），每个论断回溯到源头；把发现写入单一 Markdown 文件并逐条注明来源，存到仓库已有的笔记约定处。

关键机制是后台子代理 + 一手资料纪律——调研不阻塞会话主线，可靠性靠"逐条回溯源头"保证。

调研耗时长、几乎不需要真人介入，拆成异步子代理才能并行；二手转述会失真，所以只认一手资料。

凡是耗时且无需人工判断的工作，都可拆成 AFK 子代理异步跑；"每个论断回源"是可靠的调研标准。
