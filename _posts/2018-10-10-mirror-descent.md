---
layout: post
title: "Lecture 19: Mirror Descent"
date: 2018-10-10
use_math: true
---

## Why Mirror Descent?

One major reason for mirror descent is that it is a general algorithm
that allows us to tune the convergence guarantee to the "geometry" of
the problem. If you look at the main theorem, it asks you to fix a norm
$\\| \cdot \\|$, such that the gradients of the functions $f_t$ are
bounded by $G$ with respect to the dual norm $\\| \cdot \\|\_\*$. Then it
asks you to fix a function $h$ which is $\alpha_h$-strongly convex with
respect to the primal norm. Now the online guarantee shows that for any
$x^*$, we have the total regret is

$$ \sum_t f_t(x_t) - \sum_t f_t(x^*) \leq \frac{\eta G^2}{2\alpha_h} T +
\frac{D_h(x^* \| x_0)}{\eta}. $$

In other words, we can now trade-off the $G = \max_t \\| \nabla f_t \\|\_\*$
term with the Bregman distance term $D_h(x^* \\| x_0)$. E.g., for the
standard experts problem, using the $\ell_2$ norm and $h(x) = \frac12 \\|
x \\|^2$, we get that $G = \sqrt{n}$, but the distance $D_h(x^* \| x_0) =
\frac12 \\| x^* - x_0\\|^2 \leq 2$. On the other hand, changing the norm
to $\ell_1$, and taking the negative entropy function $h(x) = \sum_i x_i
\log x_i$ reduces the bound on the gradients to $1$, at the expense of
increasing the latter term to $O(\log n)$.

## Gradient as a Linear Functional

Let me elaborate on Pedro's question: while we usually tend to view the
gradient $\nabla f(x)$ as a vector, you can view it as a map that takes
a direction $u$, and returns the _directional derivative_

$$ D_u f(x) = \lim_{\delta \to 0} \frac{ f(x + \delta u) - f(x) }{\delta} $$

at that point. This is easily checked to be a linear map in $u$, and
hence if we define $\nabla f(x) = (\frac{\partial f}{\partial x_1},
\ldots, \frac{\partial f}{\partial x_n})$ we get $D_u f(x) = \langle
\nabla f(x), u\rangle$.  (The fact that each linear functional $\ell(u)$
has a corresponding (co)vector $w$ such that $\ell(u) = \langle w,
u\rangle$ is given by the Reisz representation theorem.) Here's a [short
note](https://people.eecs.berkeley.edu/~roydong/fa17_files/lec02.pdf)
about this that I found useful.

Mark reminded me of a good reason why we should be careful in viewing
gradients and linear functionals as vectors (and indeed, in calling them
vectors). Observe that if we scale the coordinates, say, by a factor of
$2$ then the length of vectors goes up by a factor of $2$, whereas the
gradient goes *down* by a factor of $2$ (the function is stretched out,
and hence the slope decreases). Hence it's a probably good idea to say
that gradients are co-vectors, and to keep the two concepts distinct.
I'll myself try to be more careful with these terms... :)

## The Hardness and Ease of Projections

Someone (Ellis? Greg?) asked about doing projections onto convex sets:
indeed, these can be tricky in general, requiring the full power of convex
programming. There are some cases which are easy --- e.g., projecting
$x$ with respect to the KL divergence onto the probability simplex is
achieved by taking $\frac{x}{\\|x\\|_1}$, i.e., by rescaling so that the
$\ell_1$ length of the vector becomes $1$. (See the next HW.)

However, if you don't like projecting, here's a different algorithm to
try. Given a point $x_t \in K$ and the gradient $\nabla f(x_t)$, do not
take the step $x_t - \eta \nabla f(x_t)$. Instead, find the linear
optimizer $\min_{x \in K} \langle \nabla f(x_t), x \rangle$. I.e., you
are now opimizing a linear constraint (instead of a convex constraint)
over the convex body. This algorithm is called the Frank-Wolfe
method. And if the function $f$ is smooth, this algorithm actually
converges pretty fast (and has some other nice properties). We'll see
more of the Frank-Wolfe algorithm in a HW4 problem.

