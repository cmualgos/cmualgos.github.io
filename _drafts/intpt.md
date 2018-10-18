
## Newton's Method

## The Method of Lagrange Multipliers

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

### The Simple Algorithm

Let $x^*_t$ denote the optimal solution for the function $\eta_t$. And
for brevity, let $f_t$ denote $f_{\eta_{t}}(x)$. Assume we know
$x^*_0$. Here's one simple algorithm:

1. $\eta_{t+1} \gets \eta_t (1 - c/\nu)$.

2. Find $x_{t+1} \approx x^*_{t+1}$ by minimizing $f_{t+1}(x)$ using
     Newton's method, starting at $x_t \approx x^*_t$.

Since Newton's method is great if you start off with a point in the
region of convergence, we want to prove that the optimizer $x^*_t$ for
$f_t$ is in the region of convergence for $f_{t+1}$. Hopefully this
proof will be robust enough to show that the point $x_t \approx x^*_t$
found by our algorithm is also in the region of convergence. And then
we'll be done.

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
two-step algorithm above to be the cleanest: change $\eta$ and hence the
function slightly, show that the old minimizer is a good starting point
for the new function, minimize the new function using Newton, and
repeat.

Finally, imagine that $\eta_T$ is smaller than XXXXXX. Then we claim the
resulting optimal point $x^*_T$ is within XXXXXXX of the LP optimizer.
And if $\lambda_T(x_T) < \varepsilon$ then $c^\intercal x_T \leq
XXXXX$. So we have a near optimal LP solution $x_T$.

### What's The Norm?

The secret sauce? It's which norm to use in $\lambda_t(x)$. The
Euclidean norm is difficult to work with, so instead we use a "local"
norm. In particular, for any positive semidefinite matrix $A$, define
the $A$-norm of vector $x$ to be

$$ \| x \|_A := \sqrt{x^\intercal A x}. $$

and recall that the Hessian of a function $f$ is the matrix $H(x)$ of
second-derivatives at the point $x$. Then the definition of $\lambda_t$
is formally:

$$ \lambda_t(x) := \\| \nabla f_t(x) \\|_{H(x)^-1}. $$

It is for this definition of $\lambda_t$ that we prove the two facts
above. The proofs of the two facts above appear in standard texts
(e.g., these fine books by [Yurii
Nesterov](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.693.855&rep=rep1&type=pdf)
and Jim
Renegar](https://epubs.siam.org/doi/book/10.1137/1.9780898718812)), and
in [these
notes](http://people.seas.harvard.edu/~cs224/fall14/lec/lec18.pdf) by
Jelani Nelson. These details are interesting, but perhaps not central to
the course (no pun intended). The main goal is to understand what the
interior point methods are trying to do at a high level, and to
understand in more detail some of the technical ideas behind these
methods (the method of Lagrange multipliers, Newton's method, linear
system solving, etc.)


