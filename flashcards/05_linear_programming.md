+++
order = 5
subject = "mathematics"
tags = ["math", "optimization", "linear-programming", "lp", "polyhedra", "standard-form"]
+++

# Optimization — Linear Programming: Formulation and Geometry

## 5.1 What LP Is

Q: What is a [linear program]?
A: An optimization problem where the objective and all constraints are linear functions of the decision variables. Canonical form: $\min \mathbf{c}^T \mathbf{x}$ subject to $A\mathbf{x} \leq \mathbf{b}$, $\mathbf{x} \geq \mathbf{0}$. The workhorse of operations research and a foundation for integer programming, network flows, and combinatorial optimization.

Q: Why is LP so important historically and practically?
A: Because LP is the most widely used optimization tool in industry — from supply-chain logistics to airline scheduling — AND the first broadly nontrivial class shown to be polynomial-time solvable (Khachiyan's ellipsoid method, 1979; Karmarkar's interior-point method, 1984). LP theory forms the mathematical core of optimization; generalizations (QP, SOCP, SDP, NLP) inherit its duality framework.

## 5.2 Standard and Canonical Forms

C: The [LP standard form] is $\min \mathbf{c}^T \mathbf{x}$ subject to $A\mathbf{x} = \mathbf{b}$, $\mathbf{x} \geq \mathbf{0}$ — equality constraints with nonnegative variables.

C: The [LP canonical form] is $\min \mathbf{c}^T \mathbf{x}$ subject to $A\mathbf{x} \leq \mathbf{b}$, $\mathbf{x} \geq \mathbf{0}$ — inequality constraints.

Q: When converting an LP to standard form, how is a $\max$ objective handled?
A: Flip the sign of $\mathbf{c}$ to always minimize: $\max \mathbf{c}^T\mathbf{x}$ becomes $\min (-\mathbf{c})^T\mathbf{x}$.

Q: When converting an LP to standard form, how is an inequality constraint $A\mathbf{x} \leq \mathbf{b}$ handled?
A: Add a [slack variable] $\mathbf{s} \geq \mathbf{0}$ giving $A\mathbf{x} + \mathbf{s} = \mathbf{b}$.

Q: When converting an LP to standard form, how is a $\geq$ constraint $A\mathbf{x} \geq \mathbf{b}$ handled?
A: Subtract a [surplus variable] $\mathbf{s} \geq \mathbf{0}$ giving $A\mathbf{x} - \mathbf{s} = \mathbf{b}$.

Q: When converting an LP to standard form, how is a free variable $x_i \in \mathbb{R}$ handled?
A: Split as $x_i = x_i^+ - x_i^-$ with $x_i^+, x_i^- \geq 0$. With the sign-flip, slack, and surplus tricks, any LP can be brought to standard form mechanically.

## 5.3 The Feasible Polytope

C: The [feasible region] of an LP is a [polyhedron] — the intersection of finitely many half-spaces, $\{\mathbf{x} : A\mathbf{x} \leq \mathbf{b}, \mathbf{x} \geq \mathbf{0}\}$.

Q: Why is the feasible region of an LP always convex?
A: Because it is the intersection of half-spaces (the inequality constraints) and nonnegative orthants (variable bounds) — each of which is a convex set, and intersections of convex sets are convex. LP is a convex optimization problem; all convex-optimization results apply.

## 5.4 Extreme Points and Vertices

C: A [vertex] (or [extreme point]) of a polyhedron $P$ is a point $\mathbf{x} \in P$ that cannot be written as $\mathbf{x} = \lambda \mathbf{y} + (1-\lambda)\mathbf{z}$ for distinct $\mathbf{y}, \mathbf{z} \in P$ with $\lambda \in (0, 1)$.

Q: Algebraic characterization: when is $\mathbf{x}$ a vertex of $\{A\mathbf{x} = \mathbf{b}, \mathbf{x} \geq \mathbf{0}\}$?
A: When at least $n$ of the constraints hold with equality AND those active constraints are linearly independent. Equivalently: $\mathbf{x}$ is a [basic feasible solution] (BFS) — has at most $m$ positive components (where $A$ is $m \times n$) corresponding to linearly independent columns of $A$.

## 5.5 Fundamental Theorem of LP

Q: State the [fundamental theorem of linear programming].
A: If an LP has a finite optimal value, then at least one optimal solution is a vertex (BFS) of the feasible polyhedron. We never need to search the interior: the optimum is always attained at a corner (possibly tied with other corners or an entire face). This is why simplex — which walks between vertices — suffices for LP.

Q: Why is the optimum of an LP always attained at a vertex (when finite)?
A: Because the objective $\mathbf{c}^T \mathbf{x}$ is linear, hence has constant gradient. Moving along any direction within the feasible set either strictly decreases, strictly increases, or leaves $\mathbf{c}^T \mathbf{x}$ unchanged. At an interior point or non-vertex boundary point: some direction strictly decreases $\mathbf{c}^T \mathbf{x}$ without leaving the region — so you can move to a "more extreme" point.

## 5.6 Three Possible Outcomes

Q: What are the three possible outcomes for any LP?
A: (i) Optimal solution exists (finite optimum at a vertex). (ii) [Infeasible]: the feasible region is empty. (iii) [Unbounded]: the objective can be made arbitrarily small along a feasible direction. These three exhaust all cases — an LP never has, e.g., a unique supremum that isn't attained (finite LP duality forbids this).

Q: Why does LP's "three outcomes" structure simplify solver design?
A: Because any solver need only detect which case holds and report accordingly. Simplex naturally terminates with one of three verdicts: optimal BFS, infeasibility certificate (via Farkas), or unbounded direction. No "solver failed to converge" ambiguity (unlike general NLP).

## 5.7 Geometric Intuition

Q: What does LP look like geometrically in 2D?
A: The feasible region is a polygon (often bounded). The objective $c_1 x_1 + c_2 x_2$ defines a family of parallel lines (level sets). Minimizing $\mathbf{c}^T \mathbf{x}$: translate the level line in the $-\mathbf{c}$ direction until it just touches the polygon — that contact point is the optimal vertex. Simplex walks corner-to-corner along the polygon boundary toward this touching vertex.

## 5.8 Common LP Formulations

Q: Formulate the [diet problem] as an LP.
A: Variables: $x_j$ = amount of food $j$ to eat. Constraints: $\sum_j a_{ij} x_j \geq b_i$ for each nutrient $i$ (meet nutritional minimum). Bounds: $x_j \geq 0$. Objective: $\min \sum_j c_j x_j$ (minimize total cost). Classic formulation — Dantzig's original motivation for simplex (1947): feed US Army adequately at minimum cost.

Q: Formulate the [transportation problem] as an LP.
A: Variables: $x_{ij}$ = units shipped from source $i$ to destination $j$. Constraints: $\sum_j x_{ij} \leq s_i$ (source capacity), $\sum_i x_{ij} \geq d_j$ (demand), $x_{ij} \geq 0$. Objective: $\min \sum_{ij} c_{ij} x_{ij}$ (shipping cost). Network flow specialization; solvable by transportation simplex more efficiently than generic LP.

Q: Formulate [linear regression with $\ell_1$ loss] (minimum absolute deviation) as an LP.
A: Goal: minimize $\sum_i |y_i - \mathbf{a}_i^T \mathbf{x}|$. Introduce auxiliary variables $t_i \geq 0$ bounding each residual: add constraints $y_i - \mathbf{a}_i^T \mathbf{x} \leq t_i$ and $-(y_i - \mathbf{a}_i^T \mathbf{x}) \leq t_i$. Objective: $\min \sum_i t_i$. The LP trick "replace $|z|$ with a bounding $t$" is extensively used to convert $\ell_1$-norm problems to LPs.

## 5.9 The Chebyshev Center

Q: How is the [Chebyshev center] of a polytope $\{A\mathbf{x} \leq \mathbf{b}\}$ found via LP?
A: Maximize the radius of a ball inscribed in the polytope. Variables: center $\mathbf{x}_c$, radius $r \geq 0$. Constraint: $\mathbf{a}_i^T \mathbf{x}_c + r \|\mathbf{a}_i\| \leq b_i$ for each row (center plus radius in normal direction still satisfies the constraint). Maximize $r$. Clean LP formulation of a geometric optimization problem.

## 5.10 Polyhedral Cones and Recession Directions

C: The [recession cone] of a polyhedron $P$ is $\{\mathbf{d} : \mathbf{x} + \alpha \mathbf{d} \in P \text{ for all } \mathbf{x} \in P, \alpha \geq 0\}$ — directions along which $P$ extends to infinity.

Q: When is an LP unbounded?
A: When the feasible polyhedron has a recession direction $\mathbf{d}$ with $\mathbf{c}^T \mathbf{d} < 0$ — you can move arbitrarily far in direction $\mathbf{d}$ while decreasing the objective. Simplex detects this automatically: if any nonbasic variable has negative reduced cost AND its column has no positive entries, the LP is unbounded.

## 5.11 LP Relaxation

Q: What is an [LP relaxation] of an integer program?
A: Drop the integrality constraints $x_i \in \mathbb{Z}$ and replace with $x_i \in \mathbb{R}$ (or $x_i \in [0, 1]$ for binary). The relaxation is an LP, solvable in polynomial time. Its optimum is a lower bound on the integer optimum. Core subroutine in branch-and-bound for integer programming. Without LP relaxation as the workhorse, large-scale integer programming wouldn't be practical.

## 5.12 Modeling Patterns

Q: Why does introducing slack, surplus, and auxiliary variables let LPs express a wider class of problems than they appear to?
A: Because seemingly nonlinear structures (absolute values, max/min, piecewise-linear functions, ratios with positive denominators) can be encoded by adding variables and linear constraints that enforce their meaning. "LP tricks": $|z| \leq t$ becomes $z \leq t \wedge -z \leq t$; $\max(a, b) \leq t$ becomes $a \leq t \wedge b \leq t$. Every LP modeler's working vocabulary.
