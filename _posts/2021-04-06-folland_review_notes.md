---
toc: true
layout: post
description: Diversions and extra thoughts to try and build more pictures while rereading this book
categories: [markdown, math]
title:  Review Notes for Folland's Advanced Calculus
image: /images/folland.jpg
search_exclude: false
---

# Chapter 1

### Cauchy-Schwartz
I found the first piece of "magic", magic meaning here an unintuitive explanation, in Folland's proof of *Cauchy's Inequality*

$$|v\cdot w| \leq |v||w| \quad v,w \in \mathbb{R}^n$$

The first step of the proof introduces the expression $\vert v - tw \vert^2$ where $t \in \mathbb{R}$ seemingly out of nowhere.  The truth is, there's a buildup to this point that's missing that would let us both come up with this form, and also see how to use it to prove the inequality.

There is a geometric picture that motivates this.  The dot project of any two vectors is equivalently the projection either one of those vectors onto the other times the length of the other, so
$$ v \cdot w = proj_w v |w| = proj_v w |v|$$

where $proj_v$ is the function "project along $v$". If you've ever thought it strange that ignoring the obvious commutative nature of the dot product, the last part of this equality seems unexpected, I'd suggest watching this [visual explanation](https://youtu.be/LyGKycYT2v0?t=131).

Now, thinking of the dot product in this way, clearly the length of the projection of $v$ onto any vector that doesn't share the same direction will be less than the length of $v$.

![](/images/2d-projection.png)


$$ |v\cdot w| =  |proj_w v| |w| \leq |v||w| $$


We have a guiding explanation now, and we can use these to the notion of projections to guide the proof.  If $v$ is to project along $w$, then $prov_w v = tw$ for some scalar $t$.  What we're going to make use of here is that the vector $n = v - \vert proj_w v\vert w$ always has non-negative length.

How can we define the function $proj_w$? Well, it's the scalar multiple of $w$ which has the shortest distance from $v$.  So we need to solve
$$ \min_t |v - wt|$$

Where does this occur?  Well, let's square it (no worries, it will have the same minimum) which will make things more manageable

$$\begin{aligned}
  |v - wt|^2 = (v - wt)\cdot(v - wt) &= |w|^2t^2 - 2(w \cdot v) t + |v|^2\\
\end{aligned}$$

Finding the value $t$ for which the function is at a minimum requires finding values of $t$ for which the derivative of the quadratic function above is zero.

$$ \frac{df}{dt} = 2|w|^2t_0 - 2(w \cdot v) = 0 \implies t_0 = \frac{w \cdot v}{|w|^2}$$

So now, we've come up with a working definition of a projection that doesn't rely on all our euclidean tools like straight-edges and compasses.

$$ proj_w (v) = v - w\frac{(w\cdot v)}{|w|^2}$$

And we know that the length of this projection can never be negative, so

$$\begin{aligned}
|proj_w(v)| &= |v - wt_0|\\ &= |w|^2\frac{(w \cdot v)^2}{|w|^4} - 2(w \cdot v)\frac{w \cdot v}{|w|^2} + |v|^2 \\
&= -\frac{(w \cdot v)^2}{|w|^2} + |v|^2
\end{aligned}$$

A projection vector, like any vector, must always have positive length, therefore setting this as greater or equal to zero and a little manipulation shows the desired inequality squared

$$ proj_w(v) \geq 0 \implies |w|^2|v|^2 \geq (w\cdot v)^2$$


#### Another way:

Another way to prove this inequality is to make sure the quadratic function in $t$ has no real roots.  If it had real-roots, then $\vert v - tw\vert$ would be negative valued for some $t$, which is impossible since it is a sum of squares.

 $$ t = \frac{-4(v \cdot w)^2 \pm \sqrt{2^2(v\cdot w)^2 - 4|v|^2|w|^2}}{2|w|^2}$$
<!--  = -2 \frac{(v \cdot w)^2}{|w|^2} -->
 well, we can see right away if we want complex solutions, the expression under the square root sign must stay negative.  Wait a second - there's the cauchy-schwartz inequality!   

### The cross product

Alright, if I remember correctly I'm pretty sure we're going to see the cross product used in a few places in the books, so might as well get confortable with it early on.  Instead of Folland's **BAM there you go** style, I'd like to motivate the definition a little bit.
