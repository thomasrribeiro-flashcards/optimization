+++
order = 6
subject = "Math"
tags = ["math", "optimization", "simplex", "interior-point", "lp-algorithms", "pivoting"]
+++

# Optimization — Simplex and Interior-Point Methods

## 6.1 The Simplex Idea

Q: What is the high-level [simplex algorithm]?
A: Start at any basic feasible solution (BFS) — a vertex of the feasible polyhedron. At each iteration, move along an edge of the polyhedron to an adjacent vertex with lower objective value. Terminate when no neighboring vertex improves. Dantzig (1947). The breakthrough: LP doesn't require an "all-points-search" — a walk on vertices suffices by the fundamental theorem.

Q: Why does the simplex algorithm move between adjacent BFSs?
A: Because adjacent BFSs differ by exactly one [basic variable] — one variable enters the basis, another leaves. This "pivot" operation is a single linear-algebra step (Gauss elimination on one column). Walking between adjacent vertices is computationally cheap; walking between arbitrary vertices would be much harder.

## 6.2 Basic Variables and Basis

C: For an LP in standard form $\min \mathbf{c}^T \mathbf{x}$ s.t. $A\mathbf{x} = \mathbf{b}$, $\mathbf{x} \geq \mathbf{0}$: a [basis] $B$ is a set of $m$ linearly independent columns of $A$. The corresponding $B$-variables are [basic]; the rest are [nonbasic].

Q: How is a [basic feasible solution] (BFS) constructed from a basis $B$?
A: Set nonbasic variables to zero; solve $A_B \mathbf{x}_B = \mathbf{b}$ for the basic variables (since $A_B$ is $m \times m$ invertible). Feasibility requires $\mathbf{x}_B \geq \mathbf{0}$. Each BFS corresponds to choosing $m$ columns to make nonzero — combinatorial structure: $\binom{n}{m}$ potential bases.

## 6.3 Reduced Costs

C: The [reduced cost] of a nonbasic variable $x_j$ is $\bar{c}_j = c_j - \mathbf{c}_B^T A_B^{-1} \mathbf{a}_j$ — the net change in objective per unit increase in $x_j$ after accounting for forced adjustments to basic variables.

Q: How does simplex decide which nonbasic variable enters the basis?
A: [Dantzig's rule]: pick the nonbasic variable with the most negative reduced cost $\bar{c}_j$ — largest per-unit objective decrease. [Bland's rule]: pick the lowest-index variable with negative $\bar{c}_j$ — slower per step but prevents cycling. [Steepest-edge] pivoting: scale $\bar{c}_j$ by edge length for faster practical convergence. Different pivoting rules = different per-step tradeoffs.

Q: Why does optimality require all reduced costs to be nonnegative?
A: Because a negative reduced cost $\bar{c}_j < 0$ means increasing $x_j$ from zero strictly decreases the objective — so the current BFS isn't optimal. When all $\bar{c}_j \geq 0$ for nonbasics, no single-variable move improves: by LP's convexity, no move improves at all. Terminate with optimal BFS.

## 6.4 The Ratio Test

Q: How does simplex decide which variable [leaves the basis]?
A: Ratio test: as the entering variable $x_j$ increases, basic variables change proportionally; the first one to hit zero determines which leaves. Pick $i^* = \arg\min_{i : d_i > 0} x_{B,i}/d_i$ where $d_i$ is the change per unit increase in $x_j$. Ensures the new BFS remains feasible.

Q: What happens if no basic variable binds as the entering variable increases?
A: The LP is [unbounded]: $x_j$ can grow arbitrarily, decreasing the objective indefinitely. Simplex terminates with a certificate of unboundedness — an explicit feasible direction of objective decrease. Detection is immediate from the ratio test: no positive $d_i$ means no variable ever hits zero.

## 6.5 Degeneracy and Cycling

C: A BFS is [degenerate] if one or more basic variables equals zero — more than $n - m$ constraints are active, creating redundant basis choices.

Q: Why can simplex cycle (loop forever) at a degenerate BFS?
A: Because degenerate pivots can leave the BFS unchanged (same point, different basis). Without a tie-breaking rule, simplex can cycle through a set of bases without objective improvement. Fix: [Bland's rule] (always pick lowest-index candidate) provably prevents cycling; [lexicographic pivoting] is another standard anti-cycling rule.

## 6.6 Initialization: Two-Phase and Big-M

Q: How does simplex find an initial BFS?
A: If the LP is in the form $\min \mathbf{c}^T\mathbf{x}$ s.t. $A\mathbf{x} \leq \mathbf{b}$ ($\mathbf{b} \geq \mathbf{0}$), $\mathbf{x} \geq \mathbf{0}$: slack variables $\mathbf{s} = \mathbf{b}$ give an initial BFS. Otherwise: introduce [artificial variables] in the equality constraints and solve a [Phase I] LP that minimizes the sum of artificials — driving them to zero produces a BFS of the original.

Q: What is the [Big-M method] as an alternative to two-phase simplex?
A: Add artificial variables with a huge penalty coefficient $M$ in the objective: $\min \mathbf{c}^T \mathbf{x} + M \sum_i a_i$. If the original LP is feasible, the artificials are driven to zero at optimality. Simpler conceptually than two-phase but numerically unstable — $M$ too small lets artificials persist, $M$ too large causes roundoff. Two-phase is preferred in practice.

## 6.7 Complexity and Practical Performance

Q: What is the worst-case complexity of the simplex method?
A: [Exponential]: Klee–Minty cubes (1972) construct LPs on which simplex visits all $2^n$ vertices. Most pivoting rules admit similar examples. Despite this, simplex is FAST IN PRACTICE — typical runtime is $O(m + n)$ iterations. The gap between worst-case and typical performance is one of the most famous open questions in optimization theory.

Q: Why does simplex dominate in practice despite exponential worst case?
A: Because real-world LPs don't look like Klee–Minty cubes: structure (sparsity, network structure) keeps typical iteration counts modest ($O(m + n)$). Decades of engineering (revised simplex, LU updates, presolve, scaling) make commercial simplex solvers (CPLEX, Gurobi) handle millions of variables. Theoretical worst case ≠ practical performance.

## 6.8 Interior-Point Methods

Q: What does an [interior-point method] (IPM) for LP do?
A: Approaches the optimum through the INTERIOR of the feasible region, not along vertices. Combine a [logarithmic barrier] $-\sum \log x_i$ that keeps iterates strictly feasible with Newton's method that follows a [central path] to the optimum. Unlike simplex, IPMs have provably polynomial complexity ($O(n^{3.5} L)$ for Karmarkar) — resolving the theoretical concern.

Q: Why is $-\sum \log x_i$ used as the barrier for $\mathbf{x} \geq \mathbf{0}$?
A: Because it blows up to $+\infty$ as any $x_i \to 0^+$, discouraging the iterate from leaving the interior. Smooth and convex — Newton's method applies directly. The [Lagrangian relaxation] $\min \mathbf{c}^T\mathbf{x} - t \sum \log x_i$ s.t. $A\mathbf{x} = \mathbf{b}$ has a unique minimizer for each $t > 0$; as $t \to 0$, these minimizers trace the central path to the optimal vertex.

Q: What is the [central path]?
A: The set of minimizers $\mathbf{x}(t)$ of $\min \mathbf{c}^T \mathbf{x} - t \sum \log x_i$ s.t. $A\mathbf{x} = \mathbf{b}$ as $t$ varies over $(0, \infty)$. The IPM follows this path from large $t$ (deep interior) to small $t$ (optimal vertex) by alternating Newton steps with shrinking $t$. Each Newton step costs $O(n^3)$; polynomial iteration count gives polynomial total complexity.

## 6.9 Simplex vs. Interior-Point

Q: When does [simplex] win over [interior-point]?
A: Warm-starting: simplex reuses prior BFSs efficiently, crucial for branch-and-bound solving many related LPs. Sparse, small LPs: simplex has smaller per-iteration cost. Problems where exact vertex solutions matter (transportation, assignment). Modeling tricks (hot restart after constraint addition) favor simplex.

Q: When does [interior-point] win over [simplex]?
A: Large dense LPs (cost per Newton step grows as $n^3$ but iterations are few — $O(\sqrt{n} \log(1/\varepsilon))$). LPs arising from SOCP / SDP — IPMs are the only scalable option there. LPs where polynomial worst-case guarantees matter (theoretical proofs). Modern commercial solvers automatically choose based on problem structure.

## 6.10 Revised Simplex

Q: What does [revised simplex] compute differently from naïve simplex?
A: Stores only the basis inverse $A_B^{-1}$ (or a factorization) and updates it incrementally each pivot via $O(m^2)$ operations. Computes reduced costs and ratio-test entries on demand — avoiding the $O(mn)$ per-step cost of updating the full tableau. Standard in production: memory-efficient and faster for sparse LPs.

## 6.11 Presolve and Scaling

Q: What is [LP presolve]?
A: A preprocessing phase that simplifies the LP before handing it to simplex / IPM: removes empty rows, forced bounds, redundant constraints, and duplicated variables; tightens variable bounds via constraint propagation; fixes variables with implied singleton equalities. Can shrink a problem by 50%+ — often more impactful than pivoting-rule choice.

Q: Why is [scaling] applied to LP matrices before solving?
A: Because well-scaled matrices (columns and rows of similar magnitude) give simplex / IPM better numerical behavior: pivoting decisions less affected by roundoff, linear solves more stable. Standard: Curtis–Reid scaling, geometric scaling. Solvers scale automatically; user-provided matrices with wild coefficient spreads (ratios $10^{10}$ or more) exhaust even good preconditioners.

## 6.12 Computational Reality

P: A small LP: $\max 3x_1 + 5x_2$ subject to $x_1 \leq 4$, $2x_2 \leq 12$, $3x_1 + 2x_2 \leq 18$, $x_1, x_2 \geq 0$.
S:
**IDENTIFY**: 2 variables, 3 constraints. Simple enough to graph.

**PLAN**: Convert to standard form, find initial BFS via slacks, iterate simplex.

**EXECUTE**: Add slacks $s_1, s_2, s_3 \geq 0$: $x_1 + s_1 = 4$, $2x_2 + s_2 = 12$, $3x_1 + 2x_2 + s_3 = 18$. Initial BFS: $\mathbf{x} = (0, 0, 4, 12, 18)$. Reduced costs of $x_1, x_2$: $-3, -5$. Most negative: $x_2$ enters. Ratio test: min of $(12/2, 18/2) = 6$ at $s_2$; $s_2$ leaves. New BFS: $x_2 = 6$. Iterate: $x_1$ enters next, $s_3$ leaves. Terminate at $(x_1, x_2) = (2, 6)$, objective $3(2) + 5(6) = 36$.

**EVALUATE**: Optimal at a vertex, as LP theory predicted. Two pivots sufficed for this small example. The method generalizes unchanged to millions of variables — only the per-pivot linear algebra gets heavier.
