---
layout: post
title:  "ELI5 Proof By Induction"
date:   2020-02-25 08:00:00 +0800
categories: eli5 math
---

# Prerequisites

- \> 5 Years old

## What you want to prove

Consider this **infinitely long** string attached to a wall

The labels are increasing in value

$$
|
\stackrel{(1)}{-}
\stackrel{(2)}{-}
\stackrel{(3)}{-}
\stackrel{(4)}{-}
\stackrel{(5)}{-}
\stackrel{(6)}{-}
\stackrel{(7)}{-}
\stackrel{(8)}{-}
\stackrel{(9)}{-}
\stackrel{(...)}{-}
\rightarrow$$

*The $\rightarrow$ indicates it goes on forever*

Prove that label $Z$ is on the string.

- *$Z$ is a positive integer*

## Prove the 1st Case

$$
|
\stackrel{(1)}{-}
--------
\stackrel{(...)}{-}
\rightarrow$$

Consider the first location here $(1)$

Looking at it,

- $(1)$ is on the string

## Assume Case X

$$
\leftarrow---------\stackrel{(X)}{-}----------\rightarrow \\
$$

Assume that Step 1 is true for any location $X$

What you **assume**,

- $(X)$ is on the string

## Prove Case X+1

$$
\leftarrow---------\stackrel{(X)}{-}\stackrel{(X+1)}{-}----------\rightarrow \\
$$

Prove that the next location, $(X+1)$ holds the assumption

Since we know that the string is infinite, the next label should exist.

- $(X+1)$ is on the string

## Conclusion

$$\stackrel{(1)}{-}
\stackrel{(2)}{-}
\stackrel{(3)}{-}
---------------------\rightarrow$$

Since $(1)$ is on the string,

using the proof from **Case X+1**,

$(1+1) = (2)$ is on the string

$...$

Since $(2)$ is on the string,

using the proof from **Case X+1**,

$(2+1) = (3)$ is on the string 

$...$

Since $(Y)$ is on the string,

using the proof from **Case X+1**,

$(Y+1)$ is on the string 

$...$

This proof follows until we get $Z$



