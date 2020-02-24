---
layout: post
title:  "Laplace Transform for Circuits"
date:   2020-02-24 00:00:00 +0800
categories: jekyll update
---

# Prerequisites

- Laplace Transformation
- Inverse Laplace Transformation
- Kirchoff's Loop Laws
- Inductor Equation
- Capacitor Equation

# TL;DR

Note that this is grossly simplified as a cheatsheet.

We denote a **Laplace Transform of f(t)** as

$$\mathcal{L}\{f(t)\} = F(s)$$

$$\mathcal{L}^{-1}\{F(s)\} = f(t)$$

| Type                | t-domain  | s-domain |
| ------------------- | --------- | -------- |
| Resistor            | $x\Omega$ | $x$      |
| Ind. Voltage Source | $xV$      | $x/s$    |
| Ind. Current Source | $xA$      | $x/s$    |
| Inductor            | $xH$      | $x*s$    |
| Capacitor           | $xF$      | $1/xs$   |

# Introduction

Laplace Transform makes analysing a circuit much easier, here's a taste of it.

Consider the following.
We will use mesh-analysis to work this out:

**Original Circuit**
```
+--------{25H}--------+
|          →          |
|        ↑ 1 ↓        |
|          ←          |
+-------<6.25F>-------+
|          →          |
|        ↑ 2 ↓        |
|          ←          |
+--[800Ω]--[+ 30V -]--+
```

$$Loop 1\rightarrow-25*\frac{di_1(t)}{dt}-\frac{1}{6.25}*\int_{0^-}^{\inf}\frac{d(i_1(t)-i_2(t))}{dt}dt=0$$

$$Loop 2\rightarrow-\frac{1}{6.25}*\int_{0^-}^{\inf}\frac{d(i_2(t)-i_1(t))}{dt}dt+30-800*i_2(t)=0$$

Well, forget Loop 2, Loop 1 is already daunting to calculate. This should be enough reason to want to learn Laplace.

---

The Laplace transformation transforms something from the **t-domain** to the **s-domain**.

In other words, we want to transform something in terms of **time** to **frequency**.

**Laplace Circuit**
```
+--------{25s}--------+
|          →          |
|        ↑*1*↓        |
|          ←          |
+-----<1/(6.25s)>-----+
|          →          |
|        ↑*2*↓        |
|          ←          |
+--[800]--[+ 30/s -]--+
```
*Notes*:
- Currents are also **Laplace Transformed**
- Assumption that Capacitor and Inductor aren't charged before $0^-$
- Detailed explanation of transformation later

$$Loop 1\rightarrow-25s*I_1(s)-\frac{I_1(s)-I_2(s)}{6.25s}=0$$

$$Loop 2\rightarrow-\frac{I_2(s)-I_1(s)}{6.25s}+\frac{30}{s}-800*I_2(s)=0$$

Looks simpler, but now we have an extra variable **s** in there.

The main goal is the express everything that you need in terms of **s**, and decompose the fraction.

I'll do $i_1$:

$$I_1(s)=
\frac{0.0125}{s+160}-\frac{0.05}{s+40}+\frac{0.0375}{s}$$

$$\mathcal{L}^{-1}\{I_1(s)\}=
\mathcal{L}^{-1}\bigg\{\frac{0.0125}{s+160}\bigg\} -
\mathcal{L}^{-1}\bigg\{\frac{0.05}{s+40}\bigg\} +
\mathcal{L}^{-1}\bigg\{\frac{0.0375}{s}\bigg\}$$

$$i_1(t) =
\left[0.0125e^{-160t}-0.05e^{-40t}+0.0375\right]u(t)$$

We can find the voltage of the inductor

$$L\frac{di_1(t)}{dt} = v_{ind.} =
50e^{-40t}-50e^{-160t}$$

# Proofs

To understand why it works, let's look at the equations

## Resistor

$$v(t) = r * i(t)$$

$$\mathcal{L}\{v(t)\} = \mathcal{L}\{r*i(t)\}$$

$$\mathcal{L}\{v(t)\} = r*\mathcal{L}\{i(t)\}$$

$$V(s) = r*I(s)$$

## Voltage Source

$$v(t) = v$$

$$\mathcal{L}\{v(t)\} = \mathcal{L}\{v\}$$

$$V(s) = \frac{v}{s}$$

*Transforming a Current Source to a Voltage Source would make analysis easier*

## Inductor

$$v(t) = L*\frac{di(t)}{dt}$$

$$\mathcal{L}\{v(t)\} = \mathcal{L}\bigg\{L*\frac{di(t)}{dt}\bigg\}$$

$$V(s) = L*\mathcal{L}\bigg\{\frac{di(t)}{dt}\bigg\} \stackrel{(1)}{=} L *\left[s*I(s)-i(0^-)\right]$$

$$V(s) = LsI(s)-Li(0^-)$$

## Capacitor

$$i(t) = C*\frac{dv(t)}{dt}$$

$$\mathcal{L}\{i(t)\} = \mathcal{L}\bigg\{C*\frac{dv(t)}{dt}\bigg\}$$

$$I(s) = C*\mathcal{L}\bigg\{\frac{dv(t)}{dt}\bigg\} \stackrel{(1)}{=} C *\left[s*V(s)-v(0^-)\right]$$

$$I(s) = CsV(s)-Cv(0^-)$$

$$V(s) = \frac{I(s)+Cv(0^-)}{Cs} = \frac{I(s)}{Cs} + \frac{v(0^-)}{s}$$

**Laplace of a Derivative (1)**

Using the Identity:

$$s\mathcal{L}\{f(t)\}=\mathcal{L}\{f^{\prime}(t)\}+f(0^-)$$

We have

$$\mathcal{L}\{f^{\prime}(t)\} = s\mathcal{L}\{f(t)\}-f(0^-)$$

## Putting it into a Circuit

Notice that we expressed everything in **Voltage** (excluding current source), this is so that we can insert them in easily.


* $V_0, v_0$ are grounds

### Resistor

$$v(t) = r * i(t)$$

$$V(s) = r * I(s)$$

```
 v        v       V       V
[0]      [a]     [0]     [a] 
 +--[xΩ]--+       +--[x]--+
    <---      =      <--   
    i(t)             I(t)
```

Note that if you measure $v_a$ and $V_a$, they tally with the transformation.

### Voltage Source

$$v(t) = v$$

$$V(s) = \frac{v}{s}$$

```
 v            v       V         V
[0]          [a]     [0]       [a] 
 +--[- xV +]--+   =   +--[x/s]--+
    <-------             <----
      i(t)                I(t)
```

*Transforming a Current Source to a Voltage Source would make analysis easier*

### Inductor

$$v(t) = L*\frac{di(t)}{dt}$$

$$V(s) = LsI(s)-Li(0^-)$$

```
 v        v       V                     V
[0]      [a]     [0]                   [a] 
 +--{xH}--+   =   +--{xs}--[+ xi(0) -]--+
    <---             <----------------
    i(t)                    I(t)
```

Notes:
- The direction of the additional Voltage Source is important.
- If the Inductor starts with 0 current, then we can omit it.

### Capacitor

$$i(t) = C*\frac{dv(t)}{dt}$$

$$V(s) = \frac{I(s)+Cv(0^-)}{Cs} = \frac{I(s)}{Cs} + \frac{v(0^-)}{s}$$


```
 v        v       V                       V
[0]      [a]     [0]                     [a] 
 +--<xF>--+   =   +--{x/s}--[- v(0)/s +]--+
    <---             <------------------
    i(t)                     I(t)
```

Notes:
- The direction of the additional Voltage Source is important.
- If the Capacitor starts with 0 voltage, then we can omit it.

# Summary

As mentioned in the TL;DR

At least you understand why now

$$\mathcal{L}\{f(t)\} = F(s)$$

$$\mathcal{L}^{-1}\{F(s)\} = f(t)$$

| Type                | t-domain  | s-domain |
| ------------------- | --------- | -------- |
| Resistor            | $x\Omega$ | $x$      |
| Ind. Voltage Source | $xV$      | $x/s$    |
| Ind. Current Source | $xA$      | $x/s$    |
| Inductor            | $xH$      | $x*s$    |
| Capacitor           | $xF$      | $1/xs$   |
