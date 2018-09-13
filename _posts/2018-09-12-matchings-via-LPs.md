---
layout: post
title: "Lecture 7: Matchings via Linear Programming"
date: 2018-09-12
use_math: true
---

## Finishing Edmonds' Blossom Algorithm

In today's lecture, we first finished the proof for Edmonds' Blossom
Algorithm: we showed a linear time procedure that given graph $G$ and
matching $M$ eithers find an $M$-augmenting path, or a $M$-flower, or
else reports that there is no $M$-augmenting path. The idea was to start
from the free (unmatched) vertices and explore the graph almost in a
BFS-like fashion, but alternating between exploring unmatched edges from
the even levels, and then exploring matched ones from the odd ones. The
proof showed that if there was an $M$-augmenting path we would find
either such a path, or a flower, and hence make progress.

## Linear Programming Basics 

Then we discussed some basics about linear programming. We defined
convex polyhedra (intersections of finite number of half-spaces), and
convex polytopes (bounded polyhedra). We defined a _vertex_ of a
polyhedron $K$ (a point $x \in K$ such that there exists some cost
vector $c$ for which $x$ is the unique optimizer), an _extreme point_ of
$K$ (which cannot be written as a convex combination of two other points
in $K$), and a _basic feasible solution_ (bfs) of $K$ (which is a point
in $K$ lying on $n$ linearly independent hyperplanes, when we are in $n$
dimensional space).

In retrospect, I feel I should have spent more time on was the notion of
a _basic feasible solution_ (bfs), since it's not as intuitive as the
other two definitions. But it is algorithmically very useful. So let me
do it again. Consider the polyhedron $K := \\{ x \in \mathbb{R}^n \mid
a_i^\intercal x \geq b_i \\}$. It's the intersection of a collection of
half-spaces. Consider the _bounding hyperplanes_ $a_i^\intercal x = b_i$
of these half-spaces. If we pick any $n$ of these hyperplanes that are
linearly independent, then their intersection is a single point. (Just
like the intersection of $n=2$ non-parallel lines in $\mathbb{R}^2$ is a
point, as is the like the intersection of $n=3$ linearly independent
planes in $\mathbb{R}^3$.)  A bfs is a point in the polyhedron $K$ that
is the intersection of $n$ of these constraints.

The important fact here is that for polyhedra, these three objects are
equivalent. (It is a useful exercise to draw some $2$-dimensional
polygons to mull over this.) Moreover, for a polytope $K$ (i.e., a
bounded polyhedron), for any cost vector $c$, the optimal value of $\max
\{ c^\intercal x \mid x \in K\}$ is achieved at a vertex.  As an aside,
if we have an unbounded polyhedron, this may not be true. E.g., consider
the LP $max \\{ x_1 + x_2 \mid x_1 + x_2 \leq 1 \\}$, which has optimal
value $1$ but no extreme points.

Finally, we also talked about the fact that _each polytope is the convex
hull of its vertices_. In case you havent seen convex hulls before,
let's do this slower. In $2$-dimensions, given a set of points in
$\mathbb{R}^2$, think of these as being nails hammered into the plane, and the
convex hull is the region enclosed by a rubber band that is wrapped
around these nails. Kinda like [this picture](http://www.clear-lines.com/blog/post/Convex-Hull.aspx).
For $3$-dimensions, you now wrap a rubber balloon around the points in
$\mathbb{R}^3$. (For higher dimensions, you have to use your
imagination, but you get the idea...) Formally, the convex hull of $S$
is the set of points that can be written as the convex combinations of
the points in $S$.

Now we discussed the notion of integer polytopes, where the vertices are
at integer points. In fact, we will mostly care about polytopes whose
vertices encode combinatorial structures. E.g., matchings, trees,
arborescences, TSP tours, etc. Given a graph $G$ with $m$ edges, we can
use Boolean vectors in $\{0,1\}^m$ to encode sets of edges. E.g., given
the collection of Boolean vectors representing perfect matchings in $G$,
we defined the _perfect matching polytope_ to be the convex hull of
these vectors. Hence, we could find a max-weight perfect matching by
optimizing over this polytope. The problem is that this polytope has a
very awkward description (as the convex hull).

### The Perfect Matching Polytopes

The main result we proved at the end of the lecture was that another way
to represent the perfect matching polytope for _bipartite_ graphs is as
follows:

$$ K_{\text{pm-bip}} := \{ x \in \mathbb{R}^m \mid \sum_{e \in \partial v} x_e =
1, x \geq 0 \}. $$

Recall that $\partial v$ is the set of edges incident to vertex $v$.
(The proof showed that the extreme points of this polytope are precisely
perfect matchings in $G$.) For non-bipartite graphs, we obseved that the
above LP was not an accurate representation of the perfect matching
polytope: e.g., for the $3$-cycle, the above polytope is non-empty, even
though there are no perfect matchings. And we mentioned Edmonds' theorem
saying that now the perfect matching polytope for _non-bipartite_ graphs
is as follows:

$$ K_{\text{pm-nonbip}} := \{ x \in \mathbb{R}^m \mid \sum_{e \in \partial v} x_e =
1, \sum_{e \in \partial S} x_e \geq
1 \text{ for all odd sets } S, x \geq 0 \}. $$

Here $\partial S$ is the set of edges with one endpoint in $S$ and
another not in $S$.

### Getting a Smaller LP for Perfect Matchings

The perfect matching LP we wrote down is an exponential-sized
LP. However, we can solve this LP in polynomial time. (We will discuss
this later in the course, when we talk about the Ellipsoid method.)
Basically, there is a poly-time algorithm that given a potential
solution $x$ to this LP, outputs a violated constraint, if any. (This is
called a "separation oracle" in the jargon.)

Finally, we mentioned an amazing theorem of [Thomas
Rothvoss](https://sites.math.washington.edu/~rothvoss/) saying that any
linear program that "captures" the perfect matching polytope for general
graphs must be of exponential size. (What does it mean to "capture" the
perfect matching polytope?  It's like HW1 exercise 3d, where you took
the exponential-sized LP for arborescences from lecture, and wrote a
equivalent poly-sized LP by adding some extra variables and using some
new constraints.

In 1988 [Mihalis Yannakakis](http://www.cs.columbia.edu/~mihalis/) had
asked whether a compact LP for perfect matchings and TSP existed, and
started [building some beautiful 
machinery](http://www.tcs.tifr.res.in/~prahladh/teaching/2011-12/comm/papers/Yannakakis1991.pdf)
to prove lower bounds on LP size. In 2012, [Fiorini and
others](https://homepages.cwi.nl/~rdewolf/publ/qc/stoc130-fiorini.pdf)
showed that the TSP would require LPs of exponential size. The question
for matchings remained open for a couple more years, until Rothvoss
result proved his reslt. See [this
tutorial](https://simons.berkeley.edu/talks/extended-formulations1) on
"extended formulations" and lower bounds to get started on these
questions.

