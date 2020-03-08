---
layout: post
title:  "Convergence Tests Summary"
date:   2020-03-08 08:00:00 +0800
categories: math series
---

# Summary

## Good to Know

Any Series diverges if:
- $\lim\limits_{n\rightarrow\infty}s_n = c \neq 0$

If $n$ is close enough to infinity, we'll be constantly adding a constant $c$

Regardless of it's value, if it's not 0, it'll tend to $+\infty$ or $-\infty$

## Alternating Series Test

Series **Converges** if:
- $\lim\limits_{n\rightarrow\infty}s_n = 0$
- $|a_{n+1}| \leq |a_{n}|$
  - It can be $\leq$ because
    $\lim\limits_{n\rightarrow\infty}s_n = 0$ guarantees it'll end up at $0$

## Integral Test

If it's possible to map $a_n = f(n)$

If $f(n)$ is:
- Continuous
- Decreasing
- Positive, *hence $a_n$ must be all positive*

If $N$ is some positive integer

$\int_{N}^{\infty} f(n)$ converges then $\sum_{n=N}^{\infty}a_n$ converges

$\int_{N}^{\infty} f(n)$ diverges then $\sum_{n=N}^{\infty}a_n$ diverges

## Comparison Test

If $a_n > 0$ and $b_n > 0$ 

If $a_n \leq b_n$ and $\sum b_n$ converges, then $\sum a_n$ converges

If $a_n \geq b_n$ and $\sum b_n$ diverges, then $\sum a_n$ diverges

## Limit Comparison Test

If $a_n > 0$ and $b_n > 0$ 

If $N$ is some positive integer

$\lim_{n\rightarrow\infty}(a_n/b_n)$ = Test Result

|Result|a;b|
|-|-|
|$> 0$|Converge;Converge OR Diverge;Diverge|
|$= 0$|Converge;Converge|
|$\infty$|Diverge;Diverge|
|Other|No Conclusion|

## Absolute/Conditional Convergence

If $\sum a_n$ converges, it's **Convergent**

If $\sum\lvert a_n\rvert$ converges, it's **Absolutely Convergent**

If a series is **Absolutely Convergent**, it's **Convergent**.

If a series is **Convergent** but not **Absolutely Convergent**, it's **Conditionally Convergent**

## P-Series

$$\sum_{n=0}^{\infty}1/(p^n)$$

|p||
|-|-|
|$< 1$|Converge|
|$\geq 1$|Diverge|

## Ratio Test

$\lim\limits_{n\rightarrow\infty}\lvert a_{n+1}/a_n\rvert$ = Test Result

---

|Result||
|-|-|
|$< 1$|Absolute Converge|
|$> 1$|Diverge|
|$= 1$|No Conclusion|

## Root Test

$\lim\limits_{n\rightarrow\infty}(a_n)^{1/n}$ = Test Result

|Result||
|-|-|
|$< 1$|Absolute Converge|
|$> 1$|Diverge|
|$= 1$|No Conclusion|
