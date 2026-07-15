# 可预测的 SKILL：Matt Pocock 写作方法论

> 理念、模式与反模式 · 基于 Matt Pocock [`writing-great-skills`][wgs] 及其流行技能分析

---

## 目录

- [Part 1：核心理念](#part-1核心理念)
- [Part 2：信息层级](#part-2信息层级)
- [Part 3：触发设计](#part-3触发设计)
- [Part 4：步骤写作](#part-4步骤写作)
- [Part 5：结构模式](#part-5结构模式)
- [Part 6：反模式](#part-6反模式)
- [Part 7：将理念落地](#part-7将理念落地)

---

## Part 1：核心理念

### 1.1 可预测性（Predictability）是根本目标

一个 SKILL 存在的意义，是从一个随机系统中约束出确定性。[Matt Pocock][mattpocock] 在 [`writing-great-skills`][wgs] 中把核心目标定义为 **可预测性（predictability）**——不是每次输出相同的内容，而是代理（Agent）每次执行相同的**流程**。

对比下面两个 SKILL 的开头：

```yaml
# ❌ 低可预测性
name: review-code
description: Review code changes.
---
Analyze the diff and find bugs.
```

```yaml
# ✅ 高可预测性
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

差别在哪？第二份 SKILL 告诉了代理：做什么、按什么顺序做、做到什么程度算完、什么不该做。第一份什么都没有——代理只能凭猜测。

可预测性不意味着僵化。它意味着**同一技能在同一输入下走相同流程**，这样调试（debugging）、优化、信任才成为可能。

### 1.2 可预测性的三大支柱

| 支柱                                    | 含义                               | 对应结构                                                        |
| --------------------------------------- | ---------------------------------- | --------------------------------------------------------------- |
| **结构一致性（Structure Consistency）** | 代理知道自己处于流程的哪一步       | 编号步骤、标题层级                                              |
| **词汇锚定（Vocabulary Anchoring）**    | 用模型预训练中的稳定概念做锚点     | 引导词（leading word，见 1.3）                                  |
| **边界明确（Boundary Clarity）**        | 什么该做、什么不该做、什么该问用户 | 约束（constraints）、完成条件（completion criterion）、异常处理 |

基于 Matt Pocock 的可预测性理念，我们可以将其拆解为三个可操作的维度。在一个成熟的 SKILL 中，三条都应该被满足。

### 1.3 引导词（Leading Word）

引导词（leading word）是模型预训练中已经存在的紧凑概念。把它嵌入 SKILL 中，可以用最少的令牌（token）数量激活稳定的行为模式。

**案例**：

| 写作方式                                                | 令牌数 | 效果                                    |
| ------------------------------------------------------- | ------ | --------------------------------------- |
| "Be fast, deterministic, and low-overhead in your loop" | 9      | 一般                                    |
| 只用 `tight`                                            | 1      | 更好——模型对 "tight loop" 已有稳定理解  |
| "Be thorough and don't miss any edge cases"             | 8      | 模糊                                    |
| 只用 `relentless`                                       | 1      | 更好——"relentless" 隐式地覆盖了"不遗漏" |

[Matt Pocock][mattpocock] 的 [`grill-me`][grill-me]（557K 安装量）用 `relentlessly interviewed` 作为描述（description）。一个词就传达了"持续追问、不放过任何分支"的行为模式。

**关于引导词的一些注意：**

- 引导词是锦上添花，不是核心替代。优先保证指令清晰具体，仅在上下文高度受限（如 description 字段）或确认该词在目标领域有稳定语义时使用。
- 引导词的效果依赖模型规模——在 Claude 4 或 GPT-4 上效果显著，在小模型上可能无效。
- 如果你不确定一个词是否比一段描述更好，保留描述。精确 > 简洁。

如何发现引导词：

- 你的 SKILL 中用了一段话来描述一个概念 → 问自己：模型预训练中是否有这个词？
- 理想候选项：工程术语（`tight`、`red`、`tracer bullet`）、强动词（`relentless`、`relentlessly`）

### 1.4 何时放松可预测性

可预测性不是所有场景都需要同等的严格程度：

| 任务类型                 | 可预测性要求 | 原因                   |
| ------------------------ | ------------ | ---------------------- |
| 部署、代码审查、数据操作 | 严格         | 操作有副作用，错不起   |
| 文案写作、头脑风暴       | 中等         | 需要创意空间           |
| 纯生成（写诗、构思）     | 宽松         | 流程本身反而抑制创造力 |

一条原则：**任务的风险等级决定可预测性的严格程度。**

### 1.5 适用边界说明

本文的方法论主要来自 Matt Pocock 的 `writing-great-skills`，其技能是为 Claude Code 生态设计的。大部分概念（可预测性、完成条件、渐进式揭露）具有跨平台通用性，但以下内容在非 Claude Code 的代理平台上可能需要调整：

- **引导词**的效果在 Claude 4 / GPT-4 上显著，在小模型上可能无效
- **用户调用 vs 模型调用**的区分方式（`disable-model-invocation` 字段）是 Claude Code / skills.sh 生态特有的
- **指针措辞**（"See GLOSSARY.md"）在支持主动文件加载的代理上有效，在不支持的系统上退化为建议

如果你在其他平台上编写 SKILL，建议先验证平台的指令注入机制和上下文加载策略。

---

## Part 2：信息层级

### 2.1 三层模型

一个 SKILL 有多个文件可用，但核心决策是：**什么内容放 SKILL.md 本体，什么内容放外部？**

参考 [Matt Pocock][mattpocock] 的信息层级（information hierarchy）和 [Anthropic][anthropic] [`mcp-builder`][mcp-builder] 的模式：

```
第一层：SKILL.md 本体
  放：核心步骤、必须遵守的规则、入口流程、关键约束
  不放：可被"按需查阅"的细节、只有部分分支需要的参考

第二层：同目录文件（GLOSSARY.md、references/*.md）
  放：术语定义、背景知识、长规则列表、完整示例
  触发方式：代理读到指针（pointer）时主动去查阅（"See GLOSSARY.md"）

第三层：外部系统（scripts/、外部 API）
  放：可执行的脚本、外部文档、运行时数据
  适用：代理不需要理解内容，只需要执行
```

**判断标准：分支测试（Branch Test）**

> 如果一个信息需要被**所有分支**使用 → 放第一层  
> 如果只被**部分分支**使用 → 放到第二层，用指针引用  
> 如果代理不需要理解只需要执行 → 放到第三层

### 2.2 案例对照

**[Anthropic][anthropic] [`mcp-builder`][mcp-builder]**（安装量 89K）是渐进式揭露（progressive disclosure）的典型代表：

```
mcp-builder/
├── SKILL.md              # 核心步骤（约 300 行）
├── reference/
│   ├── mcp_best_practices.md    # MCP 通用规范
│   ├── node_mcp_server.md       # TypeScript 实现指南
│   ├── python_mcp_server.md     # Python 实现指南
│   └── evaluation.md            # 评估指南
```

SKILL.md 中每个阶段（Phase）只引用对应的 reference 文件。代理只加载它当时需要的上下文。

**对比 [Matt Pocock][mattpocock] [`grill-me`][grill-me]（557K）**——这是一个**极薄**的 SKILL：

```
grill-me/
└── SKILL.md    # 核心步骤，不到 100 行
```

不需要外部文件，因为它的逻辑只有一条"持续追问直到所有分支闭合"。薄到不需要分层。

> 这引出一个重要原则：**不要过早抽象（over-abstract）。** 如果你的技能只有 50 行，先别建 reference 目录。等 SKILL.md 超过 200 行且不同分支需要不同知识时再分层。作为参考，300 行通常是一个警戒线——超过这个长度后，无论是否分支，都值得考虑分层。

### 2.3 指针措辞（Pointer Wording）

第二层的引用靠 SKILL.md 中的**指针（pointer）**触发。指针的措辞决定了代理是否能可靠地加载外部内容。

| 推荐                                                                                | 不推荐                        |
| ----------------------------------------------------------------------------------- | ----------------------------- |
| "See GLOSSARY.md for full definitions."                                             | "更多参考见 GLOSSARY.md"      |
| "Load [reference/evaluation.md](./reference/evaluation.md) when you reach Phase 4." | "参考文件在 reference 目录下" |
| 明确告诉代理什么时候加载                                                            | 模糊地提一句                  |

---

## Part 3：触发设计

### 3.1 两种触发模式

一个 SKILL 由谁调用、怎么调用，决定了它的描述该怎么写：

|            | 用户调用（User-Invoked）         | 模型调用（Model-Invoked）    |
| ---------- | -------------------------------- | ---------------------------- |
| 配置       | `disable-model-invocation: true` | 省略该字段                   |
| 描述       | 一句话使用场景（面向人类）       | 多分支触发词列表（面向模型） |
| 上下文成本 | 零                               | 描述每轮都加载               |
| 认知成本   | 你得记住它的存在                 | 零                           |
| 适合场景   | 工具型、有副作用、低频使用       | 通用能力、被其他 skill 调用  |

**决策原则**：

> 只有当代理必须自主调用该技能、或其他技能需要调用它时，才使用模型调用。如果只能靠手动输入调用，就设为用户调用，不浪费上下文。
>
> — [Matt Pocock][mattpocock], [`writing-great-skills`][wgs]

### 3.2 描述（Description）的写作

#### 模型调用型

描述要做两件事：说清技能是什么 + 列出应该触发的场景。每个场景是一个**分支（branch）**，而不是一个同义词。

```yaml
# ✅ 好的模型调用描述（多分支触发）
description: >
  Review a pull request diff for logic bugs, security issues, and
  performance regressions. Use when:
  - the user says "review this PR" or "check my changes"
  - the user mentions "code review" or "CR"
  - another skill verifies output before completing

# ❌ 不好的模型调用描述（同义词堆砌）
description: >
  Review code changes. Use when the user wants code review, CR, PR review,
  change review, diff review, review changes, or needs a code check.
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

每个 "Use when" 后面列的是**独立的触发场景**，不是同义词。

#### 用户调用型

描述只对人类说一句话——说清什么时候该用。

```yaml
# 用户调用型描述
name: grill-me
description: Get relentlessly interviewed about a plan or design until every
  branch of the decision tree is resolved.
disable-model-invocation: true
```

[Matt Pocock][mattpocock] 的 [`grill-me`][grill-me] 就是一个标准样本。一句话，不占上下文，人类看了就知道什么时候该输 `/grill-me`。

### 3.3 路由器技能（Router Skill）模式

当用户调用型 SKILL 多到记不住时，写一个**路由器技能（router skill）**：

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

[Matt Pocock][mattpocock] 的 [`setup-matt-pocock-skills`][setup-skills]（396K 安装量）就是这个模式——一个入口，列出所有技能。

---

## Part 4：步骤写作

### 4.1 步骤结构

SKILL 中的指令有三种基本的组织方式：

| 结构                       | 适用场景              |
| -------------------------- | --------------------- |
| 编号步骤（numbered steps） | 有顺序依赖的任务      |
| 子弹列表（bullet lists）   | 无顺序的规则集/检查点 |
| 纯段落（prose）            | 简单任务或纯参考型    |

这并非要求所有技能都必须编号——如果你的技能是纯参考型（如 [`writing-great-skills`][wgs]），子弹列表或标题分节更合适。具体选择参考 Part 5 的模式选择决策树。

#### 位置效应：指令的位置影响权重

LLM 对指令不同位置的关注度不均衡：开头（primacy effect）和末尾（recency effect）的内容权重最高，中间区域（"lost in the middle"）的内容容易被稀释。这意味着：

- **最重要的指令——角色声明、核心约束——放在开头或末尾**，不要埋在中间段落
- **每个步骤的完成条件紧跟在步骤描述之后**，不要集中到文档末尾
- 如果需要强调中间的内容，用显式的元指令（"注意，以下规则很重要"）对抗位置衰减

### 4.2 完成条件（Completion Criterion）

每个步骤末尾需要明确的完成条件。完成条件必须：

- **可检查（checkable）**：代理能判断 done 还是 not done
- **彻底（exhaustive）**：没有遗漏的边角

```markdown
# ❌ 模糊的完成标准

1. Analyze the diff.
2. Find bugs.

# ✅ 可检查的完成标准

1. Read the diff line by line.
   Complete when every changed line has been read.
2. Identify bugs in three categories: logic, security, performance.
   Complete when all three categories have been checked.
```

[Matt Pocock][mattpocock] 的 [`writing-great-skills`][wgs] 描述了完成标准的重要性：

> 严格的完成标准驱动彻底的执行（thorough legwork）——无论技能是否有步骤结构，因为"每一条规则都应用了"和"每一个步骤都做完了"具有同等的约束力。

**直觉检查**：如果一行写"找 bug"，代理找了 2 个就停了——这是过早完成（premature completion）。如果写"找到所有 bug，确保每条代码行至少被检查过一次"，代理会继续直到没有漏的。

### 4.3 分支（Branching）

如果同一 SKILL 有多个入口，明确定义分支条件：

```markdown
If the user's goal is:

- A new feature → follow Steps 1-5
- A bug fix → skip to Step 3
- A refactor → skip to Step 4

In all cases, end with Step 6 (review).
```

参考 [`triage`][triage]（[Matt Pocock][mattpocock]，369K），它根据问题类型走不同路径。分支条件写在开头，代理不需要读完整份 SKILL 就知道该走哪条路。

---

## Part 5：结构模式

以下是从多个发布者的流行 SKILL 中提取的四种结构模式。每个模式是一个可套用的骨架，但需要根据你的任务类型调整。

### 模式选择

```
你的任务是否有严格的先后顺序？
   ├── 是 → Role-Action 模式（模式 1）
   └── 否 → 质量是否取决于代理知道多少规则？
              ├── 是 → Reference-First 模式（模式 2）
              └── 否 → 是否有多个入口路径？
                         ├── 是 → Branch 模式（模式 3）
                         └── 否 → 上述任一模式 + 渐进式揭露（模式 4）做内容组织
```

这些模式可以组合使用，例如 Branch + Role-Action 是常见组合（每个分支内再用编号步骤）。

### 模式 1：角色-行动（Role-Action）

**适用**：需要代理以特定身份执行的多步骤任务。

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

| 来源                                                        | 角色声明                                                | 安装量 |
| ----------------------------------------------------------- | ------------------------------------------------------- | ------ |
| [Matt Pocock][mattpocock] [`grill-me`][grill-me]            | 无角色声明（直接指令式："Run a `/grilling` session."）  | 557K   |
| [Anthropic][anthropic] [`frontend-design`][frontend-design] | "Approach this as the design lead at a small studio..." | 661K   |
| [Matt Pocock][mattpocock] [`tdd`][tdd]                      | "You are a TDD practitioner."                           | 442K   |

三个技能，三个完全不同的领域，但都使用"角色声明 + 步骤 + 完成条件"的结构。

**适用检查**：任务是否可以拆解为线性步骤？如果步骤之间有严格的先后顺序，用这个模式。

### 模式 2：参考优先（Reference-First）

**适用**：需要大量规则/约束的知识密集型任务。代理不需要顺序执行，而是在规则约束下自主判断。

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

| 来源                                                    | 形式             | 安装量 |
| ------------------------------------------------------- | ---------------- | ------ |
| [Matt Pocock][mattpocock] [`writing-great-skills`][wgs] | 全参考型，无步骤 | 168K   |
| [Anthropic][anthropic] [`mcp-builder`][mcp-builder]     | 步骤 + 深度参考  | 89K    |

观察：纯参考模式需要更高质量的写作，因为没有步骤帮代理"结构化"思考。如果你选择这个模式，确保你的规则足够清晰和具体。

**适用检查**：任务的产品质量主要取决于代理知道多少规则（而非执行顺序）？如果是，用这个模式。

### 模式 3：分支（Branch）

**适用**：同一 SKILL 有多个入口/路径。

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

| 来源                                         | 分支逻辑                                   | 安装量 |
| -------------------------------------------- | ------------------------------------------ | ------ |
| [Matt Pocock][mattpocock] [`triage`][triage] | 按问题类型（bug / feature / question）分支 | 369K   |
| [Vercel][vercel] 各技能                      | 用 "Use when" 列触发场景，隐式分支         | ≤550K  |

**关键**：分支条件必须**互斥且覆盖全面（mutually exclusive and collectively exhaustive）**。如果分支有重叠，代理可能选错路。如果有未覆盖的场景，代理会自己猜——猜对猜错都不好。

### 模式 4：渐进式揭露（Progressive Disclosure）

**适用**：内容量大的技能。信息组织策略，而非独立的写法模式——详见 Part 2（信息层级）。当 SKILL.md 超过 200 行且不同分支需要不同知识时，使用此策略。

**骨架**：

```
<skill>/
├── SKILL.md              # 核心步骤 + 指针
├── references/
│   ├── detailed-rules.md  # 分支 A 需要的知识
│   └── examples.md        # 分支 B 需要的示例
└── scripts/               # 可执行脚本
```

**关键**：指针（pointer）措辞决定代理是否能可靠加载。明确写"When you reach Phase 3, load references/evaluation.md"比"参考文件在 references 目录下"有效得多。

---

## Part 6：反模式

以下反模式都有命名。命名本身就是价值——团队能用同一个词指代同一个问题。

### 臃肿（Sprawl）

SKILL.md 超过 500 行，所有东西塞在一个文件里。症状：看不完、不好改、代理迷失在中间段落。

**解法**：信息层级——把参考内容移出 SKILL.md，只保留核心步骤和关键约束。详见 Part 2（信息层级）。

**自查**：你的 SKILL.md 有多少行？超过 300 行就该考虑分层了。

### 沉积（Sediment）

旧上下文从之前的调用泄漏到当前技能。Matt Pocock 将沉积描述为"因为增加感觉安全、删除感觉危险而导致的多余层"——每个版本都在旧层上堆新层，从不清理。

**解法**：每次迭代时至少删除一行旧指令。设置定期审查（review）来清理已经不再适用的上下文。

**自我检查**：你的 SKILL 中有没有即使 agent 不执行也不会影响质量的内容？如果有，它可能是沉积。

### 过早完成（Premature Completion）

代理在一个步骤做到一半就跳到下一步。原因通常是完成条件模糊——"分析代码"没有定义"分析到什么时候算完"。

**解法**：每个步骤加可检查且彻底的完成条件。参考 Part 4.2。

**案例**：[Matt Pocock][mattpocock] 在 [`writing-great-skills`][wgs] 中明确指出：

> 如果后面的步骤对代理可见（post-completion steps），它会被"做完"的诱惑吸引而跳过当前步骤的深度执行。

**修复示例**：

```markdown
# ❌

1. Review the changes.

# ✅

1. Review each changed file.
   Complete when every file in the diff has been opened and examined.
```

### 空指令（No-op）

指令说了模型本来就会做的事。你支付了令牌和上下文空间，但行为没有变化。

**测试**：去掉这句指令，代理的行为变不变？不变 → 它是空指令（no-op）。

**常见的空指令**：

| 指令                  | 为什么是空指令           |
| --------------------- | ------------------------ |
| "Be thorough."        | 代理本来就不会故意不全面 |
| "Write good code."    | 代理本来就在尝试写好代码 |
| "Use your expertise." | 这是废话                 |

**解法**：要么删掉，要么用一个更强的引导词替换（"be thorough" → "relentless"）。

### 否定陷阱（Negation）

"不要做 X"反而激活了 X 的概念。这是认知心理学中的"白熊效应"——告诉一个人不要想白熊，他满脑子都是白熊。类似机制在 LLM 中也存在。

**解法**：正面陈述目标行为。只在硬性安全护栏（"不要删除生产数据库"）时保留否定，且必须和正面指令配对。

```markdown
# ❌ 否定

Don't add unnecessary dependencies.

# ✅ 正面

Only add a dependency when the same functionality cannot be achieved with
standard library or existing dependencies.
```

### 重复（Duplication）

Matt Pocock 定义的 duplication 是指代理在处理过程中反复产生相同输出（如重复审查同一段代码），这不仅浪费 token，还表明技能没有引导代理向前推进。

**解法**：确保每个步骤默认推进流程，而不是允许代理在原地循环。如果代理可能在两个步骤之间来回，显式说明"如果已经处理过，跳过"。

### 描述错配（Description Mismatch）

最常犯的错误：

- **模型调用型**技能描述写得像用户调用型：太短，没有触发词。结果代理永远不会自动激活它。
- **用户调用型**技能描述写得像模型调用型：一长串触发词，白白吃掉每轮的上下文。

**解法**：根据 Part 3 的决策树确定调用模式，然后按对应风格写描述。

---

## Part 7：将理念落地

一份 SKILL 从构思到发布，需要反复穿过本文阐述的三层：**决策 → 构建 → 修剪**。操作步骤留给了 [`docs/skills-guide.md`](./skills-guide.md)，这里只列出理念层的检查点。

### 启动前

- [ ] **确定调用模式**：用户调用还是模型调用？前者省上下文、后者省认知成本
- [ ] **确定结构模式**：顺序型 → Role-Action；知识型 → Reference-First；多入口 → Branch；内容量大 → 叠加 Progressive Disclosure

### 构建中

- [ ] **角色声明**放开头（Primacy 效应），**关键约束**放末尾（Recency 效应）
- [ ] 每个步骤有可检查的**完成条件**，不要用"分析代码"这种模糊终点
- [ ] **示例**优于抽象描述——至少一个完整 I/O 对

### 发布前

- [ ] **空指令测试**：去掉一句，行为不变？删掉
- [ ] **否定检查**：搜索"不要"、"避免"，看能否转为正面表述
- [ ] **引导词机会**：一段话能否用一个词取代？搜索"be thorough"、"carefully"等模糊词
- [ ] **描述验证**：模型调用型的描述是否列出了真正独立的分支，而非同义词堆砌？
- [ ] **反例测试**：在描述或 README 中注明什么情况下不触发

---

## 参考来源

- [Matt Pocock `writing-great-skills`][wgs] — SKILL 写作的核心概念（可预测性、引导词、渐进式揭露）
- [Matt Pocock/skills][mattpocock-skills] — 社区 SKILL 实施案例
- [Anthropic/skills][anthropic-skills] — 平台方 SKILL 实施案例（frontend-design, mcp-builder）
- [Vercel agent-skills][vercel-agent-skills] — 产品类 SKILL 实施案例（react-best-practices, web-design-guidelines）
- [Firebase agent-skills][firebase-agent-skills] — 产品类 SKILL 案例
- [Agent Skills 规范][agentskills-io] — 跨平台 SKILL 规范

[wgs]: https://www.skills.sh/mattpocock/skills/writing-great-skills
[grill-me]: https://www.skills.sh/mattpocock/skills/grill-me
[tdd]: https://www.skills.sh/mattpocock/skills/tdd
[triage]: https://www.skills.sh/mattpocock/skills/triage
[handoff]: https://www.skills.sh/mattpocock/skills/handoff
[setup-skills]: https://www.skills.sh/mattpocock/skills/setup-matt-pocock-skills
[frontend-design]: https://www.skills.sh/anthropics/skills/frontend-design
[mcp-builder]: https://www.skills.sh/anthropics/skills/mcp-builder
[mattpocock]: https://www.skills.sh/mattpocock/skills
[anthropic]: https://www.skills.sh/anthropics/skills
[vercel]: https://www.skills.sh/vercel-labs/agent-skills
[mattpocock-skills]: https://github.com/mattpocock/skills
[anthropic-skills]: https://github.com/anthropics/skills
[vercel-agent-skills]: https://github.com/vercel-labs/agent-skills
[firebase-agent-skills]: https://github.com/firebase/agent-skills
[agentskills-io]: https://agentskills.io/

---
