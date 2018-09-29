---
layout: post
title: "Lecture 14: Applications of Concentration Inequalities"
date: 2018-09-28
use_math: true
---

## Random walk application: 2-SAT

Suppose we want to solve the classic 2-SAT formula. As a reminder of the problem, you are given a 2-CNF formula like $(x_1 \lor \neg x_2) \land (x_2 \lor x_5) \land (\neg x_5 \lor x_3) \ldots$ for which we want to find a satisfying assignment or report none exists. Polynomial time solutions were known for a long time for this problem and there even exists [linear-time solutions](http://www.math.ucsd.edu/~sbuss/CourseWeb/Math268_2007WS/2SAT.pdf) based on finding strongly connected components in a graph.

