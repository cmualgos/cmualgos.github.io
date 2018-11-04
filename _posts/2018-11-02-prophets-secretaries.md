---
layout: post
title: "Lecture 28: Prophets and Secretaries"
date: 2018-11-02
use_math: true
---

For today's lecture we considered the prophet inequality problem, and
the secretary problem.

## Many Prophets

Recall the LP we wrote for the (single item) prophet inequality problem
(where the variable $y_{iv}$ is trying to capture the probability that
the r.v. $X_i$ takes on value $v$, and we pick it. 

$$
\begin{align*}
  \max \sum_{i,v} &y_{iv} \cdot v \\
  \sum_{i,v} y_{iv} &\leq 1 \\
  y_{iv} &\in [0, p_{iv}]
\end{align*}
$$

The algorithm we used was: if we have not picked an item among the first
$i-1$, and $X_i$ takes on value $v$ (which happens with probability
$p_{i,v}$) then pick it with probability $\frac{y_{i,v}}{2p_{i,v}}$. We
showed that the probability that no item is picked using this scheme was
at least $1/2$ (exactly because we scaled down the probability values),
and hence so every r.v. has at least a one-half chance of being seen. We
then get expected value
$\sum_v v\cdot p_{iv} \cdot \frac{y_{iv}}{2p_{i,v}}$ from it, which is
half of what the LP got. Thus a loss of $1/4$. The lecture notes talk
about how to save this loss.

Klas suggested that we don't shade down the probabilities. I feel like
we may be able to use the fact that we're rounding an *optimal* LP
solution to get something reasonable, but the per-item analysis we were
doing in lecture will break. E.g., say $p_{iv} = 1$ for some value
$v$. (I.e., everyone takes on value $v$ with probability $1$. consider
the first item is picked with probability $y_{1v} = (1-\varepsilon)$ and
the second with probability $y_{2v} = \varepsilon$. Now if we just use
our scheme without scaling-down, we pick the second item with
probability $\varepsilon^2$ (probability $\varepsilon$ of not being
blocked by the first item having been chosen, and probability
$\varepsilon$ from the actual rounding). That's a problem when
$\varepsilon$ is small, since then $\varepsilon^2 \ll \varepsilon$.

ALso, the [lecture notes](http://www.cs.cmu.edu/~anupamg/ipco17/ipco-talk3.pdf) give another way of doing some adaptive
scaling that loses less, and gets the factor of $2$ we wanted.

BTW, Roie asked a question about "how many samples are needed"? We
assumed that we know the distribution of the r.v.s $X_i$. What if we
need to learn them as well? [Azar, Kleinberg and Weinberg](https://arxiv.org/abs/1307.3736) show that
if we get one sample from each $X_i$, we can still get a constant
competitive algorithm, but the detailed tradeoffs between sample
complexity and competitiveness are still not fully understood.

## Secretaries

For the graphic matroid problem, we proposed the following algorithm:
color the vertices red and blue with equal probability. Now for each red
vertex, run the single-item secretary algorithm on the bichromatic edges
hitting that vertex. (Corwin pointd out that we need to know the graph
to find out how many such bichromatic edges exist, incident to each
node. This is because the secretary algorithm needs to know $n$ to work
correctly. Do you see a way around this? Answers on a postcard to me,
please!)

The analysis is the following: the output is clearly feasible. Now
consider OPT for the original instance. The random coloring means we
lose a factor of $2$ of the weight (in expectation). Now consider the
bichromatic edges in the remaining forest. For each node, associate with
it the edge to its parent. So either the red nodes have half the weight
of the forest in their parent edges, or the blue nodes do. And since we
chose the coloring randomly, these are symmetric. So we've lost a factor
of $4$. Finally, running secretary loses another factor of $e$. You can
get a better algorithm losing only $2e$ this way. Can you lose only a
factor of $e$ for graphic matroids? I don't know.

The matroid secretary problem was defined by Babaioff, Immorlica and
Kleinberg in [this paper](https://www.cs.cornell.edu/~rdk/papers/matsec.pdf). The current best $O(\log \log r)$, where
$r$ is the rank of the matroid, is due to [Feldman, Svensson, and
Zenklusen](https://arxiv.org/abs/1404.4473). And here's a [slightly older survey](http://www.cs.jhu.edu/~mdinitz/papers/secretary-survey.pdf) by our own Mike Dinitz;
he and I had spent a bunch of time on this problem when he was at CMU.

BTW, the matroid version of the propher inequality problem is understood
by now, for general matroids---showing that prophets are easier than
secretaries, in this setting.
