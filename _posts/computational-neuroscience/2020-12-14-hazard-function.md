---
title: "The Hazard Function"
subtitle: "How do we derive the Hazard Function in the context of Survival Anaysis?"
layout: post
author: "Andrea Bonvini"
header-style: text
tags:
  - Computational Neuroscience
mathjax: true
---

In *Survival Analysis* we are interested in understanding the *risk* of an event happening at a particular point in time, where time is a continuous variable.

For example, let's consider the event *firing of a neuron*: we define the time of firing as $X$, and time in general as $t$.

The *hazard function*, which is a function of time, is defined as:

$$
h(t) = \lim_{\Delta t\to0}\frac{P(t<X<t+\Delta t|X>t)}{\Delta t}
$$

We are conditioning on $X>t$ because we want to condition our probability on the fact that the event *hasn't occurred yet*.

It's important to note that $h(t)$ doesn't represent a probability, it can assume values bigger than $1$.

Is there a way to rewrite $h(t)$ in a different way?

$$
h(t) = \lim_{\Delta t\to0}\frac{P(t<X<t+\Delta t|X>t)}{\Delta t}\\
h(t) = \lim_{\Delta t\to0}\frac{P(t<X<t+\Delta t,X>t)}{P(X>t)\Delta t}\\
$$

It is easy to see that $(t<X<t+\Delta t)$ is just a subset of $X>t$

```
    O---------------------- {     X > t    }
    |		    o-------------- { t < X < t+Δt }
    |       |	 	
----.-------.----.---------
    t       X   t+Δt
```


$$
  h(t) = \lim_{\Delta t\to0}\frac{P(t<X<t+\Delta t)}{P(X>t)\Delta t}
$$

$P(X>t)$ is called the *survival function* and is just $1$ minus the cumulative distribution function (*CDF*):

$$
  P(X>t) = 1-F(t)=1-\int_{t_0}^tp(t)dt
$$

The remaining part is the definition of the derivative of the *CDF*, which is just the *probability density function* (*PDF*) at time $t$ 

$$
  \lim_{\Delta t\to0}\frac{P(t<X<t+\Delta t)}{\Delta t}= \lim_{\Delta t\to0}\frac{P(X<t+\Delta t)-P(X <t)}{\Delta t}=\\
  \lim_{\Delta t\to0}\frac{F(t+\Delta t)-F(t)}{\Delta t}=p(t)
$$

So, finally we can rewrite the *hazard function* as:

$$
  h(t) = \frac{p(t)}{1-\int_{t_0}^tp(t)dt}
$$

