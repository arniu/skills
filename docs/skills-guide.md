# SKILL 入门指南

## 引言

SKILL 格式最早是 Anthropic 内部给 Claude Code 用的。2025-12 交给中立的 agentskills.io 托管，成了跨厂商开放标准；2026-01 Vercel 上线 skills.sh 当公开目录。到今天，Claude Code、OpenAI Codex、GitHub Copilot、Cursor、Gemini CLI 等 30+ 工具认的是同一份格式——写一次，处处通用。

本文综合最新行业共识，写成一份 SKILL 入门指南。

## 认识 SKILL

### 和提示词、MCP、子代理的区别

SKILL 不是唯一一种"指挥 Agent"的方式，还有三样常被混着说：

|            | 是什么                | 一句话                          |
| ---------- | --------------------- | ------------------------------- |
| **提示词** | 会话里的一句指令      | 你当场说"帮我这么做"            |
| **SKILL**  | 打包的说明书          | 写一次，以后每次都照着办        |
| **MCP**    | 进程外的数据/工具服务 | 给 Agent 装手：连数据库、调 API |
| **子代理** | 独立上下文的小 Agent  | 派另一个人去干                  |

关系一句话：提示词是原料，SKILL 把它做成可复用的说明书；MCP 提供工具，子代理提供隔离。SKILL 站在中间——借提示词的写法、调 MCP 的工具、必要时派子代理。四者是配合，不是替代。

### 一个 SKILL 长什么样

一个技能是一个文件夹，最简形态只要一个 `SKILL.md`：

```
meeting-notes/
└── SKILL.md
```

`SKILL.md` 本体分两部分——frontmatter 和正文：

```markdown
---
name: meeting-notes
description: >
  How to turn meeting transcripts into structured notes with decisions,
  owners, and next steps. Use whenever the user shares a transcript or
  asks to take notes on a meeting, even if they don't say "summarize".
---

# 会议纪要

把发言记录整理成结构化纪要：

1. 提取决策（谁拍板、拍板了什么）
2. 提取待办（负责人 + 截止时间）
3. 未决事项单列

按这个模板输出：

## 决策

## 待办

## 未决
```

- **YAML frontmatter**：`name` + `description` 两个必填字段——管"叫什么、什么时候用"
- **Markdown 正文**：教 Agent 干活的步骤，尽量 500 行以内

技能还可以打包可选资源：

```
meeting-notes/
├── SKILL.md          # 说明书本体（必填）
├── scripts/          # 可选：确定性、重复的活，写成脚本
├── references/       # 可选：按需加载的资料
└── assets/           # 可选：模板、图标等
```

规范还给技能按成熟度分了级：L1 可加载（Loadable）→ L2 结构完整（Structured）→ L3 可信（Trustworthy）→ L4 已验证（Verified，要求附公开评估用例）。新手目标：L2 起步。

### 它怎么被加载

Agent 不会一次读完整个技能，而是分三级、需要才读：

1. **元数据**（name + description）——一直挂在上下文里，约 100 词
2. **正文** —— 触发了才读
3. **打包资源** —— 用到才取

就像一本只露书脊的书：书名天天见，内容借了才翻开。技能的效率正来自"不全量加载"——平时只读元数据，正文和打包资源用了才读。

## 编写 SKILL

### 动手前先想清三件事

这个技能让 Agent 做什么？什么时候该触发？输出长什么样？

### 正文：说明书怎么写

正文按固定顺序走，读起来才顺：

**标题 + 一句话摘要 → 产出什么 → 必需输入 → 输出格式 → 质量检查 → 反模式**

- **产出什么**：列清单，别让 Agent 猜
- **必需输入**：缺了要问，不得默默编
- **输出格式**：模板定死，别让它自由发挥
- **质量检查**：交付前的核对清单
- **反模式**：明说"不要做"，省得踩坑

整本说明书要**自足**：只读它就能干活，不靠联网、装包或凭据。

排版也讲章法：LLM 读长文是 U 形注意力，开头和结尾记得最牢。所以**关键规则放前 20%，动作/检查清单放最后 10%**，中间只放参考性细节。

### description 与 frontmatter

Agent 判断要不要读这份说明书，靠的是 `description` 这一小段字——它是**主要触发机制**，不是正文。

所以 description 要同时说清**做什么**和**什么时候用**，而且要写得主动（pushy）：模型容易低估技能，光写"它做什么"不够，要把该用的场景列全，连用户没明说的也算上。

```yaml
# 坏：只说做什么（Agent 容易不触发）
description: Build a simple dashboard to display internal data.

# 好：做什么 + 触发场景 + 没明说也用
description: >
  How to build a simple fast dashboard to display internal data.
  Make sure to use this skill whenever the user mentions dashboards,
  data visualization, internal metrics, or wants to display any kind
  of company data, even if they don't explicitly ask for a "dashboard".
```

frontmatter 格式要求：`name` 为 kebab-case（小写字母 + 连字符）、1-64 字符，且必须等于目录名；`description` 1-1024 字符，写"做什么 + 何时用"，是模型决定是否触发的主要依据。可选字段：`version`、`license`、`metadata`（自定义键值）、`allowed-tools`（可调用的宿主工具白名单）、`compatibility`（兼容的宿主与版本）。

### 一个技能管一个领域

想支持多朵云、多个框架？别都堆在正文里，按变体拆文件：

```
cloud-deploy/
├── SKILL.md              # 公共流程 + 怎么选
└── references/
    ├── aws.md
    ├── gcp.md
    └── azure.md
```

Agent 只读相关那份。大资料（>300 行）加目录；正文逼近 500 行，就再加一层，写清"什么时候去读哪个"。

### 写作原则

- 用**祈使句**：直接说"做 X"，别说"X 应该被执行"
- 讲**为什么**，别堆"必须"——Agent 懂意图就自己推，死记规则只会僵
- **示例 > 抽象**：至少给一个完整的输入/输出对
- 目标、约束写死；步骤、失败处理留白——规定太死，技能变成流水线，Agent 的智能就用不上了
- 技能不得夹带恶意/越权内容，做的要和说的一致
- 装别人的技能前先审：2026 年生态审计发现约 1/3 的社区技能带安全缺陷

### 装多少：少而精

技能的省是省在"不全量加载"，但它不是免费的：每个技能至少把 name + description（约 100 词）常驻上下文。装得越多，常驻元数据越挤占上下文窗口；生态观测也显示，技能一多 Agent 选错技能的概率随之上升，description 饱和后效果不升反降。所以别贪多——几个好技能，胜过一堆凑数的。

## 参考资料

- **格式规范**：skills.sh 生态及各主流 SKILL 文档（agentskills.io 开放标准）
- **标准与安全**：Snyk 2026 技能生态安全审计
- **写作方法论**：Matt Pocock 的 writing-for-agents 系列
