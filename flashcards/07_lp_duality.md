+++
order = 7
subject = "Math"
tags = ["math", "optimization", "duality", "linear-programming", "weak-duality", "strong-duality", "complementary-slackness"]
+++

# Optimization — Linear Programming Duality

## 7.1 The Dual of an LP

Q: Given the [primal LP] $\min \mathbf{c}^T \mathbf{x}$ s.t. $A\mathbf{x} \geq \mathbf{b}$, $\mathbf{x} \geq \mathbf{0}$, what is its [dual LP]?
A: $\max \mathbf{b}^T \mathbf{y}$ s.t. $A^T \mathbf{y} \leq \mathbf{c}$, $\mathbf{y} \geq \mathbf{0}$. The dual has one variable per primal constraint and one constraint per primal variable. Every LP has a dual; the dual of the dual is the primal again — involutive symmetry.

Q: How do primal and dual dimensions relate?
A: If the primal has $n$ variables and $m$ constraints, the dual has $m$ variables and $n$ constraints. "Tall skinny" primal ↔ "short fat" dual. Why it matters: if one direction has few constraints and many variables, solve the other.

## 7.2 Where the Dual Comes From

Q: Why does every primal constraint $\mathbf{a}_i^T \mathbf{x} \geq b_i$ produce a dual variable $y_i \geq 0$?
A: Because $y_i$ is the [Lagrange multiplier] on that constraint — the "shadow price" for relaxing it. Nonnegativity of $y_i$ reflects the $\geq$ direction of the inequality (would be free if equality, $\leq 0$ if reversed). Multipliers measure the marginal value of each constraint.

Q: Why does every nonnegative primal variable $x_j \geq 0$ produce a dual constraint $(A^T \mathbf{y})_j \leq c_j$?
A: Because to ensure the Lagrangian $\mathbf{c}^T \mathbf{x} - \mathbf{y}^T (A\mathbf{x} - \mathbf{b})$ is bounded below over $\mathbf{x} \geq \mathbf{0}$, the coefficient of each $x_j$ must be nonnegative: $c_j - (A^T\mathbf{y})_j \geq 0$. Otherwise, $x_j \to \infty$ drives the Lagrangian to $-\infty$ and weak duality fails.

## 7.3 Weak Duality

Q: State [weak duality] for LP.
A: If $\mathbf{x}$ is feasible for the primal and $\mathbf{y}$ is feasible for the dual, then $\mathbf{c}^T \mathbf{x} \geq \mathbf{b}^T \mathbf{y}$ — every primal feasible value is an upper bound for every dual feasible value. The primal minimum is at least as large as the dual maximum. Certificate: any dual feasible $\mathbf{y}$ proves "primal optimum $\geq \mathbf{b}^T\mathbf{y}$."

Q: Prove weak duality in one line.
A: $\mathbf{c}^T \mathbf{x} \geq (A^T \mathbf{y})^T \mathbf{x} = \mathbf{y}^T (A\mathbf{x}) \geq \mathbf{y}^T \mathbf{b}$. First step uses $A^T \mathbf{y} \leq \mathbf{c}$ and $\mathbf{x} \geq 0$; last uses $A\mathbf{x} \geq \mathbf{b}$ and $\mathbf{y} \geq 0$. Bound is always in the correct direction, tightness is a separate theorem.

## 7.4 Strong Duality

Q: State [strong duality] for LP.
A: If the primal has an optimal solution, then so does the dual AND their optimal values are EQUAL: $\min \mathbf{c}^T\mathbf{x} = \max \mathbf{b}^T \mathbf{y}$. The duality gap is zero — weak duality's inequality becomes equality at the optimum. Proved via Farkas' lemma (LP-specific; generally fails for nonconvex problems).

Q: Why is strong duality a STRONG property that general nonlinear programs don't automatically get?
A: Because linearity gives tight bounds: LP's "bowl" is the intersection of hyperplanes, and the optimum is pinned to a vertex. For nonconvex problems, the primal optimum may exceed the dual optimum — [duality gap] reflects nonconvexity. LP's strong duality is a hallmark of its clean geometry.

## 7.5 Complementary Slackness

Q: State [complementary slackness] for an LP primal-dual optimal pair $(\mathbf{x}^*, \mathbf{y}^*)$.
A: For each $i$: $y_i^* \cdot (\mathbf{a}_i^T \mathbf{x}^* - b_i) = 0$, and for each $j$: $x_j^* \cdot (c_j - (A^T\mathbf{y}^*)_j) = 0$. Either the primal constraint is active or its dual variable is zero (not both nonzero simultaneously). "Sparsity-enforcing" optimality conditions.

Q: How is complementary slackness used to verify or find an LP optimum?
A: Given candidates $(\mathbf{x}, \mathbf{y})$: check feasibility AND check each complementary slackness product. If all hold, the pair is optimal by strong duality. Computationally: at the optimum, the basis structure determines which variables are zero and which constraints are active — complementary slackness is a certificate.

## 7.6 Economic Interpretation

Q: What is the economic interpretation of dual variables in LP?
A: Dual variable $y_i^*$ is the [shadow price] of constraint $i$: the marginal rate at which the optimal objective decreases per unit relaxation of $b_i$. In the diet problem: $y_i^*$ is how much cheaper the optimal diet becomes per unit decrease in nutrient requirement $i$. Used by managers to price resources and plan expansions.

Q: Why can dual variables be used to identify binding constraints for sensitivity analysis?
A: Because complementary slackness says $y_i^* > 0$ only if constraint $i$ is active ($\mathbf{a}_i^T \mathbf{x}^* = b_i$). Shadow price of an inactive constraint is always zero: changing a non-binding constraint has no marginal effect. Managers focus attention on binding resources — they drive the optimum.

## 7.7 Farkas' Lemma and LP Duality

Q: How does [Farkas' lemma] prove LP strong duality?
A: Farkas gives a "theorem of alternatives" for linear systems: exactly one of (i) $A\mathbf{x} = \mathbf{b}$, $\mathbf{x} \geq \mathbf{0}$ feasible and (ii) $A^T \mathbf{y} \leq \mathbf{0}$, $\mathbf{b}^T \mathbf{y} > 0$ holds. Applied to LP: if dual is feasible, its optimum is attained at the primal value; if primal is feasible, its optimum is attained at the dual value. No gap.

## 7.8 Infeasibility and Unboundedness

Q: What happens to the dual if the primal is [unbounded below]?
A: The dual is [infeasible]: no dual feasible point can give a finite upper bound on an unbounded objective. Formally: weak duality forces $\mathbf{b}^T \mathbf{y} \leq \mathbf{c}^T \mathbf{x}$; if the primal descends to $-\infty$, no $\mathbf{y}$ can satisfy this.

Q: What happens to the primal if the dual is unbounded above?
A: The primal is infeasible. Symmetric: an unbounded dual objective means no primal feasible point can upper-bound it. Combined with primal unboundedness ↔ dual infeasibility, we get FOUR primal-dual combinations:
- primal finite, dual finite (both optimal, values equal)
- primal unbounded, dual infeasible
- primal infeasible, dual unbounded
- BOTH infeasible (can happen — no symmetry breaks this)

## 7.9 Constructing the Dual Systematically

Q: What are the rules for forming the dual of a general LP?
A: Use the "[primal-dual transformation]" rules: (i) primal $\min$ ↔ dual $\max$. (ii) primal constraint $\geq$ ↔ dual variable $\geq 0$; $\leq$ ↔ dual variable $\leq 0$; $=$ ↔ dual variable free. (iii) primal variable $\geq 0$ ↔ dual constraint $\leq$; $\leq 0$ ↔ dual constraint $\geq$; free ↔ dual constraint $=$. A table is easy to memorize; the pattern is sign conventions.

## 7.10 The Dual Simplex Method

Q: What does the [dual simplex method] do?
A: Maintains [dual feasibility] (all reduced costs nonnegative) instead of [primal feasibility] (all basics nonnegative) throughout iterations. At each step, primal constraints may be violated; ratio test fixes them one at a time. Dual-feasibility-preserving. Used as the default when adding constraints to a previously solved LP (the prior optimum is dual feasible for the new problem).

Q: Why is the dual simplex method useful in branch-and-bound for integer programming?
A: Because branching adds constraints to the relaxation, making prior primal solutions infeasible but keeping them dual feasible. Dual simplex re-optimizes from the previous optimum in few pivots rather than restarting from scratch. Without dual simplex, branch-and-bound would be prohibitively slow.

## 7.11 Sensitivity Analysis

Q: How does LP duality enable [sensitivity analysis] of optimal solutions to right-hand-side changes?
A: For small perturbations $\mathbf{b} \to \mathbf{b} + \Delta \mathbf{b}$ (while the optimal basis is unchanged), the change in optimal value is $(\mathbf{y}^*)^T \Delta \mathbf{b}$ — a linear function of the shadow prices. Commercial LP solvers automatically report ranges over which each $\mathbf{b}_i$ can change without the basis changing. Crucial for "what-if" analysis.

Q: Why can cost perturbations $\Delta \mathbf{c}$ also be analyzed via duality?
A: Because by symmetry (duality of dual = primal), cost changes have dual-side interpretation: $\Delta c_j$ changes the reduced cost of $x_j$ and tightens/slackens a dual constraint. The optimal basis remains optimal as long as no reduced cost changes sign. Ranging analysis: compute allowable cost intervals per variable.

## 7.12 A Worked Dual

P: Given primal LP: $\min 4x_1 + 3x_2$ s.t. $x_1 + x_2 \geq 5$, $2x_1 + x_2 \geq 6$, $x_1, x_2 \geq 0$. Form the dual and verify weak duality with a specific pair.
S:
**IDENTIFY**: Two variables, two $\geq$ constraints with $\mathbf{b} = (5, 6)^T$, $\mathbf{c} = (4, 3)^T$, $A = \begin{pmatrix} 1 & 1 \\ 2 & 1 \end{pmatrix}$.

**PLAN**: Apply the LP-duality transformation rules; evaluate primal and dual at sample points.

**EXECUTE**: Dual: $\max 5y_1 + 6y_2$ s.t. $y_1 + 2y_2 \leq 4$, $y_1 + y_2 \leq 3$, $y_1, y_2 \geq 0$. Primal feasible $\mathbf{x} = (3, 2)$: objective $12 + 6 = 18$. Dual feasible $\mathbf{y} = (1, 1)$: checks $1 + 2 = 3 \leq 4$ ✓, $1 + 1 = 2 \leq 3$ ✓, objective $5 + 6 = 11$.

**EVALUATE**: Weak duality holds: $18 \geq 11$. To find optimum: solve either problem via simplex; by strong duality, both attain the same value. For this LP: optimal primal $(1, 4)$ with objective $16$; optimal dual $(2, 1)$ with objective $16$. Gap closes exactly.
