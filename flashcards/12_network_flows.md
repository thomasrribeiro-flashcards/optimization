+++
order = 12
subject = "mathematics"
tags = ["math", "optimization", "network-flows", "max-flow", "min-cost-flow", "combinatorial", "matching"]
+++

# Optimization — Network Flows and Combinatorial Optimization

## 12.1 What a Flow Network Is

C: A [flow network] is a directed graph $G = (V, E)$ with a nonnegative capacity $u_{ij}$ on each edge $(i, j) \in E$, a distinguished source $s \in V$, and a distinguished sink $t \in V$.

C: In a flow network, the nonnegative number $u_{ij}$ attached to each edge $(i, j) \in E$ is its [capacity].

C: In a flow network, the distinguished node $s$ is the [source] and the distinguished node $t$ is the [sink].

Q: Why is network flow a special class of LP worth studying separately?
A: Because flow LPs have a TOTALLY UNIMODULAR constraint matrix: the LP relaxation of an integer flow problem automatically has integer-valued optimal vertices. Integer flow is solvable as an LP — no branch-and-bound needed. Additionally, specialized algorithms (augmenting path, push-relabel) beat general LP by orders of magnitude on network structure.

## 12.2 Flows and Feasibility

C: A [flow] in a network is a function $f: E \to \mathbb{R}_{\geq 0}$ satisfying the capacity condition $0 \leq f_{ij} \leq u_{ij}$ on each edge and conservation $\sum_j f_{ji} = \sum_j f_{ij}$ at every node except $s$ and $t$.

C: In the definition of a network flow, the condition $0 \leq f_{ij} \leq u_{ij}$ on each edge is [capacity] and the condition $\sum_j f_{ji} = \sum_j f_{ij}$ at every node except $s$ and $t$ is [conservation] — what comes in equals what goes out.

Q: What is the [value] of an $s$-$t$ flow?
A: $|f| = \sum_j f_{sj} - \sum_j f_{js}$ — net flow out of the source. By conservation, equivalent to net flow into the sink: $\sum_j f_{jt} - \sum_j f_{tj}$. Total throughput the network carries from $s$ to $t$.

## 12.3 The Maximum Flow Problem

Q: What is the [maximum flow problem]?
A: Given a flow network with source $s$ and sink $t$: find a feasible $s$-$t$ flow maximizing $|f|$. Models: shipping goods through a transportation network, routing packets through the internet, assigning tasks to workers. Polynomial-time solvable; underpins many combinatorial algorithms.

Q: Write the maximum flow problem as an LP.
A: Variables $f_{ij}$ for each edge. Maximize $\sum_j f_{sj} - \sum_j f_{js}$ s.t. $0 \leq f_{ij} \leq u_{ij}$ for all $(i, j)$; $\sum_j f_{ji} - \sum_j f_{ij} = 0$ for all $i \neq s, t$. The constraint matrix is [totally unimodular] (every square submatrix has determinant in $\{-1, 0, 1\}$) — integer capacities give integer optimal flows automatically.

## 12.4 s-t Cuts

C: An [s-t cut] is a partition of $V$ into sets $S$ and $T = V \setminus S$ with $s \in S$, $t \in T$. Its [capacity] is $C(S, T) = \sum_{(i, j) \in E : i \in S, j \in T} u_{ij}$ — the sum of $u_{ij}$ over edges crossing from $S$ to $T$.

Q: What is the [max-flow min-cut theorem]?
A: For any flow network: the maximum value of an $s$-$t$ flow EQUALS the minimum capacity of an $s$-$t$ cut. "Bottleneck" intuition: the flow is limited exactly by the smallest "choke point" across the network. Proved via augmenting paths; a fundamental LP-duality instance (max-flow LP and min-cut LP are dual).

Q: Why is max-flow min-cut an instance of LP duality?
A: Because the LP dual of max-flow is exactly the min-cut LP: dual variables correspond to node potentials and edge indicators; dual constraints encode the cut structure. Totally unimodular matrix makes the dual's LP relaxation attain an integer cut. Strong duality then gives equality — the combinatorial result is LP duality in disguise.

## 12.5 Augmenting Paths

C: An [augmenting path] is a simple path from $s$ to $t$ in the residual graph along which the flow can be increased.

Q: Given a flow $f$, what edges make up the [residual graph]?
A: Forward edges $(i, j)$ with residual capacity $u_{ij} - f_{ij}$ and backward edges $(j, i)$ with residual capacity $f_{ij}$.

Q: Describe the [Ford-Fulkerson] augmenting path algorithm.
A: Start with zero flow. Repeat: find an augmenting path in the residual graph; push as much flow as the bottleneck edge allows; update residual graph. Terminate when no augmenting path exists. By max-flow min-cut, this gives an optimal flow. Termination guaranteed for integer capacities (each augmentation adds $\geq 1$ flow).

Q: Why doesn't Ford-Fulkerson terminate in polynomial time with arbitrary path choice?
A: Because with irrational capacities it may run forever; even with integer capacities, poor path choices give pseudo-polynomial runtime $O(E \cdot |f^*|)$. Fixed by [Edmonds-Karp]: always pick the SHORTEST augmenting path (BFS) — gives $O(V E^2)$ polynomial runtime regardless of capacities.

## 12.6 Push-Relabel Algorithm

Q: What does the [push-relabel] algorithm for maximum flow do differently from augmenting paths?
A: Maintains a "preflow" — conservation may be violated (nodes have excess). Each node has a height label. Two operations: [push] excess from higher to lower neighbor along residual edge; [relabel] increase a node's height when no lower neighbor is available. Eventually, excess drains to $s$ or $t$. Best variant: $O(V^2 \sqrt{E})$. Outperforms augmenting paths on dense graphs.

## 12.7 Min-Cost Flow

Q: What is the [minimum cost flow problem]?
A: Given capacities $u_{ij}$ and per-unit costs $c_{ij}$ on edges, and a required total flow value $F$: find a feasible flow minimizing total cost $\sum_{ij} c_{ij} f_{ij}$. Generalizes max-flow (constant cost) and shortest paths (unit flow). Solvable in polynomial time via cycle-canceling, successive shortest paths, or network simplex.

Q: What is [network simplex], and how does it exploit flow structure?
A: A specialization of simplex tailored to minimum-cost flow. Basic feasible solutions correspond to [spanning trees] of the network: tree edges carry variable flow, non-tree edges are at capacity bounds. Each pivot changes one tree edge. Pivots take $O(E)$ vs. $O(VE)$ for generic simplex. One of the fastest practical algorithms for min-cost flow.

## 12.8 Shortest Path as Min-Cost Flow

Q: How is the [shortest path problem] a special case of minimum cost flow?
A: Set flow requirement $F = 1$, unit edge capacities $u_{ij} = 1$, edge costs $c_{ij}$ = distances. Minimum cost flow = shortest path (if the graph is acyclic with nonnegative costs). Totally unimodular structure gives integer flow, which traces out one path. Shortest-path algorithms (Dijkstra, Bellman-Ford) solve this specialized case faster than general min-cost flow.

## 12.9 Bipartite Matching

Q: What is the [maximum bipartite matching problem]?
A: Given a bipartite graph $G = (L \cup R, E)$: find the largest subset of edges such that no two edges share a vertex. Models: workers ↔ tasks, students ↔ projects, kidneys ↔ recipients. Reduces to max-flow: add source $s$ connected to $L$, sink $t$ connected from $R$, all capacities 1; max flow = max matching size. Solvable in $O(E \sqrt{V})$ via Hopcroft-Karp.

Q: How does [König's theorem] connect bipartite matching to vertex cover?
A: In any bipartite graph: the size of a [maximum matching] equals the size of a [minimum vertex cover]. A direct consequence of LP duality applied to the matching LP (which has integer vertices for bipartite graphs). Vertex cover is NP-hard on general graphs but polynomial on bipartite ones — exactly because of this LP integrality property.

## 12.10 Assignment and Transportation

Q: What is the [assignment problem]?
A: Given a cost matrix $c_{ij}$ (cost of assigning worker $i$ to task $j$): find a one-to-one matching between workers and tasks minimizing total cost. Special bipartite min-cost flow with unit capacities. Hungarian algorithm solves it in $O(n^3)$. LP relaxation is automatically integer (Birkhoff-von Neumann theorem: vertices of the doubly stochastic polytope are permutation matrices).

Q: What is the [transportation problem]?
A: Given supplies $s_i$ at sources and demands $d_j$ at destinations with per-unit shipping cost $c_{ij}$: minimize total cost subject to supply/demand constraints. A bipartite min-cost flow; transportation simplex is a specialized network simplex exploiting the bipartite structure. Dates to Kantorovich (1939), Dantzig (1947).

## 12.11 Total Unimodularity

C: A matrix is [totally unimodular (TU)] if every square submatrix has determinant $\in \{-1, 0, 1\}$.

Q: Why does total unimodularity of the constraint matrix guarantee integer LP optima?
A: Because for an LP $\{\min \mathbf{c}^T\mathbf{x} : A\mathbf{x} = \mathbf{b}, \mathbf{x} \geq \mathbf{0}\}$ with TU $A$ and integer $\mathbf{b}$: Cramer's rule gives every basic feasible solution as $\mathbf{x}_B = A_B^{-1} \mathbf{b}$ with $\det(A_B) \in \{\pm 1\}$ and integer numerators — so $\mathbf{x}_B$ is integer. Every vertex is integer; LP relaxation solves the IP. Applies to node-arc incidence matrices of directed graphs — why network flow integer-LP = network flow LP.

## 12.12 A Worked Max-Flow

P: Find the maximum flow in a network with source $s$, sink $t$, and edges $s \to a$ (capacity 3), $s \to b$ (2), $a \to b$ (1), $a \to t$ (2), $b \to t$ (3).
S:
**IDENTIFY**: 4 nodes, 5 edges. Small enough to trace augmenting paths by hand.

**PLAN**: Apply Ford-Fulkerson with BFS (Edmonds-Karp): repeatedly find shortest augmenting path, push bottleneck flow, update residual.

**EXECUTE**: Start with zero flow. Path 1: $s \to a \to t$, bottleneck $\min(3, 2) = 2$; push 2. Residual: $s\to a$ has 1 left, $a \to t$ full. Path 2: $s \to b \to t$, bottleneck $\min(2, 3) = 2$; push 2. Residual: $s \to b$ full, $b \to t$ has 1 left. Path 3: $s \to a \to b \to t$, bottleneck $\min(1, 1, 1) = 1$; push 1. No more augmenting paths ($s \to a$ and $s \to b$ are both saturated).

**EVALUATE**: Total flow $= 2 + 2 + 1 = 5$. Verify by min-cut: cut $S = \{s\}$ has capacity $3 + 2 = 5$ — matches the max flow, confirming optimality via max-flow min-cut theorem. All edges from $s$ are saturated; $\{s\}$ is the tight cut. The example shows augmenting paths can re-route flow (path 3 sends through $a \to b$, effectively re-using the saturated $s \to a$ capacity).
