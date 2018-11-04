---
layout: post
title: "Lecture 26 and 27: SDPs and SoS"
date: 2018-10-31
use_math: true
---

## The Univariate SoS Case

I gave a proof that any positive univariate polynomial is indeed
SOS. But it seems my wits were wool-gathering, Alex correctly pointed
out that I did not correctly account for the non-zero roots. Let me 
give the proof, more precisely this time.

Consider the roots of $P(x)$. Some are real, others are complex. The
complex ones occur in conjugate pairs, so when you take the terms in the
factorization $(x-a)(x-\bar{a})$ corresponding to them, you get $(x^2 +
|a|^2)$.

For the real roots, the observation is that you cannot have a simple
root: they must all be repeated. Indeed, if there was a simple root, the
polynomial would dip below zero, and hence not be positive. In fact, the
multiplicity of these roots must be even for the same reason. So the
terms for these roots give us $(x-b)^2$-like terms.

Hence the factorization of $P(x)$ looks like:

$$ P(x) = \prod_i (x-b_i)^2 \prod_j (x^2 + |a_j|^2). $$

The first set of terms (for the real roots) are square polynomials anyways, and multiplying
out the second set of terms gives us an SoS polynomial. QED.

## More on SOS

There is a fair bit of literature on the SoS hierarchy. The [course](https://www.boazbarak.org/sos/) by
Barak and Kothari and Parillo and Steurer is a very good resource. Also
[this one](https://homepages.cwi.nl/~dadush/teaching/sos-2017/) by Bansal and Dadush. 
Or this one by [Parillo](https://learning-modules.mit.edu/class/index.html?uuid=/course/6/sp16/6.256#info) has a different, more 
algebraic-geometry perspective. Another good CS-y intro to the Lasserre hierarchy, which directly deals with the SDP constraints, instead of the SoS proofs, is [this one](https://sites.math.washington.edu/~rothvoss/lecturenotes/lasserresurvey.pdf) by Thomas Rothvoss (and here are his [slides](https://sites.math.washington.edu/~rothvoss/lecturenotes/LasserreSurveySlides.pdf)).

