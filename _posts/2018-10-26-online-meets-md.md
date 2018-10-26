---
layout: post
title: "Lecture 25 (and also Lecture 22): Method of Lagrange Multipliers"
date: 2018-10-26
use_math: true
---

In the lecture on interior-point methods, we encountered the method of
Lagrange multipliers. We went over it quickly, so here's a refresher and
some discussion. It will be useful for tomorrow's lecture as well.

## Lagrangian Duality: A Primer

Consider the constrained problem with some $m_1$ inequality constraints,
and $m_2$ equality constraints:

$$
\begin{align}
 \min & f(x) \tag{\star} \\
 \text{ subject to } g_i(x) &\leq 0 \forall\, i \in [m_1] \\
 h_j(x) &= 0 \forall j.
\end{align}
$$

We will mostly consider _convex_ problems, where $f(x)$ and $g_i(x)$ are
convex and $h_i(x)$ are affine functions (which means we'll minimize a
convex function over a convex set), but let's keep things general for
now.  The idea behind Lagrange duality is to move the constraints into
the objective function. Define the _Lagrange function_ (or simply the
_Lagrangian_) to be

$$ L(x,\lambda, \mu) := f(x) + \sum_i \lambda_i g_i(x) + \sum_j \mu_j h_j(x).  $$

Think of the _multipliers_ $\lambda \in \R^{m_1}$ that correspond to the
inequality constraints as being non-negative, whereas those
corresponding to the equality constraints can have arbitrary sign.
Now the _Lagrange dual_ is defined as:

$$ g(\lambda, \mu) := \min_x L(x, \lambda, \mu). $$

The first claim is that the dual is _concave_ (as a function with
variables $(\lambda, \mu)$), even if the original problem was not
convex. Indeed, it's a minimum of a collection of linear functions of
these variables, one linear function for each $x$.

The second claim is that the dual is always a lower bound on the
original problem (as long as $\lambda \geq 0$)! Indeed, take the optimal
solution $x^*$ for the problem $(\star)$. Then substituting that $x^*$
into $L(\cdot)$ gives

$$ L(x^*,\lambda, \mu) = f(x^*) + \sum_i \lambda_i \cdot \text{something
non-positive} + \sum_j \mu_j \cdot 0. $$

Since $\lambda \geq 0$, we get $ L(x^*,\lambda, \mu) \leq f(x^*)$. So,

$$ g(\lambda, \mu) = \min_x L(x, \lambda, \mu) \leq L(x^*, \lambda, \mu)
\leq f(x^*). $$

Great, we have a family of lower bounds on the optimum given by the dual
function $g$, so that we get a dual for each setting of $\lambda \geq 0$
and $\mu$. And $g$ is a concave function, so why not _maximize_ it?
(Remember, we usually want to minimize convex functions, and maximize
concave ones.)  This should give the largest lower bound! So define the
dual problem

$$
\begin{align}
\max g(\lambda, \mu) \quad s.t. \quad \lambda_i \geq 0 \;\; \forall
\, i.  \tag{\star\star}
\end{align}
$$

Weak duality follows from the arguments above: this best lower bound is
still a lower bound, so the optimum for the dual $(\star\star)$ is a
lower bound on the optimum for the primal $(\star)$. For strong duality
we need some extra conditions, since strong duality may not hold in such
generality. If course, if the original primal problem was linear, this
is just reproving linear programming duality: the dual problem will also
be linear, and strong duality will hold.

For more general problems, even for convex problems, we need some
niceness conditions to guarantee strong duality. The most commonly ones
are the _Slater conditions_ which say that if the primal problem is
convex, and there is a _strictly feasible_ point (i.e., $f_i(x) < 0$ for
all $x$, and $h_j(x) = 0$), then strong duality holds.

## The KKT Optimality Conditions

For now, assume that the problem is convex, and strong duality holds.
Then we have particularly nice necessary and sufficient conditions for
optimality. The point $x$ is optimal for $(\star)$ if and only if there
exist $\lambda \geq 0$ and $\mu$ such that (a)~$x$ is primal feasible:

$$ g_i(x) \leq 0, h_i(x) = 0, $$

(b)~the gradient of the Lagrangian (with respect to $x$) is zero:

$$ \nabla_x L(x,\lambda, \mu) := \nabla f(x) + \sum_i \lambda_i \nabla g_i(x) + \sum_j \mu_j \nabla h_j(x) = 0. $$

and (c)~the complementary slackness conditions hold:

$$ \lambda_i \times f_i(x) = 0. $$

And then $(\lambda, \mu)$ are also an optimal dual solution to
$(\star\star)$.

This set of conditions above are called the
[_KKT_](https://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions)
conditions. These were initially called the Kuhn-Tucker conditions, and attributed to a conference paper of [Harold Kuhn](https://en.wikipedia.org/wiki/Harold_W._Kuhn) (who gave
the Hungarian method for max-weight matchings) and [Albert Tucker](https://en.wikipedia.org/wiki/Albert_W._Tucker) (who
was later the president of the Math Association of America), but later found
to have been given in the masters' thesis of [William Karush](https://en.wikipedia.org/wiki/William_Karush).
There are weaker conditions under which the KKT conditions are
necessary and sufficient, which you can find in standard texts.
