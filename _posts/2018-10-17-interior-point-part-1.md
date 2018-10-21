---
layout: post
title: "Lecture 22: Overview of Interior-Point Methods"
date: 2018-10-17
use_math: true
---

## Newton's Method

To warm-up, let's recall Newton's method (or the Newton-Raphson method)
again. Suppose we want to find a root of a univariate function $f(x)$,
i.e., some point $x^+$ such that $f(x^+) = 0$ Then one way is to start
with a "good" guess $x_0$, and then do

$$ x_{t+1} \gets x_t - \frac{f(x_t)}{f'(x_t)}. $$

The intuition for this comes from drawing [this picture](http://mathfaculty.fullerton.edu/mathews/n2003/newtonsmethod/Newton'sMethodProof/Images/Newton'sMethodProof_gr_17.gif). Or looking at
this (fascinating!) [animation from Wikipedia](https://en.wikipedia.org/wiki/Newton%27s_method#/media/File:NewtonIteration_Ani.gif).

Now that can find roots, we can also solve equational constraints $f(x)
= g(x)$ (by finding a root of $h(x) := f(x) - g(x)$), or minimize $f(x)$
(by finding a root of the derivative $f'(x)$)). Specifically, to
minimize $f(x)$, the update rule just changes by taking one extra
derivative in $f$, and hence becomes

$$ x_{t+1} \gets x_t - \frac{f'(x_t)}{f''(x_t)}. $$

This extends to multi-variate functions $f: \mathbb{R}^n \to
\mathbb{R}$: indeed, if the Hessian of the function $f$ at the point $x$
is defined to be $H(x)$, where $(H(x))_{ij} = \frac{\partial^2
f(x)}{\partial x_i \partial x_j}$, the update rule becomes

$$ x_{t+1} \gets x_t - [H(x_t)]^{-1} (\nabla f(x_t)), $$

which is syntactically the same expression as the preceding one. And
this also looks very much like the gradient descent update rule, where
you are preconditioning by the Hessian of $f$ instead of identity (as in
basic gradient descent), or by the Hessian of the mirror map $h$ (as in
mirror descent). Hence, updates given by $x_{t+1} \gets x_t - Q^{-1}
(\nabla f(x_t))$ for some PSD $Q = Q(x_t)$ are called quasi-Newton methods.

An aside: to implement an update like $x_{t+1} \gets x_t - Q^{-1} \nabla
f(x_t)$, you need to solve for $x_{t+1}$. I.e., you want to find $z$
such that $Qz = \nabla f(x_t)$, and then set $x_{t+1} \gets x_t -
z$. Hence solving linear systems is a common subroutine when running
Newton methods.

### Why Newton Methods?

The advatange of the these methods is their covergence guarantee. If we
are close to the solution, Newton's method converges very rapidly:
indeed,
[here's](https://cs.nyu.edu/overton/NumericalComputing/newton.pdf) a
simple analysis for the univariate case showing that we can get to
within $x^+ \pm \varepsilon$ in $O(\log \log \frac1\varepsilon)$ steps!
This is amazingly fast: we're not increasing the number of bits of
precision linearly, but quadratically!

What's the catch? If we're not "close" to the root, then the method may
diverge. So we really want to first get to this "region of convergence"
using some simpler method, say a first-order method, and then use Newton
to seal the deal. Or, as interior-piont methods do: use Newton at
several scales, each time working on a coarse function for which we have
a point that's in the region of convergence, taking a step or two, and
then refining the function so that the new point is in its region of
convergence.

## Interior Point Overview (Take 2)

The discussion of interior point methods on Wednesday was a bit more
detail-oriented than I'd hoped, so let me give a different way to view
the method. For a barrier function $B(x)$, define

$$ f_\eta(x) := c^\intercal x + \eta B(x). $$

We start off with the $\eta_0$ value being high, and reduce it
$\eta_{t+1} \gets \eta_t (1 - c/\nu)$, where $\nu$ is some parameter
that relates to the barrier function. If the polytope is defined by an
LP in equational form $K := \{ Ax = b, x \geq 0\}$ then we can choose
the barrier function $B(x) := - \sum_i \log x_i$, and the parameter $\nu
= \sqrt{n}$. Note that in equational form, the number of variables $n$
is at least the number of constraints $m$ if we have a non-empty
polytope and linearly independent constraints.

### A Simple Algorithm

Let $x_t^\*$ denote the optimal solution for the function $\eta_t$. And
for brevity, let $f_t$ denote $f_{\eta_{t}}(x)$. Assume we know
$x_0^\*$. Here's one simple algorithm:

1. $\eta_{t+1} \gets \eta_t (1 - c/\nu)$.

2. Find $x_{t+1} \approx x_{t+1}^\*$ by minimizing $f_{t+1}(x)$ using
     Newton's method, starting at $x_t \approx x^*_t$.

Since Newton's method is great if you start off with a point in the
region of convergence, we want to prove that the point $x_t$ we find by
approximately minimizing $f_t$ is in the region of convergence for
$f_{t+1}$.  Let's try to show a simpler guarantee, that the _optimizer_
$x_t^\*$ for $f_t$ is in the region of convergence for $f_{t+1}$.
Hopefully this proof will be robust enough to show that the point $x_t
\approx x_t^\*$ found by our algorithm is also in the region of
convergence. And then we'll be done.

Here's the proof sketch. We would like to measure the goodness of a
point $x$ with respect to a function $f_t$ by how small the gradient of
the function is at the point $x$. After all, we're looking for a point
where the gradient is zero! So define the measure of goodness of a point
$x$ to be 

$$ \lambda_t(x) := \\| \nabla f_t(x) \\|. $$

So now we'd like to prove the following two facts:

a. If $\lambda_t(x) < 1$, then one step of Newton's method results in a
new point $x^+$ with $\lambda_t(x^+) < \lambda_t(x)^2$. This would mean
that the magnitude of the gradient drops *doubly-exponentially*! And
hence $\lambda_t(x)$ would be smaller than $\varepsilon$ in $\approx
\log \log 1/\varepsilon$ iterations. This also means that we can define
$\lambda_t(x) < 1$ to be our definition of the _region of convergence_.

b. Now suppose we're at some point $x_t$ with $\lambda_t(x_t) \leq
\varepsilon$. And we now change the function from $f_t$ to $f_{t+1}$.
Then we want to say that $\lambda_{t+1}(x_t) < 1$, and so we're still in
the region of convergence for the new function $f_{t+1}$.

Indeed, both these facts can be shown. In fact, much more can be shown:
we don't need to optimize all the way using Newton's method, we could do
only a single step, etc. But those are details. Conceptually I find the
two-step algorithm above to be the cleanest: reduce $\eta$ and hence the
function slightly, show that the old minimizer is a good starting point
for the new function, minimize the new function using Newton, and
repeat. Finally, when $\eta_T$ is small, then we're almost minimizing
$c^\intercal x$. I.e., we're still adding in $\eta_T B(x)$, which still
prevents us from going outside the feasible region, but its effect for
points strictly inside the polytope is minimal. And then we'll be done.
See the references below for details and proofs.

### What's The Norm?

Finally, what's the secret sauce? It's the choice of norm to use in the
definition of $\lambda_t(x)$. The Euclidean norm is difficult to work
with, so instead we use a "local" norm. In particular, for any positive
semidefinite matrix $A$, define the $A$-norm of vector $x$ to be

$$ \| x \|_A := \sqrt{x^\intercal A x}. $$

and recall that the Hessian of a function $f$ is the matrix $H(x)$ of
second-derivatives at the point $x$. Then the definition of $\lambda_t$
is formally:

$$ \lambda_t(x) := \\| \nabla f_t(x) \\|_{H(x)^-1}. $$

It is for this definition of $\lambda_t$ that we prove the two facts
above. The proofs of the two facts above appear in standard texts (e.g.,
these downloadable books by [Yurii
Nesterov](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.693.855&rep=rep1&type=pdf)
and Jim
Renegar](https://epubs.siam.org/doi/book/10.1137/1.9780898718812)), and
in [these
notes](http://people.seas.harvard.edu/~cs224/fall14/lec/lec18.pdf) by
Jelani Nelson. These details are very interesting, but perhaps not
central to the course (no pun intended). The main goal this semester was
to understand at a high level what the interior point methods are trying
to do, and to understand how to use the technical ideas behind these
methods: Newton's method, linear system solving, and the method of
Lagrange multipliers --- which I will talk more about in a follow-up
post, etc.)

### Self-Concordance (slightly advanced)

If you read the [convergence
proof](https://cs.nyu.edu/overton/NumericalComputing/newton.pdf) above,
it works if $f$ satisfies $f''(x) \leq 2 f'(x)$. For finding the
minimizer, this translates to requiring that $|f'''(x)| \leq 2
f''(x)$. This has a resemblance to the definition of
[self-concordance](https://en.wikipedia.org/wiki/Self-concordant_function),
in case you've seen it before---self-concordance for a univariate
function requires that $|f'''(x)| \leq 2 f''(x)^{3/2}$ (and for higher
dimensions, this condition holds along every direction). Indeed, the two
ideas are intimately connected: the latter is an affine invariant
version of the former. For more details, see Chapter 4 of [Nesterov's 
book](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.693.855&rep=rep1&type=pdf).
