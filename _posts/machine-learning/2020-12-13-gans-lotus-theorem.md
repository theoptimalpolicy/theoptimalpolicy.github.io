---
title: "LOTUS Theorem in GANs"
subtitle: "How do we apply LOTUS Theorem to GANs' original value-function."
layout: post
author: "Andrea Bonvini"
header-style: text
tags:
  - Generative Modeling
  - Generative Adversarial Networks
  - Statistics
mathjax: true
---



In this brief we want to prove a passage of the original GANs Paper by Ian Goodfellow (check [this](https://theoptimalpolicy.github.io/2020/12/15/gans-1/) one if you want a full overview of the paper). Specifically, we want to prove that the following equation is satisfied:

$$
\int_{\mathbf{z}}p_{z}(\mathbf{z})\log(1-\mathcal{D}(\mathcal{G}(\mathbf{z})))d\mathbf{z}
=
\int_{\mathbf{x}}p_{g}(\mathbf{x})
\log(1-\mathcal{D}(\mathbf{x}))
d\mathbf{x}
$$

For a continuous random variable $\mathbf{z}$, let $\mathbf{x} = \mathcal{G}(\mathbf{z})$ and $\mathbf{z}=\mathcal{G}^{-1}(\mathbf{x})$ suppose that $\mathcal{G}$ is differentiable and that its inverse $\mathcal{G}^{-1}$ is monotonic. By the formula for inverse functions and differentiation we have that

$$
\frac{d\mathbf{z}}{d \mathbf{x}}\cdot
\frac{d \mathbf{x}}{d \mathbf{z}}
=
1
$$

$$
\frac{d\mathbf{z}}{d \mathbf{x}}
=
\frac{1}{\frac{d \mathbf{x}}{d \mathbf{z}}}\\
$$

$$
\frac
{d\mathbf{z}}
{d\mathbf{x}}
=
\frac
{1}
{\frac{d\mathcal{G}(\mathcal{G}^{-1}(\mathbf{x}))}{d(\mathcal{G}^{-1}(\mathbf{x}))}}
$$

$$
\frac
{d\mathbf{z}}
{d\mathbf{x}}
=
\frac
{1}
{\mathcal{G}^{'}(\mathcal{G}^{-1}(\mathbf{x}))}
$$

$$
d\mathbf{z}
=
\frac
{1}
{\mathcal{G}^{'}(\mathcal{G}^{-1}(\mathbf{x}))}d\mathbf{x}
$$

so that by a change of variables we can rewrite everything in function of $\mathbf{x}$,

$$
\int_{-\infty}^{\infty}
p_{z}(\mathbf{z})
\log
\left(
1-\mathcal{D}(\mathcal{G}(\mathbf{z}))
\right)
d\mathbf{z}
=
$$

$$
\int_{-\infty}^{\infty}
p_{z}(\mathbf{z})
\log
\left(
1-\mathcal{D}(\mathbf{x})
\right)
\frac{1}{\mathcal{G}^{'}(\mathcal{G}^{-1}(\mathbf{x}))}d\mathbf{x}=
$$

$$
\int_{-\infty}^{\infty}
\color{orange}{p_{z}(\mathbf{z})}
\log
\left(
1-\mathcal{D}(\mathbf{x})
\right)
\color{orange}{
\frac{1}{\mathcal{G}^{'}(\mathcal{G}^{-1}(\mathbf{x}))}}d\mathbf{x}=
$$

$$
\int_{-\infty}^{\infty}
\color{orange}{p_{z}(\mathbf{z})}
\color{orange}{
\frac{1}{\mathcal{G}^{'}(\mathcal{G}^{-1}(\mathbf{x}))}}
\log
\left(
1-\mathcal{D}(\mathbf{x})
\right)
d\mathbf{x}=
$$

Now, notice that the cumulative distribution function $P_\mathbf{X}(\mathbf{x}):\mathbb{R}^n\to[0,1]$ is $P_\mathbf{X}(\mathbf{x})=Pr(\mathbf{X}<\mathbf{x})=Pr(X_1\leq x_1,X_2\leq x_2,\dots,X_n\leq x_n)$, we observe that:

$$
P_\mathbf{X}(\mathbf{x})=
$$

$$
Pr(\mathbf{X}<\mathbf{x})=
$$

$$
Pr(\mathcal{G}(\mathbf{Z})\leq\mathbf{x})=
$$

$$
Pr(\mathbf{Z}\leq\mathcal{G}^{-1}(\mathbf{x}))=
$$

$$
P_{\mathbf{Z}}(\mathcal{G}^{-1}(\mathbf{x}))
$$

From here 

$$
P_\mathbf{X}(\mathbf{x})=P_{\mathbf{Z}}(\mathcal{G}^{-1}(\mathbf{x}))\\
P_\mathbf{X}(\mathbf{x})=P_{\mathbf{Z}}(\mathbf{z})
$$

We take the derivative w.r.t $\mathbf{x}$:

$$
\frac{dP_\mathbf{X}(\mathbf{x})}{d\mathbf{x}} =\frac{P_{\mathbf{Z}}(\mathbf{z})}{d\mathbf{x}}
$$

$$
\frac{dP_\mathbf{X}(\mathbf{x})}{d\mathbf{x}} =\frac{P_{\mathbf{Z}}(\mathbf{z})}{d\mathbf{z}}\frac{d\mathbf{z}}{d\mathbf{x}}
$$

$$
p_{X}(\mathbf{x}) = p_{\mathbf{z}}(\mathbf{z})\frac{1}{\mathcal{G}^{'}(\mathcal{G}^{-1}(\mathbf{x}))}
$$

For clarity we rename $p_{X}(\mathbf{x})$ as $p_{g}(\mathbf{x})$ since it represents the distribution learned from our generator $\mathcal{G}$, we have

$$
\color{orange}{p_{g}(\mathbf{x}) = p_{\mathbf{z}}(\mathbf{z})\frac{1}{\mathcal{G}^{'}(\mathcal{G}^{-1}(\mathbf{x}))}}
$$