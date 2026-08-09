---
name: model-thinker
description: >-
  Analyzes a problem with the best-fitting model(s) from Scott Page's The Model Thinker.
  Problem domains: free-rider & collective action, power-law inequality, network & information flow,
  diffusion & adoption tipping points, path dependence & lock-in, strategic games, systems dynamics.
  Invoke on /model-thinker, "Model Thinking", "Scott Page", or a problem in one of those domains.
---

# Model Thinker Skill

## Language Adaptation

- **Model names**: English user -> English name; non-English user -> user's language name (parenthesize English name)
- **Reference**: `references/models.md` is reference only. All output in user's language

## Core Workflow

### Step 1: Understand Problem

Clarify through questions:

- Background and domain (business / social / technical / personal?)
- Desired insight type (prediction / explanation / optimization / strategy?)
- Available data and information

### Step 2: Match Model

Select best model from `references/models.md`. Consider:

**Selection principles:**

- One primary model (best fit) + 0-2 supplementary models (additional perspective)
- Precision over quantity
- If multiple fit, pick simplest and strongest
- If problem vague, pick most broadly applicable

**Quick selection guide** (full definitions in reference):

| Problem characteristic                 | Recommended model                            |
| -------------------------------------- | -------------------------------------------- |
| Data distribution, outliers            | Normal / Power-Law                           |
| Factor analysis, attribution           | Linear / Concavity                           |
| Power structure, resource allocation   | Value & Power                                |
| Networks, information flow             | Network                                      |
| Spread, diffusion, epidemics           | Broadcast / Diffusion / Contagion            |
| Uncertainty, information content       | Entropy                                      |
| Time series, trend analysis            | Random Walk / Markov                         |
| Historical lock-in, path analysis      | Path Dependence                              |
| Local -> global emergence              | Local Interaction                            |
| System stability                       | Lyapunov / Systems Dynamics                  |
| Collective behavior, tipping points    | Threshold                                    |
| Competition, cooperation, strategy     | Game Theory / Cooperation / Mechanism Design |
| Free riding, tragedy of commons        | Collective Action                            |
| Information asymmetry                  | Signaling                                    |
| Learning, adaptation                   | Learning / Multi-Armed Bandit                |
| Complex optimization, innovation       | Rugged Landscape                             |
| Location, positioning, differentiation | Spatial & Hedonic Choice                     |

### Step 3: Explain Choice

Explain model selection to user:

- Core idea (one sentence)
- Why it fits this problem
- Model limitations (when it doesn't apply)

### Step 4: Apply Model Analysis

Analyze step by step per model's framework. Output structure matches model's character (see each model's "Output" in `references/models.md`).

**Generic analysis framework** (adjust per model. Headers and content in user's language):

```
## Model Analysis: {model name}

### Problem Definition
{problem statement}

### Framework Application
{step-by-step analysis per model dimensions}

### Key Insights
{core findings revealed by model}

### Limitations
{aspects this model doesn't cover — model logic simplifies; reality is messier}
```

### Step 5: Synthesis & Recommendations

After model analysis, step back for broader view:

**Cover multiple dimensions:**

- Actionable recommendations based on model analysis
- Risks and trade-offs: risks revealed by model + risks not covered
- Alternatives: if model analysis inconclusive, suggest alternate paths

**Model is a lens; output stays comprehensive.**

### Step 6: Supplementary Perspective (Optional)

For complex problems, briefly add second model's perspective after primary analysis:

```
### Supplementary: {second model}
{what different insights this model adds}
```

If the user follows up, continue deeper analysis or switch models.

## Example

**User:** "Our team of 10 has an 80/20 output problem — 2 people do 80% of the work. Why?"

**Your response structure:**

```
## Recommended Model: Collective Action Problems

Why this model: free riding + cost-benefit mismatch in group work.

### Framework Application
- Cost-benefit analysis: contribution cost borne individually, benefit shared across 10
- Free rider dynamics: low observability enables shirking
- Equilibrium: top performers compensate -> others disengage further

### Key Insights
Structural incentive problem, not a people problem.

### Limitations
Doesn't capture skill gaps or management failures.

### Recommendations
1. Make individual contributions visible
2. Introduce selective incentives
3. Reduce team size into smaller pods
```

## Important Reminders

- **Use only the 24 models** in `references/models.md`.
- **Explain in plain language** — assume no prior model knowledge; define each concept before using it.
