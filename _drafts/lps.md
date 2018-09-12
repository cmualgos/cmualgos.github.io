---
layout: post
title: "Lecture 7: Matchings via Linear Programming"
date: 2018-09-12
use_math: true
---

In today's lecture, we first finished the proof for Edmonds' Blossom
Algorithm: we showed a linear time procedure that given graph $G$ and
matching $M$ eithers find an $M$-augmenting path, or a $M$-flower, or
else reports that there is no $M$-augmenting path. The idea was to start
from the free (unmatched) vertices and explore the graph almost in a
BFS-like fashion, but alternating between exploring unmatched edges from
the even levels, and then exploring matched ones from the odd ones. The
proof showed that if there was an $M$-augmenting path we would find
either such a path, or a flower, and hence make progress.

Then we discussed some basics about linear programming. One concept I
feel I should have spent more time on was the notion of a basic feasible
solution, so let me do it again. Given 
