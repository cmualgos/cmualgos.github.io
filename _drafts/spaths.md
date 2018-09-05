---
layout: post
title: "Lecture 4: Shortest-Paths"
date: 2018-09-05
use_math: true
---

+ We defined the notion of a feasible potential: this was an assignment
of values to the nodes so that the "reduced" weight of an edge:
$\hat{w}(u,v) = \phi_u + w(u,v) - \phi_v$ becomes non-negative. E.g., if
the original costs were non-negative, then the all-zeros potential is
feasible. 

We claim that $\phi_t - \phi_s$ is a _lower bound_ on the shortest-path
distance from $s$ to $t$ (with respect to weights $w$). Indeed, consider
the shortest-path $P$ from $s$ to $t$ w.r.t $w$: summing the reduced
weight over the edges in $P$ gives us $\phi_s + w(P) - \phi_t \geq 0$,
or $\phi_t - \phi_s \leq w(P)$. 

Why is this interesting? Suppose we set $\phi_s = 0$, and try to
maximize $\phi_t$ - i.e., we try to get the _best lower bound_ we can
get via feasible potentials. It turns out that the maximum value of
$\phi_t$ will be the shortest-path distance from $s$ to $t$. (Indeed,
just setting $\phi_u = d_w(s,u)$ gives you such a solution.) Similarly,
if you try to maximize $\sum_u \phi_u$ subject to $\phi$ being a
feasible potential and $\phi_s = 0$, setting $\phi_u = d_w(s,u)$ gives
you the maximum value for this LP as well. So this is a way to solve
SSSP using linear programs. 

Another way to solve 

+ Goran had mentioned some improvement to Bellman-Ford. Here's the
paper: it solves the min-cost flow problem on unit-capacity undirected
graphs (and hence the single-source shortest-path problem with negative
edge weights) in time $\tilde{O}(m^{10/7} \log M)$ time. It's an
improvement to Goldberg's $O(m\sqrt{n} \log M)$-time algorithm I'd
mentioned in class (for graphs that are sparse enough), but does not
really directly improve Bellman-Ford when the edge-weights are not
small integers. 

+ Today we only discussed computing the _lengths_ of the shortest path,
and not finding the paths themselves. For all the classical algorithms,
this is clear. For Seidel's algorithm, not so much. Indeed, the first
question is: can we even write down the shortest paths in $n^{\omega}$
time? The insight is that we don't need to write down the paths: we just
need to write down the _successor_ pointers -- i.e., for each pair
$i,j$, define $succ_j(i)$ to be the second node on a shortest $i$-$j$
path (the first being $i$, and the last being $j$). Then to get the
entire $i$-$j$ shortest path, we just follow these pointers: $i,
succ_j(i), succ_j(succ_j(i)), \ldots, j$. So there is a compact representation.
