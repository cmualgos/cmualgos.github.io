---
layout: post
title: "Lecture 14: Applications of Concentration Inequalities"
date: 2018-09-28
use_math: true
---

## Random walk application: 2-SAT

Suppose we want to solve the classic 2-SAT formula. As a reminder of the problem, you are given a 2-CNF formula like $(x_1 \lor \neg x_2) \land (x_2 \lor x_5) \land (\neg x_5 \lor x_3) \ldots \land ( \ldots )$ for which we want to find a satisfying assignment or report none exists. Polynomial time solutions are known for a long time and there even exists a [linear-time solutions](http://www.math.ucsd.edu/~sbuss/CourseWeb/Math268_2007WS/2SAT.pdf) based on finding strongly connected components in a graph. We will, however, focus on a surprisingly simple randomized algorithm that runs in polynomial time.

**Algorithm** Start with a random assignment $(x_1, \ldots, x_n)$ where we initialize $x_i$ by flipping an unbiased coin. While there exists a unsatisfied clause $C$ involving $\{ x_i, x_j \}$ we randomly choose $k \gets \mathrm{random}(\\{i, j\\})$ and assign $x_k \gets \neg x_j$. If we ever find an assignment that satisfies all the clauses, we output the solution $x$. Otherwise, we stop the loop after $O(n^2)$ steps.

Note that $x_k \gets \neg x_k$ will satisfy the $C$, it might make some other clauses unsatisfied. Also note that if there are no satisfying assignments, the algorithm is always correct. We only need to prove the converse:

**Lemma** If the 2-SAT formula is satisfiable, the above algorithm will find a satisfying assignment with at least constant probability.

**Proof** Let $y = (y_1, \ldots, y_n)$ be any satisfying assignment and let $D = |\\{ i \in [n] \mid x_i \neq y_i \\}|$ be the number of variables in $x$ whose value differs from $y$. Initially, $D$ is distributed as a binomial distribution $\mathrm{Bin}(n, 1/2)$. The algorithm successfully terminates if ever reaches the value of $D = 0$. Suppose that at some iteration of the while loop we find that a clause involving $\\{x_i, x_j\\}$ is not satisfied. We note that then either $x_i \neq y_i$ or $x_j \neq y_j$ (or both), hence $D$ increases by $+1$ with probability at least $1/2$ (and decreases by $1$ with probability at most $1/2$). In other words, the distribution of $D$ is smaller (better) than the distribution of an unbiased random walk $D'$ with the same initial distribution. More formally, $\forall \lambda, \Pr[D \le \lambda] \ge \Pr[D' \le \lambda]$.