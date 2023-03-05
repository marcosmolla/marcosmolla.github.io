---
layout: post
title: A new post from me
date: 2023-01-03 00:59:00-0400
description: an example of a blog post with giscus comments
categories: sample-posts thoughts
giscus_comments: false
related_posts: false
---
Just trying out what this $$L$$ooks like.

```julia
using Statistics, Distributed, Graphs, RCall
@everywhere using SharedArrays
addprocs(6);

grid = collect(Base.product(
    500,                            # N, Number of individuals
    10,                             # K, Degree
    [-1 1 1e-1 1e-2 1e-3 1e-4 0],      # Beta, rewiring
    5e4                             # S, number of simulations
));
```