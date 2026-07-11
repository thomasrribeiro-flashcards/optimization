+++
order = 10
subject = "mathematics"
tags = ["math", "optimization", "duality", "nonlinear-programming", "slater", "duality-gap", "saddle-point"]
+++

# Optimization — Duality in Nonlinear Programming

## 10.1 The Lagrangian Dual Function

C: For the problem $\min f(\mathbf{x})$ s.t. $g_i(\mathbf{x}) \leq 0$, $h_j(\mathbf{x}) = 0$, the [Lagrangian dual function] is $q(\boldsymbol{\mu}, \boldsymbol{\lambda}) = \inf_{\mathbf{x}} \mathcal{L}(\mathbf{x}, \boldsymbol{\mu}, \boldsymbol{\lambda}) = \inf_{\mathbf{x}} \lbrack f(\mathbf{x}) + \boldsymbol{\mu}^T \mathbf{g}(\mathbf{x}) + \boldsymbol{\lambda}^T \mathbf{h}(\mathbf{x})\rbrack $.

Q: Why is the dual function $q(\boldsymbol{\mu}, \boldsymbol{\lambda})$ always CONCAVE, even for nonconvex primal problems?
A: Because $q$ is the pointwise INFIMUM of affine functions in $(\boldsymbol{\mu}, \boldsymbol{\lambda})$: for each fixed $\mathbf{x}$, $\mathcal{L}(\mathbf{x}, \cdot, \cdot)$ is affine in $(\boldsymbol{\mu}, \boldsymbol{\lambda})$; infimum of affine functions is concave. Beautifully robust: the dual is always a convex optimization problem regardless of primal structure.

## 10.2 The Dual Problem

C: The [Lagrangian dual problem] is $\max_{\boldsymbol{\mu} \geq 0, \boldsymbol{\lambda}} q(\boldsymbol{\mu}, \boldsymbol{\lambda})$ — maximize the concave dual function over the dual feasible cone.

Q: Why is the dual problem always a CONVEX optimization problem, even when the primal is nonconvex?
A: Because we are maximizing a concave function over a convex cone (the dual feasible set $\boldsymbol{\mu} \geq \mathbf{0}$, $\boldsymbol{\lambda}$ free — a half-space intersection). Even for NP-hard combinatorial primals, the dual is a convex problem solvable in polynomial time — giving tractable lower bounds on the primal.

## 10.3 Weak Duality

Q: State [weak duality] in nonlinear programming.
A: For any primal feasible $\mathbf{x}$ and dual feasible $(\boldsymbol{\mu}, \boldsymbol{\lambda})$: $q(\boldsymbol{\mu}, \boldsymbol{\lambda}) \leq f(\mathbf{x})$. The dual value is a lower bound on the primal objective. Always holds — no assumptions on convexity. Used extensively: any dual-feasible pair is a certificate of a lower bound.

Q: Why does weak duality hold in such generality?
A: Because at any primal feasible $\mathbf{x}$ with $\mathbf{g}(\mathbf{x}) \leq \mathbf{0}$ and $\mathbf{h}(\mathbf{x}) = \mathbf{0}$: $q(\boldsymbol{\mu}, \boldsymbol{\lambda}) = \inf_{\mathbf{z}} \mathcal{L}(\mathbf{z}, \boldsymbol{\mu}, \boldsymbol{\lambda}) \leq \mathcal{L}(\mathbf{x}, \boldsymbol{\mu}, \boldsymbol{\lambda}) = f(\mathbf{x}) + \boldsymbol{\mu}^T \mathbf{g}(\mathbf{x}) + \boldsymbol{\lambda}^T \mathbf{h}(\mathbf{x}) \leq f(\mathbf{x})$ (last step: $\boldsymbol{\mu}^T \mathbf{g} \leq 0$, $\boldsymbol{\lambda}^T \mathbf{h} = 0$).

## 10.4 The Duality Gap

C: The [duality gap] is $p^* - d^*$ where $p^* = \min f$ (primal optimum) and $d^* = \max q$ (dual optimum). By weak duality, $p^* \geq d^*$, so the gap is nonnegative.

Q: What does a nonzero duality gap tell us about the primal problem?
A: A positive duality gap ($p^* > d^*$) indicates that the problem is nonconvex in a way that prevents tight Lagrangian bounds. Specifically: no linear lower bound (a valid $(\boldsymbol{\mu}, \boldsymbol{\lambda})$) equals the primal optimum. Combinatorial / nonconvex problems often have positive gaps. Gap magnitude measures how "far" from convex the problem behaves at the optimum.

## 10.5 Strong Duality and Slater's Condition

Q: State [Slater's condition] for convex optimization.
A: For a convex primal ($f, g_i$ convex, $h_j$ affine): there EXISTS $\bar{\mathbf{x}}$ with $g_i(\bar{\mathbf{x}}) < 0$ for all inequality constraints and $h_j(\bar{\mathbf{x}}) = 0$ — a strictly feasible point. Strict feasibility of inequalities is the key requirement. Slater's condition is easy to check and holds for most real problems.

Q: Why does Slater's condition imply strong duality in convex optimization?
A: Because Slater gives a separating-hyperplane argument: the primal optimal value and the "graph" of attainable $(\mathbf{g}, f)$ values can be separated, producing the dual optimal multipliers achieving $q^* = p^*$. Geometrically: strict feasibility rules out degenerate cases where the primal "touches" the boundary asymptotically. Under Slater, the KKT conditions are both necessary AND sufficient.

## 10.6 Saddle Points of the Lagrangian

C: $(\mathbf{x}^*, \boldsymbol{\mu}^*, \boldsymbol{\lambda}^*)$ is a [saddle point] of the Lagrangian if $\mathcal{L}(\mathbf{x}^*, \boldsymbol{\mu}, \boldsymbol{\lambda}) \leq \mathcal{L}(\mathbf{x}^*, \boldsymbol{\mu}^*, \boldsymbol{\lambda}^*) \leq \mathcal{L}(\mathbf{x}, \boldsymbol{\mu}^*, \boldsymbol{\lambda}^*)$ for all feasible $\mathbf{x}$ and $\boldsymbol{\mu} \geq \mathbf{0}$.

Q: Why does a saddle point of the Lagrangian imply strong duality with no gap?
A: Because the saddle-point condition implies $\min_{\mathbf{x}} \max_{\boldsymbol{\mu} \geq 0, \boldsymbol{\lambda}} \mathcal{L} = \max_{\boldsymbol{\mu} \geq 0, \boldsymbol{\lambda}} \min_{\mathbf{x}} \mathcal{L}$ — the inner min and outer max can be interchanged. The inner-min/outer-max is exactly the dual problem; the outer-min/inner-max recovers the primal (max over $\boldsymbol{\mu}$ of a penalized objective equals the primal when inequality is satisfied). Saddle ⇔ no gap ⇔ KKT with strong duality.

## 10.7 Economic Interpretation Revisited

Q: In nonlinear programming, what do Lagrangian dual variables represent?
A: $\mu_i^*$ and $\lambda_j^*$ are shadow prices — marginal rates of change of the optimal value per unit perturbation of the corresponding constraint RHS. Under strong duality and CS: $\mu_i^* = -\partial V / \partial b_i$ where $V$ is the value function and $b_i$ is the RHS of $g_i(\mathbf{x}) \leq b_i$. Generalizes the LP shadow price interpretation.

## 10.8 Dual Decomposition

Q: What does [dual decomposition] exploit in structured problems?
A: When the primal problem has SEPARABLE structure across blocks linked by shared constraints, the dual decomposes: for fixed $\boldsymbol{\mu}$, the Lagrangian decomposes into independent subproblems per block. Solve each block in parallel, then update $\boldsymbol{\mu}$ via a dual ascent step. Used in distributed optimization, column generation, Dantzig-Wolfe decomposition.

Q: Why is dual decomposition scalable for large-scale structured problems?
A: Because each subproblem is solved independently (possibly on separate machines), and only the dual variables $\boldsymbol{\mu}$ are exchanged between them. Communication is low: only the coupling constraints' dual. Convex primals yield convex subproblems; dual convergence is ensured by weak duality + concavity. Backbone of distributed ADMM, parallel LP/IP solvers, consensus optimization.

## 10.9 Dual of Convex QP

Q: Write the Lagrangian dual of $\min \frac{1}{2}\mathbf{x}^T Q \mathbf{x} + \mathbf{c}^T \mathbf{x}$ s.t. $A\mathbf{x} \leq \mathbf{b}$ with $Q \succ 0$.
A: Dual function: $q(\boldsymbol{\mu}) = -\frac{1}{2}(\mathbf{c} + A^T \boldsymbol{\mu})^T Q^{-1} (\mathbf{c} + A^T \boldsymbol{\mu}) - \boldsymbol{\mu}^T \mathbf{b}$ (from stationarity: $\mathbf{x} = -Q^{-1}(\mathbf{c} + A^T \boldsymbol{\mu})$). Dual problem: $\max_{\boldsymbol{\mu} \geq 0} q(\boldsymbol{\mu})$ — another convex QP in the dual variables $\boldsymbol{\mu}$. Primal has $n$ variables, $m$ constraints; dual has $m$ variables, $n$ constraints — swap if $m \ll n$.

## 10.10 Fenchel Duality

Q: What is [Fenchel duality] and how does it relate to Lagrangian duality?
A: Given $\min f(\mathbf{x}) + g(A\mathbf{x})$ with convex $f, g$: Fenchel dual is $\max -f^*(-A^T \boldsymbol{\mu}) - g^*(\boldsymbol{\mu})$ where $f^*, g^*$ are convex conjugates. A specialization of Lagrangian duality tailored to "split" objective structure. Underlies primal-dual algorithms for imaging (Chambolle-Pock, FISTA), compressed sensing, and regularized optimization.

## 10.11 Conic Duality

Q: State [conic duality] for the conic LP $\min \mathbf{c}^T \mathbf{x}$ s.t. $A\mathbf{x} = \mathbf{b}$, $\mathbf{x} \in K$.
A: Dual: $\max \mathbf{b}^T \mathbf{y}$ s.t. $\mathbf{c} - A^T \mathbf{y} \in K^*$ (dual cone). If $K = \mathbb{R}^n_+$: standard LP duality. If $K = \mathcal{L}^n$: SOCP duality. If $K = \mathcal{S}^n_+$: SDP duality. Unifies LP/SOCP/SDP under one framework — conic programming. Strong duality holds under Slater-like conditions on the cone structure.

## 10.12 Algorithmic Consequences

Q: How is duality used to BOUND and CERTIFY solutions in nonlinear optimization?
A: Dual feasible points give lower bounds on the primal (used in branch-and-bound and cutting planes), and a primal-dual pair with zero gap satisfying complementary slackness PROVES optimality.

Q: Beyond bounds and optimality certificates, how is duality used ALGORITHMICALLY in nonlinear optimization?
A: Primal-dual (interior-point) methods update primal and dual variables simultaneously; dual variables quantify constraint-relaxation value (sensitivity analysis); dual decomposition decouples structured problems into parallel subproblems.
