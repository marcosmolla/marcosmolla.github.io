---
layout: post
title:  Julia's Argument Passing Behaviour
date:   2023-10-05 13:00:00
description: A quick explanation of the "passing-by-sharing" behaviour 
tags: julia
categories: coding
---
There is an interesting behaviour that exists in various programming languages, where a mutable (i.e. it can be changed) object (such as an `Array`) is changed in place, if it is passed as an argument to a function and then changed inside that function.

Let's begin with a matrix as the object we will pass to a function, say: 

```julia
m = [0 1 1 0 1; 1 0 0 1 0; 1 0 0 1 0; 0 1 1 0 0; 1 0 0 0 0]
new_m = m[:,:] # make a copy of the original matrix
new_m
```

This will return the following matrix:

```julia
5×5 Matrix{Int64}:
 0  1  1  0  1
 1  0  0  1  0
 1  0  0  1  0
 0  1  1  0  0
 1  0  0  0  0
```

Assume we pass this matrix to a function to work with it

```julia
function update_m(x)
    len = size(x)[1];
    x = ones(Int, len, len);
end

update_m(new_m)
new_m
```

`update_m()` passes the matrix on to the `x` argument. Inside the function, we create a new matrix, which we then assign to x. The function ends with nothing being returned. When we now take a look at our original matrix we see: 

```julia
5×5 Matrix{Int64}:
 0  1  1  0  1
 1  0  0  1  0
 1  0  0  1  0
 0  1  1  0  0
 1  0  0  0  0
```

that nothing has changed. 

But what happens when, instead of assigning a new value to `x`, we change `x` by indexing it with `[:]`?

```julia
function update_m_mutate(x)
    len = size(x)[1];
    x[:] = ones(Int, len, len);
end

update_m_mutate(new_m)
new_m

5×5 Matrix{Int64}:
 1  1  1  1  1
 1  1  1  1  1
 1  1  1  1  1
 1  1  1  1  1
 1  1  1  1  1
 ```

Now, we find that the original `Array` to which `new_m` pointed has been changed, according to the changes made inside the function. 

The is because Julia passes arguments by sharing them. Instead of creating a copy specifically for the use inside the function, Julia simply points to the same object. This binding will only be broken, when there is a new assignment to the argument (as we have seen above with `x = ones(Int, len, len)`) but not if we make changes to the object. 

This behaviour is also well documented in Julia's manual [right here](https://docs.julialang.org/en/v1/manual/functions/#man-argument-passing), and [discussed here](https://docs.julialang.org/en/v1/manual/faq/#I-passed-an-argument-x-to-a-function,-modified-it-inside-that-function,-but-on-the-outside,-the-variable-x-is-still-unchanged.-Why?).

One big advantage (in case you are not changing the original argument) is that your computer does not have to create copies of objects that are only referenced, which will make code execution a lot more efficient. 

It is just important to keep this behaviour in mind. 
