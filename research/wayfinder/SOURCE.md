# 来源

- 仓库:https://github.com/mattpocock/skills
- 路径:`skills/engineering/wayfinder/`
- 提交:`84fdeff`(2026-08-06)
- 许可:MIT — Copyright (c) 2026 Matt Pocock

## 研究笔记

- Matt Pocock 当前的主攻方向(2026-07-08 创建,三周 25.8 万安装量)。重新定义了"规划":不是产出 spec,而是一张共享的决策 ticket 地图,每次会话解析一张,直到路线清晰。
- 关键思想:"Plan, don't do"(只规划、不执行);迷雾 vs ticket 的判定测试(现在能否精确陈述这个问题?);HITL 与 AFK 两种 ticket 类型(research = AFK 子代理,grilling = HITL,agent 绝不替真人回答自己的拷问);并行会话先认领(claim)再做;按名称引用(用 issue 标题,绝不用裸 id);每次会话只解析一张 ticket 的纪律。
- 建议与 `to-spec`、`to-tickets` 连读(它们是配套组件)。

## 依赖(均已一并复制)

- `/setup-matt-pocock-skills` — 提供 issue tracker 配置(`../setup-matt-pocock-skills/`)
- `/grilling` + `/domain-modeling` — 用于制图与解析 HITL ticket(`../grilling/`、`../domain-modeling/`)
- `/research` — AFK 子代理解析 research ticket(`../research/`)
- `/prototype` — prototype 类 ticket(`../prototype/`)
