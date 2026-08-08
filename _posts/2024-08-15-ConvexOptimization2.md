---
layout: distill
title: "[Theoretical Background] Convex Optimization 2"
date: 2024-08-15 00:00:00 +0000
description: >
  I’m back with the second post related to Optimization. Last time, we examined how Gradient Descent converges when the function is $L$-Lipschitz. This time, let's examine what happens when stronger constraints or assumptions are applied. Without further ado, let's jump right in.
authors:
  - name: Jae-Jun Lee
    affiliations:
      name: UNIST
bibliography: blogs.bib
comments: true
hidden: false
tags: [Convex, Optimization, Gradient Descent]
mathjax: true
_meta: >
  <meta name="twitter:card" content="summary">
  <meta name="twitter:title" content="[Theoretical Background] Convex Optimization 2">
  <meta name="twitter:description" content="I’m back with the second post related to Optimization. Last time, we examined how Gradient Descent converges when the function is L-Lipschitz. This time, let's examine what happens when stronger constraints or assumptions are applied. Without further ado, let's jump right in.">
---

### $\star$ Recap [(Previous Post)](https://johnjaejunlee95.github.io/kr/Optimization_1/)

In the previous post, assuming that a function is <mark style="background: orange">convex</mark> and $L$-Lipschitz, we derived the following Theorem:

<mark style="background:skyblue" >Theorem</mark> Let $f$ be convex and $L$-Lipschitz continuous. Then gradient descent with $\gamma = \frac{\mid\mid x\_1 - x^\star\mid\mid}{L\sqrt{T}}$  satisfies:

$$f \left ( \frac{1}{T} \sum_{k=1}^T x_k  \right) - f(x^{\star}) \leq \frac{\mid\mid x_1 - x^\star \mid\mid L}{\sqrt{T}} \Rightarrow \mathcal{O}(\frac{1}{\sqrt{T}})$$

So, what happens if we apply a stronger constraint or assumption than $L$-Lipschitz? Intuitively, we would expect the method to converge much faster. Let's verify through the proof whether it actually converges at a faster rate. (All examples use Gradient Descent.)



### Before we begin...

When dealing with optimization problems, assumptions and constraints can be applied in various ways. (From now on, I will refer to all situations where assumptions or limits are imposed as an ***assumption***.) The stronger the assumption, the fewer functions satisfy it, but the convergence becomes faster. Therefore, by imposing appropriate constraints, we can understand "why it works well" from a Learning Theory perspective. The first assumption we examined was $L$-Lipschitz continuity, and this time we will look at how Gradient Descent converges under the $\beta$-Smoothness condition.

## Problem Formulation 2 <br> (Assumption: $\beta$ -Smooth)

Basically, the definition of $\beta$-Smoothness is as follows:

<mark style="background:lightgreen" >Definition</mark> A continuously differentiable function $f$ is $\beta$-smooth if the gradient $\nabla f$ is $\beta$-Lipschitz:


$$
||  \nabla f(x) - \nabla f(y) ||  \leq \beta || x - y ||
$$


In other words, if an arbitrary function $f$ is differentiable, then its gradient is $\beta$-Lipschitz. If you want to understand why this is a stronger assumption, you can think about the concept of differentiation that we learned in high school.

In the $L$-Lipschitz case, the rate of change of an arbitrary function $f$ is limited in proportion to $L$. However, $\beta$-Smoothness limits the rate of change of the derivative of $f$ in proportion to $\beta$. In other words, $L$-Lipschitz continuity limits the rate of change of the function itself, while $\beta$-Smoothness limits the rate of change of its derivative, or roughly its second derivative. In this sense, all $\beta$-Smooth functions belong to the $L$-Lipschitz class, but not all $L$-Lipschitz functions are $\beta$-Smooth. Therefore, **$\beta$-Smoothness can be seen as a stronger assumption.**

Returning to the main point, let's check how Gradient Descent converges when an arbitrary function $f$ is $\beta$-Smooth.

<mark style="background:skyblue" >Theorem 2:</mark> Let $f$ be convex and $\beta$-smooth on $\mathbb{R}^n$ \. Then, gradient descent with $\gamma = \frac{1}{\beta}$ satisfies


$$
f(x_T) - f(x^\star) \leq \frac{2\beta || x_1 - x^\star || ^2}{T-1}
$$


The derivation is similar to the $L$-Lipschitz case. However, since we have a stronger assumption, we need to examine a few additional points.

### Proof:

Before we begin, there is one more definition that follows from the $\beta$-Smoothness property:


$$
f(y)  \leq f(x) + \left< \nabla f(x) , y-x\right> + \frac{\beta}{2} || y - x ||^2
$$


Since we are assuming that the function $f$ is twice differentiable, this formula can be derived using a Taylor expansion. (Since this is not the main point and is used here as a definition, I will leave the proof as a [link](https://angms.science/doc/CVX/CVX_betasmoothsandwich.pdf).)

Now, let's proceed with the proof of Gradient Descent convergence under $\beta$-Smoothness. The overall derivation is similar to the $L$-Lipschitz case. However, we now have additional properties available to use. First, using the $\beta$-Smoothness property, the Gradient Descent equation can be expanded as follows.


$$
\begin{align}
  f(x_{t+1}) - f(x_t) &\leq \left< \nabla f(x_{t}) , x_{t+1} - x_t \right> + \frac{\beta}{2} || x_{t+1} - x_t||^2 \\
  &= \left< \nabla f(x_t) , -\gamma\nabla f(x_t) \right> + \frac{\beta}{2} ||-\gamma\nabla f(x_t)||^2 \\
  &= -\gamma ||\nabla f(x_t)||^2 + \frac{\beta}{2}*\gamma^2 ||\nabla f(x_t)||^2\\
  &= -\frac{1}{\beta} ||\nabla f(x_t)||^2 + \frac{1}{2\beta}||\nabla f(x_t)||^2 \;\; \text{where } \gamma = \frac{1}{\beta}\\
  &= -\frac{1}{2\beta}||\nabla f(x_t)||^2
  
\end{align}
$$



As in the $L$-Lipschitz proof, convexity of $f$ gives us the following bound: $f(x_t) - f(x^\star) \leq \left< \nabla f(x_t) , x_t - x^\star \right>$

The useful term here is $\nabla f(x_t)$. Since the previous derivation already gives us a bound involving $\mid\mid \nabla f(x_{t})\mid\mid^2$, the squared norm of $\nabla f(x_t)$, we can use it to expand the proof more smoothly. This can be done using the **Cauchy-Schwarz Inequality**.

 ($\star$ **Cauchy-Schwarz Inequality**: $\left< a,b \right> \leq \mid\mid a \mid\mid \cdot \mid\mid b \mid\mid $)


$$
\begin{align}
\left[f(x_t) - f(x^\star)\right]^2 &\leq \left| \left< \nabla f(x_t) , x_t - x^\star \right>\right|^2 \\
&\leq || \nabla f(x_t) ||^2 \cdot || x_t - x^\star ||^2 \\
\Rightarrow \frac{\left[f(x_t) - f(x^\star)\right]^2}{ || x_t - x^\star ||^2} &\leq || \nabla f(x_t) ||^2 
\end{align}
$$


For the next step, we need to rearrange the result into a more manageable form. The easiest way is to work with the bound for $f(x_{t+1}) - f(x_t)$. By subtracting $f(x^\star)$ from both sides of the inequality, we can use the results derived above.

However, before doing that, there is one more point to check. Before examining $f$, we need to understand how the bound changes with step $t$. Therefore, before looking at $f(x_{t+1}) - f(x_t)$, let's first examine the relationship between $x_t$ and $x_{t+1}$.


$$
\begin{align}
    || x_{t+1} - x^\star ||^2 &= || x_t - \frac{1}{\beta} \nabla f(x_t) - x^\star ||^2 \\
    &= ||x_t -x^\star||^2 - \frac{2}{\beta} \underbrace{\left< \nabla f(x_t), x_t - x^\star \right>}_{\leq \frac{1}{\beta} ||\nabla f(x_t)||^2} +\frac{1}{\beta^2} ||\nabla f(x_t)||^2 \\
    &\leq ||x_t - x^\star||^2 \underbrace{- \frac{1}{\beta^2} ||\nabla f(x_t)||^2}_{\text{always decreases}} \\
    &\leq ||x_t - x^\star||^2 \underbrace{- \frac{1}{\beta^2}\frac{\left[f(x_t) - f(x^\star)\right]^2}{ || x_t - x^\star ||^2}}_{\text{smaller decreases}}
\end{align}
$$

Using this property, we can continue the proof in terms of $f(x^\star)$ as follows.


$$
\begin{align}
    f(x_{t+1}) &\leq f(x_t) - \frac{1}{2\beta} ||\nabla f(x_t)||^2 \\
    f(x_{t+1}) - f(x^\star) = &\leq f(x_t) - f(x^\star) - \frac{1}{2\beta}||\nabla f(x_t)||^2 \\
    D_{t+1} &\leq D_t  - \frac{1}{2\beta}||\nabla f(x_t)||^2 \quad \text{where } D_{t+1} = f(x_{t+1}) - f(x^\star)\\
    D_{t+1} &\leq D_t - \frac{1}{2\beta}\cdot \frac{\left[f(x_t) - f(x^\star)\right]^2}{||x_t - x^\star ||^2} \\
    D_{t+1} &\leq D_t - \frac{1}{2\beta} \cdot \frac{D_t^2}{||x_t - x^\star ||^2}
\end{align}
$$



If we divide the inequality above by $D_t D_{T+1}$,


$$
\begin{align}
    \frac{1}{D_{t}} - \frac{1}{D_{t+1}} &\leq - \frac{1}{2\beta||x_t - x^\star ||^2}\frac{D_t}{  D_{t+1}}
\end{align}
$$


we obtain the following. However, we can tighten the bound using two conditions:

1. Since ${D_t}/{D_{t+1}}  = \left[f(x_t) - f(x^\star)\right] / \left[f(x_{t+1}) - f(x^\star)\right]$ and $f(x_{t+1})$ is smaller than $f(x_t)$, we know that ${D_t}/{D_{t+1}} \geq 1$.
2. Since ${x_t - x^\star} \leq x_1 - x^\star $, we know that $1 /{\mid\mid x_t - x^\star \mid\mid^2} \geq 1 / {\mid\mid x_1 - x^\star \mid\mid^2}$.

Since the Right-Hand Side (RHS) term is negative, tightening the bound gives us the following:


$$
\frac{1}{D_{t+1}} - \frac{1}{D_{t}} \geq \frac{1}{2\beta||x_1 - x^\star ||^2}
$$


Just as in the previous derivation, if we substitute $t=1, \ldots , {T-1}$ sequentially,


$$
\begin{align}
\frac{1}{D_{2}} - \frac{1}{D_{1}} &\geq \frac{1}{2\beta||x_1 - x^\star ||^2} \\
\frac{1}{D_{3}} - \frac{1}{D_{2}} &\geq \frac{1}{2\beta||x_1 - x^\star ||^2} \\
    &\;\;\vdots \\
\frac{1}{D_{T}} - \frac{1}{D_{T-1}} &\geq \frac{1}{2\beta||x_1 - x^\star ||^2} \\
\end{align}
$$


Summing the inequalities above gives:



$$
\begin{align}
    \sum_{t=1}^{T-1} \left[\frac{1}{D_{t+1}} - \frac{1}{D_{t}} \right] &\geq \sum_{t=1}^{T-1} \left[\frac{1}{2\beta ||x_1 - x^\star ||^2}\right] \\
    \frac{1}{D_T} - \frac{1}{D_1} &\geq \frac{T-1}{2\beta ||x_1 - x^\star ||^2} \\
    \frac{1}{D_T} &\geq \frac{1}{D_1} + \frac{T-1}{2\beta ||x_1 - x^\star ||^2}
    \end{align}
$$


Here, using the $\beta$-Smoothness property and convexity, we can bound $D_1$ as follows.



$$
\begin{align}
    D_1 = f(x_1) - f(x^\star) &\leq \left< \nabla f(x^\star), x_1 - x^\star \right> + \frac{\beta}{2} ||x_1 - x^\star ||^2\\
    &\leq \frac{\beta}{2} ||x_1 - x^\star ||^2

\end{align}
$$

This is possible because $x^\star$ is a minimizer, so we can assume that the derivative of $f$ is zero at that point. Therefore, since $\nabla f(x^\star) = 0$, we can derive the equation above.



Rearranging the formula gives:


$$
\begin{align}
    \frac{1}{D_T} &\geq \frac{1}{D_1} + \frac{T-1}{2\beta ||x_1 - x^\star ||^2}\\
    \frac{1}{f(x_T) - f(x^\star)} &\geq \frac{1}{f(x_1) - f(x^\star)} + \frac{T-1}{2\beta ||x_1 - x^\star ||^2} \\
    \frac{1}{f(x_T) - f(x^\star)} &\geq \frac{2}{\beta ||x_1 - x^\star ||^2} + \frac{T-1}{2\beta ||x_1 - x^\star ||^2} \\
    \frac{1}{f(x_T) - f(x^\star)} &\geq \frac{T + 3}{2\beta||x_1 - x^\star ||^2}\\
    &\geq \frac{T -1}{2\beta||x_1 - x^\star ||^2}
\end{align}
$$


The reason for changing the numerator from $T-3 \Rightarrow T-1$ above is that we summed from step $1$ to $T-1$. Matching the form makes it easier to set the bound.

Now, for the final step, we take the reciprocal of each term. The proof is complete. **(The inequality direction changes.)**


$$
\begin{align}
    f(x_T) - f(x^\star) \leq \frac{2\beta || x_1 - x^\star || ^2}{T-1} \Rightarrow \mathcal{O} \left(\frac{\beta}{T}\right)
    \end{align}
$$


## Afterwards...

While expanding the formulas, I tried to explain each step as clearly as possible. However, as the number of conditions increased, the explanation became a bit verbose. Because of that, the readability and flow may not feel completely smooth. I will continue polishing it whenever I have time.

I will stop the proofs related to Convex Optimization here. (Most of them can be proved in a similar way.) I will return with different content in the next post.


**$\*\$Thank you very much for reading. If you find any incorrect parts or have any advice, I would appreciate it if you shared your thoughts anytime.**
