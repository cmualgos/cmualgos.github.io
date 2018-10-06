
## Rounding to a feasible solution

(Here's a sketch: all this is optional.)  The idea is conceptually
simple, but technically non-trivial. The high-level idea is something
you've seen before in this course. Suppose I want to find the value of
an unknown rational number $z$. All I get are claims that this unknown
is at least $z-\varepsilon, for ever decreasing values of $\varepsilon.
How do I figure out $z$?

It looks hopeless, but suppose you told me that $z = p/q$, with $q \leq
N$. Then if when $\varepsilon \leq 1/N^2$, I know that cannot be any
other valid rational (i.e., with denominator at most $N$) between the
unknown $z$ and my current lower bound of $z - \varepsilon$. (Why?)  Now
I can try to find the best rational approximation to $z - \varepsilon$
having denominator at most $N$. How? You can use continued fractions for
this. 

How can we use this for LPs? Suppose you have a LP which is feasible,
and where each of the numbers can be represented using $L$ bits
each. Then the claim is that there exists a solution $x^*$ to the LP
with each entry using at most $poly(n L)$ bits. (The proof of this is
cute: choose a basic feasible solution, this is given by the solution to
the linear system given by some $n$ tight constraints.  We can use
Cramer's rule to find this b.f.s. point $x^*$. Cramer's rule gives the
entries of the solution using ratios of determinants, and we can use the
Hadamard inequality to bound the value of these determinants.)

So once we have $\hat{x}$ which is a good-enough
Now we would have to "round" the fractional solution to the closest 
