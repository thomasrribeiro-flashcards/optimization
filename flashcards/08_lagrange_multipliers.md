+++
order = 8
subject = "mathematics"
tags = ["math", "optimization", "lagrange-multipliers", "equality-constraints", "constrained-optimization"]
+++

# Optimization — Lagrange Multipliers (Equality Constraints)

## 8.1 The Constrained Minimization Problem

Q: What is the [equality-constrained optimization problem]?
A: $\min f(\mathbf{x})$ subject to $h_i(\mathbf{x}) = 0$ for $i = 1, \dots, m$. No inequalities — just level surfaces as constraints. The feasible set is the intersection of zero-level sets, a $(n - m)$-dimensional manifold (when constraint gradients are independent).

Q: Why are unconstrained optimality conditions (gradient = zero) INSUFFICIENT when equality constraints are present?
A: Because at a constrained minimum, the unconstrained gradient need NOT be zero — only the projection onto the feasible manifold must be zero. $\nabla f$ can point out of the feasible set while the component along feasible directions vanishes. Constraints redirect "admissible" directions.

## 8.2 The Lagrangian

C: The [Lagrangian] of an equality-constrained problem is $\mathcal{L}(\mathbf{x}, \boldsymbol{\lambda}) = f(\mathbf{x}) + \sum_i \lambda_i h_i(\mathbf{x})$ — original objective plus a weighted sum of constraints.

Q: Why does introducing [Lagrange multipliers] $\lambda_i$ reduce a constrained problem to an unconstrained one?
A: Because stationarity of the Lagrangian with respect to BOTH $\mathbf{x}$ AND $\boldsymbol{\lambda}$ encodes the constraint simultaneously: $\nabla_{\mathbf{x}} \mathcal{L} = 0$ gives the balance condition, $\nabla_{\boldsymbol{\lambda}} \mathcal{L} = 0$ recovers $h_i(\mathbf{x}) = 0$. Elegant: $n + m$ equations in $n + m$ unknowns, no constraint-handling machinery needed.

## 8.3 First-Order Conditions

Q: State the [Lagrange first-order necessary conditions] for equality-constrained optimization.
A: If $\mathbf{x}^*$ is a regular local minimum of $f$ subject to $h_i(\mathbf{x}) = 0$, then there exist multipliers $\lambda_i^*$ such that $\nabla f(\mathbf{x}^*) + \sum_i \lambda_i^* \nabla h_i(\mathbf{x}^*) = \mathbf{0}$ AND $h_i(\mathbf{x}^*) = 0$. "Gradient balance": the objective gradient lies in the span of the constraint gradients.

Q: What does it mean geometrically for $\nabla f$ to be a linear combination of constraint gradients at an optimum?
A: It means $\nabla f$ is [NORMAL] to the feasible manifold — no feasible direction gives a first-order improvement. Any direction $\mathbf{d}$ tangent to the manifold satisfies $\nabla h_i^T \mathbf{d} = 0$, so $\nabla f^T \mathbf{d} = -\sum \lambda_i \nabla h_i^T \mathbf{d} = 0$. "Can't move along the manifold without leaving it."

## 8.4 Regularity and LICQ

C: A feasible point $\mathbf{x}^*$ is [regular] if the gradients $\{\nabla h_i(\mathbf{x}^*)\}$ are linearly independent — the [Linear Independence Constraint Qualification (LICQ)] holds.

Q: Why does LICQ matter for the Lagrange conditions to apply?
A: Because the Lagrange theorem proves existence of multipliers UNDER LICQ. If constraint gradients are linearly dependent (redundant constraints), multipliers may not exist or be non-unique. Without LICQ, the standard first-order conditions can fail silently. Standard constraint qualification; if violated, use alternative qualifications (MFCQ, CPLD).

## 8.5 Second-Order Conditions

Q: State the [Lagrange second-order sufficient condition] for a strict local minimum.
A: If $(\mathbf{x}^*, \boldsymbol{\lambda}^*)$ satisfies first-order conditions AND the Hessian of the Lagrangian $\nabla^2_{\mathbf{x}\mathbf{x}} \mathcal{L}(\mathbf{x}^*, \boldsymbol{\lambda}^*)$ is positive definite when RESTRICTED to the tangent space $\{\mathbf{d} : \nabla h_i(\mathbf{x}^*)^T \mathbf{d} = 0 \text{ for all } i\}$, then $\mathbf{x}^*$ is a strict local minimum. Curvature only needs to be positive in feasible directions, not all directions.

Q: Why does the second-order condition use the Lagrangian Hessian, not the objective Hessian?
A: Because the Lagrangian Hessian $\nabla^2_{\mathbf{x}\mathbf{x}} \mathcal{L} = \nabla^2 f + \sum \lambda_i \nabla^2 h_i$ accounts for CURVATURE of both the objective and the constraints. Even if the objective is convex, curved constraints can make the problem locally nonconvex or hyperconvex. The Lagrangian Hessian captures the true local geometry of the constrained landscape.

## 8.6 Economic Interpretation

Q: What is the economic interpretation of a Lagrange multiplier $\lambda_i$?
A: $\lambda_i^*$ is the [shadow price] of constraint $i$: the rate of change of the optimal objective value with respect to relaxing $h_i$. Formally: if $h_i(\mathbf{x}) = c_i$ and we perturb $c_i \to c_i + \delta$, the optimal objective changes by approximately $-\lambda_i^* \delta$. Measures the marginal value of constraint relaxation.

Q: Why can Lagrange multipliers be negative in equality-constrained problems (unlike in LP)?
A: Because equality constraints $h_i = 0$ can be approached from either side — relaxing to $h_i = \epsilon$ or $h_i = -\epsilon$ are both valid perturbations. The sign of $\lambda_i$ says which side helps: positive $\lambda_i$ means increasing $h_i$ improves the objective, negative means decreasing it does. Inequalities ($h_i \geq 0$) force $\lambda_i \geq 0$ because only one direction is admissible.

## 8.7 Eliminating Constraints by Substitution

Q: When can an equality constraint be eliminated by substitution rather than via Lagrange multipliers?
A: When the constraint can be solved explicitly for one variable in terms of the others (e.g., $x_n = g(x_1, \dots, x_{n-1})$). Substitute into $f$, giving an unconstrained problem in $n - 1$ variables. Advantage: smaller dimension. Disadvantage: loses constraint symmetry and can hide algebraic structure — Lagrange multipliers often cleaner.

## 8.8 Isoperimetric Example

P: Minimize $x^2 + y^2$ subject to $x + y = 1$. (Distance from origin to the line.)
S:
**IDENTIFY**: Two variables, one equality constraint. Convex objective, linear constraint.

**PLAN**: Form Lagrangian, set gradient to zero, solve.

**EXECUTE**: $\mathcal{L} = x^2 + y^2 + \lambda(x + y - 1)$. Partial derivatives: $2x + \lambda = 0$, $2y + \lambda = 0$. So $x = y = -\lambda/2$. Plug into constraint: $-\lambda/2 - \lambda/2 = 1 \Rightarrow \lambda = -1$. Thus $x = y = 1/2$.

**EVALUATE**: Optimal value $= 1/4 + 1/4 = 1/2$. Distance $= 1/\sqrt{2}$. Geometrically: the closest point on the line $x+y=1$ to the origin is indeed $(1/2, 1/2)$ where the gradient of $f$ (radially outward) aligns with the line's normal $(1,1)$. Shadow price $\lambda^* = -1$: increasing the constraint RHS to $x + y = 1 + \delta$ increases the minimum distance squared by $\delta$ at rate $-\lambda^* = 1$.

## 8.9 Equality-Constrained QP

Q: State the [equality-constrained quadratic program] and its KKT system.
A: $\min \frac{1}{2} \mathbf{x}^T Q \mathbf{x} + \mathbf{c}^T \mathbf{x}$ s.t. $A\mathbf{x} = \mathbf{b}$ with $Q$ symmetric. First-order conditions: $Q \mathbf{x}^* + \mathbf{c} + A^T \boldsymbol{\lambda}^* = \mathbf{0}$ AND $A\mathbf{x}^* = \mathbf{b}$. Together a linear system: $\begin{pmatrix} Q & A^T \\ A & 0 \end{pmatrix} \begin{pmatrix} \mathbf{x} \\ \boldsymbol{\lambda} \end{pmatrix} = \begin{pmatrix} -\mathbf{c} \\ \mathbf{b} \end{pmatrix}$. This [KKT matrix] is solved by LU factorization or specialized iterative methods.

Q: Why is the KKT matrix for equality-constrained QP indefinite, not positive definite?
A: Because it has the block structure $\begin{pmatrix} Q & A^T \\ A & 0 \end{pmatrix}$. Even if $Q \succ 0$, the bottom-right zero block makes the overall matrix indefinite — $m$ negative eigenvalues. Specialized solvers (MINRES, null-space methods, range-space methods) exploit this saddle-point structure; standard Cholesky fails.

## 8.10 Null-Space and Range-Space Methods

Q: What does the [null-space method] do for equality-constrained QP?
A: Parametrize all feasible points as $\mathbf{x} = \mathbf{x}_0 + Z \mathbf{z}$ where $\mathbf{x}_0$ is a particular solution of $A\mathbf{x} = \mathbf{b}$ and $Z$ is a basis for $\text{null}(A)$. The problem reduces to unconstrained minimization of $f(\mathbf{x}_0 + Z\mathbf{z})$ over $\mathbf{z}$ — smaller dimension $n - m$. Efficient when constraints are few relative to variables.

Q: What does the [range-space method] do?
A: Solves the reduced problem in $\boldsymbol{\lambda}$ first by substitution: $A Q^{-1} A^T \boldsymbol{\lambda} = A Q^{-1} \mathbf{c} + \mathbf{b}$, then recovers $\mathbf{x}$. Requires $Q$ invertible. Efficient when there are more variables than constraints AND $Q^{-1}$ is cheap to apply. Opposite tradeoff to null-space method.

## 8.11 Sensitivity and Envelope Theorem

Q: State the [envelope theorem] for equality-constrained optimization.
A: Let $V(\mathbf{c}) = \min f(\mathbf{x}; \mathbf{c})$ subject to $h_i(\mathbf{x}; \mathbf{c}) = 0$ be the value function as $\mathbf{c}$ varies. Then $\nabla V = \nabla_{\mathbf{c}} \mathcal{L}(\mathbf{x}^*(\mathbf{c}), \boldsymbol{\lambda}^*(\mathbf{c}); \mathbf{c})$ — derivative of value function equals partial of Lagrangian at the optimum. Multipliers give comparative statics.

## 8.12 Dangerous Non-Regular Cases

Q: What is an example where Lagrange multipliers FAIL to exist?
A: Minimize $x$ subject to $h(x, y) = y^2 - x^3 = 0$ at $(0, 0)$. The constraint gradient $(-3x^2, 2y) = (0, 0)$ at the origin — LICQ fails. Yet $(0, 0)$ is a local minimum ($x \geq 0$ on the constraint). No multiplier $\lambda$ makes $\nabla f + \lambda \nabla h = (1, 0) + \lambda (0, 0) = \mathbf{0}$. Classic "abnormal" multiplier pathology; handled by Fritz John conditions (adding a multiplier on the objective itself).
