---
layout: post
title: "Lecture 8: Matchings via Algebraic Techniques"
date: 2018-09-14
use_math: true
---

Today we saw how to use the notion of _polynomial identity testing_ and
the _Tutte_ and _Edmonds_ matrices to find matchings in graphs. Towards
the end we talked about the red-blue matchings problem, but we did not
connect all the pieces, so let us recap the first part and finish the
second, answering some questions along the way.

## Polynomial Identity Testing

In this problem we are given a polynomial $P(x_1, x_2, \ldots, x_n)$
over some field $\mathbb{F}$, and we want to test if it is identically
zero or not. (For the rest of this post I will just write $P(x)$, where
$x \in \mathbb{F}^n$.) If $P$ were written out explicitly as a list of
monomials and their coefficients, this would not be a problem, since we
could just check that all the coefficients are zero. But if $P$ is
represented implicitly, say as a determinant, then things get more
tricky.

The algorithm we saw, based on the [Schwartz-Zippel
lemma](https://en.wikipedia.org/wiki/Schwartz%E2%80%93Zippel_lemma),
says the following: suppose we are given $P$ via a _value oracle_. This
oracle is some black-box that given some value $a \in \mathbb{F}^n$,
evaluates $P(a)$ in constant time. Then we can decide whether $P$ is
identically zero or not, and be correct with probability at least $1 -
d/|S|$, where $d$ is the degree of $P$, and $S \subseteq \mathbb{F}$ is
the size of the set we choose to pick from. Even if $\mathbb{F}$ is
infinite, the case of finite $S$ makes sense, because we are probably
working with finite precision arithmetic operations anyways.

A big question is whether polynomial identity testing (PIT) can be
derandomized. We don't know deterministic algorithms that given $P$ can
decide whether $P$ is identically zero or not, in poly-time. How is $P$
given, for this question? Even if $P$ is given as an arithmetic circuit
(a circuit whose gates are addition, subtraction and multiplication, and
inputs are the variables and constants), it turns out that derandomizing
PIT will result in surprising circuit lower bounds (see [this
paper](https://www.cs.sfu.ca/~kabanets/Research/poly.html) of Kabanets
and Impagliazzo). In other words, there is evidence that the problem is
difficult in general.

BTW, what about derandomizing just the PIT instances that come from
matchings? This can be done deterministically, and was shown by [Jim
Geelen](http://www.math.uwaterloo.ca/~jfgeelen/Publications/matching.pdf),
but the runtime seems to get much worse. (Or as he says, "the algorithm
remains computationally unattractive".) For the rest of this post, let's
stick with the randomized algorithms for PIT.

## PIT for Matchings

Using the Edmonds and Tutte matrices, and the fact that their
determinants are identically zero if the underlying graph has no perfect
matchings, we immediately get randomized (Monte Carlo) algorithms to
detect whether $G$ has a perfect matching or not. There is some
probability of error, but you can make $S$ large enough to make this
error probability tiny. (Or you could repeat the test independently many
times.)

Then, using the idea of "self-reducibility", i.e., the idea that you can
remove an edge and see if the graph still has a perfect matching or not,
you can find the matching. The naive approach took $O(m)$ calls to PIT,
and each call to PIT required $O(n^\omega)$ time. We then saw how binary
search reduces the number of calls to $O(n \log n)$, and hence the
runtime becomes $O(n^{\omega + 1} \log n)$.

We can do faster: [Rabin and
Vazirani](http://web.eecs.umich.edu/~pettie/matching/Rabin-Vazirani-randomized-maximum-matching.pdf)
showed how to reduce the time by a logarithmic factor to $O(n^{\omega +
1})$. Then a substantial improvement came via work of [Mucha and
Sankowski](https://www.mimuw.edu.pl/~mucha/pub/mucha_sankowski_focs04.pdf),
who showed how to find the perfect matching in randomized $O(n^\omega)$
time. For the _maximum_ matching problem, if the optimal matching size
$k$ is much smaller than $n$, work of [Cheung, Kwok, and
Lau](https://cs.uwaterloo.ca/~lapchi/papers/matrix-rank.pdf) improves
the runtime to $O(m + k^\omega)$.

## The Red-Blue Matching Problem

In this problem we had a bipartite graph with edges colored red and
blue, and the goal was to detect whether $G$ contained a perfect
matching with exactly $k$ red edges. (For concreteness, call this a
_$k$-red matching_.) If so, we could use the "self-reducibility" idea
again to find this matching losing an additional $O(n \log n)$ factor in
the runtime. This is work of [Ketan
Mulmuley, Umesh Vazirani, and Vijay
Vazirani](https://link.springer.com/article/10.1007%2FBF02579206).

We first saw a deterministic algorithm that worked when the graph had a
unique $k$-red matching. (An odd number of such matchings would
suffice.) We put down a matrix $M$ with $0$s for non-edges, $1$ for blue
edges, and $y$ for red edges. Now the determinant of $M$ was a
univariate polynomial (in $y$) where the coefficient of $y^k$ has the
same parity as the number of $k$-red matchings. (Hence if there is a
unique such matching, it is non-zero.)

### Removing the Uniqueness Assumption (Easier Approach)

Here's an approach that came up in discussions after lecture (thanks
Ziye, Corwin, and others) which is better than the one I was proposing.
Now set $M$ to be $0$ for non-edges, $x_{ij}$ for blue edges, and $y
x_{ij}$ for red edges. The determinant of $M$ is now a polynomial in
$m+1$ variables and degree at most $2n$. If you write $P(x,y)$ as
$\sum_{i = 0}^n y^i Q_i(x)$, then $Q_k(x)$ is a multilinear polynomial
corresponding to $k$-red matchings. Hence $Q_k(x)$ is a non-zero
polynomial if there is at least one such matching. Now set the $x$
variables randomly (say, to values $x_{ij} = a_{ij}$) from a large
enough set $S$. This will result in a polynomial $R(y) = P(a,y)$ whose
only variable is $y$. And $Q_k(a)$ will be non-zero with high
probability, by Schwartz-Zippel. Now trying $n+1$ different values of
$y$, you can interpolate this polynomial $R(y)$, and see that the
coefficient of $y^k$ is non-zero.

We focused on bipartite graphs for simplicity: these ideas extend to
non-bipartite graphs too, where the replacements of $x_{ij}$ by $y
x_{ij}$ are done in the Tutte matrix. The only thing to do is to check
that $Q_k(x)$ is still a non-zero polynomial if there is at least one
such matching, and for this you need to run through the standard Tutte
matrix argument and see it works here too. The traditional argument for
the Tutte matrix goes via the jargon of Pfaffians and whatnot, but the
essential idea is very clean; see, e.g., Lemma 3.4 in [Marcin Mucha's
thesis](http://web.eecs.umich.edu/~pettie/matching/Mucha-PhD-thesis.pdf)
or [Virginia's
notes](http://theory.stanford.edu/~virgi/cs367/lecture5-1.pdf).

### Removing the Uniqueness Assumption (More Indirect Approach)

In lecture I started explaining the approach that Mulmuley Vazirani
Vazirani propose, via the "isolation lemma". It's very pretty in its own
right, but given the above argument, it seems overkill for now. I will
say more here later, both about their approach and the isolation lemma,
but that will be for another post (to come shortly).
