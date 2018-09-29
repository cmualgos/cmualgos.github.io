---
layout: post
title: "Lecture 14: Applications of Concentration Inequalities"
date: 2018-09-28
use_math: true
---

## Random walk application: 2-SAT

Suppose we want to solve the classic 2-SAT task. As a reminder of the problem, we are given a 2-CNF formula like $(x_1 \lor \neg x_2) \land (x_2 \lor x_5) \land (\neg x_5 \lor x_3) \ldots \land ( \ldots )$ for which we want to find a satisfying assignment or report that none exists. Polynomial time solutions for the 2-SAT are known for a long time and there even exist [linear-time solutions](http://www.math.ucsd.edu/~sbuss/CourseWeb/Math268_2007WS/2SAT.pdf) based on finding strongly connected components in a directed graph. We will, however, focus on a surprisingly simple randomized algorithm that runs in polynomial time.

**Algorithm** Start with a random assignment $(x_1, \ldots, x_n)$ where we initialize $x_i$ by flipping an unbiased coin. While there exists a unsatisfied clause $C$ involving $\\{ x_i, x_j \\}$ we randomly choose $k \gets \mathrm{random}(\\{i, j\\})$ and assign $x_k \gets \neg x_k$. If we ever find an assignment that satisfies all clauses, we output the solution $x$. Otherwise, we stop after $2 n^2$ iterations of the while loop.

Note that after performing $x_k \gets \neg x_k$ the solution always satisfies the clause $C$ (although some other clause might get unsatisfied by it). Also note that if there are no satisfying assignments, the algorithm is always correct. We only need to prove the converse:

**Lemma** If the 2-SAT formula is satisfiable, the above algorithm will find a satisfying assignment with at least constant probability.

**Proof** Let $y = (y_1, \ldots, y_n)$ be any satisfying assignment and let $D = \\|\\{ i \in [n] \mid x_i \neq y_i \\}\\|$ be the number of variables in $x$ whose value differs from $y$. Initially, $D^0$ is distributed as a binomial distribution $\mathrm{Bin}(n, 1/2)$, hence it is symmetric around $n/2$ and will be highly concentrated around it. For concreteness, we can assert that $\Pr[D^0 \in [\frac{1}{4} n, \frac{3}{4} n]] \ge 1 - 2\exp(-\Omega(n)) \ge 1/2$.

  The algorithm successfully terminates if it ever reaches the value of $D^\tau = 0$. Suppose that at some iteration of the while loop we find that a clause involving $\\{x_i, x_j\\}$ is not satisfied. We note that then either $x_i \neq y_i$ or $x_j \neq y_j$ (or both), hence $D^{k+1}$ increases by $+1$ with probability at least $1/2$ (and decreases by $1$ with probability at most $1/2$). In other words, the distribution of $D^{k}$ is smaller (better) than the distribution of an unbiased random walk $D'^{k}$ with the same initial distribution. More formally, for any number of steps $k \ge 0$ we have that $\forall \lambda,\ \Pr[D^k \le \lambda] \ge \Pr[D'^k \le \lambda]$.

  By the stopped random walk property mentioned in the lecture we know that an unbiased random walk that starts at $D'^0 \in [\frac{1}{4}n, \frac{3}{4}n]$ and stops when $D'^\tau = 0$ or $D'^\tau = n$ takes in expectation $\mathbb{E}[\tau] = \frac{3}{4}n \frac{3}{4}n \le n^2$ time to stop. Hence if we condition $D'$ to start at the aforementioned interval, after $2n^2$ iterations it stops with probability at least $1/2$ by Markov, and conditioned on stopping it hits $D' = 0$ with probability exactly $1/2$ by symmetry. In summary, after $2 n^2$ steps, the algorithm successfully termintes with probability at least $1/8$.
  