---
repo: https://github.com/mattpocock/skills
path: skills/productivity/writing-for-agents/
commit: 84fdeff (2026-08-06)
license: MIT
---

# 研究笔记

面向"agent 会读的文档"的通用写作理论——技能、AGENTS.md/CLAUDE.md、经指针触达的文档，形式不同，道理相通。

用一组杠杆让文档可预测——上下文指针决定何时触达材料；信息层级（in-file step → in-file reference → disclosed reference）决定材料放多深；每个步骤配完成标准（clarity + demand）；按序列、按调用方式拆分。

关键机制是上下文指针（context pointer，措辞决定触发可靠性，而不是目标本身）、两种负载（context load 与 cognitive load）、引导词（leading word，调动模型的先验）vs 否定表述（"别想大象"式的失效）、渐进披露与修剪（单一事实来源、环境即事实、no-op 是相对模型的判断）。

材料放得越深越省常驻上下文，但越难被触达——信息层级就是在这两种成本之间取舍，按分支决定放多深；否定式表述会把禁词拖进上下文、反而更可用，所以只提示正向行为。

与本仓库自身的技能写作最直接可迁移；`SKILL-MECHANICS.md`（已一并复制）专门讲技能的 frontmatter、调用方式选择与路由技能。
