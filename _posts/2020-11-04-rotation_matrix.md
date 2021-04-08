---
layout: post
categories: [markdown]
title:  Rotation matrix
keywords: linear-algebra
categories: [blurb]
tags: blurb
hide: true
---

# Rotation matrix
### how to think about it intuitively?

A rotation matrix is just the result of two dot-products, the top ![](nk:56,eNrjYGBgkABiRiCeN1fW/vAhJ/vTp07ZMTA02DMBxRjmtDoweAU6JDjPdqx4t9yRgcHBAYQBa7o=)row with our vector, and the bottom row with our vector.

But remember what dot-products are?  They're projections with scaling (link to 3Blue1Brown video here).

If you look at the top row dot product with our vector, think of the top row itself as a vector.  What vector does it form?  It forms the vector that would be the x-axis if the x-axis was one unit long, and had rotated $\theta$ degrees.
![](nk:44,eNrbwsjAIMjAwACkGObNlbU/fMjJ/vSpU3YMDA32ILEpSjecDzwLd2RgcHA=)
Similar thing can be said about the bottom row and the y-axis.  So a rotation matrix can just be constructed by rotating the x and y-axis, and finding the projection of our vector onto that, instead of thinking rotating our vector![](nk:44,eNpjY2BgEARiRgZ00GAPFpN1d1jQmeEIZDkAACQ4A48=)and then finding the new projections.
