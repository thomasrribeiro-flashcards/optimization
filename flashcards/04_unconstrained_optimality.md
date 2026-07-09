+++
order = 4
subject = "Mathematics"
tags = ["math", "optimization", "unconstrained", "optimality-conditions", "gradient", "hessian"]
+++

# Optimization — Unconstrained Optimality Conditions

## 4.1 The Unconstrained Problem

Q: What is [unconstrained optimization]?
A: Minimization of $f: \mathbb{R}^n \to \mathbb{R}$ with NO explicit constraints — the feasible set is all of $\mathbb{R}^n$. "Unconstrained" is almost never literally true in applications, but any problem where constraints are automatically satisfied (parameters of a regression, for example) can be analyzed this way. Simplest setting for deriving optimality theory.

## 4.2 First-Order Necessary Condition

Q: State the [first-order necessary condition] for a local minimum of smooth $f$.
A: If $\mathbf{x}^*$ is a local minimum of a differentiable $f$, then $\nabla f(\mathbf{x}^*) = \mathbf{0}$. At a minimum, no direction gives a first-order decrease — all directional derivatives vanish, forcing the gradient to be zero. This is necessary for local minimization but not sufficient (could be max, saddle, or inflection).

Q: Why is $\nabla f(\mathbf{x}^*) = \mathbf{0}$ necessary at a minimum?
A: Because if $\nabla f(\mathbf{x}^*) \neq \mathbf{0}$, moving infinitesimally in the direction $-\nabla f(\mathbf{x}^*)$ decreases $f$ to first order — contradicting local minimality. Formally: $f(\mathbf{x}^* - \varepsilon \nabla f(\mathbf{x}^*)) = f(\mathbf{x}^*) - \varepsilon \|\nabla f(\mathbf{x}^*)\|^2 + O(\varepsilon^2) < f(\mathbf{x}^*)$ for small $\varepsilon$.

## 4.3 Stationary Points

C: A [stationary point] (or [critical point]) of $f$ is any $\mathbf{x}$ with $\nabla f(\mathbf{x}) = \mathbf{0}$.

Q: What are the three kinds of stationary points?
A: (i) [Local minimum]: $f$ curves up in every direction. (ii) [Local maximum]: $f$ curves down in every direction. (iii) [Saddle point]: $f$ curves up in some directions, down in others. All three satisfy $\nabla f = \mathbf{0}$; the Hessian tells them apart.

## 4.4 Second-Order Conditions

Q: State the [second-order necessary condition] for a local minimum.
A: If $\mathbf{x}^*$ is a local minimum and $f$ is twice differentiable, then $\nabla f(\mathbf{x}^*) = \mathbf{0}$ AND the Hessian $\nabla^2 f(\mathbf{x}^*)$ is positive semidefinite (PSD). Necessary only: PSD + $\nabla f = \mathbf{0}$ could also describe a saddle in degenerate cases (eigenvalue exactly zero).

Q: State the [second-order sufficient condition] for a strict local minimum.
A: If $\nabla f(\mathbf{x}^*) = \mathbf{0}$ AND the Hessian $\nabla^2 f(\mathbf{x}^*)$ is POSITIVE DEFINITE (PD, all eigenvalues $> 0$), then $\mathbf{x}^*$ is a strict local minimum. Sufficient: we can conclude minimization from algebra alone. PD Hessian means $f$ curves strictly upward in every direction near $\mathbf{x}^*$.

Q: Why does a positive-definite Hessian at a stationary point guarantee a local minimum?
A: Because the Taylor expansion $f(\mathbf{x}^* + \mathbf{h}) = f(\mathbf{x}^*) + 0 + \frac{1}{2} \mathbf{h}^T \nabla^2 f(\mathbf{x}^*) \mathbf{h} + O(\|\mathbf{h}\|^3)$. PD Hessian makes the quadratic term strictly positive for $\mathbf{h} \neq \mathbf{0}$, and for small $\|\mathbf{h}\|$ it dominates the $O(\|\mathbf{h}\|^3)$ remainder — $f(\mathbf{x}^* + \mathbf{h}) > f(\mathbf{x}^*)$.

## 4.5 Saddle Points

C: A [saddle point] is a stationary point where the Hessian has both positive and negative eigenvalues — $f$ curves up in some directions, down in others.

Q: Why are saddle points often more problematic than local minima in high-dimensional nonconvex optimization?
A: Because in high dimensions, stationary points are overwhelmingly saddles: getting the Hessian to be PD requires ALL $n$ eigenvalues positive, increasingly unlikely as $n$ grows. Neural network loss landscapes are littered with saddles. Gradient methods stall near saddles; escaping them requires either noise (SGD) or Hessian-aware methods (Newton, trust region).

## 4.6 Convex Special Case

Q: Why is optimization of a convex function qualitatively easier than nonconvex?
A: Because for convex $f$: every stationary point is a global minimum (first-order condition + convexity). So gradient descent always converges to the global optimum, saddles don't exist, and there are no local minima to worry about. The second-order test is automatic: convex $f$ has PSD Hessian everywhere.

Q: Why does strict convexity imply at most one minimum exists?
A: Because if $\mathbf{x}^*$ and $\mathbf{y}^*$ were two distinct global minimums with $f(\mathbf{x}^*) = f(\mathbf{y}^*) = m$, strict convexity gives $f(\frac{\mathbf{x}^* + \mathbf{y}^*}{2}) < \frac{m + m}{2} = m$ — contradicting $m$ being the minimum. Strict convexity eliminates plateau minima.

## 4.7 Coercive Functions

C: $f$ is [coercive] if $f(\mathbf{x}) \to \infty$ as $\|\mathbf{x}\| \to \infty$ — the function grows to infinity at the horizon of its domain.

Q: Why does coercivity plus continuity guarantee the existence of a global minimum?
A: Because coercive $f$ has bounded sublevel sets (the set where $f \leq c$ is contained in some ball), and continuous functions attain their infimum on closed bounded sets (Weierstrass extreme value theorem). Combines to: global minimum exists on any closed feasible set. A standard "existence theorem" for optimization.

## 4.8 Descent Directions

Q: What is a [descent direction] for $f$ at $\mathbf{x}$?
A: Any vector $\mathbf{p}$ with $\nabla f(\mathbf{x})^T \mathbf{p} < 0$. Moving in direction $\mathbf{p}$ decreases $f$ to first order: $f(\mathbf{x} + \alpha \mathbf{p}) = f(\mathbf{x}) + \alpha \nabla f(\mathbf{x})^T \mathbf{p} + O(\alpha^2)$. Descent algorithms (gradient descent, Newton, BFGS) proceed by choosing descent directions each iteration.

Q: Why is the negative gradient the [steepest descent] direction?
A: Because among all unit vectors $\mathbf{p}$, the Cauchy–Schwarz inequality $\nabla f^T \mathbf{p} \geq -\|\nabla f\|$ achieves equality iff $\mathbf{p} = -\nabla f / \|\nabla f\|$. Steepest descent is "steepest" only in the Euclidean metric; different metrics (Newton uses the Hessian metric) give different "steepest" directions that may converge faster.

## 4.9 Newton's Direction

Q: What is [Newton's direction] for optimization?
A: $\mathbf{p}_N = -[\nabla^2 f(\mathbf{x})]^{-1} \nabla f(\mathbf{x})$ — the direction that sets the gradient of the local quadratic Taylor approximation to zero. When $\nabla^2 f \succ 0$ (PD), Newton's direction is a descent direction: $\nabla f^T \mathbf{p}_N = -\nabla f^T H^{-1} \nabla f < 0$.

Q: Why does Newton's method rescale based on curvature rather than just following the gradient?
A: Because gradient descent treats all directions equally, but curvatures differ: tight curves need small steps, flat directions could take large ones. Newton's direction premultiplies by $H^{-1}$ — divides by curvature per direction — giving the "correctly scaled" step. Result: Newton converges in $O(\log \log(1/\varepsilon))$ iterations near a strict minimum vs. gradient's $O(\kappa \log(1/\varepsilon))$.

## 4.10 Rate of Convergence

Q: Define [linear], [superlinear], and [quadratic convergence] for iterates $\{\mathbf{x}_k\}$ converging to $\mathbf{x}^*$.
A: [Linear]: $\|\mathbf{x}_{k+1} - \mathbf{x}^*\| \leq r \|\mathbf{x}_k - \mathbf{x}^*\|$ for some $r \in (0, 1)$. [Superlinear]: $\|\mathbf{x}_{k+1} - \mathbf{x}^*\|/\|\mathbf{x}_k - \mathbf{x}^*\| \to 0$ (rate $r \to 0$). [Quadratic]: $\|\mathbf{x}_{k+1} - \mathbf{x}^*\| \leq C \|\mathbf{x}_k - \mathbf{x}^*\|^2$ — errors square each step, doubling correct digits.

Q: Why does gradient descent only achieve linear convergence on strongly convex functions?
A: Because each step reduces error by a constant factor bounded away from zero; the recurrence $\|\mathbf{e}_{k+1}\| \leq r \|\mathbf{e}_k\|$ is inherent to the iteration $\mathbf{x}_{k+1} = \mathbf{x}_k - \alpha \nabla f$. Even with optimal $\alpha$, the rate $r \approx (\kappa - 1)/(\kappa + 1) \to 1$ as conditioning worsens — never quadratic without second-order information.

## 4.11 Global Minimization Is Hard

Q: Why is global nonconvex minimization NP-hard in general?
A: Because indicator-function-style gadgets encode NP-complete problems (3-SAT, subset-sum) as minimization of specific nonconvex functions. No polynomial-time algorithm can find the global minimum of a generic nonconvex smooth $f$ in high dimensions — even with the gradient and Hessian oracle. Practical success relies on problem-specific structure (convexity, low-rank, sparsity).

## 4.12 Practical Takeaways

Q: How do unconstrained optimality conditions guide algorithm design?
A: (i) Find stationary points: $\nabla f = \mathbf{0}$ — solve by iterative methods. (ii) Check sufficient conditions: $\nabla^2 f \succ 0$ → minimum; indefinite → saddle; $\nabla^2 f \prec 0$ → maximum. (iii) Near a strong-convex minimum, Newton-family methods achieve quadratic convergence. (iv) For nonconvex $f$, all guarantees are LOCAL; global optimization requires different tools (branch-and-bound, global solvers, or convex relaxations).

## 4.13 Pattern Recognition

Q: You found $\mathbf{x}^*$ with $\nabla f(\mathbf{x}^*) = \mathbf{0}$. How do you classify it?
A: Check Hessian eigenvalues: all $> 0$ → strict local min; all $< 0$ → strict local max; mixed signs → saddle; some zero → degenerate (need higher-order test or direct evaluation).

Q: You're optimizing convex $f$ and gradient descent is slow. What's the structural fix?
A: Use second-order information: Newton's method (full Hessian) or quasi-Newton (BFGS, L-BFGS) — quadratic/superlinear convergence vs. gradient's linear rate.

Q: You're optimizing in millions of dimensions and the Hessian won't fit in memory. What method?
A: L-BFGS — limited-memory quasi-Newton. Stores only the last $m \approx 10$ gradient differences; gives near-Newton convergence with $O(mn)$ memory.

Q: You're stuck near a saddle point in a nonconvex problem (gradient is small but you're not at a minimum). What's the fix?
A: Add curvature awareness — Newton or trust-region methods escape saddles by using negative-curvature directions; pure gradient descent stalls. Stochastic noise (SGD) also escapes saddles in practice.

Q: You have a smooth convex problem AND a closed-form gradient. What method?
A: Accelerated gradient (Nesterov's momentum) — achieves $O(1/k^2)$ rate vs. plain gradient's $O(1/k)$, no Hessian required.

Q: The function isn't differentiable (e.g., contains $|\cdot|$ or $\max$). What method class?
A: Subgradient methods or proximal methods — replace gradient with a subgradient or use proximal operator of the nonsmooth part. Standard for L1-regularized problems.
