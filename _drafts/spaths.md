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

