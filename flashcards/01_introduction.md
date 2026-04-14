+++
order = 1
subject = "Math"
tags = ["math", "optimization", "introduction", "feasibility", "minimization"]
+++

# Optimization — Introduction and Problem Formulation

## 1.1 What Optimization Is

Q: What is an [optimization problem] in its most general form?
A: Choose a decision variable $\mathbf{x}$ from a set $\mathcal{X}$ to minimize (or maximize) an [objective function] $f(\mathbf{x})$ subject to constraints: $\min_{\mathbf{x} \in \mathcal{X}} f(\mathbf{x})$ subject to $g_i(\mathbf{x}) \leq 0$ and $h_j(\mathbf{x}) = 0$. Every optimization problem instantiates this template — the art is modeling a real situation in this form.

Q: Why are maximization and minimization problems interchangeable?
A: Because $\max f(\mathbf{x}) = -\min (-f(\mathbf{x}))$ over the same feasible set. So "maximize revenue" and "minimize negative revenue" give the same optimal $\mathbf{x}$. Optimization theory is traditionally stated for minimization; every result carries over to maximization by flipping signs.

## 1.2 Components of a Problem

C: The [decision variable] (also called [optimization variable]) $\mathbf{x} \in \mathbb{R}^n$ is the quantity chosen by the optimizer — the "levers" being tuned.

C: The [objective function] $f: \mathbb{R}^n \to \mathbb{R}$ assigns a real-valued score to every choice of $\mathbf{x}$; the optimizer seeks $\mathbf{x}$ minimizing $f$.

C: A [constraint] is a condition restricting which $\mathbf{x}$ are permissible — typically written $g_i(\mathbf{x}) \leq 0$ ([inequality constraint]) or $h_j(\mathbf{x}) = 0$ ([equality constraint]).

Q: What is the [feasible set] of an optimization problem?
A: The set $\mathcal{F} = \{\mathbf{x} \in \mathcal{X} : g_i(\mathbf{x}) \leq 0, h_j(\mathbf{x}) = 0 \text{ for all } i, j\}$ of all $\mathbf{x}$ satisfying every constraint. The optimizer searches only within $\mathcal{F}$; points outside are [infeasible].

## 1.3 Kinds of Solutions

C: A point $\mathbf{x}^*$ is a [global minimum] if $f(\mathbf{x}^*) \leq f(\mathbf{x})$ for every feasible $\mathbf{x}$. It is a [local minimum] if this inequality holds only on some neighborhood of $\mathbf{x}^*$.

Q: Why do global minima matter more than local minima in principle, but local minima dominate in practice?
A: Because a global minimum is the true best answer — but finding one on a nonconvex problem is NP-hard in general. Practical algorithms (gradient descent, Newton) guarantee only local minima. Convex problems are the exception: every local minimum is automatically global, eliminating the distinction.

Q: What is a [strict local minimum]?
A: A point $\mathbf{x}^*$ with $f(\mathbf{x}^*) < f(\mathbf{x})$ strictly for every feasible $\mathbf{x} \neq \mathbf{x}^*$ in some neighborhood. "Strict" rules out flat valleys where multiple points share the minimum value. Strict local minima are isolated; nonstrict ones may lie in a continuum.

## 1.4 Problem Classes by Structure

Q: What is a [linear program] (LP)?
A: An optimization problem where the objective is linear and all constraints are linear: $\min \mathbf{c}^T \mathbf{x}$ subject to $A\mathbf{x} \leq \mathbf{b}$, $\mathbf{x} \geq \mathbf{0}$. The feasible set is a polyhedron. LPs are solvable in polynomial time; this structure is exploited by the simplex and interior-point methods.

Q: What is a [quadratic program] (QP)?
A: An optimization problem with a quadratic objective and linear constraints: $\min \frac{1}{2}\mathbf{x}^T Q \mathbf{x} + \mathbf{c}^T \mathbf{x}$ subject to $A\mathbf{x} \leq \mathbf{b}$. When $Q$ is positive-semidefinite, the QP is convex and efficiently solvable; nonconvex QPs (indefinite $Q$) are NP-hard. Appears in portfolio optimization, support vector machines, model predictive control.

Q: What is [nonlinear programming] (NLP)?
A: Any optimization problem where the objective or constraints are nonlinear (and not LP / QP). NLPs range from tractable convex cases to arbitrarily hard nonconvex problems. General-purpose NLP solvers (IPOPT, SNOPT, KNITRO) use sequential quadratic programming or interior-point methods.

Q: What is [integer programming]?
A: An optimization problem where some decision variables are constrained to take integer values: $\mathbf{x} \in \mathbb{Z}^n$ or $\mathbf{x} \in \{0, 1\}^n$. NP-hard even with linear objective and constraints. Handles combinatorial decisions: scheduling, routing, facility location, network design.

## 1.5 Why Problem Structure Matters

Q: Why does the difficulty of optimization problems vary enormously across problem classes?
A: Because structure dictates which algorithms apply. LP: polynomial time. Convex QP: polynomial time. Nonconvex NLP: local minima only, no global guarantees in general. Integer LP: NP-hard. Recognizing the class of a problem (convex? linear? integer?) determines whether it's tractable at scale and which solver to use.

## 1.6 Infeasibility and Unboundedness

C: An optimization problem is [infeasible] if $\mathcal{F} = \emptyset$ — no choice of $\mathbf{x}$ satisfies all constraints simultaneously.

C: An optimization problem is [unbounded] if $\inf_{\mathbf{x} \in \mathcal{F}} f(\mathbf{x}) = -\infty$ — feasible points achieve arbitrarily small objective values.

Q: Why is detecting infeasibility useful even when it means no solution exists?
A: Because it tells the modeler their constraints are over-specified — a concrete diagnostic. Modern solvers return not just "infeasible" but an [irreducible infeasible subset] of constraints: a minimal conflict certificate. Helps practitioners identify which constraint to relax, which model assumption is wrong.

## 1.7 Modeling as an Optimization Problem

Q: Why is formulating a real-world problem as an optimization problem often harder than solving it?
A: Because real situations rarely present themselves with a clear objective, variables, and constraints. The modeler must: (i) identify decisions vs. fixed parameters, (ii) choose an objective that aligns with actual goals, (iii) translate messy physical/business rules into algebraic constraints. A well-formulated model is half the solution; a poorly formulated one gives mathematically optimal nonsense.

P: Formulate "a bakery has $10$ kg of flour and $6$ kg of sugar; a cake uses $2$ kg flour + $1$ kg sugar for $\$30$ profit, a pastry uses $1$ kg + $1$ kg for $\$20$; maximize profit" as an LP.
S:
**IDENTIFY**: Decisions: cakes $c$ and pastries $p$ to make. Resources: flour (10 kg), sugar (6 kg). Objective: maximize profit $30c + 20p$.

**PLAN**: List every constraint as a linear inequality; enforce nonnegativity.

**EXECUTE**: Variables $c, p \geq 0$. Flour: $2c + p \leq 10$. Sugar: $c + p \leq 6$. Objective: $\max 30 c + 20 p$. In standard LP form: $\min -30c - 20p$ s.t. $2c + p \leq 10$, $c + p \leq 6$, $c, p \geq 0$.

**EVALUATE**: The model captures both resources and the profit rate cleanly. Solution (by inspection or simplex): $c = 4, p = 2$ using all flour and sugar — profit $\$160$. The formulation — not the solve — was the main work.

## 1.8 Convex vs. Nonconvex

Q: Why is the [convex vs. nonconvex] distinction the fundamental dividing line in optimization?
A: Because convex problems admit provably-optimal polynomial-time algorithms: every local minimum is global, duality gives certificates of optimality, interior-point methods solve them efficiently. Nonconvex problems: local minima proliferate, no global certificates, NP-hardness is the norm. "Is this problem convex?" is the first question to ask.

## 1.9 Standard Forms

Q: Why are optimization problems cast in "standard forms"?
A: Because standard forms let solvers exploit structure without re-deriving algorithms per problem. LP standard form: $\min \mathbf{c}^T\mathbf{x}$ s.t. $A\mathbf{x} = \mathbf{b}$, $\mathbf{x} \geq \mathbf{0}$. Conic form: $\min \mathbf{c}^T \mathbf{x}$ s.t. $A\mathbf{x} = \mathbf{b}$, $\mathbf{x} \in \mathcal{K}$ for a convex cone $\mathcal{K}$. Modeling languages (CVXPY, JuMP) auto-convert user problems to solver standard forms.

## 1.10 Optimization Software Landscape

Q: What software stack does modern optimization use?
A: [Modeling language] (CVXPY, JuMP, AMPL, GAMS) lets users write problems symbolically. [Solvers] (CPLEX, Gurobi, Mosek for LP/QP; IPOPT for NLP; SCIP for integer) execute algorithms. The modeling layer converts user formulations to solver standard forms and routes to the right solver. This separation let the same model target different solvers.

## 1.11 Optimization in Applications

Q: Name three representative application domains and their dominant optimization flavors.
A: [Machine learning]: unconstrained smooth nonconvex (SGD on neural nets), convex QP (SVM), LP (linear regression with $\ell_1$). [Operations research]: LP (production planning), integer LP (scheduling, routing). [Control theory]: convex QP (MPC), SDP (robust control). Different communities, same underlying framework.

## 1.12 Taxonomy Summary

Q: How do you classify an optimization problem at a glance?
A: Ask in order: (i) Continuous or discrete variables? (ii) Constraints: linear, convex nonlinear, or general? (iii) Objective: linear, quadratic, convex nonlinear, or general? This places the problem in LP / QP / convex NLP / general NLP / IP / MINLP. Classification determines both solvability and choice of algorithm.
