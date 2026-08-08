# 可预测的 SKILL：Matt Pocock 写作方法论

本文基于 Matt Pocock [`writing-for-agents`][wfa] 中的方法，结合流行技能里的实例，教你怎么写出可预测的 SKILL。

## 目录

- [Part 1：核心理念](#part-1核心理念) —— 概念 · 顺序读
- [Part 2：信息层级](#part-2信息层级) —— 方法 · 顺序读
- [Part 3：触发设计](#part-3触发设计) —— 方法 · 顺序读
- [Part 4：步骤写作](#part-4步骤写作) —— 方法 · 顺序读
- [Part 5：结构模式](#part-5结构模式) —— 模式 · 按需查阅
- [Part 6：通病](#part-6通病) —— 通病 · 按需查阅
- [Part 7：应用](#part-7应用) —— 练习与检查 · 写作时对照
- [附录：一页速查](#附录一页速查) —— 全文浓缩表

> 体例说明：
>
> - 每个模式含「适用 · 骨架 · 案例 · 适用检查」；
> - 每个通病含「命名 · 症状 · 解法 · 自查」；
> - 术语首次出现括注原文，标题中除外。

---

开始之前，先介绍一下 [Matt Pocock][mattpocock]，这位 SKILL 领域的莎士比亚。根据 skills.sh 排行榜，剔除产品 SKILL，榜单前十名几乎全部出自他之手！他不只写了一批爆款，更总结出了一整套撰写高质量 SKILL 的方法。

## Part 1：核心理念

### 1.1 可预测性是根本目标

写技能，就是让一个随机的系统尽可能按你的意思走。核心目标叫**可预测性（predictability）**——不是让代理每次输出相同的内容，而是让它每次执行相同的**流程**：同样的输入，永远走同一条路。

对比下面两个 SKILL 的开头：

```yaml
# ✗ 低可预测性
name: review-code
description: Review code changes.
---
Analyze the diff and find bugs.
```

```yaml
# ✓ 高可预测性
name: review-code
description: Review a pull request diff for logic bugs, security issues, and performance regressions.
---
You are a code reviewer.

## Steps

1. Read the diff line by line.
2. Identify issues in three categories: logic, security, performance.
3. For each issue: state the problem, explain why it matters, suggest a fix.
   Complete when every changed line has been examined.

## Constraints

- Focus on logic and correctness. Do not comment on style or formatting.
- If you cannot determine whether a change is correct, state what information is missing.
```

两者的差别在于：第二份 SKILL 明确告诉代理做什么、按什么顺序做、做到什么程度算完、什么不该做；第一份什么都没说——代理只能靠猜。

可预测性不等于僵化。它意味着**同一技能在同一输入下走同一套流程**，调试、优化、信任才成为可能。

**适用范围**：这套方法不只对技能有用——**任何 agent 消费的文档**（SKILL.md、`AGENTS.md` / `CLAUDE.md`、被指针触达的参考文件）都适用。包装不同，写作之道不变。

本文以 SKILL 为主线，但所有概念同样适用于 AGENTS.md 之类的文档。

### 1.2 两种负载

你每添加一个文档或指针，都要消耗两种负载之一。

- **上下文负载（context load）**——常驻在 agent 窗口中的成本。`AGENTS.md` 的一行、技能的 `description`、任何每轮都留在上下文里的东西，无论触发与否，都在消耗 token 和注意力。
- **认知负载（cognitive load）**——落在人身上的成本。你得记得有哪些文档、什么时候该用哪个。**索引在你脑子里**。

只经指针触达的材料，用指针那一行的代价免于上下文负载；完全没有指针的材料，则整个压在认知负载上。

两种负载**此消彼长**：想省上下文负载，就得用认知负载来抵；想省认知负载，就得让上下文多担一些。关键不是"哪个更小"，而是**这样取舍值不值**。认知负载不是要最小化的成本，它是人类能动性的价格——该由人判断的地方，就值得多花认知（把调用时机交给人类）；纯机械的查找，不该占上下文（应当内联或交给环境）。

### 1.3 可预测性的三大支柱

如果说两种负载（1.2）决定**投入多少**，三大支柱决定**投在哪三个方向**——两者正交。

- **结构一致性（Structure Consistency）**——代理知道自己处于流程的哪一步。对应：编号步骤、标题层级。
- **词汇锚定（Vocabulary Anchoring）**——用模型预训练中的稳定概念做锚点。对应：引导词（leading word，见 1.4）。
- **边界明确（Boundary Clarity）**——什么该做、什么不该做、什么该问用户。对应：约束（constraints）、完成条件（completion criterion，见 4.2）。

这些杠杆可以归成三个可操作的维度，成熟的 SKILL 三者兼备。其中两条已经单独成章：词汇锚定 → 引导词（1.4）；边界明确 → 完成条件的 clarity + demand 双属性（4.2）。

### 1.4 引导词

引导词（leading word）是模型预训练中已经存在的紧凑概念。把它嵌入文档，可以用最少的 token 激活稳定的行为模式——原理是**借用模型已有的先验（recruiting priors the model already holds）**。

**案例**：

把一段话换成单个词，效果立见：

- "Be fast, deterministic, and low-overhead in your loop"（9 个 token）→ 一般
- 只用 `tight`（1 个 token）→ 更好——模型对 "tight loop" 已有稳定理解
- "Be thorough and don't miss any edge cases"（8 个 token）→ 模糊
- 只用 `relentless`（1 个 token）→ 更好——"relentless" 隐式覆盖了"不遗漏"

[Matt Pocock][mattpocock] 的 [`grill-me`][grill-me] 用 `relentless` 一词开头写描述："A relentless interview to sharpen a plan or design."——一个词就传达了"持续追问、不放过任何分支"。

**引导词的两重作用**：

- 在正文里，它作用于**执行**：同一个词每次出现，代理就伸手去取同一行为；在扁平参考里，它把注意力聚焦到该找的一类东西上。
- 在指针里，它作用于**调用**：当同一个词同时出现在你的提示词、文档和代码库里，代理就会把这套共享语言和材料挂上钩，以后触达得更准。

主动找机会用引导词改写：一个在三个地方展开的短语、一个要花一句话才指到同一概念的指针，都该浓缩成一个词：

- "fast, deterministic, low-overhead" → `tight`（一个 _tight_ 循环）
- "a loop you believe in" → `red`——从说不清的关口变成看得见的是非（循环在 bug 上变 _red_，或者不变）

**几点注意**：

- 优先用已有的词：自造词也能用，但招募不到模型先验——已有的词先验是白送的，自造词得用定义去换。
- 引导词是锦上添花，不是核心替代。先保证指令清晰具体；只在上下文高度受限（如 description 字段）或确认该词在目标领域有稳定语义时，才用得上。
- 引导词的效果依赖模型规模——在 Claude 4 / GPT-4 上显著，在小模型上可能无效。
- 拿不准一个词是否比一段描述更好时，保留描述。精确 > 简洁。
- **否定（negation）是伴随引导词的通病**：用禁令引导，会把被禁止的行为拖进上下文，反而让它更活跃——"别想大象"，满脑子都是大象。**正面陈述目标行为**（"write one-line comments"），让被禁的那个从不被提起。禁令只在无法正面表述的硬性护栏处才该用，且必须与正面目标配对。

### 1.5 何时放松可预测性

可预测性不是所有场景都需要同等严格：

- 部署、代码审查、数据操作：严格——操作有副作用，错不起
- 文案写作、头脑风暴：中等——需要创意空间
- 纯生成（写诗、构思）：宽松——流程反而抑制创造力

一条原则：**任务的风险等级决定可预测性的严格程度。**

### 1.6 适用边界说明

本文方法论主要来自 Matt Pocock 的 [`writing-for-agents`][wfa]。写作层面的概念（两种负载、信息层级、完成条件、引导词、修剪）是跨平台的通用理论；但触发机制（Part 3）以 skills.sh / Claude Code 生态为例，以下内容在非 Claude Code 的平台上可能需要调整：

- **引导词**的效果在 Claude 4 / GPT-4 上显著，在小模型上可能无效
- **用户调用 vs 模型调用**的区分方式（`disable-model-invocation` 字段）是 Claude Code / skills.sh 生态特有的
- **指针（context pointer）**在支持主动加载文件的代理上有效，在不支持的系统上就只是一句普通建议

如果面向其他平台写 SKILL，建议先验证平台的指令注入机制和上下文加载策略。

---

## Part 2：信息层级

信息层级回答内容组织的四个问题：放**多深**（2.1）、放**什么旁边**（2.2）、拆**不拆**（2.5）、怎么**找得到**（2.4）。

### 2.1 三阶阶梯：放多深

信息层级是一个阶梯，按 agent 需要材料的紧迫程度排序：

1. **文件内步骤（in-file step）**——第一层：agent 按顺序执行的动作。
2. **文件内参考（in-file reference）**——按需查阅的定义、规则、事实。一组扁平的规则（比如审查的全部规则）放在同一层是合理的安排，不是坏味道。
3. **披露参考（disclosed reference）**——推送到单独文件、经上下文指针触达、只在指针触发时加载。从同目录的兄弟文件，到任何文档都能指向的外部参考，都在此列。

推得太少，顶部臃肿；推得太多，又把 agent 真正需要的东西藏了起来。这个分寸，就是决策的全部。

**渐进披露（progressive disclosure）**就是沿阶梯下移的动作——移出主文件、放到指针后面，让顶部保持可读。它首先不是 token 优化，而是**保护层级本身**的方式。

**判断标准：分支测试（Branch Test）**

> 分支是判断披露的最干净测试：所有分支都要的，内联；只有部分分支触达的，推到指针后面。

当文档带步骤时，该披露而未披露的文件内参考会**淹没步骤**，让"按步骤执行"变成掷硬币——这会放大行为的不确定性（variance），不只是可读性问题。

### 2.2 同位聚合：放什么旁边

阶梯决定内容放**多深**，同位聚合（co-location）决定**什么内容放什么旁边**。把同一个概念的**定义、规则、注意事项**收在同一个标题下，而不是散落各处——读了一部分，邻接的部分自然跟来。

判断标准：文档读起来应该像为 agent 写的文档——聚合的内容读起来是"为 agent 写的"，散落的不是。（区别于重复：重复是同一个意思出现在多处；散落是一个意思被拆碎在多个地方。）

### 2.3 案例对照

三个案例将贯穿本节：它们分别演示"内容量大的组织形态（渐进披露）""按调用方式拆分"和"极薄路由壳"三种形态。

**[Anthropic][anthropic] [`mcp-builder`][mcp-builder]** 是渐进披露的典型：

```
mcp-builder/
├── SKILL.md              # 核心步骤（约 300 行）
├── reference/
│   ├── mcp_best_practices.md    # MCP 通用规范
│   ├── node_mcp_server.md       # TypeScript 实现指南
│   ├── python_mcp_server.md     # Python 实现指南
│   └── evaluation.md            # 评估指南
```

SKILL.md 中每个阶段（Phase）只引用对应的 reference 文件，代理只加载它当时需要的上下文。

**[Matt Pocock][mattpocock] [`writing-for-agents`][wfa] 自身**——全参考型文档 + 按调用方式披露：

```
writing-for-agents/
├── SKILL.md               # 通用写作理论（全参考，无步骤）
└── SKILL-MECHANICS.md     # 技能特有机制：frontmatter、调用方式、路由技能
```

SKILL.md 开头的指针说：当你在写的是技能时，去读 SKILL-MECHANICS.md。这就是"按调用方式拆分"（见 2.5）的实例——文档分为两半，只有写技能的人才需要后一半。

**[`grill-me`][grill-me]**——一个**极薄**的路由壳（7 行），把活儿委托给 `/grilling`（见 3.3）：

```
grill-me/
└── SKILL.md    # 只有一行实质内容："Run a /grilling session."
```

不需要外部文件，因为它的逻辑只有一条"拷问直到分支闭合"——而这一条，也转手交给了 /grilling。薄到不需要分层。

> 由此引出一条重要原则：**不要过早抽象（over-abstract）。** 技能只有 50 行时，先别建 reference 目录。等 SKILL.md 超过 200 行、且不同分支需要不同知识时再分层。作为参考，300 行通常是一条警戒线——超过这个长度，无论是否分支，都值得考虑分层。

### 2.4 指针：怎么找得到（措辞决定触发）

第二层的引用靠**上下文指针（context pointer）**触发——一个常驻在 agent 上下文里、指向外部材料、并写明何时该去取它的引用。技能的 `description` 是一个；`AGENTS.md` 里指向某文档的一行也是。（`description` 这个"顶层指针"的完整写法见 3.2。）

**指针的措辞，而不是它的目标，决定 agent 何时、以多可靠地触达材料。**

一个"必须触达的目标 + 措辞松垮的指针"，会让行为飘忽不定：先锐化措辞，锐化不了才把材料内联。

指针做两件事：

1. **说明材料是什么**
2. **列出应当触发它的分支**（一个分支 = 文档处理的一种情况，不同运行走不同路径）

常驻指针的每个词每轮都在消耗上下文，所以它比正文更该狠修剪：

- **引导词前置（front-load the leading word）**——指针正是靠这里完成触发的
- **一个分支一个触发词**——同义词只是把一个分支写两遍，合并；只保留真正不同的分支
- **删掉正文已带的信息**

- ✓ "See GLOSSARY.md for full definitions."
- ✗ "更多参考见 GLOSSARY.md"
- ✓ "Load [reference/evaluation.md](./reference/evaluation.md) when you reach Phase 4."
- ✗ "参考文件在 reference 目录下"
- ✓ 明确告诉代理什么时候加载
- ✗ 模糊地提一句

### 2.5 何时移出主文档（决策地图）

把内容移出主文档有两条路：**披露**和**拆分**——动作不同，别混为一谈。这里只给地图，细节在各自主场：

- **披露（按分支）**——内容移到指针后，文档还是一个：只把部分分支才需要的内容藏到后面（2.1 分支测试）
- **拆分（按序列）**——一个文档变成两个：防过早完成，把后置步骤移出视野（4.2 完成条件）
- **拆分（按调用方式）**——一个文档变成两个：可发现性，让一个引导词独立触发（3.3 路由器技能）

共同前提：都不免费——披露要付指针那一行，拆分要消耗两种负载之一（1.2）。没必要时不动作，保护层级与完成质量才是目的。

---

## Part 3：触发设计

### 3.1 两种触发模式：对两种负载的取舍

调用方式是对两种负载（1.2）的取舍：

|            | 用户调用（User-Invoked）         | 模型调用（Model-Invoked）    |
| ---------- | -------------------------------- | ---------------------------- |
| 配置       | `disable-model-invocation: true` | 省略该字段                   |
| 描述       | 一句话使用场景（面向人类）       | 多分支触发词列表（面向模型） |
| 上下文负载 | 零                               | 描述每轮都加载               |
| 认知负载   | 你得记住它的存在                 | 零                           |
| 谁能触发   | 只有人                           | 代理自主 + 其他技能 + 人     |
| 适合场景   | 工具型、有副作用、低频使用       | 通用能力、被其他 skill 调用  |

**决策原则**：只有当代理必须自主调用该技能、或其他技能需要调用它时，才用模型调用；只能靠手动输入调用，就设为用户调用，不浪费上下文。

补充三点机制细节：

- 模型调用**总是包含**用户触达——`description` 只会给代理增添一份发现能力，你照样可以打字调用它，人的触达从来不会因此消失。
- 模型调用型技能还有一个隐蔽用途：**共享参考的家**。内容全是参考的模型调用型技能可以被其他技能调用，所以几个技能都要用的参考可以放在同一个地方。
- 两个用户调用型技能需要同一份共享参考时，哪里都放不下——都没有 description，谁也触发不了谁。把它推到技能系统外的普通文件：任何技能都能指向的外部参考。

### 3.2 描述的写作

#### 模型调用型

模型的 `description` 是技能的**顶层上下文指针**（承接 2.4），必须常驻加载——以永久的上下文负载换取可发现性。2.4 的指针写作规则在这里全量适用。

描述要做两件事：说清技能是什么 + 列出应当触发的场景。每个场景是一个**分支（branch）**，而不是一个同义词。

```yaml
# ✗ 不好的模型调用描述（同义词堆砌）
description: >
  Review code changes. Use when the user wants code review, CR, PR review,
  change review, diff review, review changes, or needs a code check.

# ✓ 好的模型调用描述（多分支触发）
description: >
  Review a pull request diff for logic bugs, security issues, and
  performance regressions. Use when:
  - the user says "review this PR" or "check my changes"
  - the user mentions "code review" or "CR"
  - another skill verifies output before completing
```

**关键**：`"build features using TDD"` 和 `"test-first development"` 是同一个分支的两种说法，不是两个分支。只保留真正不同的触发场景。

参考 [Vercel][vercel] 各技能中 "Use when" 的写法：

```yaml
# Vercel `react-best-practices`
description: >
  React and Next.js performance optimization guidelines from Vercel Engineering.
  Contains 40+ rules across 8 categories, prioritized by impact.
  Use when writing new React components, implementing data fetching,
  reviewing code for performance issues, or optimizing bundle size.
```

每个 "Use when" 后面列的都是**独立的触发场景**，不是同义词。

#### 用户调用型

描述只对人类说一句话——说清什么时候该用。

```yaml
# 用户调用型描述
name: grill-me
description: A relentless interview to sharpen a plan or design.
disable-model-invocation: true
```

[Matt Pocock][mattpocock] 的 [`grill-me`][grill-me] 就是标准样本。一句话，不占上下文，人看了就知道什么时候该输入 `/grill-me`。

### 3.3 路由器技能与按调用方式拆分

当用户调用型 SKILL 多到记不住时，堆积的认知负载靠**路由器技能（router skill）**化解——一个用户调用型技能，列出其他技能以及各自何时用，让人只记住一个而不是一堆：

```markdown
---
name: my-skills
description: List available skills and when to use each.
disable-model-invocation: true
---

我可以用以下技能帮助你：

- `/grill-me` — 评估计划或设计
- `/tdd` — 测试驱动开发
- `/handoff` — 交接任务
```

**router 只能提示，永远不能触发**：用户调用型技能没有 description，除了人，没有任何东西能触达它们——所以 router 的价值在于让人"记得"，而不是让代理"调用"。[Matt Pocock][mattpocock] 的 [`setup-matt-pocock-skills`][setup-skills] 就是这个模式。

**按调用方式拆分（splitting by invocation）**（拆分的两种方式之一，地图见 2.5）：当你有一个值得单独触发的引导词——一个你确实会在提示词里用到的词——或者别的技能必须触达它时，把它拆成一个模型调用型技能。但拆出去，这份新常驻的 description 要额外消耗上下文负载——这份独立触达必须值得。

---

## Part 4：步骤写作

### 4.1 步骤结构

SKILL 中的指令有三种基本的组织方式：

- **编号步骤（numbered steps）**——有顺序依赖的任务
- **子弹列表（bullet lists）**——无顺序的规则集/检查点
- **纯段落（prose）**——简单任务或纯参考型

这不是要求所有技能都必须编号——如果你的技能是纯参考型（如 [`writing-for-agents`][wfa] 自身），子弹列表或标题分节更合适。具体选择参考 Part 5 的模式决策树。

**位置效应：指令的位置影响权重**

LLM 对不同位置的指令关注度不均衡：开头（primacy effect）和末尾（recency effect）的内容权重最高，中间区域（"lost in the middle"）的内容容易被稀释。这意味着：

- **最重要的指令——角色声明、核心约束——放在开头或末尾**，不要埋在中间段落
- **每个步骤的完成条件紧跟在步骤描述之后**，不要集中到文档末尾
- 需要强调中间的内容时，用显式的元指令（"注意，以下规则很重要"）对抗位置衰减

**示例：规则配例子**

至少给一个完整的输入/输出对。抽象规则留有余地，例子定下边界——好的例子让 agent 能对照"我做到的样子对不对"：

```markdown
# ✗ 只有规则

Write clear comments.

# ✓ 规则 + 示例

Explain the why, not the what:

- ✗ // compute total
- ✓ // total = price * qty; tax applied later in checkout
```

### 4.2 完成条件

每个步骤都以**完成条件（completion criterion）**收尾——明确告诉 agent 做到什么程度算完。两个属性让它成为有力的写作杠杆：

- **清晰度（clarity）**——agent 能否分清 done 与 not done？一个模糊的边界（"理解达成"）会招致**过早完成（premature completion）**：在真正做完之前就收尾，注意力滑向"做完了"本身。可见的后置步骤（post-completion steps）提供拉力，标准的清晰度是阻力。防御按顺序：**先锐化边界**（改起来最省事）；只有当边界确实模糊**且**你观察到冲刺时，才通过拆分序列把后续步骤移出视野（序列拆分的总述见 2.5）——而隐藏只在真实上下文边界（一次交接或子代理派发）处有效，内联调用会把后续步骤留在上下文里，什么也清不掉。
- **要求度（demand）**——标准要求做多少。"每个被修改的模型都解释清楚"迫使彻底执行，而"产出一份变更清单"不会。demand 驱动 **legwork**——潜伏在措辞里、不写成独立步骤的挖掘工作。它不只约束步骤序列："每条规则都应用了"对一堆扁平参考的约束力，与"每步都做完了"对序列的约束力相同——这正是全参考文档也要讲穷尽性的道理。

**最强的标准既可检查、又穷尽。**

```markdown
# ✗ 模糊的完成标准

1. Analyze the diff.
2. Find bugs.

# ✓ 可检查且彻底（demand 足够）的完成标准

1. Read the diff line by line.
   Complete when every changed line has been read.
2. Identify bugs in three categories: logic, security, performance.
   Complete when all three categories have been checked.
```

**直觉检查**：一行写"找 bug"，代理找了 2 个就停——这是过早完成；写"找到所有 bug，确保每条代码行至少被检查过一次"，代理会继续直到没有漏的。

### 4.3 分支

同一 SKILL 有多个入口（分支，branch）时，明确定义分支条件：

```markdown
If the user's goal is:

- A new feature → follow Steps 1-5
- A bug fix → skip to Step 3
- A refactor → skip to Step 4

In all cases, end with Step 6 (review).
```

参考 [Matt Pocock][mattpocock] 的 [`triage`][triage]，它按问题类型走不同路径。分支条件写在开头，代理不必读完整份 SKILL 就知道该走哪条路。

---

## Part 5：结构模式

> 分工说明：结构模式决定文档**走哪种形状**（线性步骤 / 规则集 / 多入口）；信息层级（Part 2）决定**内容放多深**。两者正交——选定模式后，仍按 Part 2 组织内容。

### 模式选择

```
你的任务是否有严格的先后顺序？
   ├── 是 → 角色-行动模式（模式 1）
   └── 否 → 质量是否取决于代理知道多少规则？
              ├── 是 → 参考优先模式（模式 2）
              └── 否 → 是否有多个入口路径？
                         ├── 是 → 分支模式（模式 3）
                         └── 否 → 上述任一模式 + 渐进披露（叠加策略）做内容组织
```

这些模式可以组合使用，例如分支 + 角色-行动是常见组合（每个分支内再用编号步骤）。

### 模式 1：角色-行动

**适用**：角色-行动（Role-Action）模式适合需要代理以特定身份执行的多步骤任务。

**骨架**：

```yaml
---
name: <skill-name>
description: <触发词 + 功能>
---

You are <角色>. <一句话目标>.

## Steps

1. <动作>. Complete when <完成标准>.
2. <动作>. Complete when <完成标准>.
3. <动作>. Complete when <完成标准>.

## Constraints（可选）

- <约束 1>
- <约束 2>
```

**案例**：

- [Matt Pocock][mattpocock] [`tdd`][tdd]——"You are a TDD practitioner."
- [Anthropic][anthropic] [`frontend-design`][frontend-design]——"Approach this as the design lead at a small studio..."

两个技能，领域完全不同，却都用"角色声明 + 步骤 + 完成条件"的结构。注意：角色声明不是必需的——极薄的路由壳（如 [`grill-me`][grill-me]）直接委托给别的技能，就没有角色声明（见 2.3）。

**适用检查**：任务能否拆成线性步骤？步骤之间有严格先后顺序的，用这个模式。

### 模式 2：参考优先

**适用**：参考优先（Reference-First）模式适合需要大量规则/约束的知识密集型任务。代理不需要顺序执行，而是在规则约束下自主判断。

**骨架**：

```yaml
---
name: <skill-name>
description: <触发词>
---

## <原则/规则标题>

- <规则 1>
- <规则 2>
- <规则 3>

## <另一个规则集>

- <规则 4>
- <规则 5>

## Checklist（可选）

- [ ] <检查项 1>
- [ ] <检查项 2>
```

**案例**：

- [Matt Pocock][mattpocock] [`writing-for-agents`][wfa]——全参考型，无步骤
- [Anthropic][anthropic] [`mcp-builder`][mcp-builder]——步骤 + 深度参考

观察：纯参考模式需要更高质量的写作，因为没有步骤帮代理理清思路结构。选这个模式，就要确保规则足够清晰具体——[`writing-for-agents`][wfa] 自身正是如此：它写"如何写 agent 文档"，自己就是一篇全参考文档，每个概念都是可操作的杠杆。

**适用检查**：任务的质量主要取决于代理知道多少规则（而非执行顺序）？如果是，用这个模式。

### 模式 3：分支

**适用**：分支（Branch）模式适合同一 SKILL 有多个入口/路径的情况。

**骨架**：

```yaml
---
name: <skill-name>
description: <涵盖所有分支的触发词>
---

You are <角色>.

## Entry

如果用户的请求是：
- <场景 A> → 执行 Path A
- <场景 B> → 执行 Path B
- 不确定 → 询问用户

## Path A

<步骤或规则>

## Path B

<步骤或规则>
```

**案例**：

- [Matt Pocock][mattpocock] [`triage`][triage]——按问题类型（bug / feature / question）分支
- [Vercel][vercel] [`react-best-practices`][vrbp]——用 "Use when" 列触发场景，隐式分支

**关键**：分支条件必须**互斥且覆盖全面（mutually exclusive and collectively exhaustive）**。分支有重叠，代理可能选错路；有未覆盖的场景，代理会自己猜——猜对猜错都不好。

### 叠加策略：渐进披露

**适用**：内容量大的技能。这不是与模式 1-3 并列的独立模式，而是**叠加在其上的内容组织策略**——模式决定形状，它决定"内容放多深"（详细原理见 2.1）。当 SKILL.md 超过 200 行、且不同分支需要不同知识时，启用它。

**骨架**：

```
<skill>/
├── SKILL.md              # 核心步骤 + 指针
├── references/
│   ├── detailed-rules.md  # 分支 A 需要的知识
│   └── examples.md        # 分支 B 需要的示例
└── scripts/               # 可执行脚本
```

**关键**：指针（pointer）措辞决定代理能否可靠加载。明确写"When you reach Phase 3, load references/evaluation.md"比"参考文件在 references 目录下"有效得多（见 2.4）。

---

## Part 6：通病

这些通病从何而来？多数是长期写作实践里反复出现的坑；过度披露、描述错配两个是本文补充的。每个通病都对应某个正向杠杆的失效面：

- **臃肿**——信息层级（Part 2）：该披露的没披露
- **过度披露**——信息层级·分支测试（2.1）：不该披露的也披露了
- **沉积**——修剪：增易删难，旧层越堆越厚
- **过早完成**——完成条件·清晰度（4.2）
- **空指令**——修剪·no-op：说了模型默认会做的事
- **否定陷阱**——引导词（1.4）：禁令激活了被禁的概念
- **重复**——修剪·单一事实来源
- **描述错配**——调用模式（3.1）：描述风格与调用模式对不上

怎么用？两个场合：

- **写作时自查**：写完对照每个通病的「自查」逐条过，踩没踩坑一目了然；
- **评审时诊断**：技能出了问题，先叫出名字——能命名才能对症下药，而不是笼统地"改改看"。

### 臃肿

**症状**：臃肿（sprawl）——文档太长——**即使每一行都还有用、都无可替代**。注意力在过量内容中被稀释，每一行额外内容都是需要保持相关性的负担。

**解法**：信息层级——把参考内容沿阶梯披露到指针后面，按分支或序列拆分，让每条路径只带自己需要的。详见 Part 2（信息层级）与 2.5（何时拆分）。

**自查**：你的 SKILL.md 有多少行？超过 300 行就该考虑分层了（见 2.3）。

### 过度披露

**症状**：过度披露（over-disclosure，本文补充）——与臃肿相反——把该内联的内容也推了出去。顶部空了，但关键材料（核心约束、完成条件）被藏在指针后，代理每次要用都得先多跳一层，"按步骤执行"变成掷硬币。

**解法**：分支测试（2.1）——所有分支都要的，必须内联；只有部分分支用的，才推到指针后。披露的目的是保护层级，不是瘦身表演。

**自查**：把外部文件全删掉，你的 SKILL.md 还完整吗？如果缺了核心约束，就是过度披露。

### 沉积

**症状**：沉积（sediment）——旧层在文档里越堆越厚，从不清理。沉积是那种"因为增加感觉安全、删除感觉危险"而积下来的陈旧层——直到你必须往下钻穿它们，才能找到还活着的内容。

**解法**：每次迭代至少删除一行旧指令，并定期审查（review）清理不再适用的内容。每一行都问**相关性（relevance）**：它还影响文档要做的事吗？从不影响任务的行（纯粹的解释、应当披露的分支）或随世界变化而过时的行，都该删。

**自查**：你的 SKILL 中有没有即使 agent 不执行也不影响质量的内容？如果有，它可能是沉积。

### 过早完成

**症状**：过早完成（premature completion）——代理在一个步骤做到一半就跳到下一步，原因通常是完成条件模糊——"分析代码"没有定义"分析到什么时候算完"。

**解法**：每个步骤加可检查且彻底（clarity + demand）的完成条件，详见 4.2。

**自查**：你的完成条件里有没有"理解""分析"这类不可检查的词？

### 空指令

**症状**：空指令（no-op）——指令说了模型本来就会做的事——你白白花掉 token 和上下文空间，行为却没有任何变化。

**测试**：去掉这句指令，代理的行为变不变？不变 → 空指令（no-op）。这个判断是**相对模型的，不是相对读者的**：两个人对 no-op 有分歧，是对默认行为有分歧——用跑文档来裁决，而不是辩论。

**常见的空指令**：

- "Be thorough."——代理本来就不会故意不全面
- "Write good code."——代理本来就在尝试写好代码
- "Use your expertise."——毫无信息量

**解法**：要么删掉，要么用一个更强的引导词替换（"be thorough" → "relentless"）。一句失效时**删整句**，不要只删词——测试同样给引导词打分：一个弱到敌不过默认行为的词（代理本来就挺 thorough 时写 "be thorough"）是空指令，修法是更强的词，不是另一种写法。

### 否定陷阱

**症状**：否定陷阱（negation）——"不要做 X"反而激活了 X 的概念。这是认知心理学中的"白熊效应"——告诉一个人不要想白熊，他满脑子都是白熊。类似机制在 LLM 中也存在：禁令是个弱修饰词，被强烈激活的概念会盖过它，禁令有一半会被读成"去做那件事"。

**解法**：正面陈述目标行为，详见 1.4。

```markdown
# ✗ 否定

Don't add unnecessary dependencies.

# ✓ 正面

Only add a dependency when the same functionality cannot be achieved with
standard library or existing dependencies.
```

### 重复

**症状**：重复（duplication）——同一个意思出现在多个地方——它花费维护成本和 token，还会把它的地位抬得比它在信息层级里的真实位置更高。重复是引导词的意外反面：引导词是**故意**重复一个 token（从不重复意思），重复则是无意的同义反复。

**注意区分**：散落（scattering，见 2.2）是把一个意思拆碎在多处；重复是同一个意思抄在多处。

**环境即事实**：`package.json` 脚本、配置文件、目录布局、`--help` 输出也是事实来源——文档复述它们就是**缓存**，只有查找很贵时才值得做。缓存 agent 找不到的东西：不成文的约定、选择背后的原因、配置不会承认的坑。把"一个文件一条命令"的查找留给环境，那里不会过期。

**解法**：每个意思保持**单一事实来源**——一个权威位置，改行为就是改一处。

### 描述错配

**症状**：描述错配（description mismatch，本文补充）——最常犯的错误——

- **模型调用型**技能描述写得像用户调用型：太短，没有触发词。结果代理永远不会自动激活它。
- **用户调用型**技能描述写得像模型调用型：一长串触发词，白白吃掉每轮的上下文。

**解法**：按 Part 3 的决策原则确定调用模式，再按对应风格写描述（模型调用型 = 多分支触发；用户调用型 = 一句话）。

---

## Part 7：应用

### 7.1 贯穿自测（理解性）

学完本文，拿手头一个 SKILL（或你熟悉的技能）逐条过一遍，答不上来的条目回到对应 Part：

- [ ] 它的 description 是哪种指针？措辞决定触发吗？一分支一触发词吗？（2.4、3.2）
- [ ] 内容在信息层级上放对了吗？该内联的内联、该披露的披露了吗？（Part 2）
- [ ] 完成条件清晰且有 demand 吗？代理会不会过早完成？（4.2）
- [ ] 调用模式选对了吗？耗的是哪种负载，值不值？（3.1）
- [ ] 它触犯了八个通病里的哪一个？（Part 6）

### 7.2 写作流程检查点（验收性）

一份 SKILL 从构思到发布，需要反复走过三个阶段：**决策 → 构建 → 修剪**。操作步骤留给了 [`docs/skills-guide.md`](./skills-guide.md)，这里只列出理念层的检查点。

**启动前**

- [ ] **确定调用模式**：用户调用还是模型调用？前者省上下文负载、后者省认知负载——这样取舍值不值？（见 1.2、3.1）
- [ ] **确定结构模式**：顺序型 → 角色-行动；知识型 → 参考优先；多入口 → 分支；内容量大 → 叠加渐进披露

**构建中**

- [ ] **角色声明**放开头（Primacy 效应），**关键约束**放末尾（Recency 效应）
- [ ] 每个步骤有可检查、有要求度（demand）的**完成条件**，不要用"分析代码"这种模糊终点（见 4.2）
- [ ] **指针措辞**：一个分支一个触发词，引导词前置——措辞决定触发可靠性（见 2.4）
- [ ] **示例**优于抽象描述——至少一个完整的输入/输出对（见 4.1）

**发布前**

- [ ] **空指令测试**：去掉一句，行为不变？删掉——注意这是相对模型的判断（见 Part 6）
- [ ] **否定检查**：搜索"不要"、"避免"，看能否转为正面表述
- [ ] **引导词机会**：一段话能否用一个词取代？搜索"be thorough"、"carefully"等模糊词
- [ ] **描述验证**：模型调用型的描述是否列出了真正独立的分支，而非同义词堆砌？
- [ ] **负载审计**：常驻的东西每轮都在消耗上下文——这份上下文负载换来了什么？值不值？（见 1.2）
- [ ] **反例测试**：在描述或 README 中注明什么情况下不触发

---

## 附录：一页速查

**概念**：可预测性（同流程 ≠ 同输出）· 两种负载（上下文 ↔ 认知，此消彼长）· 三支柱（结构一致性 / 词汇锚定 / 边界明确）

**信息层级四问**：放多深（三阶阶梯）· 放什么旁边（同位聚合）· 拆不拆（两条路：披露按分支 / 拆分按序列、调用方式）· 怎么找得到（指针措辞）

**调用**：模型调用 = 花上下文换发现力；用户调用 = 花认知换零负载；router 只能提示，不能触发

**完成条件**：清晰（防过早完成）+ demand（驱动 legwork）；先锐化边界，再考虑拆分

**模式**：角色-行动（顺序任务）· 参考优先（规则多）· 分支（多入口）+ 渐进披露（内容量大时叠加）

**八通病**：臃肿 · 过度披露 · 沉积 · 过早完成 · 空指令 · 否定陷阱 · 重复 · 描述错配

**检查点**：启动前（模式 + 调用）→ 构建中（位置 / 条件 / 指针 / 示例）→ 发布前（空指令 / 否定 / 引导词 / 描述 / 负载 / 反例）

---

## 参考来源

- [Matt Pocock `writing-for-agents`][wfa] — 核心概念（可预测性、两种负载、信息层级、完成条件、引导词、修剪）
- [Matt Pocock `SKILL-MECHANICS.md`][wfa-mechanics] — 技能特有机制（frontmatter、调用方式、路由技能）
- 本地研究副本：[`research/writing-for-agents/`](../research/writing-for-agents/)（含 SKILL-MECHANICS.md）
- [Matt Pocock/skills][mattpocock-skills] — 社区 SKILL 实施案例
- [Anthropic/skills][anthropic-skills] — 平台方 SKILL 实施案例（frontend-design, mcp-builder）
- [Vercel agent-skills][vercel-agent-skills] — 产品类 SKILL 实施案例（react-best-practices, web-design-guidelines）

[wfa]: https://www.skills.sh/mattpocock/skills/writing-for-agents
[wfa-mechanics]: https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-for-agents/SKILL-MECHANICS.md
[grill-me]: https://www.skills.sh/mattpocock/skills/grill-me
[tdd]: https://www.skills.sh/mattpocock/skills/tdd
[triage]: https://www.skills.sh/mattpocock/skills/triage
[handoff]: https://www.skills.sh/mattpocock/skills/handoff
[setup-skills]: https://www.skills.sh/mattpocock/skills/setup-matt-pocock-skills
[frontend-design]: https://www.skills.sh/anthropics/skills/frontend-design
[mcp-builder]: https://www.skills.sh/anthropics/skills/mcp-builder
[vrbp]: https://www.skills.sh/vercel-labs/agent-skills/vercel-react-best-practices
[mattpocock]: https://www.skills.sh/mattpocock/skills
[anthropic]: https://www.skills.sh/anthropics/skills
[vercel]: https://www.skills.sh/vercel-labs/agent-skills
[mattpocock-skills]: https://github.com/mattpocock/skills
[anthropic-skills]: https://github.com/anthropics/skills
[vercel-agent-skills]: https://github.com/vercel-labs/agent-skills
