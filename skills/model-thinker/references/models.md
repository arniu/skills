# Models Reference — Scott Page's The Model Thinker

24 models for multi-model thinking. Each entry: **Use when** (problem fit), **Logic** (core framework), **Output** (analysis structure), **See also** (related models).

---

## 1. Normal Distributions

**Alias**: 正态分布, Bell Curve

**Use when**: Analyzing phenomena where values cluster around an average — heights, test scores, measurement errors. Identifying normal ranges and outliers.

**Logic**:

- 68-95-99.7 rule: ±1σ = 68%, ±2σ = 95%, ±3σ = 99.7%
- Central Limit Theorem: sum of independent random variables approximates normal
- Key metrics: mean, standard deviation, skewness, kurtosis

**Output**: Assess normality, report mean & stddev, identify outliers, explain deviation from normal.

**See also**: Power-Law (opposite tail behavior), Linear Models (CLT underpins regression assumptions)

---

## 2. Power-Law Distributions

**Alias**: 幂律分布, Long Tails

**Use when**: Analyzing extreme inequality — wealth, city sizes, web traffic, word frequency, 80/20 patterns.

**Logic**:

- Few individuals hold majority of resources (mathematical basis of 80/20 rule)
- Linear relationship on log-log plot
- Key: exponent α, cutoff point, distinguish from lognormal via statistical tests

**Output**: Detect power-law fit, estimate α, analyze head vs tail share, assess inequality.

**See also**: Normal (opposite shape), Rugged Landscape (fat tails affect fitness landscape)

---

## 3. Linear Models

**Alias**: 线性模型

**Use when**: Analyzing how multiple factors independently affect an outcome — salary determinants, housing prices, marketing ROI.

**Logic**:

- Y = β₀ + β₁X₁ + β₂X₂ + ... + ε
- Each variable has independent marginal contribution
- Key: coefficient sign/magnitude, significance, R², interaction effects

**Output**: Identify key drivers and weights, evaluate marginal contributions, flag nonlinearities and interactions.

**See also**: Concavity (nonlinear complement), Normal (CLT enables hypothesis testing)

---

## 4. Concavity and Convexity

**Alias**: 凹凸性

**Use when**: Analyzing diminishing or increasing returns — investment returns, learning curves, economies of scale. Optimizing resource allocation.

**Logic**:

- Concave: diminishing marginal returns (learning curves, utility)
- Convex: increasing marginal returns (network effects, epidemic growth)
- Jensen's inequality: expectation of convex function ≥ function of expectation
- Combination determines optimal strategy

**Output**: Determine concavity/convexity, identify inflection points, find optimal allocation, strategic implications.

**See also**: Linear Models (linear is boundary case), Threshold Models (inflection points analogous to tipping points)

---

## 5. Models of Value and Power

**Alias**: 价值与权力模型

**Use when**: Analyzing who holds power and how value is distributed — bargaining, supply chain power, political coalitions.

**Logic**:

- Shapley value: each participant's marginal contribution determines power
- Core: set of stable allocations
- Voting power index: influence in informal coalitions

**Output**: Identify power sources for each party, calculate relative influence, evaluate allocation fairness and stability.

**See also**: Game Theory (Shapley value from cooperative games), Collective Action (power distribution affects participation)

---

## 6. Network Models

**Alias**: 网络模型

**Use when**: Analyzing how relationship structure affects information flow, influence, and resilience — social networks, org charts, knowledge diffusion.

**Logic**:

- Nodes + edges; degree centrality, betweenness, closeness
- Small-world, scale-free, random network types
- Network density, clustering coefficient, bridging

**Output**: Describe network structure, identify key nodes, trace influence/information paths, assess resilience.

**See also**: Local Interaction (interaction happens on networks), Broadcast/Diffusion/Contagion (spread depends on network structure)

---

## 7. Broadcast, Diffusion, Contagion

**Alias**: 广播、扩散与传染病模型

**Use when**: Analyzing how information, behaviors, or diseases spread — marketing campaigns, rumors, epidemic modeling.

**Logic**:

- Broadcast: one-to-many transmission
- Diffusion: gradual spread through social influence
- SIR/SIS: susceptible-infected-recovered models
- Threshold: adoption cascade after critical mass

**Output**: Classify spread type, predict speed and reach, identify key nodes and thresholds, suggest interventions.

**See also**: Network Models (spread depends on network topology), Threshold Models (adoption decisions at individual level)

---

## 8. Entropy

**Alias**: 熵

**Use when**: Quantifying uncertainty and information content — classification purity, information gain, system disorder.

**Logic**:

- H = -Σ pᵢ log(pᵢ)
- Higher entropy = more uncertainty
- Information gain = original entropy − conditional entropy

**Output**: Quantify system uncertainty, calculate information gain, identify key factors reducing uncertainty.

**See also**: Linear Models (information gain for feature selection), Learning Models (entropy drives Bayesian updating)

---

## 9. Random Walks

**Alias**: 随机游走

**Use when**: Analyzing sequences that appear patterned but may be random — stock prices, animal foraging, opinion shifts.

**Logic**:

- Each step independent of prior steps
- Long-term trends may be accumulated random fluctuations
- Key: distinguish actual trend from random walk
- Brownian motion = continuous-form random walk

**Output**: Test whether sequence follows random walk, identify false trends, assess predictability.

**See also**: Markov Models (random walk is a special Markov process)

---

## 10. Path Dependence

**Alias**: 路径依赖

**Use when**: Analyzing how historical events lock in current states — tech standards (QWERTY), institutional evolution, career paths.

**Logic**:

- Early random events locked in via positive feedback
- High switching costs even when superior alternatives exist
- Key concepts: Lock-in, Increasing Returns

**Output**: Identify lock-in points, analyze key historical decisions, assess switching cost and feasibility.

**See also**: Systems Dynamics (positive feedback drives lock-in), Threshold Models (tipping point after which path solidifies)

---

## 11. Local Interaction Models

**Alias**: 局部互动模型

**Use when**: Analyzing how local behavior produces macro patterns — residential segregation, cultural convergence, innovation diffusion.

**Logic**:

- Individuals interact only with neighbors
- Local rules → global emergent patterns
- Schelling segregation model is canonical

**Output**: Describe local interaction rules, emergent macro patterns, key parameter sensitivity (e.g., tolerance thresholds).

**See also**: Network Models (local interaction happens in network neighborhoods), Threshold Models (individual decisions depend on neighbor behavior)

---

## 12. Lyapunov Functions and Equilibria

**Alias**: 李雅普诺夫函数与均衡

**Use when**: Analyzing whether a system tends toward stable equilibrium — ecosystem balance, economic equilibrium, dynamic system stability.

**Logic**:

- If Lyapunov function exists and decreases over time, system moves toward equilibrium
- Multiple equilibria possible; initial conditions determine final state
- Key: equilibrium count, stability, basin of attraction

**Output**: Identify equilibrium states, assess stability, analyze conditions for reaching equilibrium.

**See also**: Systems Dynamics (complementary approach), Game Theory (Nash equilibrium stability analysis)

---

## 13. Markov Models

**Alias**: 马尔可夫模型

**Use when**: Analyzing state transition probabilities and long-run distributions — user behavior sequences, weather patterns, browsing paths.

**Logic**:

- Next state depends only on current state (Markov property)
- Transition probability matrix determines long-run behavior
- Steady-state distribution: long-run probability distribution
- Absorbing state: state from which system cannot leave

**Output**: Define state space, build transition matrix, predict steady-state distribution, identify absorbing states and cycles.

**See also**: Random Walks (equal-probability Markov special case), Learning Models (reinforcement learning as Markov decision process)

---

## 14. Systems Dynamics Models

**Alias**: 系统动力学模型

**Use when**: Analyzing complex systems with feedback loops and time delays — economic growth, supply chain oscillations, climate change.

**Logic**:

- Stocks and flows
- Positive feedback (reinforcing) and negative feedback (balancing) loops
- Time delays cause oscillations and overshoot

**Output**: Map stock-flow diagram, identify feedback loops, analyze time delays, predict system behavior (growth/oscillation/decay).

**See also**: Lyapunov Functions (equilibrium stability analysis), Path Dependence (positive feedback causes lock-in)

---

## 15. Threshold Models with Feedbacks

**Alias**: 阈值模型与反馈

**Use when**: Analyzing tipping points in collective behavior — group action, financial panics, tech adoption, social movements.

**Logic**:

- Each person has a participation threshold (% of others already participating)
- Threshold distribution determines aggregate behavior
- Feedback: more participants → lower remaining thresholds
- Key: Tipping Point, Cascade Effect

**Output**: Infer threshold distribution, locate tipping point, assess cascade probability, suggest intervention strategies.

**See also**: Collective Action (participation is threshold-based), Broadcast/Diffusion/Contagion (tipping point in adoption)

---

## 16. Spatial and Hedonic Choice

**Alias**: 空间与享乐选择

**Use when**: Analyzing location or feature-based preferences — site selection, product positioning, voting strategy.

**Logic**:

- Spatial: voters/consumers choose closest option in preference space
- Hedonic: product value = function of its features
- Median voter theorem, Hotelling model

**Output**: Map preference space, locate ideal points and competitor positions, suggest differentiation strategy.

**See also**: Game Theory (Hotelling model is a game), Concavity (utility curvature affects choice)

---

## 17. Game Theory Models

**Alias**: 博弈论模型

**Use when**: Analyzing strategic interactions — price competition, negotiations, arms races, auctions.

**Logic**:

- Players, strategies, payoffs
- Nash equilibrium: no player wants to unilaterally change
- Prisoner's dilemma, coordination games, hawk-dove
- Repeated games: future interaction changes present behavior

**Output**: Frame as game (players-strategies-payoffs), identify Nash equilibria, analyze cooperation vs defection, classify game type.

**See also**: Cooperation Models (cooperation in repeated games), Mechanism Design (reverse game theory)

---

## 18. Models of Cooperation

**Alias**: 合作模型

**Use when**: Analyzing how cooperation emerges and persists — team collaboration, international agreements, open-source communities.

**Logic**:

- Direct reciprocity: tit-for-tat
- Indirect reciprocity: reputation mechanisms
- Network reciprocity: clustering promotes cooperation
- Group selection: inter-group competition

**Output**: Assess conditions for cooperation, identify reciprocity mechanisms, evaluate defection risk and punishment, assess stability.

**See also**: Game Theory (cooperation as repeated game equilibrium), Collective Action (cooperation solves free-rider problem)

---

## 19. Collective Action Problems

**Alias**: 集体行动问题

**Use when**: Analyzing individual rationality leading to collective irrationality — free-riding, tragedy of commons, team contribution, environmental action.

**Logic**:

- Individuals have incentive not to contribute while enjoying collective benefits
- Free-rider problem: contribution cost > personal benefit share
- Tragedy of commons: shared resource overused
- Solutions: selective incentives, monitoring, decentralized governance

**Output**: Diagnose collective action dilemma, cost-benefit analysis, propose solutions (institutional/incentive/monitoring).

**See also**: Game Theory (prisoner's dilemma as formalization), Cooperation Models (reciprocity mitigates free-riding)

---

## 20. Mechanism Design

**Alias**: 机制设计

**Use when**: Designing rules and institutions to achieve desired outcomes — auction design, school admissions, organ donation matching.

**Logic**:

- Reverse game theory: set goal first, then design rules
- Incentive compatibility: truthful participation is optimal strategy
- Key properties: efficiency, fairness, budget balance
- Vickrey-Clarke-Groves (VCG) mechanisms

**Output**: Define goals, analyze incentives, propose mechanism, assess feasibility.

**See also**: Game Theory (mechanism design is reverse game theory), Collective Action (mechanisms solve free-riding)

---

## 21. Signaling Models

**Alias**: 信号传递模型

**Use when**: Analyzing signaling under information asymmetry — education as ability signal, brand as quality signal, certifications.

**Logic**:

- Sender has hidden type; receiver needs to distinguish
- Effective signal condition: cost negatively correlates with type
- Separating equilibrium vs pooling equilibrium
- Key: signal cost, credibility, signal-type correlation

**Output**: Diagnose information asymmetry, assess signal effectiveness, determine equilibrium type, suggest signaling strategy.

**See also**: Game Theory (signaling as incomplete information game), Mechanism Design (incentive design involves signaling)

---

## 22. Models of Learning

**Alias**: 学习模型

**Use when**: Analyzing how individuals and groups learn from experience — organizational learning, skill acquisition, belief updating.

**Logic**:

- Individual learning: trial-and-error, Bayesian updating, imitation
- Social learning: learning from others' experience
- Exploration vs exploitation
- Learning rate vs sample size

**Output**: Identify learning type, assess learning efficiency, evaluate exploration-exploitation balance, identify learning barriers.

**See also**: Multi-Armed Bandit (formal model of exploration-exploitation), Rugged Landscape (learning is search on fitness landscape)

---

## 23. Multi-Armed Bandit Problems

**Alias**: 多臂老虎机问题

**Use when**: Analyzing exploration-exploitation trade-offs under uncertainty — A/B testing, drug trials, recommendation systems.

**Logic**:

- N options (arms), each with unknown reward distribution
- Explore (try new options) vs exploit (choose known best)
- Strategies: ε-greedy, UCB, Thompson sampling
- Regret: gap between chosen strategy and optimal

**Output**: Frame exploration-exploitation dilemma, suggest optimal strategy, analyze regret.

**See also**: Learning Models (bandit as mathematical formalization of learning), Rugged Landscape (search strategy mirrors exploration)

---

## 24. Rugged-Landscape Models

**Alias**: 崎岖景观模型

**Use when**: Analyzing optimization complexity — product design, organizational change, strategic choice.

**Logic**:

- NK model: N components, K = interdependence between components
- Low K → smooth landscape (easy to optimize)
- High K → rugged landscape (many local optima, global optimum hard to find)
- Local search vs long jumps (simulated annealing)

**Output**: Assess landscape ruggedness, suggest optimization strategy (local search/jump/simulated annealing), identify local optima traps.

**See also**: Multi-Armed Bandit (search strategy = exploration-exploitation), Power-Law (fat tails increase landscape ruggedness)
