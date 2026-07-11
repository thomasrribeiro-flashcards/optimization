+++
order = 2
subject = "mathematics"
tags = ["math", "optimization", "convex-sets", "polyhedra", "cones", "hulls"]
+++

# Optimization — Convex Sets

## 2.1 Why Convex Sets Matter

Q: Why does convex optimization take convex SETS as foundational before discussing convex functions?
A: Because the feasible region of every convex optimization problem is a convex set, and the geometry of that region drives algorithm design. Simplex walks on vertices of polyhedra; interior-point methods traverse the interior; projection methods iteratively project onto convex sets. Understanding these sets precedes everything.

## 2.2 Convex Combinations

C: A [convex combination] of points $\mathbf{x}_1, \dots, \mathbf{x}_k$ is $\sum_i \lambda_i \mathbf{x}_i$ where $\lambda_i \geq 0$ and $\sum_i \lambda_i = 1$ — a weighted average with nonnegative weights summing to $1$.

Q: How is a convex combination different from a linear combination?
A: A [linear combination] allows any real weights; a convex combination requires weights to be nonnegative AND sum to exactly $1$. Geometrically: linear combinations fill the whole span; convex combinations fill the region "between" the points — a segment for $k = 2$, a triangle for $k = 3$, a simplex for general $k$.

## 2.3 Convex Sets

C: A set $C \subseteq \mathbb{R}^n$ is [convex] if for every two points $\mathbf{x}, \mathbf{y} \in C$ and every $\lambda \in \lbrack 0, 1\rbrack $, the point $\lambda \mathbf{x} + (1 - \lambda) \mathbf{y}$ is also in $C$ — the entire line segment between any two points stays inside.

Q: Why is the segment-containment definition equivalent to requiring all convex combinations to lie in the set?
A: By induction: $2$-point combinations (the definition) yield segments. A $3$-point combination can be written as a convex combination of a 2-point result and the third point — staying in $C$ by the definition. Extending to $k$ points: any convex combination is a convex combination of a convex combination, so convexity closes under arbitrary finite convex combinations.

Q: Name three basic examples of convex sets in $\mathbb{R}^n$.
A: Half-spaces, Euclidean balls, and affine subspaces.

Q: Why is every [half-space] $\{\mathbf{x} : \mathbf{a}^T \mathbf{x} \leq b\}$ convex?
A: Because the map $\mathbf{x} \mapsto \mathbf{a}^T \mathbf{x}$ is linear, so for any $\mathbf{x}, \mathbf{y}$ in the half-space and $\lambda \in [0, 1]$: $\mathbf{a}^T (\lambda \mathbf{x} + (1-\lambda)\mathbf{y}) = \lambda \mathbf{a}^T\mathbf{x} + (1-\lambda) \mathbf{a}^T\mathbf{y} \leq \lambda b + (1-\lambda) b = b$. The weighted average of two numbers both $\leq b$ is $\leq b$.

Q: Why is every [Euclidean ball] $\{\mathbf{x} : \|\mathbf{x} - \mathbf{c}\| \leq r\}$ convex?
A: Because by the triangle inequality, $\|\lambda \mathbf{x} + (1-\lambda)\mathbf{y} - \mathbf{c}\| \leq \lambda \|\mathbf{x} - \mathbf{c}\| + (1-\lambda)\|\mathbf{y} - \mathbf{c}\| \leq \lambda r + (1-\lambda) r = r$. Norms are convex functions; their sublevel sets are convex sets.

Q: Why is every [affine subspace] $\{\mathbf{x} : A\mathbf{x} = \mathbf{b}\}$ convex?
A: Because linearity gives $A(\lambda \mathbf{x} + (1-\lambda)\mathbf{y}) = \lambda A\mathbf{x} + (1-\lambda) A\mathbf{y} = \lambda \mathbf{b} + (1-\lambda)\mathbf{b} = \mathbf{b}$. Affine sets are stronger than convex — they contain the entire LINE through any two of their points, not just the segment.

## 2.4 Operations Preserving Convexity

Q: What operations preserve convexity of sets?
A: Intersection, affine images and preimages, Cartesian products, and Minkowski sums. Union does NOT preserve convexity.

C: The [Minkowski sum] of convex sets, $C_1 + C_2 = \{\mathbf{x} + \mathbf{y} : \mathbf{x} \in C_1, \mathbf{y} \in C_2\}$, is convex.

C: The [affine image] $\{A\mathbf{x} + \mathbf{b} : \mathbf{x} \in C\}$ and [affine preimage] $\{\mathbf{x} : A\mathbf{x} + \mathbf{b} \in C\}$ of a convex set $C$ are convex.

Q: Why is the intersection of arbitrarily many convex sets still convex?
A: Because convexity is a pointwise property of pairs: if $\mathbf{x}, \mathbf{y}$ are both in every $C_i$, then so is $\lambda \mathbf{x} + (1-\lambda)\mathbf{y}$ (from each $C_i$ individually) — hence in the intersection. Extends to infinite intersections. Consequence: the feasible region of any problem with convex inequality constraints is convex (intersection of convex half-spaces / level sets).

## 2.5 The Convex Hull

C: The [convex hull] of a set $S$, written $\text{conv}(S)$, is the smallest convex set containing $S$ — equivalently, the set of all finite convex combinations of points in $S$.

Q: What does [Carathéodory's theorem] say about convex hulls in $\mathbb{R}^n$?
A: Every point in $\text{conv}(S) \subset \mathbb{R}^n$ can be written as a convex combination of at most $n + 1$ points from $S$. You never need more than $n + 1$ support points, regardless of how many $S$ has. Used in combinatorial optimization and linear programming to prove extreme-point representation theorems.

## 2.6 Affine Sets

C: An [affine set] is a translate of a linear subspace: $C = \mathbf{x}_0 + V$ for some linear subspace $V$. Equivalently, $C$ contains the entire LINE through any two of its points.

Q: Why is every affine set also a convex set?
A: Because "contains every line through two points" is strictly stronger than "contains every segment." Lines $\supseteq$ segments. So affine $\Rightarrow$ convex; the converse fails (a disk is convex but not affine).

## 2.7 Cones

C: A [cone] is a set closed under nonnegative scalar multiplication: if $\mathbf{x} \in K$ and $\alpha \geq 0$, then $\alpha \mathbf{x} \in K$.

C: A [convex cone] is a cone that is also convex — equivalently, closed under nonnegative linear combinations: $\alpha_1 \mathbf{x}_1 + \alpha_2 \mathbf{x}_2 \in K$ for all $\alpha_i \geq 0$, $\mathbf{x}_i \in K$.

Q: Give three convex cones that appear throughout optimization.
A: The nonnegative orthant $\mathbb{R}^n_+$, the second-order cone $\mathcal{L}^n$, and the positive semidefinite cone $\mathcal{S}_+^n$.

Q: What is the [nonnegative orthant] as a convex cone?
A: $\mathbb{R}^n_+ = \{\mathbf{x} \in \mathbb{R}^n : x_i \geq 0 \text{ for all } i\}$ — the set of componentwise-nonnegative vectors. The feasible cone of LPs in standard form. Trivially a convex cone: sum of componentwise-nonnegative vectors is componentwise-nonnegative.

Q: What is the [second-order cone] (Lorentz cone)?
A: $\mathcal{L}^n = \{(\mathbf{x}, t) \in \mathbb{R}^{n-1} \times \mathbb{R} : \|\mathbf{x}\| \leq t\}$ — an "ice cream cone" whose height axis bounds the radial cross-section. Foundational for [second-order cone programming (SOCP)]: problems with constraints like $\|A_i \mathbf{x} + \mathbf{b}_i\| \leq \mathbf{c}_i^T \mathbf{x} + d_i$.

Q: What is the [positive semidefinite cone]?
A: $\mathcal{S}_+^n = \{X \in \mathbb{R}^{n \times n} : X = X^T, \mathbf{v}^T X \mathbf{v} \geq 0 \text{ for all } \mathbf{v}\}$ — the cone of symmetric PSD matrices. Foundation of [semidefinite programming (SDP)]. A genuine infinite-dimensional cone of enormous practical importance: relaxations of NP-hard combinatorial problems, control theory, quantum state tomography.

## 2.8 Polyhedra

C: A [polyhedron] is the intersection of finitely many half-spaces: $P = \{\mathbf{x} : A\mathbf{x} \leq \mathbf{b}\}$. A [polytope] is a bounded polyhedron.

Q: Why is every polyhedron convex?
A: Because each half-space is convex, and convexity is preserved under intersection. Even more: polyhedra inherit a rich combinatorial structure — vertices, edges, faces — that simplex exploits directly by walking along edges toward decreasing objective.

Q: What is a [vertex] (or extreme point) of a polyhedron?
A: A point $\mathbf{x} \in P$ that cannot be written as a proper convex combination of two other points in $P$ — algebraically, a point where at least $n$ of the defining inequalities hold with equality (assuming they are linearly independent). LP optimality is attained at a vertex whenever the optimum is finite.

Q: State the [Minkowski–Weyl representation theorem] for polytopes.
A: A set $P$ is a polytope if and only if $P$ is the convex hull of finitely many points. So a polytope has TWO representations: (i) [H-representation] as $\{A\mathbf{x} \leq \mathbf{b}\}$ (intersection of half-spaces); (ii) [V-representation] as $\text{conv}\{\mathbf{v}_1, \dots, \mathbf{v}_k\}$ (convex hull of vertices). Converting between them is sometimes exponentially hard.

## 2.9 Separating Hyperplane Theorem

Q: State the [separating hyperplane theorem] and why it underlies duality.
A: If $C$ and $D$ are disjoint nonempty convex sets (at least one open, or under closed-set compactness conditions), there exists a hyperplane $\mathbf{a}^T \mathbf{x} = b$ with $\mathbf{a}^T \mathbf{x} \leq b$ for all $\mathbf{x} \in C$ and $\mathbf{a}^T \mathbf{x} \geq b$ for all $\mathbf{x} \in D$. Foundation of linear programming duality, the Hahn–Banach theorem, and the Farkas lemma.

Q: State the [supporting hyperplane theorem].
A: For any convex set $C$ and any boundary point $\mathbf{x}_0 \in \partial C$, there exists a [supporting hyperplane] at $\mathbf{x}_0$: a half-space $\mathbf{a}^T \mathbf{x} \leq \mathbf{a}^T \mathbf{x}_0$ containing all of $C$. Every convex body can be reconstructed as the intersection of its supporting half-spaces — a "dual description" of convex sets.

## 2.10 Farkas' Lemma

Q: State [Farkas' lemma] and why it is central to LP duality.
A: Exactly one of the following holds: (i) $\exists \mathbf{x} \geq \mathbf{0}$ with $A\mathbf{x} = \mathbf{b}$; (ii) $\exists \mathbf{y}$ with $A^T \mathbf{y} \leq \mathbf{0}$ and $\mathbf{b}^T \mathbf{y} > 0$. A "theorem of the alternative": either the system is feasible, or there is a certificate of infeasibility. The dual LP encodes precisely this certificate.

## 2.11 Projections onto Convex Sets

Q: Why is the [projection onto a closed convex set] always unique?
A: Because minimizing $\|\mathbf{x} - \mathbf{y}\|^2$ over $\mathbf{y} \in C$ is a strictly convex problem on a convex set: the squared-distance function has positive-definite Hessian ($2I$), so the minimizer is unique. Projections onto half-spaces, balls, boxes, and affine sets all have closed-form formulas.

Q: What is the [projection onto a hyperplane] $\{\mathbf{x} : \mathbf{a}^T \mathbf{x} = b\}$?
A: $\text{proj}(\mathbf{y}) = \mathbf{y} - \frac{\mathbf{a}^T \mathbf{y} - b}{\|\mathbf{a}\|^2} \mathbf{a}$ — subtract the component of $\mathbf{y} - \mathbf{x}_0$ along $\mathbf{a}$. Basis for the Kaczmarz method (random projections for sparse linear systems) and many first-order optimization methods.

## 2.12 Dual Cones

C: The [dual cone] of $K$ is $K^* = \{\mathbf{y} : \mathbf{y}^T \mathbf{x} \geq 0 \text{ for all } \mathbf{x} \in K\}$ — the set of vectors whose inner product with every $\mathbf{x} \in K$ is nonnegative.

Q: Why is each of $\mathbb{R}^n_+$, $\mathcal{L}^n$, $\mathcal{S}_+^n$ [self-dual] ($K^* = K$)?
A: Because these are the three fundamental "symmetric cones": (i) nonnegative vectors have nonnegative inner products with each other; (ii) the second-order cone's "$\|\mathbf{x}\| \leq t$" is preserved under inner product; (iii) for PSD matrices, $\text{tr}(XY) \geq 0$ iff both are PSD. Self-duality unifies LP, SOCP, and SDP under the [conic programming] umbrella.
