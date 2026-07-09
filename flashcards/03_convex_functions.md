+++
order = 3
subject = "Mathematics"
tags = ["math", "optimization", "convex-functions", "jensen", "epigraph", "smoothness"]
+++

# Optimization — Convex Functions

## 3.1 Defining Convex Functions

C: A function $f: \mathbb{R}^n \to \mathbb{R}$ is [convex] if its domain is a convex set AND for every $\mathbf{x}, \mathbf{y}$ and $\lambda \in \lbrack 0, 1\rbrack $: $f(\lambda \mathbf{x} + (1 - \lambda)\mathbf{y}) \leq \lambda f(\mathbf{x}) + (1 - \lambda) f(\mathbf{y})$.

Q: What does the convexity inequality $f(\lambda \mathbf{x} + (1 - \lambda) \mathbf{y}) \leq \lambda f(\mathbf{x}) + (1 - \lambda) f(\mathbf{y})$ mean geometrically?
A: The graph of $f$ lies ON OR BELOW every chord connecting two points on the graph. "Curves up (or is straight)." Equivalently: the chord from $(\mathbf{x}, f(\mathbf{x}))$ to $(\mathbf{y}, f(\mathbf{y}))$ is never below the graph between them. Captures the intuition of a "bowl-shaped" function.

Q: What is a [strictly convex] function?
A: $f$ is strictly convex if $f(\lambda \mathbf{x} + (1 - \lambda)\mathbf{y}) < \lambda f(\mathbf{x}) + (1 - \lambda) f(\mathbf{y})$ STRICTLY for every distinct $\mathbf{x} \neq \mathbf{y}$ and $\lambda \in (0, 1)$. Graph lies strictly below every chord at intermediate points. Guarantees uniqueness of any minimizer; $x^2$ is strictly convex but $|x|$ is only (weakly) convex at the kink.

Q: What is a [concave] function?
A: $f$ is concave iff $-f$ is convex. Equivalently: $f(\lambda \mathbf{x} + (1-\lambda)\mathbf{y}) \geq \lambda f(\mathbf{x}) + (1-\lambda) f(\mathbf{y})$ — graph lies above chords. Maximizing concave $=$ minimizing convex. Log, square root, and entropy are canonical concave functions.

## 3.2 The Epigraph

C: The [epigraph] of $f$ is $\text{epi}(f) = \{(\mathbf{x}, t) \in \mathbb{R}^{n+1} : f(\mathbf{x}) \leq t\}$ — all points lying on or above the graph.

Q: Why is $f$ convex iff its epigraph is a convex set?
A: Because segment-containment in the epigraph translates directly into the convexity inequality: $(\mathbf{x}, f(\mathbf{x}))$ and $(\mathbf{y}, f(\mathbf{y}))$ in epi$(f)$ plus convex combinations in epi$(f)$ is equivalent to the $\leq$ inequality defining convexity. Lets us study convex functions via the geometry of convex sets — the set-function correspondence unifies both theories.

## 3.3 Jensen's Inequality

Q: State [Jensen's inequality] for convex functions.
A: For any convex $f$ and any convex combination $\mathbf{x} = \sum_i \lambda_i \mathbf{x}_i$ (with $\lambda_i \geq 0$, $\sum \lambda_i = 1$): $f(\sum_i \lambda_i \mathbf{x}_i) \leq \sum_i \lambda_i f(\mathbf{x}_i)$. Generalizes to probability distributions: $f(\mathbb{E}[\mathbf{X}]) \leq \mathbb{E}[f(\mathbf{X})]$. One of the most heavily used inequalities in probability, statistics, and information theory.

Q: Why does Jensen's inequality imply $\mathbb{E}[\mathbf{X}]^2 \leq \mathbb{E}[\mathbf{X}^2]$?
A: Because $f(x) = x^2$ is convex; applying Jensen gives $(\mathbb{E}[X])^2 \leq \mathbb{E}[X^2]$, which rearranges to $\text{Var}(X) \geq 0$. The nonnegativity of variance is, at its core, a convexity fact.

## 3.4 First-Order Condition

Q: What does the [first-order condition] for convexity say for a differentiable $f$?
A: $f$ is convex iff its domain is convex AND $f(\mathbf{y}) \geq f(\mathbf{x}) + \nabla f(\mathbf{x})^T (\mathbf{y} - \mathbf{x})$ for all $\mathbf{x}, \mathbf{y}$. Each tangent hyperplane lies ON OR BELOW the graph — the graph is bounded below by its linear approximations. Means that the first-order Taylor expansion is a global underestimator.

Q: Why does the first-order condition imply that a stationary point of a convex function is a global minimum?
A: Because if $\nabla f(\mathbf{x}^*) = \mathbf{0}$, the first-order condition gives $f(\mathbf{y}) \geq f(\mathbf{x}^*) + 0 = f(\mathbf{x}^*)$ for every $\mathbf{y}$. No local/global distinction survives convexity: every stationary point is automatically global. This is what makes convex optimization tractable.

## 3.5 Second-Order Condition

Q: What does the [second-order condition] for convexity say for a twice-differentiable $f$?
A: $f$ is convex iff its domain is convex AND its Hessian $\nabla^2 f(\mathbf{x})$ is positive semidefinite (PSD) for every $\mathbf{x}$ in the domain. Strict positive-definiteness (PD) at every point implies strict convexity (but not conversely — $x^4$ is strictly convex with zero Hessian at $0$).

Q: Why does a PSD Hessian everywhere imply convexity?
A: Because the Taylor expansion $f(\mathbf{y}) = f(\mathbf{x}) + \nabla f(\mathbf{x})^T(\mathbf{y} - \mathbf{x}) + \frac{1}{2}(\mathbf{y} - \mathbf{x})^T \nabla^2 f(\mathbf{z})(\mathbf{y} - \mathbf{x})$ for some intermediate $\mathbf{z}$. PSD Hessian makes the quadratic term $\geq 0$, giving $f(\mathbf{y}) \geq f(\mathbf{x}) + \nabla f(\mathbf{x})^T(\mathbf{y}-\mathbf{x})$ — the first-order condition.

## 3.6 Strong Convexity

C: $f$ is [$\mu$-strongly convex] (for $\mu > 0$) if $f(\mathbf{x}) - \frac{\mu}{2}\|\mathbf{x}\|^2$ is still convex — equivalently, $\nabla^2 f(\mathbf{x}) \succeq \mu I$ (Hessian bounded below by $\mu I$).

Q: Why does strong convexity give quantitative error bounds, not just existence of a minimum?
A: Because $\mu$-strong convexity implies $f(\mathbf{y}) \geq f(\mathbf{x}) + \nabla f(\mathbf{x})^T (\mathbf{y} - \mathbf{x}) + \frac{\mu}{2}\|\mathbf{y} - \mathbf{x}\|^2$. At a minimum $\mathbf{x}^*$ (where $\nabla f = 0$): $f(\mathbf{y}) - f(\mathbf{x}^*) \geq \frac{\mu}{2}\|\mathbf{y} - \mathbf{x}^*\|^2$. Function-value suboptimality bounds the distance to the minimizer — critical for convergence rate proofs.

## 3.7 Smoothness

C: $f$ is [$L$-smooth] (also called [$L$-Lipschitz gradient]) if $\|\nabla f(\mathbf{x}) - \nabla f(\mathbf{y})\| \leq L \|\mathbf{x} - \mathbf{y}\|$ — equivalently, $\nabla^2 f(\mathbf{x}) \preceq L I$ (Hessian bounded above).

Q: Why do gradient-descent convergence rates depend on the ratio $L/\mu$?
A: Because $\mu$ bounds the valley curvature from below (how curved is the function?) and $L$ bounds the maximum curvature (how steep can gradients change?). The [condition number] $\kappa = L/\mu$ measures how "elongated" the contours are. Gradient descent takes $O(\kappa \log(1/\varepsilon))$ iterations; Nesterov acceleration takes $O(\sqrt{\kappa} \log(1/\varepsilon))$; Newton: $O(\log \log(1/\varepsilon))$ near the optimum.

## 3.8 Operations Preserving Convexity

Q: What operations on convex functions preserve convexity?
A: Nonnegative weighted sums, composition with affine maps, pointwise suprema, partial minimization, and the perspective transform.

C: Convexity is preserved under [nonnegative weighted sums]: $\alpha f + \beta g$ is convex for convex $f, g$ and $\alpha, \beta \geq 0$.

C: Convexity is preserved under [composition with an affine map]: $f(A\mathbf{x} + \mathbf{b})$ is convex if $f$ is.

C: Convexity is preserved under [partial minimization]: $\inf_{\mathbf{y}} f(\mathbf{x}, \mathbf{y})$ is convex in $\mathbf{x}$ if $f$ is jointly convex.

C: The [perspective] of a convex $f$, $(\mathbf{x}, t) \mapsto t f(\mathbf{x}/t)$ for $t > 0$, is convex.

Q: Why is the [pointwise supremum] of convex functions convex?
A: Because $\text{epi}(\sup_i f_i) = \bigcap_i \text{epi}(f_i)$ — intersection of convex sets is convex. Extremely useful: the max of many affine functions is a [piecewise-linear convex function]; the max of convex quadratics is convex. Lets you build complicated convex functions by "envelope" constructions.

## 3.9 Common Convex Functions

Q: Give three simple convex functions of one variable.
A: Powers $x^p$ (for $p \geq 1$, $x \geq 0$), exponentials $e^{ax}$, and the negative logarithm $-\log x$ on $(0, \infty)$.

Q: Why is $f(x) = x^p$ convex for $p \geq 1$ and $x \geq 0$?
A: Because its second derivative $f''(x) = p(p-1)x^{p-2} \geq 0$ for $p \geq 1$, $x \geq 0$. Strictly convex for $p > 1$. Includes $x^2$ (strictly convex — the workhorse quadratic), $|x|^p$ (convex for $p \geq 1$, giving $\ell_p$ norms).

Q: Why is $f(x) = e^{ax}$ convex for any $a$?
A: Because $f''(x) = a^2 e^{ax} \geq 0$ always. Exponentials are a fundamental building block: negative log-likelihoods with exponential-family distributions are convex, and log-sum-exp (the "soft max") is convex.

Q: Why is $f(x) = -\log x$ convex on $(0, \infty)$?
A: Because $f''(x) = 1/x^2 > 0$. Crucial: log-barriers in interior-point methods, KL divergence in information theory, and log-likelihoods for exponential-family distributions all use $-\log$ or similar concave/convex log transforms.

## 3.10 Quasiconvex Functions

C: $f$ is [quasiconvex] if every [sublevel set] $\{\mathbf{x} : f(\mathbf{x}) \leq \alpha\}$ is convex.

Q: How does quasiconvexity relate to convexity, and why does it matter?
A: Convex $\Rightarrow$ quasiconvex (sublevel sets of convex functions are always convex), but not conversely. Quasiconvex functions may have nonconvex shapes (e.g., $\sqrt{|x|}$ is quasiconvex but not convex). Useful because many problems are quasiconvex but not convex — and bisection-based methods solve quasiconvex problems efficiently.

## 3.11 Convex Conjugate

C: The [convex conjugate] (or Fenchel–Legendre transform) of $f$ is $f^*(\mathbf{y}) = \sup_{\mathbf{x}} (\mathbf{y}^T \mathbf{x} - f(\mathbf{x}))$.

Q: Why is $f^*$ always convex, even when $f$ is not?
A: Because $f^*$ is the pointwise supremum of the affine functions $\mathbf{y} \mapsto \mathbf{y}^T \mathbf{x} - f(\mathbf{x})$ (one per $\mathbf{x}$). Pointwise suprema of affine — hence convex — functions are convex. The conjugate "convexifies" its argument. Duality theory is built on conjugate pairs $(f, f^*)$.

## 3.12 Sublevel Sets

Q: Why are [sublevel sets] $\{\mathbf{x} : f(\mathbf{x}) \leq \alpha\}$ of convex $f$ themselves convex?
A: Because if $f(\mathbf{x}), f(\mathbf{y}) \leq \alpha$, convexity gives $f(\lambda \mathbf{x} + (1-\lambda)\mathbf{y}) \leq \lambda \alpha + (1-\lambda)\alpha = \alpha$. Consequence: feasible regions defined by convex inequality constraints $g_i(\mathbf{x}) \leq 0$ are convex — this is how convex optimization problems get their convex feasible set.
