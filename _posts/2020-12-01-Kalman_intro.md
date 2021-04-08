---
toc: true
layout: post
description:
categories: [markdown, inquiry-based-learning, recall]
title:  What do I remember about Kalman filters?
keywords: robotics, computer-vision
categories: [blurb]
tags: blurb
search_exclude: false
---

# What do I remember about Kalman Filters...

### What are Kalman filters

Core idea: Kalman filters are used to combine different measurements of some state, make use of the the variance of each measurements errors distribution to make a "best" guess.  An unbiased estimator?

Kalman filters are used to solve the problem: how to track a moving object.

More abstractly, Kalman filters are a way to track state when we have one or several measurements of the state, from possibly different sensors with different error distributions, as well as knowing the previous state.

Always working with gaussian errors allows us to to stay in a space.  Gaussian distributions are a "closed" space under addition and scalar multiplication.  If our way of combining distributions makes use of only these operations, then maybe we can always add information as it comes in.  

### Questions about Kalman Filters

Are there any restrictions on our motion model?

If there are restrictions on our motion model, what sort of problems have motion models that allow Kalman Filters to be applicable, and which don't?

Is applying a Kalman filter in some sense the "optimal" way to make use of the information we have in order to infer the state of the object?

Are predictions by our motion model treated any differently from another sensor providing a measurement when we combine our information to make an estimate of state with the Kalman filter?  How much weight do we give to our model vs our measurements?

### Trying to reconstruct the gist of Kalman filters

Say we have two measurements $m_1, m_2$ with gaussian error terms, $\epsilon(0, \sigma_1), \epsilon(0, \sigma_2)$, or alternatively written $m_1 \sim N(\mu_1, \sigma_1)$, and if $m_2$ is measuring the same thing and it is unbiased, $m_1 \sim N(\mu_1, \sigma_2)$.  I can't imagine we would be able to do anything if the errors where unbiased ($\mu \neq 0$).  How would we combine these two measurements in a way that gave an unbiased estimator?  One might be to just take an average

$$ \frac{m_1 + m_2}{2} $$

and the estimator would be unbiased, since $E(\frac{m_1 + m_2}{2}) =  0$.  However, is this the really the best we can do?  What if the variance of the second measure is huge, and the first measurement has nearly no variance?  Our intuition would be to almost just take the first measurement and forget the second, or at least heavily weight towards the first.  Why?  Because the first has much lower variance.  So can we combine the measurements in a way such that the variance of the combined variance is lower than the variance of either of the measurements on the their own?

$$
\begin{aligned}
Var(m_1 + m_2) &= \sum ((m_1 + m_2) - (u_1 + u_2))^2p(m_1, m_2) \\
  &= \sum((m_1 - u_1) + (m_2 - u_1))^2p(m_1, m_2)
\end{aligned}
$$


Continuing to expand the lhs term in the product

$$
\begin{aligned}
&= \sum_{m_1,m_2 \in D} \big [(m_1 - u_1)^2 + (m_2 - u_2)^2 + (m_1 - u_1)(m_2 - u_2)\big ]p(m_1, m_2) \\
&= \sigma_1^2 + \sigma_2^2 + 2\sum_{m_1}\sum_{m_2} (m_1 - u_1)(m_2 - u_1)p(m_1, m_2)
\end{aligned}
$$

but that last term is just 2 times the covariance of $m_1$ and $m_2$!  

<details>
<summary>tangent</summary>
We can actually continue this step to expand the covariance to find a formula of covariance in terms of expected values.
$$
\begin{aligned}
= \sigma_1^2 + \sigma_2^2  + 2u_1^2 + \sum_{m_1}\sum_{m_2} (m_1m_2 - (m_1 + m_2)u_1)p(m_1, m_2) \\
&= \sigma_1^2 + \sigma_2^2  + u_1^2 - u_1E(m_1) -u_1E(m_2) + E(m_1m_2) \\
&= \sigma_1^2 + \sigma_2^2  - u_1^2 + E(m_1m_2)
\end{aligned}
$$
And to reduce $E(m_1m_2)$ further,
$$E(u_1 + \epsilon(0, \sigma_1)(u_1 + \epsilon(0, \sigma_2))) \\
= E(u_1^2 + \epsilon_2u_1, + \epsilon_1u_2 + \epsilon_1\epsilon_2)\\
= E(u_1^2) + E(\epsilon_1\epsilon_2))\quad \text{by linearity and $E(\epsilon_i) = 0$}$$

If $\epsilon_1$ and $\epsilon_2$ are independent, then $E(\epsilon_1\epsilon_2) = 0$, because we can write $p(m_1,m_2)$ as $p(m_1)p(m_2)$ and

$$\sum_{\epsilon_1}\sum_{\epsilon_2} \epsilon_1 \epsilon_2 p(m_1)p(m_2) = \sum_{\epsilon_1}\epsilon_1 p(m_1)E(\epsilon_1) = 0 $$
</details>

Here's the thing.  If the covariance of these errors is zero, then our equation is much simplified.  We'll just have to remember later to keep this assumption in mind when we start to apply this to real-world problems.

But remember, we wanted to take a linear combination of these two measurements.  Because $Var(am_1) = a^2Var(m_1)$, we have

$$ Var(\lambda m_1 + (1 - \lambda) m_2) = \lambda^2 \sigma_1^2 + (1 - \lambda)^2 \sigma_2^2 $$

We parametrize our linear combination of measurements this way, because it is the only way to create an unbiased estimator if $\lambda$ is to be our coefficient of $m_1$.  If we want to minimize the variance, we need to solve for the derivative with respect to $\lambda$

$$
2\lambda \sigma_1^2 - 2(1 - \lambda)\sigma_2^2 = 0 \implies \left (\frac{1}{\lambda} - 1\right)
= \left ( \frac{\sigma_1}{\sigma_2} \right) \\ \implies \lambda = \frac{1}{\left(\frac{\sigma_1}{\sigma_2}\right) + 1}
$$

A quick check with our intuition also tells us this seems right - if two measurements have the same variation in the error, our formula for $\lambda$ tells us they should be equally weighted.  If the relative variance of $m_1$ is much lower, our $\lambda$ approaches 1, telling us to pretty much just use measurement 1 and ignore the second measurement.

However, notice in what we've done so far, we haven't needed to assume our error is gaussian.  All we've assumed is that our measurements are unbiased, and that the errors have zero covariance.

Now let's apply this to an example.  

Thoughts for example here:
- temperature datasets - combining satellite and ground-station measured temperatures

### Adding in additional measurements as they come (or "realtime" filtering)

Let's say we're trying to measure some quantity that doesn't vary over time, or vary very much other a short time period over which we're taking consecutive samples.  It could be something like sticking a thermometer in the ocean and reading out measurements every minute for an hour to try and get an accurate picture of ocean temperature in that place.  Perhaps this isn't such a great example, because the ocean isn't uniform enough in temperature, with currents, solar radiation of the top layer, tides, etc.

Perhaps let's contrive an example out of something we know is constant.  Like the force of gravity at sea level at the lattitude of the equator.  Now suppose every day we drop the same rubber ball 10 feet until some sensor detects it hits ground level.  Our sensor is unbiased, but it's error has variance $0.2s^2$.  We want to keep performing this experiment until we have an estimate of the gravitational constant with variance $10^-3$.  How many times do we repeat this experiment?

One way of doing this is to repeat the experiment, at each iteration finding some way to combine the variance of the current best estimate with the current trial.  If at iteration $i$ our best estimate is $c_i$ with variance $\sigma_i$, we can treat this as if we just made a observation $c_i$ with a instrument that had error variance $\sigma_i$, and apply the above formula.

### Kalman filtering for multiple variables

Now suppose instead of measuring just temperature, we're measuring temperature and current flow at a stationary bouy in the ocean.  

Notice that, while we could treat each of the variables separately, we're missing out on some possibly important information.  That is, it is likely that temperature and flow are correlated in this scenario.  Perhaps the bouy is in an inlet, where water warms up, and hence when the tide flows out it is warmer than when it flows in.

To get a complete picture of variation in our system, we look at something called the covariance matrix.  We generalize our notion of covariance from one dimension


$$ E[(m_1 - \mu_1)^2)]$$


to multiple dimensions

$$\mathbf
E[(\vec{m} - \vec{\mu})(\vec{m} - \vec{\mu})^T]
$$


Or written in matrix form in 2 dimensions:


$$
\sum_{\vec{m}} \begin{bmatrix} (m_1 - \mu_1)^2 & (m_1 - \mu_1)(m_2 - \mu_2) \\ (m_1 - \mu_1)(m_2 - \mu_2) & (m_2 - \mu_2)^2 \end{bmatrix}p(\vec{m})
$$

Now, we'll repeat the investigation that we made early in this higher-dimensional space. What's the covariance matrix of two measurements added together?


$$\begin{aligned}
\mathbf E[(\vec{m + n} - (\vec{\mu_m + \mu_n}))(\vec{m + n} - (\vec{\mu_m + \mu_n}))^T] \\
&= \sum_{\vec{m}}
  \begin{bmatrix}
    (m_1 + n_1 - (\mu_{m_1} + \mu_{n_1}))^2 & (m_1 + n_1 - (\mu_{m_1} + \mu_{n_1}))(m_2 + n_2 - (\mu_{m_2} + \mu_{n_2})) \\
    (m_1 + n_1 - (\mu_{m_1} + \mu_{n_1}))(m_2 + n_2 - (\mu_{m_2} + \mu_{n_2})) & (m_2 + n_2 - (\mu_{m_2} + \mu_{n_2}))^2  
  \end{bmatrix}
p(\vec{m})
\end{aligned}
$$


Yikes.  That's looking like a handful to make sense of at first sight.  However, if we look at just the diagonal entries, each one just repeats the signle-dimensional case we did earlier of calculating variance for two measurements added together.  If we again make the assumption that measurements $m_i$ and $n_i$ are pairwise uncorrelated (have zero covariance), then each diagonal entry simplifies to $\sigma_{m_i}^2 + \sigma_{n_i}^2$.  The off-diagonal entries measure the cross-correlation between variables $m_i$ and $n_j$.  Should these as well be zero?  Our intuition says yes, given our example ----.   

It's possible that we have non-zero covariance on the non-diagonals though.  Intuitively, remember our tidal example from above.  Adding together the temperature/flow vectors from two different instruments will maintain a covariance between summed temperature and summed vector flow values.

However, we're confusing two different notions here.  One is that current flow and temperature are related, and hence should show a correlation.  The other is that, at a given point in time where we assume that there is a fixed flow and temperature, two instruments (really, likely four), one providing measure $\vec{m}$ and the other providing measurement $\vec{n}$ have uncorrelated errors, both in components with the same indice, but also between components with different indices (e.g. flow $m_2$ measured from instrument 1 and temperature $n_1$ from instrument 2 should remain uncorrelated).

Instead of describing our estimator as a linear combination of measurements, we can also describe it as a linear operator (matrix) on our measurements

$$ \vec{y} = A_{\vec{m}}\vec{m} + A_{\vec{n}}\vec{n} $$

where the matrices are diagonal.


$$
\begin{aligned}
&\sigma^2_{m_1 + n_1, m_2 + n_2}\\
=&\sum_{m_1,n_1}(m_1 + n_1 - (\mu_{m_1} + \mu_{n_1}))(m_2 + n_2 - (\mu_{m_2} + \mu_{n_2}))p(m,n)\\
=&\sum_{m_1,n_1}((m_1 - \mu_{m_1}) + (n_1 - \mu_{n_1}))((m_2- \mu_{m_2}) + (n_2 - \mu_{n_2})))p(m,n)\\
  =&\sum_{m_1,n_1} \big [(m_1 - \mu_{m_1})(m_2 - \mu_{m_2}) \\
  & \quad\quad - (m_1 - \mu_{m_1})(n_1 - \mu_{n_1})\\
  & \quad\quad- (m_2 - \mu_{m_2})(n_2 - \mu_{n_2}) \\
  & \quad\quad + (n_1 - \mu_{n_1})(n_2 - \mu_{n_2}) \big ]p(m_1, n_1)\\
=& \sigma^2_{m_1 m_2} - \sigma^2_{m_1 n_2} - \sigma^2_{n_1 m_2} + \sigma^2_{n_1 n_2}
\end{aligned}
$$
