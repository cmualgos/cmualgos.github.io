;; This buffer is for notes you don't want to save, and for Lisp evaluation.
;; If you want to create a file, visit that file with C-x C-f,
;; then enter the text in that file's own buffer.


# Pedro's question

Maybe to elaborate on Pedro's question: while we usually tend to
view the gradient $\nabla f(x)$ as a vector, you can view it as a
map that takes a direction $u$, and returns the _directional
derivative_

$$ D_u f(x) = \lim_{\delta \to 0} \frac{ f(x + \delta u) - f(x) }{\delta} $$

at that point. This is a linear map , and hence is often
represented as the vector $\nabla f(x) = (\frac{\partial
f}{\partial x_1}, \ldots, \frac{\partial f}{\partial x_n})$ so
that $D_u f(x) = \langle \nabla f(x), u\rangle$.
(The fact that each linear functional $\ell(x)$ has a
corresponding (co)vector $w$ such that $\ell(x) = \langle w,
x\rangle$ is given by the Reisz representation theorem.)
(Here's a [short
note](https://people.eecs.berkeley.edu/~roydong/fa17_files/lec02.pdf)
about this, that I found very useful.)

In lecture Mark gave a good reason why should be careful in
viewing gradients and linear functionals as vectors. If we scale
the coordinates (say by a factor of $2$) then the length of
vectors goes up by a factor of $2$, whereas the gradient goes
*down* by a factor of $2$ (the function is stretched out, and
hence the slope decreases). Hence maybe it's a good idea to
clearly say that gradients are co-vectors, and to keep the two
concepts distinct. I'll try to be more careful with these things
myself... :)

# Roie and Ellis' question

So my mistake was in 


# Alireza's question




