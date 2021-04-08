---
toc: true
layout: post
description: Some musings on uniform random number generators
categories: [markdown, inquiry-based-learning]
title:  Empirical Tests for Uniformness
tags: blurb
search_exclude: false
---

# Empirical Tests for Uniformness

These musings take off from chapter 7 of [Simulation Modelling and Analysis](https://books.google.ca/books/about/Simulation_Modeling_and_Analysis.html?id=6cC_QgAACAAJ&redir_esc=y).

The beginning of this chapter asks the question *what makes a good arithmetic random number generator*? In taking his question to the reader as real and not rhetorical, here is an idea I explored.

That it doesn't "miss" numbers.  Though this is unreasonable for arithmetic generators with finite period, the most ideal $Unif(0,1)$ generator should be able to generate given enough time, generate any given number in the interval $[0,1]$.  Just the fact that generators spit out numbers at discrete-time intervals means that this is impossible though, given that it only makes countably many numbers on a uncountable set $\[0,1\]$.

So maybe some weaker version of this might be possible.  Something that has the spirit of "that there are no gaps" that can exist in the sequence given enough time.  That is, given any interval that were a subset of $\[0,1\]$, no matter how small, we could expect given enough time that our generator would yield a numbe r in that interval.  Any of the generators like the linear congruential generators can't do this because of their finite period.  So we at least need a generator with infinite period.  Such a generator can't exist in an electronic computer because the computer can only assume finitely many states, and hence can only represent finitely-many states, and hence any generator must have a finite period.

So we've thought up an ideal properties of random number generators, but which can't exist in practice.  What then, after all, is Averill looking for in an ideal $Unif(0,1)$ generator?  

- No self-correlation (that is, $Cov(x_i, x_{i + j}) \to 0$)
- performance
- can reproduce the random numbers (without storing them)
