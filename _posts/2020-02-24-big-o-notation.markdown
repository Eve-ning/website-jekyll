---
layout: post
title:  "Big-O Notation"
date:   2020-02-24 16:00:00 +0800
categories: data structures and algorithms
---

# Prerequisites

- Pseudocode/Python
- Log Operations
- Summation Operations
- Comparison Operations

# TL;DR

Big O notation is a way to say how fast something is growing.

| Ex.1    | Ex.2     | Ex.3        | Notation |
| ------- | -------- | ----------- | -------- |
| $1$     | $99$     | $2/3$       | $O(1)$   |
| $n$     | $4n$     | $n/3$       | $O(n)$   |
| $n^2$   | $4n^2$   | $n(n+1)$    | $O(n^2)$ |
| $lg(n)$ | $lg(2n)$ | $(lg(n))^2$ | $lg(n)$  |

# Introduction

Big O Notation is a way to determine if your algorithm, in code, could've been improved **IF AND ONLY IF** 

We consider **very large** inputs.

As a real world example, let's say we have a search algorithm to find n documents.

$$f(n) = O(n) = 10n$$

$$g(n) = O(n^2) = 2n^2$$

$$h(n) = O(n^3) = n^3$$

Let's consider some inputs

| Inputs      | $O(n) = 10n$ | $O(n^2) = 2n^2$ | $O(n^3) = n^3$  |
| ----------- | ------------ | --------------- | --------------- |
| $1$         | $10$         | $2$             | $1$             |
| $10$        | $100$        | $200$           | $1000$          |
| $1,000$     | $100,000$    | $2,000,000$     | $1,000,000,000$ |
| $1,000,000$ | $10,000,000$ | $2 * 10^{12}$   | $10^{18}$       |

Though $h(n)$ seems to be the best for smaller inputs, it performs horribly for larger inputs. It could've been searching redundantly & recursively, hence poor performance.

---

Consider the following code in `python`

{% highlight python %}
def function(int n){
    for (int i = 0; i < n; i++){
        print("hello")
    } 
}
{% endhighlight %}

The number of times it will print = n

But when it becomes complicated

{% highlight python %}
def function(int n){
    for (int i = 0; i < n; i++){
        for (int j = 0; j < i; j++){
            print("hello")
        } 
        print("hello")
    } 
    print("hello")
}
{% endhighlight %}

it's hard to put it in words.

That's why we have the Big-O Notation, to say if your way of doing something could be improved.

## Translating into Math

### $O(n)$

I find that the best way to treat these problems is by translating to math.

Consider the previous simple for loop

{% highlight python %}
def function(int n){
    for (int i = 0; i < n; i++){
        print("hello")
    } 
}
{% endhighlight %}

We can write it as if we are counting how many `print("hello")` are executed.

$$\sum_{i=0}^{n-1}1 = n$$

It's a simple sum to calculate, we are just adding 1s together n times.

{% highlight python %}
def function(int n){
    for (int i = 0; i < n; i++){
        print("hello")
        print("hello")
        print("hello")
    } 
}
{% endhighlight %}

Here, we print 3 times each loop

$$\sum_{i=0}^{n-1}3 = 3n$$

Both of these fall into the category of $O(n)$.

### $O(n^2)$

Consider a nested loop

{% highlight python %}
def function(int n){
    for (int i = 0; i < n; i++){
        for (int j = 1; j <= i; j++){
            print("hello")
        } 
    } 
}
{% endhighlight %}

$$\sum_{i=0}^{n-1}\sum_{j=1}^{i}1 = {?}$$

Now this feels like some mental gymnastics. Best to split it into smaller problems.

$$\because\sum_{j=1}^{i}1 = i$$

$$\therefore\sum_{i=0}^{n-1}\sum_{j=1}^{i}1 = \sum_{i=0}^{n-1}i$$

$$\sum_{i=0}^{n-1}i = 0 + 1 + 2 + ... + (n-2) + (n-1)$$

If you forgetten how to sum these series:

$$1 + 2 + ... + (n-1) + n = \frac{(n)(n+1)}{2} \tag{1}$$

$$2^0 + 2^1 + ... + 2^{(n-1)} + 2^n = \frac{(n)(n+1)(2n+1)}{6} \tag{2}$$

Using $(1)$

$$\sum_{i=0}^{n-1}i = \frac{(n-1)(n)}{2} = \frac{n^2}{2} - \frac{n}{2}$$

Great, we managed to make it in terms of $n$, what's next?

**Inequality Testing**

We find what's the nearest higher value of the sum. That is, we want to only have 1 term that has $n$.

$$\frac{n^2}{2} - \frac{n}{2} \leq \frac{n^2}{2}$$

Consider something similar

{% highlight python %}
def function(int n){
    for (int i = 1; i <= n; i++){
        for (int j = 1; j <= i; j++){
            print("hello")
        } 
    } 
}
{% endhighlight %}

$$\sum_{i=1}^{n}\sum_{j=1}^{i}1 = {?}$$

Skipping some steps...

$$\sum_{i=1}^{n}i = \frac{(n)(n+1)}{2} = \frac{n^2}{2} + \frac{n}{2}$$

Same problem, but instead, don't eliminate $n/2$ !

$$\frac{n^2}{2} + \frac{n}{2} \leq \frac{n^2}{2} + \frac{n^2}{2} = n^2$$

### $O(n^3)$

Consider this

{% highlight python %}
def function(int n){
    for (int i = 1; i <= n; i++){
        for (int j = 1; j <= i; j++){
            for (int k = 1; k <= j; k++){
                print("hello")
            } 
        } 
    } 
}
{% endhighlight %}

$$\sum_{i=1}^{n}\sum_{j=1}^{i}\sum_{k=1}^{j}1 = {?}$$

We break it down and calculate them separately!

$$ 
\begin{align}

\sum_{i=1}^{n}\sum_{j=1}^{i}\sum_{k=1}^{j}1
&=\sum_{i=1}^{n}\sum_{j=1}^{i}j \\

&\stackrel{(1)}{=}\sum_{i=1}^{n}\frac{(i)(i+1)}{2} \\

&=\sum_{i=1}^{n}\left(\frac{i^2}{2} + \frac{i}{2}\right) = 
   \frac{1}{2}\sum_{i=1}^{n}i^2 + \frac{1}{2}\sum_{i=1}^{n}i
   \leq \sum_{i=1}^{n}i^2 \\

&\stackrel{(2)}{=}\frac{(n)(n+1)(2n+1)}{6} \\

& =\frac{2n^3}{6} + \frac{2n^2}{6} + \frac{n^2}{6} + \frac{n}{6}
\leq n^3 

\end{align}
$$

### $O(lg(n))$

{% highlight python %}
def function(int n){
    while(n > 1) {
        print("hello")
        n /= 2;
    }
}
{% endhighlight %}

This is odd, but by reading the code, you can kind of see a pattern.

| n    | times | $log_2(n)$       |
| ---- | ----- | ---------------- |
| $1$  | $0$   | $0$              |
| $2$  | $1$   | $1$              |
| $3$  | $1$   | $\lessapprox1.6$ |
| $4$  | $2$   | $2$              |
| $6$  | $2$   | $\lessapprox2.6$ |
| $8$  | $3$   | $3$              |
| $16$ | $4$   | $4$              |

$$2^{times} = n$$

$$times = \log_2(n)$$

Note that for $3, 5, 6, 7, ...$ those numbers that aren't a perfect power of 2 produce a fraction of $times$ when using the formula.

This is fine as long as $log_2(n)$ is the upper bound of $times$

## The elephants in the room

### Constant $k$

Yes, we just classify $kn^a$ as $O(n^a)$ if 
- k is an integer
- k > 0
- n is large

Hence,

It's valid to say

$$17n = O(n)$$

### Tightest bound

It's valid to say

$$\because17n\leq17n^2\leq17n^3$$

$$\therefore17n = O(n^3)$$

But it's rarely useful and counterproductive to have a very loose bound.

The purpose is to find out if the function is **fast**, not how **slow** it could be.

# Summary

It should be easy for you to prove the following now

| Ex.1    | Ex.2     | Ex.3        | Notation |
| ------- | -------- | ----------- | -------- |
| $1$     | $99$     | $2/3$       | $O(1)$   |
| $n$     | $4n$     | $n/3$       | $O(n)$   |
| $n^2$   | $4n^2$   | $n(n+1)$    | $O(n^2)$ |
| $lg(n)$ | $lg(2n)$ | $(lg(n))^2$ | $lg(n)$  |

## Exercises

### 1

$$\sum_{i=1}^{n}\log_{2}(i)$$

### 2

Order them in order of growth

|      |       |       |             |
| ---- | ----- | ----- | ----------- |
| $n$  | $2^n$ | $n!$  | $\log_5(n)$ |
| $9n$ | $8^n$ | $n^n$ | $\ln(n)$    |

### 3

Translate this

{% highlight python %}
def function(int n){
    for (int i = 0; i < n; i++){
        for (int j = 0; j < i; j++){
            print("hello")
        } 
        print("hello")
    } 
    print("hello")
}
{% endhighlight %}

and find the order of growth