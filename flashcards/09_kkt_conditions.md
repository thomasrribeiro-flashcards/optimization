+++
order = 9
subject = "Mathematics"
tags = ["math", "optimization", "kkt", "inequality-constraints", "constraint-qualification", "complementary-slackness"]
+++

# Optimization — KKT Conditions (Inequality Constraints)

## 9.1 The General Constrained Problem

Q: What is the [general constrained optimization problem] with both equality and inequality constraints?
A: $\min f(\mathbf{x})$ s.t. $g_i(\mathbf{x}) \leq 0$ for $i = 1, \dots, p$ AND $h_j(\mathbf{x}) = 0$ for $j = 1, \dots, m$. This is the most general smooth constrained problem. KKT conditions generalize Lagrange multipliers to this setting, accommodating inequalities.

## 9.2 Active and Inactive Constraints

C: At a feasible point $\mathbf{x}$, an inequality constraint $g_i(\mathbf{x}) \leq 0$ is [active] if $g_i(\mathbf{x}) = 0$ and [inactive] if $g_i(\mathbf{x}) < 0$.

Q: Why does it matter whether an inequality constraint is active at the optimum?
A: Because [active] constraints behave like equality constraints: they bind the solution. [Inactive] constraints exert no local influence — the problem behaves as if they weren't there. Optimality conditions only need to "see" active constraints — huge simplification when most constraints are inactive.

## 9.3 The KKT Lagrangian

C: The [KKT Lagrangian] is $\mathcal{L}(\mathbf{x}, \boldsymbol{\mu}, \boldsymbol{\lambda}) = f(\mathbf{x}) + \sum_i \mu_i g_i(\mathbf{x}) + \sum_j \lambda_j h_j(\mathbf{x})$ with $\mu_i \geq 0$ for inequality constraints.

Q: Why must [KKT multipliers] for inequality constraints be NONNEGATIVE?
A: Because $g_i(\mathbf{x}) \leq 0$ means the feasible side is where $g_i \leq 0$. At an active constraint ($g_i = 0$), the gradient $\nabla g_i$ points OUTWARD (into infeasible region). For $\nabla f$ to be balanced by $\mu_i \nabla g_i$ so that moving inward improves $f$, $\mu_i$ must be nonnegative. Sign captures "resisting an outward push."

## 9.4 The KKT Conditions

Q: Name the four condition groups of the [KKT first-order necessary conditions].
A: Stationarity, primal feasibility, dual feasibility, complementary slackness — these hold at a local minimum $\mathbf{x}^*$, for some multipliers $\boldsymbol{\mu}^*, \boldsymbol{\lambda}^*$, provided a constraint qualification holds.

Q: State the [stationarity] condition of the KKT system.
A: $\nabla f(\mathbf{x}^*) + \sum_i \mu_i^* \nabla g_i(\mathbf{x}^*) + \sum_j \lambda_j^* \nabla h_j(\mathbf{x}^*) = \mathbf{0}$.

Q: State [primal feasibility] and [dual feasibility] in the KKT system.
A: Primal feasibility: $g_i(\mathbf{x}^*) \leq 0$ and $h_j(\mathbf{x}^*) = 0$. Dual feasibility: $\mu_i^* \geq 0$.

## 9.5 Complementary Slackness

Q: State [complementary slackness] in the KKT system.
A: For each inequality constraint: $\mu_i^* g_i(\mathbf{x}^*) = 0$. Either the constraint is active ($g_i = 0$) OR the multiplier is zero ($\mu_i = 0$). Both can hold (degenerate case). Encodes the dichotomy: inactive constraints have zero multiplier (they don't bind); active constraints may have positive multiplier (they shape the solution).

Q: What is [strict complementary slackness], and why does it matter?
A: Strict CS: exactly ONE of $\mu_i^* = 0$ or $g_i(\mathbf{x}^*) = 0$ holds for each $i$ — not both. Matters because strict CS makes the active set locally constant under perturbations, which is needed for smoothness of the solution mapping and fast convergence of active-set methods. Degenerate KKT points (multipliers zero at active constraints) break warm-starts.

## 9.6 Constraint Qualifications

Q: Why are [constraint qualifications] needed for the KKT conditions?
A: Because KKT is only a necessary condition IF the active constraint gradients are "nice enough" — linearly independent, or related regularity. Without a qualification, a local minimum may NOT satisfy KKT (multipliers may not exist). Constraint qualifications ensure the nonlinear constraints locally look like linear ones.

Q: Name three common [constraint qualifications] in increasing generality.
A: LICQ, MFCQ, then Slater's condition (for convex problems).

Q: What is [LICQ (Linear Independence Constraint Qualification)]?
A: The gradients of active inequality constraints together with all equality constraint gradients are linearly independent at $\mathbf{x}^*$. Simplest CQ; implies uniqueness of KKT multipliers. Most commonly assumed in theory.

Q: What is [MFCQ (Mangasarian-Fromovitz Constraint Qualification)]?
A: Equality gradients are linearly independent AND there exists a direction $\mathbf{d}$ with $\nabla g_i(\mathbf{x}^*)^T \mathbf{d} < 0$ for all active $i$ and $\nabla h_j(\mathbf{x}^*)^T \mathbf{d} = 0$. Weaker than LICQ; equivalent to boundedness of the set of KKT multipliers. Standard in modern nonlinear programming theory.

Q: What is [Slater's condition] for convex problems?
A: For convex $f$ and convex inequality constraints $g_i$: there EXISTS a strictly feasible point $\bar{\mathbf{x}}$ with $g_i(\bar{\mathbf{x}}) < 0$ for all $i$ (strict feasibility of all inequalities) and $h_j(\bar{\mathbf{x}}) = 0$ (feasibility of equalities, which must be affine). In the convex case, Slater's condition implies strong duality and KKT sufficiency.

## 9.7 KKT Sufficiency for Convex Problems

Q: Why are KKT conditions SUFFICIENT (not just necessary) for convex problems?
A: Because for convex $f$ and convex $g_i$ (with affine $h_j$), any KKT point is a global minimum. The Lagrangian $\mathcal{L}(\cdot, \boldsymbol{\mu}^*, \boldsymbol{\lambda}^*)$ is convex in $\mathbf{x}$ under these assumptions; stationarity makes $\mathbf{x}^*$ a global minimizer of $\mathcal{L}$; complementary slackness reduces back to $f(\mathbf{x}^*)$. No gap between necessity and sufficiency in convex-land.

## 9.8 Second-Order Conditions for KKT

Q: State the [second-order sufficient condition] for KKT points.
A: At a KKT point $(\mathbf{x}^*, \boldsymbol{\mu}^*, \boldsymbol{\lambda}^*)$: if the Lagrangian Hessian $\nabla^2_{\mathbf{x}\mathbf{x}} \mathcal{L}$ is positive definite on the [critical cone] (tangent directions satisfying $\nabla g_i^T \mathbf{d} = 0$ for active $i$ with $\mu_i > 0$, $\nabla g_i^T \mathbf{d} \leq 0$ for active $i$ with $\mu_i = 0$, $\nabla h_j^T \mathbf{d} = 0$), then $\mathbf{x}^*$ is a strict local minimum. Strict-CS special case: critical cone reduces to equality-constrained tangent space.

## 9.9 KKT for Quadratic Programming

Q: How does the KKT system look for [quadratic programming] $\min \frac{1}{2} \mathbf{x}^T Q\mathbf{x} + \mathbf{c}^T \mathbf{x}$ s.t. $A\mathbf{x} \leq \mathbf{b}$?
A: The KKT conditions are: $Q\mathbf{x}^* + \mathbf{c} + A^T \boldsymbol{\mu}^* = \mathbf{0}$, $A\mathbf{x}^* \leq \mathbf{b}$, $\boldsymbol{\mu}^* \geq \mathbf{0}$, $\boldsymbol{\mu}^{*T}(A\mathbf{x}^* - \mathbf{b}) = 0$. A linear complementarity problem — solvable by active-set methods or interior-point algorithms specialized for convex QP.

## 9.10 Active-Set Methods

Q: What does an [active-set method] for inequality-constrained optimization do?
A: Maintains an estimate of the active constraint set. At each iteration: solve the equality-constrained subproblem assuming the current active set is correct; add/drop constraints based on multiplier signs and feasibility checks. Move until the correct active set is identified and the solution matches. Efficient when active set is small or guessable from a warm start.

Q: Why do active-set methods warm-start efficiently when solving a sequence of related problems?
A: Because the active set changes little between similar problems; starting from the prior active set avoids re-discovering which constraints bind. Branch-and-bound in IP, sensitivity studies, model-predictive control all exploit this. Contrast: interior-point methods don't warm-start as well.

## 9.11 Interior-Point for Nonlinear Programming

Q: How do [interior-point methods] extend to nonlinear programs with inequalities?
A: Replace each inequality $g_i(\mathbf{x}) \leq 0$ with a [log-barrier]: add $-\mu \log(-g_i(\mathbf{x}))$ to the objective (requires strict feasibility). As $\mu \to 0$, the barrier vanishes except at the boundary. A central-path following scheme solves a sequence of unconstrained barrier subproblems. The Lagrangian + barrier has KKT conditions that converge to the original KKT.

## 9.12 A Worked KKT Example

P: Minimize $(x_1 - 2)^2 + (x_2 - 2)^2$ subject to $x_1 + x_2 \leq 3$, $x_1 \geq 0$, $x_2 \geq 0$.
S:
**IDENTIFY**: Convex quadratic objective, three inequality constraints. Unconstrained minimum at $(2, 2)$ is infeasible (violates $x_1 + x_2 \leq 3$ since $2 + 2 = 4 > 3$).

**PLAN**: Solve KKT with multipliers $\mu_1$ for $x_1 + x_2 - 3 \leq 0$, $\mu_2$ for $-x_1 \leq 0$, $\mu_3$ for $-x_2 \leq 0$. Guess active set: $x_1 + x_2 = 3$ binding, other two inactive ($\mu_2 = \mu_3 = 0$).

**EXECUTE**: Stationarity: $2(x_1 - 2) + \mu_1 = 0$, $2(x_2 - 2) + \mu_1 = 0$. Implies $x_1 = x_2$. Active constraint: $x_1 + x_2 = 3 \Rightarrow x_1 = x_2 = 3/2$. Then $\mu_1 = 2(2 - 3/2) = 1 \geq 0$ ✓. Complementary slackness: $\mu_1 \cdot (x_1 + x_2 - 3) = 1 \cdot 0 = 0$ ✓; $\mu_2 \cdot (-x_1) = 0 \cdot (-3/2) = 0$ ✓; $\mu_3 \cdot (-x_2) = 0$ ✓.

**EVALUATE**: KKT satisfied; by convexity, $(3/2, 3/2)$ is the global minimum with objective $2(1/2)^2 = 1/2$. Geometrically: perpendicular projection from $(2, 2)$ onto the line $x_1 + x_2 = 3$ hits $(3/2, 3/2)$. Shadow price $\mu_1 = 1$: relaxing to $x_1 + x_2 \leq 3 + \delta$ decreases the optimal objective by $\delta$ at unit rate.
