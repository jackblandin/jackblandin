+++
title = "Variational Inference"
date = 2020-04-29T13:08:57-05:00
draft = false
tags = []
categories = []
summary="This is a written summary of the first half of Lecture 14 of UC Berkeley’s Fall 2018 CS294-112 Deep Reinforcement Learning course taught by Sergey Levine."
math="true"
+++

### Summary
This is a written summary of the first half ofthe first half of  Lecture 14 of UC Berkeley's Fall 2018 [CS294-112 Deep Reinforcement Learning](http://rail.eecs.berkeley.edu/deeprlcourse-fa18/) course taught by Sergey Levine.
* [Lecture recording](https://www.youtube.com/watch?v=1bpQ0QDPGuI&list=PLkFD6_40KJIxJMR-j5A1mkxK26gh_qg37&index=12)
* [Slides](http://rail.eecs.berkeley.edu/deeprlcourse-fa18/static/slides/lec-14.pdf)

The purpose of this lecture is to show how to use Variational Inference to learn latent variable probabilistic models (i.e. a multimodal policy is a latent model; a POMDP is also a latent model). When we think of conditional/latent variable models for reinforcement learning, we are thinking of a model $p(y|x,z)$ where $y$ is a distribution over actions, $x$ is the observations, and $z$ are the latent (unobserved) variables:
$$p(y|x,z) = \int _z p(y|x,z)dz$$

The remainder of this post shows the derivation for Variational Inference.

#### Variational Inferece Problem Formulation
To describe variational inference, we will consider a simpler model, that is just $p _\theta (x)$ where $x$ is the observed data that we want to model (i.e. $\mathcal{D} = {x_1, x_2, x_3, ..., x_N}$). Also, we assume there are latent parameters $z$.

Normally, we would fit the parameters of $p _{\theta}$ using maximum likelihood:

$$
\max _{\theta} \prod p _{\theta} (x) \tag{optimization}
$$

$$
\begin{align}
\theta &\leftarrow \underset{\theta}{\text{argmax}} \frac{1}{N} \sum _i \text{log } p _{\theta} (x_i) \tag{parameter update} \\\\\\
&\leftarrow \underset{\theta}{\text{argmax}} \frac{1}{N} \sum _i \text{log } \left( \int p _{\theta} (x_i | z) p(z) dz \right)
\end{align}
$$

However, if $p(x|z)$ is a difficult distribution, then **computing $p(x|z)$ is completely intractable**.

<!--

#### Tractable alternative to maximum likelihood
> Maybe if you don't know what the value of $z$ is for each of your data points, then maybe you can guess... If you're unsure of what cluster $z$ your data point $x_i$ falls into, then pick the most likely cluster $z$ and maximize its likelihood in that cluster. If the data point falls between two clusters, then maximize its likelihood w.r.t. both clusters, weighted by their probability $E_z \sim p(z|x_i)$. This is called _Expected Log LIkelihood_ and that's what's used in Expectation Maximization.

So instead of computing the maximum likelihood, we maximize the _expected_ log-likelihood.
$$
\theta \leftarrow \underset{\theta}{\text{argmax}} \frac{1}{N} \sum _i E _{z \sim p(z|x_i)} [ \text{log } p _{\theta} (x_i, z)]
$$

The above formulation is equivalent to Equation (1) and is derived using the definition of expectation:

$$
\mathbb{E} _{x \sim p(x)} [f(x)] = \int _x f(x) p(x) dx \tag{Expectation}
$$

and the definition of condition probability

$$
p(x|y) = \frac{p(x,y)}{p(y)} \tag {Conditional Probability}
$$

But... how do we calculate $p(z|x_i)?$ This can be very difficult since there may not be an easy mapping between the two distributions.

What if we approximate $p(z|x_i)$ with $q_i(z) = \mathcal{N}(\mu_i, \sigma_i)$ where $\mu_i$ and $\sigma_i$ are output by a neural network?

![slide-12](/img/notes/variational-inference/slide-12.png)

-->

Using some fancy probability theory, we can show a different way of maximizing the (log) likelihood $\log p(x_i)$.

$$
\begin{align}
\log p(x_i) &= \log \int _{z} p(x_i|z)p(z) \\\\\\
&= \log \int _{z} p(x_i|z) \frac{q_i(z)}{q_i(z)} \\\\\\
&= \log \mathbb{E} _{z \sim q_i(z)} \left [ \frac{p(x_i|z) p(z)}{q_i(z)} \right] \\\\\\
&\geq \mathbb{E} _{z \sim q_i(z)} \left [ \log \frac{p(x_i|z) p(z)}{q_i(z)} \right] \\\\\\
&= \mathbb{E} _{z \sim q_i(z)} \left[ \log p(x_i|z) + \log p(z) \right] - \mathbb{E} _{z \sim q_i(z)} \left[ \log q_i(z) \right]
\end{align}
$$

The rightmost expectation term (including the negative sign) is actually the entropy $\mathcal{H}(q_i)$.
The entire bottom expression is referred to as the Evidence Lower BOund (ELBO) and is represented as $\mathcal{L} _{i} (p, q_i)$.
The $\geq$ sign comes from _Jensen's Inequality_:
$$
\log \mathbb{E}[y] \geq \mathbb{E}[\log y]
$$

So we have
$$
\log p(x_i) \geq \mathcal{L} _{i} (p, q_i) \tag{1}
$$

While now showing the full derivation, it is possible to show from the definition of *KL-divergence* that

$$
\begin{align}
D _{KL} \left( q_i(x_i) || p(z|x_i) \right) = - \mathcal{L} _{i} (p, q_i) + \log p(x_i) \tag{2}
\end{align}
$$

**Because the $\log p(x_i)$ term is independent of $q_i$, we can see that maximizing $\mathcal{L} _{i} (p, q_i)$ w.r.t. $q_i$ minimizes KL-divergence.**

This is important because we are going to use $q_i$ to approximate $p(z|x_i$, which we need in order to perform gradient descent on $\theta$, which is our original objective. Note that we are going to maximzie the ELBO instead of the likilihood:

$$
\theta \leftarrow \underset{\theta}{\text{argmax}} \frac{1}{N}\ \sum _{i} \mathcal{L} _{i} (p, q_i)
$$

The full update algoirthm is

$$
\begin{align}
&\text{for each } x_i \text{ (or mini-batch)}: \\\\\\
&\text{ } \hspace{2cm} \text{calculate } \nabla _{\theta} \mathcal{L} _{i} (p, q_i) \\\\\\
&\text{ } \hspace{4cm} \text{sample } z \sim q_i (z) \\\\\\
&\text{ } \hspace{4cm} \text{sample } \nabla _{\theta} \mathcal{L} _{i} (p, q_i) \approx \nabla _{\theta} \log p _{\theta} (x_i | z) \\\\\\
&\text{ } \hspace{2cm} \theta \leftarrow \theta + \alpha \nabla _{\theta} \mathcal{L} _{i} (p, q_i) \\\\\\
\\\\\\
&\text{ } \hspace{2cm} \text{calculate } \nabla _{\theta} \mathcal{L} _{\mu_i} (p, q_i) \\\\\\
&\text{ } \hspace{4cm} \text{... } \\\\\\
&\text{ } \hspace{2cm} \mu_i \leftarrow \mu_i + \alpha \nabla _{\mu_i} \mathcal{L} _{i} (p, q_i) \\\\\\
\\\\\\
&\text{ } \hspace{2cm} \text{calculate } \nabla _{\theta} \mathcal{L} _{\sigma_i} (p, q_i) \\\\\\
&\text{ } \hspace{4cm} \text{... } \\\\\\
&\text{ } \hspace{2cm} \sigma_i \leftarrow \sigma_i + \alpha \nabla _{\sigma_i} \mathcal{L} _{i} (p, q_i) \\\\\\
\end{align}
$$

where we assume that
$$
q_i(z) = \mathcal{N}(\mu_i, \sigma_i)
$$
