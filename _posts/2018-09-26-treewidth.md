---
layout: post
title: "Lecture 13: Treewidth and Planarity"
date: 2018-09-26
use_math: true
---

Pathwidth DP: Missing Proof
------------

Sorry about handwaving the proof of the "no edges between left and right" statement in the pathwidth DP which Ellis brought up in class. Here's a formal proof below, which is hopefully an instructive read, since it showcases the importance of the properties in the path decomposition.

Suppose we have a path decomposition $(X_1,X_2,\ldots)$. We prove the following *separator theorem* on path decompositions:

Theorem: For each bag $X_i$, there is no edge connecting a vertex in $X_{\le i}\setminus X_i$ to a vertex in $X_{\ge i}\setminus X_i$. We can also say that the set $X_i$ *separates* the sets $X_{\le i}\setminus X_i$ and $X_{\ge i}\setminus X_i$.

Proof: Suppose not: there exists edge $uv$ with $u\in X_{\le i}\setminus X_i$ and $v\in X_{\ge i}\setminus X_i$ By property (3) of path decompositions, we know the bags that contain $u$ form some interval, say, $X_{l_u},X_{l_u+1},\ldots,X_{r_u}$ for some $l_u,r_u$. Also, the bags that contain $v$ form some interval, say, $X_{l_v},X_{l_v+1},\ldots,X_{r_v}$ for some $l_v,r_v$. Let's now invoke property (2) on the edge $uv$: there must be a bag containing both $u$ and $v$. This means that the two intervals must share a bag. Consider taking the union of the two intervals of bags, which is also an interval. In particular, the union, which is the set of bags that contain either $u$ or $v$ (or both) is precisely the interval from $X_{\min(l_u,l_v)}$ to $X_{\max(r_u,r_v)}$. Now, we know that $X_i$ does not contain $u$ or $v$, so either $X_i$ lies to the left of the interval (i.e., $i<\min(l_u,l_v)$), or $X_i$ lies to the right (i.e., $i>\max(l_u,l_v)$). But in the former case, $u$ cannot be inside $X_{\le i}\setminus X_i$, because all bags that contain $u$ lie to the right of $X_i$. Similarly, in the latter case, $v$ cannot be inside $X_{\ge i}\setminus X_i$. This is a contradiction.

Graph Minors:
----------

It turns out that there's a deep, general theory behind all of treewidth and planar graphs, and it has to do with *graph minors*.

Definition: A graph $H$ is a *minor* of a graph $G$ if $H$ can be obtained from $G$ by a series of vertex/edge deletions and edge contractions.

See my lecture notes (page 8) for a figure.

The following is a nice exercise on path/tree decompositions:

Theorem: If $G$ has treewidth $k$, then any minor of $G$ has treewidth $\le k$. That is, if we start with a graph of treewidth $k$ and only perform vertex/edge deletions and edge contractions, then the new graph's treewidth can only decrease (or stay the same). The same statement holds with both treewidths replaced by pathwidths.

We can also say that the class of graphs of treewidth $\le k$ is *closed under taking minors*, or simply *minor-closed*. There's a proof in the lecture notes, but I encourage you to try to prove it on your own.

It turns out that the set of planar graphs is also minor-closed (a proof sketch is in the lecture notes).

There's also a nice characterization of planar graphs in terms of graph minors:

Theorem (Kuratowski): A graph $G$ is planar iff $G$ does not contain $K_5$ and $K_{3,3}$ as minors. (Recall that $K_{3,3}$ is the complete bipartite graph with $3$ vertices on each side.)

One direction is easy: suppose that $G$ has either $K_5$ or $K_{3,3}$ as a minor. Suppose for contradiction that it's planar. Then, we can perform vertex/edge deletions and edge contractions until we get $K_5$ or $K_{3,3}$. Since planar graphs are minor-closed, the new graph is still planar. But we know by elementary graph theory that $K_5$ and $K_{3,3}$ are not planar, contradiction. The other direction is a deep and difficult result, first proved by Kuratowski.

More on Treewidth of Planar Graphs
-----------

In class, we saw an unproved theorem about a treewidth bound on planar graphs with small diameter. What if we don't restrict the diameter? After all, the diameter may not be a good measure for large-diameter graphs, since paths are planar, have diameter $n-1$, and yet have treewidth $1$. It turns out that planar graphs have treewidth at most $O(\sqrt n)$, and this is tight, achieved by the $\sqrt{n}$-by-$\sqrt{n}$ grid on $n$ vertices (see my lecture notes, page 10).

As an exercise, try to prove that a $\sqrt{n}$-by-$\sqrt{n}$ grid has treewidth $O(\sqrt{n})$. (The other direction, proving that the treewidth is $\Omega(\sqrt{n})$, requires some deeper theory of treewidth.)

How to prove that all planar graphs have treewidth $O(\sqrt n)$? This uses a *separator theorem* of Lipton and Tarjan, which can be loosely stated as follows: given a planar graph, we can remove $O(\sqrt{n})$ vertices so that every connected component of the remaining graph has at most $(5/6)n$ vertices ($5/6$ is arbitrary; could be replaced by $1-\epsilon$ for small enough $\epsilon$). Of course, this statement is false for general graphs, for example the $n$-clique. This separator theorem (actually, a weighted version of it), along with a recursive tree-decomposition-building procedure, provides an algorithm that computes a tree decomposition of a planar graph of width $O(\sqrt n)$. (It also runs in polynomial time.)
