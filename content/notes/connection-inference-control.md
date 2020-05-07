+++
title = "Connection between Probabilistic Inference and Optimal Control"
date = 2020-05-04T11:27:03-05:00
draft = false
tags = []
categories = ["Reinforcement Learning", "Probability Theory"]
math="true"
summary = "CS294 L15 -- Show how Optimal Control, Reinforcement Learning, and Planning algorithms can be derived using Probabilistic Inference."
+++

This is a written summary of Lecture 15 of UC Berkeley's Fall 2018 [CS294-112 Deep Reinforcement Learning](http://rail.eecs.berkeley.edu/deeprlcourse-fa18/) course taught by Sergey Levine. Here are links to the lecture content:
* [Lecture recording](https://www.youtube.com/watch?v=oqvTC1rTjg8&list=PLkFD6_40KJIxJMR-j5A1mkxK26gh_qg37&index=11)
* [Slides](http://rail.eecs.berkeley.edu/deeprlcourse-fa18/static/slides/lec-15.pdf)

## Summary

The purpose of this lecture is to show how Optimal Control, Reinforcement Learning, and Planning algorithms can be derived using Probabilistic Inference.

The goals are to
* Understand the connection between Inference and Control
* Understand how RL algorithms can be implemented in a probabilistic framework
* Understand the advantages of viewing RL from a probabilistic perspective.

In the next lecture (16), we will see how probabilistic algorithms are crucial for *Inverse* Reinforcement Learning.

###### Inference = Planning

In this framework, planning and taking actions amounts to solving an inference problem.

> Inference means you have evidence and you want to maximize teh posterior probability of some other variables, given that evidence.

###### Probabilistic Graphical Model of Decision Making
![Probabilistic Graphical Model of Decision Making](/img/notes/connection-inference-control/graphical-model.png)

###### Evidence Variables
* $\mathcal{O}$ are evidence variables.
* They are observations of optimality.
* Evidence variables are "what you know."
* If agent is optimal, then all $\mathcal{O}$ are true.

###### How to do inference
1. Compute *backward messages* $\beta_t (s_t, a_t) = p(\mathcal{O} _{t:T} | s_t, a_t)$
2. Compute policy $p(a_t | s_t, \mathcal{O} _{1:T})$
3. Compute *forward messages* $\alpha_t (s_t) = p(s_t | \mathcal{O} _{1:t-1})$

###### Backward messages
* Answers the question - *what is the probability that we can be optimal from now until $T$, given that we take action $a_t$ in state $s_t$?*
* States/actions where you can continue to be optimal in the future

###### Forward messages
* Answers the question - *what is the probability that you will land in state $s_t$ if you have been optimal up until this point?*
* States/actions where you could have been optimal in the past

###### Backward/Forward message intuition
Backward messages indicate the ability to be optimal in the future, given the next action that you take for a given state. Therefore, we can obtain policies from backward messages alone. However, when considering inferring a reward from expert demonstrations,Backward messages alone can't articulate the most likely reward function because they may have state/actions that are not "reachable" from an optimal perspective.For example, consider a car that is on the freeway facing the wrong way. The optimal action in this state is to floor it in reverse. The state action pair for flooring it in reverse when backwards on the highway would be present in the backward messages. However, an optimal driver never would have gotten themselves into this state (forward messages), so we shouldn't consider this state/action pair in our policy. Therefore, in order to get a good policy, we need to take the *intersection* of both the Backward and Forward messages.

###### Computing Backward Messages
This is very similar to computing HMMs.
\begin{align}
&\text{for } t = T - 1 \text{ to } 1: \\\\\\
&\text{ } \hspace{1cm} \beta_t(s_t, a_t) = p(\mathcal{O} _t | s_t, a_t) E _{s _{t+1} \sim p(s _{t+1}|s_t, a_t)} \left[ \beta _{t+1}(s _{t+1}) \right] \\\\\\
&\text{ } \hspace{1cm} \beta_t(s_t) = E _{a_t \sim p(a_t|s_t)} \left[ \beta_t(s_t, a_t) \right]
\end{align}

###### Connecting Backward Message computation to RL
With a couple slight changes, we can make computing the backward messages look like RL (Value Iteration):

\begin{align}
&\text{let } V_t(s_t) = \log \beta_t(s_t) \\\\\\
&\text{let } Q_t(s_t, a_t) = \beta_t(s_t, a_t)
\end{align}

\begin{align}
&V_t(s_t) = \log \int \exp(Q_t(s_t, a_t))da_t \\\\\\
&V_t(s_t) \rightarrow \max _{a_t} Q_t(s_t,a_t) \text{ as } Q_t(s_t,a_t) \text{ gets bigger!}
\end{align}

When you exponentiate a bunch of numbers and sum them up, the sum will be dominated by the largest value. Then, when you take the log of the sum, it will be dominated by the max as well. Therefore, taking the log of an exponentiated sum os often referred to as a "soft-max"[^1].
[^1]: This should not be confused with the soft-max loss function used in deep learning.

Similarly, we have

$$
Q_t(s_t, a_t) = r(s_t, a_t) + \log E \left[ \exp \left( V _{t+1} (s _{t+1}) \right) \right] \tag{1}
$$
The above equations now look very similar to Value Iteration:
1. set $Q(s,a) \leftarrow r(s,a) + \gamma E[V(s')]$
1. set $V(s) \leftarrow \max _{a} Q(s, a)$

For deterministic transitions, the expectation in Equation 1 will simplify neatly since the log of an exponential just cancels:
$$
Q_t(s_t, a_t) = r(s_t, a_t) + V _{t+1}(s _{t+1})
$$

So for deterministic transitions, the only difference between this and Value Iteration is that we are using a soft-max instead of a hard-max.

For stochastic transitions, we see an *optimistic expectation*:
![Optimistic transitions](/img/notes/connection-inference-control/optimistic-transition.png)

Unlike Value Iteration, the 2nd term in Equation 1 will be an overly optimistic expectation. Value Iteration takes the expectation w.r.t. all potential next states proportional to their probablity, whereas Equation 1 will take the expectation with the best next state heavily weighted (especially for large Q values).

This makes intuitive sense when we think about what our inference problem is asking -- "*What is the probability of states and actions, given that the agent was optimal?*" The agent could have stumbled onto $100 lying on the sidewalk, which is optimal, but it doesn't mean that the agent had a good policy. In other words,

> this framework doesn't distinguish between deliberately good decisions vs. lucky ones.

This becomes a problem when we're trying to infer policies, which we will get back to later.

###### Deriving a policy

We can derive a policy as

\begin{align}
p(a_t|s_t, \mathcal{O} _{t:T}) &= \pi(a_t|s_t) \\\\\\
&= p(a_t|s_t, \mathcal{O} _{t:T}) \\\\\\
&= \frac{p(a_t, s_t|\mathcal{O} _{t:T})}{p(s_t|\mathcal{O} _{t:T})} \\\\\\
&= \frac{p(\mathcal{O} _{t:T}|a_t,s_t)p(a_t, s_t)/p(\mathcal{O} _{t:T})}{p(\mathcal{O} _{t:T}|s_t)p(s_t)/p(\mathcal{O} _{t:T})} \\\\\\
&= \frac{p(\mathcal{O} _{t:T}|a_t,s_t)}{p(\mathcal{O} _{t:T}|s_t)}\frac{p(a_t,s_t)}{p(s_t)} = \frac{\beta_t(s_t, a_t)}{\beta_t(s_t)}p(a_t|s_t) \\\\\\
\end{align}
so if we assume a uniform action prior, then
$$
\pi(a_t|s_t) = \frac{\beta_t(s_t,a_t)}{\beta_t(s_t)}
$$

###### Deriving a policy with value functions

![Deriving policy using value functions](/img/notes/connection-inference-control/policy-value-functions.png)

Policy is hte exponential of the Advantage function. This makes intuitive sense, as we are more likely to take actions with high advantage.

###### Forward messages
![](/img/notes/connection-inference-control/forward-messages.png)

###### Forward/backward message intersection
But what if we want the probability of a state f, given that the entire trajectory being is optimal $p(s_t| \mathcal{O} _{1:T})$

![](/img/notes/connection-inference-control/trajectory-optimality.png)
![](/img/notes/connection-inference-control/intersection.png)


