+++
order = 11
subject = "Math"
tags = ["math", "optimization", "integer-programming", "ilp", "branch-and-bound", "cutting-planes", "combinatorial"]
+++

# Optimization — Integer Programming

## 11.1 What Integer Programming Is

Q: What is an [integer linear program] (ILP)?
A: An LP with the additional requirement that some or all variables take integer values: $\min \mathbf{c}^T \mathbf{x}$ s.t. $A\mathbf{x} \leq \mathbf{b}$, $\mathbf{x} \geq \mathbf{0}$, $x_i \in \mathbb{Z}$ for $i \in I$. If all $x_i$ are integer, it's a pure IP; if some are continuous and some integer, it's a [mixed-integer linear program] (MILP). Integer constraints vastly expand modeling power but make the problem NP-hard.

Q: What is a [binary integer program]?
A: An IP where all integer variables are restricted to $\{0, 1\}$. Models yes/no decisions: include/exclude an item, assign/not assign a resource, open/close a facility. Binary IPs are a subclass of IP but retain NP-hardness; many combinatorial problems (SAT, TSP, set cover, max cut) are naturally binary IPs.

## 11.2 Why Integer Programming Is NP-Hard

Q: Why is integer programming NP-hard while LP is polynomial-time?
A: Because the feasible set of an IP is DISCONTINUOUS — a lattice intersected with a polytope. The LP convex hull argument fails: optima can live at interior lattice points, not just vertices. Karp's 21 NP-complete problems reduce to IP; even deciding feasibility of $A\mathbf{x} = \mathbf{b}$, $\mathbf{x} \in \{0, 1\}^n$ is NP-complete. Integer constraints kill convexity.

## 11.3 LP Relaxation

Q: What is the [LP relaxation] of an integer program?
A: Drop integrality constraints: $x_i \in \mathbb{Z}$ becomes $x_i \in \mathbb{R}$ (or $x_i \in [0, 1]$ for binaries). Solves in polynomial time. Its optimum is a LOWER BOUND on the IP optimum (minimization) — provides the bounding function for branch-and-bound. Tighter relaxations yield faster IP solvers.

Q: Why does LP relaxation produce a lower bound, not the true integer optimum?
A: Because the LP relaxation's feasible set is LARGER (contains all fractional points), so its minimum is lower (or equal). The gap between LP and IP optima is called the [integrality gap]: small gap → LP-based methods work well; large gap → need tighter cuts or specialized formulations. Good IP modeling minimizes the integrality gap.

## 11.4 Branch-and-Bound

Q: Describe the [branch-and-bound] algorithm for integer programming.
A: Tree search: at each node, solve LP relaxation; if integer-feasible, update incumbent; otherwise, pick a fractional variable $x_i = f$ and BRANCH into two subproblems with $x_i \leq \lfloor f \rfloor$ and $x_i \geq \lceil f \rceil$. PRUNE nodes whose LP bound exceeds the incumbent (no improvement possible). Continue until all nodes pruned. Workhorse of commercial IP solvers.

Q: Why does branch-and-bound work efficiently despite IP's NP-hardness?
A: Because tight LP relaxations prune large portions of the search tree early — nodes with LP bounds worse than the incumbent are immediately discarded. Modern solvers add [cutting planes], employ sophisticated branching rules, and use [heuristics] for early incumbents. Despite exponential worst case, branch-and-bound solves billion-variable IPs in practice.

## 11.5 Cutting Planes

Q: What is a [cutting plane] in integer programming?
A: A valid inequality $\mathbf{a}^T \mathbf{x} \leq b$ satisfied by all integer-feasible points but CUTTING OFF a fractional LP optimum. Add to the LP relaxation to tighten the bound without excluding integer solutions. Gomory's mixed-integer cuts (1958) were the first general cutting planes; modern cuts include [lift-and-project], [clique], [flow cover], and [MIR] cuts.

Q: How do [Gomory cuts] work?
A: From the simplex tableau at a fractional LP optimum, a row with fractional entries generates an inequality that MUST hold for integer feasibility (via the fractional-part argument): $\sum_j \{a_{ij}\} x_j \geq \{b_i\}$. Adding this to the LP cuts off the current fractional optimum while preserving all integer points. Iteratively generates a sequence of tighter relaxations.

## 11.6 Branch-and-Cut

Q: What is [branch-and-cut]?
A: Branch-and-bound augmented with cutting planes at each node of the search tree: before branching, add cuts to tighten the local LP relaxation. The cuts are [locally valid] (at that subtree) or [globally valid] (anywhere). Dominant paradigm in modern MILP solvers (CPLEX, Gurobi, SCIP). Combines combinatorial branching with polyhedral analysis.

## 11.7 Common IP Formulations

Q: Formulate the [knapsack problem] as an IP.
A: Variables $x_i \in \{0, 1\}$ for each item $i$. Constraint: $\sum_i w_i x_i \leq W$ (capacity). Objective: $\max \sum_i v_i x_i$ (total value). Binary IP with one constraint. NP-hard despite its simple structure; pseudo-polynomial DP exists for integer weights.

Q: Formulate the [traveling salesman problem] as an IP.
A: Variables $x_{ij} \in \{0, 1\}$ = 1 if edge $(i,j)$ is in the tour. Constraints: each city entered and left exactly once ($\sum_j x_{ij} = 1$, $\sum_i x_{ij} = 1$); subtour elimination ($\sum_{i, j \in S} x_{ij} \leq |S| - 1$ for all $S$). Exponentially many subtour constraints — typically added lazily via separation.

Q: Formulate the [assignment problem] as an IP.
A: Variables $x_{ij} \in \{0, 1\}$ = 1 if worker $i$ assigned to task $j$. Constraints: each worker assigned to exactly one task ($\sum_j x_{ij} = 1$); each task to exactly one worker ($\sum_i x_{ij} = 1$). Objective: $\min \sum_{ij} c_{ij} x_{ij}$. Special: its LP relaxation is INTEGER (Birkhoff-von Neumann) — solvable as an LP! The Hungarian algorithm does this in $O(n^3)$.

Q: Formulate the [set cover problem] as an IP.
A: Given sets $S_1, \dots, S_m$ covering a universe $U$. Variables $x_j \in \{0, 1\}$ = 1 if set $S_j$ chosen. Constraints: $\sum_{j : e \in S_j} x_j \geq 1$ for each element $e \in U$ (cover every element). Objective: $\min \sum_j c_j x_j$. NP-hard; LP relaxation gives an $O(\log n)$-approximation via randomized rounding.

## 11.8 Logical Modeling with Binary Variables

Q: How do binary variables encode [if-then constraints] "$A \Rightarrow B$"?
A: Big-M formulation: if binary $y = 1$ means "A holds," then add $B \leq M \cdot y$ (or appropriate form). When $y = 0$, constraint is trivially satisfied; when $y = 1$, $B$ must hold within $M$. $M$ is a large upper bound; choose tightest valid $M$ to strengthen the LP relaxation. Applies to disjunctions, implications, fixed costs.

Q: How does a [fixed-charge] model encode "pay $f$ if $x > 0$, pay $0$ if $x = 0$"?
A: Introduce binary $y$ with $x \leq M y$ (so $x > 0 \Rightarrow y = 1$). Cost: $f y + c x$. When $x = 0$, $y$ can be $0$ (saving the fixed cost); when $x > 0$, $y$ must be $1$. Core model of facility location, plant opening, economies of scale. Big-M choice critical: too large gives a weak LP relaxation.

## 11.9 Strong vs. Weak Formulations

Q: Why do two mathematically equivalent IP formulations have DIFFERENT computational difficulty?
A: Because they have different LP relaxations. A [strong formulation] has an LP relaxation whose optimum is CLOSE to the IP optimum (small integrality gap); a [weak formulation] has a loose LP relaxation (large gap). Strong formulations solve faster in branch-and-bound. Modeling art: choose formulations with tighter LP relaxations, even if they have more variables or constraints.

Q: How does [disaggregation] strengthen an IP formulation?
A: Break an aggregate constraint into many smaller ones. Example: "$\sum_{j \in J} x_{ij} \leq M y_i$" is weak; replacing with "$x_{ij} \leq y_i$ for each $j \in J$" (one constraint per pair) is stronger. Each disaggregated constraint is individually tighter in the LP relaxation. Trade: more constraints but better LP bounds — usually a net win for solver performance.

## 11.10 Heuristics and Approximation

Q: Why are [heuristics] important for large-scale IP despite the existence of exact solvers?
A: Because exact branch-and-bound can be slow on huge problems; heuristics find GOOD (not necessarily optimal) solutions quickly, providing incumbents that tighten branch-and-bound. [Primal heuristics]: rounding, local search, feasibility pump, large neighborhood search. Modern solvers use dozens of heuristics interleaved with search.

Q: What is an [approximation algorithm] for an NP-hard IP?
A: A polynomial-time algorithm with a provable approximation ratio $\alpha$: the algorithm's solution value is within a factor $\alpha$ of the optimum. Example: $(1 - 1/e)$-approximation for max-coverage via greedy. LP relaxation + rounding is a major design technique. Contrast: heuristics have no quality guarantees.

## 11.11 Lagrangian Relaxation for IP

Q: What is [Lagrangian relaxation] applied to an integer program?
A: Dualize a subset of "complicating" constraints $A_2 \mathbf{x} \leq \mathbf{b}_2$ into the objective: $\min \mathbf{c}^T \mathbf{x} + \boldsymbol{\mu}^T (A_2 \mathbf{x} - \mathbf{b}_2)$ s.t. $A_1 \mathbf{x} \leq \mathbf{b}_1$, $\mathbf{x} \in \mathbb{Z}^n$. For fixed $\boldsymbol{\mu}$, the relaxed problem may be easy (e.g., decomposable). Its value lower-bounds the IP optimum; maximize over $\boldsymbol{\mu}$ via subgradient ascent. Can give tighter bounds than LP relaxation.

## 11.12 A Worked IP

P: Capital-budgeting IP: $\max 10 x_1 + 6 x_2 + 4 x_3$ s.t. $5 x_1 + 3 x_2 + 2 x_3 \leq 10$, $x_i \in \{0, 1\}$. Three projects with profits and costs; budget is 10.
S:
**IDENTIFY**: Binary IP with one budget constraint. Three variables — enumerate $2^3 = 8$ possibilities for tiny case; for illustration, use branch-and-bound with LP relaxation.

**PLAN**: Solve LP relaxation (relax $x_i \in \{0, 1\}$ to $x_i \in [0, 1]$). Branch on first fractional variable.

**EXECUTE**: LP relaxation: rank projects by profit-to-cost ratio: project 1 = 2.0, project 2 = 2.0, project 3 = 2.0 — all tied. LP allows fractional, so $x_1 = x_2 = 1$, $x_3 = 1$ gives cost $5 + 3 + 2 = 10$, profit $10 + 6 + 4 = 20$. All integer, so this IS the IP optimum — no branching needed.

**EVALUATE**: Optimal IP solution: select all three projects. Profit = $20$; budget used = $10$ (tight). Branch-and-bound terminated at the root with an integer LP solution — best case. More realistic examples: LP gives fractional, branching kicks in; cuts may tighten intermediate nodes. For large IPs, the combination of LP relaxation + branching + cuts + heuristics is the engine.
