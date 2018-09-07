---
layout: post
title: "Lecture 4: Shortest-Paths"
date: 2018-09-05
use_math: true
---

## Feasible Potentials

Recall the notion of a feasible potential: it is an assignment of values
to nodes so that the "reduced" weight $\hat{w}(u,v) = \phi_u + w(u,v) -
\phi_v$ becomes non-negative for every edge. E.g., if the original costs
were non-negative, then the all-zeros potential is feasible. Else we
showed how to compute it using a single call to Bellman-Ford. Let's
observe some facts about these potentials.

1. We claim that $\phi_t - \phi_s$ is a _lower bound_ on the
shortest-path distance from $s$ to $t$ (with respect to weights
$w$). Indeed, consider the shortest-path $P$ from $s$ to $t$ w.r.t $w$:
summing the reduced weight over the edges in $P$ gives us $\phi_s + w(P)
- \phi_t \geq 0$, or $\phi_t - \phi_s \leq w(P)$.

Why is this fact interesting? Suppose we set $\phi_s = 0$, and try to
*maximize* $\phi_t$. I.e., we try to get the _best lower bound_ we can
get via feasible potentials. It turns out that the maximum value of
$\phi_t$ is the shortest-path distance from $s$ to $t$. (Indeed, just
setting $\phi_u = d_w(s,u)$ gives you such a solution.) Similarly, if
you try to maximize $\sum_u \phi_u$ subject to $\phi$ being a feasible
potential and $\phi_s = 0$, setting $\phi_u = d_w(s,u)$ gives you the
maximum value for this LP as well. So this is a way to solve SSSP using
via a linear program:

\begin{alignat*}{2}
  \max \sum_{u \neq s} & \phi_u && \\
  \phi_v - \phi_u &\leq w_{uv} &\qquad& \forall (u,v) \in E \\
  \phi_s &\leq 0  &&
\end{alignat*}
Note that we set $\phi_s \leq 0$, but since we're maximizing in the
objective, the optimal solution will ensure $\phi_s = 0$.
  
Now suppose we take the dual of this LP. Let the dual variables be
$f_{uv}$ for the first set of primal constraints, and $g$ for the last
constraint. Since the primal constraints are inequalities, the dual
variables are non-negative. And the primal variables are unconstrained,
so the dual constraints are equalities.

\begin{alignat*}{2}
  \min \sum_{(u,v) \in E} & w_{uv} f_{uv} && \\
  \sum_{x: (x,u) \in E} f_{xu} - \sum_{x: (u,x) \in E} f_{ux} &= 1
  &\qquad& \forall u \neq s  \\
  \sum_{x: (x,s) \in E} f_{xs} - \sum_{x: (s,x) \in E} f_{sx} + g &= 0 &&   \\
  f_{uv}, g &\geq 0  &&
\end{alignat*}

This sends flow from $s$ to all nodes in $V \setminus s$, such that $1$
unit ends at each other node. The cheapest way to send this flow would
be along a shortest path, so this dual LP uses min-cost flow to compute
the SSSP!

By the way, Goran mentioned this paper [_Negative-Weight Shortest Paths
and Unit Capacity Minimum Cost Flow_](https://arxiv.org/abs/1605.01717)
getting an improvement to Bellman-Ford. it solves the min-cost flow
problem on unit-capacity undirected graphs (and hence the single-source
shortest-path problem with negative edge weights) in time
$\tilde{O}(m^{10/7} \log M)$ time. And indeed, it's an improvement (for
graphs that are sparse enough) to Goldberg's $O(m\sqrt{n} \log M)$-time
algorithm I'd mentioned in class, but does not improve Bellman-Ford when
the edge-weights are not small integers.

## Finding the Paths, not just Lengths

In lecture we only discussed computing the _lengths_ of the shortest
path, and not finding the paths themselves. For all the classical
algorithms, this is easy to implement. (Please make sure you see how to
find the shortest paths.)

For Seidel's algorithm, it's a little more tricky. Indeed, since the
runtime of Seidel's algorithm is strictly sub-cubic, a first question
is: can we even write down the shortest paths in $n^{\omega}$ time, even
though the total length of all these paths may be $\Omega(n^3)$? The
insight is that we don't need to write down the paths: we just need to
write down the _successor_ pointers -- i.e., for each pair $i,j$, define
$S_j(i)$ to be the *second* node on a shortest $i$-$j$ path (the first
node being $i$, and the last being $j$). Then to get the entire $i$-$j$
shortest path, we just follow these pointers: $i, S_j(i), S_j(S_j(i)),
\ldots, j$. So there is a representation of all shortest paths that uses
at most $O(n^2 \log n)$ bits.

The main idea for computing the successor matrix for Seidel's algorithm
is to solve the Boolean Product Matrix Witness problem: given $n \times
n$ Boolean matrices $A, B$, compute an $n \times n$ matrix $W$ such that
$W_{ij} = k$ if $A_{ik} = B_{kj} = 1$, and $W_{ij} = 0$ if no such $k$
exists. We will hopefully see (and solve) this problem in Homework 2.

## Fast Matrix Multiplication

If you've not seen [Strassen's
algorithm](https://en.wikipedia.org/wiki/Strassen_algorithm) before, it
is an algorithm for multiplying $n \times n$ matrices in time $n^{\log_2
7} \approx n^{2.81}}$. It's quite simple to state, and one can think of
it as a 2-dimensional version of [Karatsuba's
algorithm](https://en.wikipedia.org/wiki/Karatsuba_algorithm) for
multiplying two numbers. Mike Paterson has a very beautiful [geometric
interpretation](http://www.cs.cmu.edu/afs/cs.cmu.edu/academic/class/15850-s17/www/handouts/paterson.pdf)
of the sub-problems Strassen comes up with, and how they relate to
Karatsuba.

The time for matrix multiplication was later improved by several people,
culminating in the
[Coppersmith-Winograd](https://en.wikipedia.org/wiki/Coppersmith-Winograd_algorithm)
algorithm to get $\omega = 2.376$.  The C-W algorithm looks being
similar to Strassen's algorithm in that it breaks the matrices into
smaller blocks and recurses on them. But the recursion is quite
mysterious, at least to me. Smaller improvements were recently given by
Andrew Strothers and (our own) Virginia Vassilevska-Williams. Here's a
[survey](http://people.csail.mit.edu/virgi/sigactcolumn.pdf) Virginia
wrote about these developments, giving an overview of these results.

In 2005, Cohn, Kleinberg, Szegedy, and Umans gave group-theoretic
approaches that combine some of the C-W ideas with some group-theoretic
ideas, to match the C-W bound. They also gave conjectures that would
lead to $\omega = 2$. However, two years back, in 2016 [Blasiak et
al.](https://arxiv.org/abs/1605.06702) showed hurdles in proving their
conjectures.

Even more recently, Alman and Vassilevska-Williams (in [this ITCS 2018
paper](https://arxiv.org/pdf/1712.07246.pdf) and an upcoming FOCS 2018
paper) have shown the limitations of all known approaches. Here's a
[blog
post](https://rjlipton.wordpress.com/2018/08/30/limits-on-matrix-multiplication/)
giving an overview of their results.
