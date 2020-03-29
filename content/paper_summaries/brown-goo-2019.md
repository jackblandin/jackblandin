+++
title = "Extrapolating Beyond Suboptimal Demonstrations via Inverse Reinforcement Learning from Observations"
date = 2020-03-21T11:25:13-05:00
draft = false
categories = ["Imitation Learning", "Inverse Reinforcement Learning", "Risk"]
summary = "While standard IRL approaches seek a reward function that justifies the demonstrations, TREX instead seeks a reward that explains the _ranking_ over demonstrations, allowing for potentially better-than-demonstrator performance."
math = true
+++

### Source

[Brown, Daniel S., et al. "Extrapolating beyond suboptimal demonstrations via inverse reinforcement learning from observations." arXiv preprint arXiv:1904.06387 (2019).](https://arxiv.org/abs/1904.06387)

### Key Points

* Attempts to resolve the IL problem where the agent cannot significantly outperform the demonstrator.
* Introduces _IRL_ algorithm Trajectory-ranked Reward Extrapolation (T-REX).
    * Extrapolates user's underling intent beyond the best demonstration, even when all demonstrations are highly suboptimal.
    * Uses ranked demonstrations to learn a state-based reward function that assigns greater total return to higher-ranked trajectories.
    * Identifies features that are correlated with rankings, in a manner that can be extrapolated beyond the demonstrations.
* While standard IRL approaches seek a reward function that justifies the demonstrations, TREX instead seeks a reward that explains the _ranking_ over demonstrations, allowing for potentially better-than-demonstrator performance.
* Pairwise ranking of trajectories worksk as an effective regularizer that eliminates degenerate solutions, as well as prevents overfitting to the small fraction of state space visited by the demonstrator.
* Uses deep learning and ranked demonstrations to automatically learn complex features.
* Our work can be seen as a form of preference-based policy learning and preference-based IRL (PBIRL) which both seek to optimize a policy based on preference rankings over demonstrations.
* Experiments rank demonstrations based on ground-truth rewards (not a human).

### Problem Definition
* Given a sequence of _m_ ranked trajectories $\tau_t$ for $t=1,...,m$ where $\tau_i \prec  \tau_j$ if $i<j$.we wish to find aparameterized reward function $\hat{r}_\theta$ that approximates the true reward function $r$ that the demonstrator is attempting to optimize.
* Given $\hat{r}_\theta$, we then week to optimize a policy $\hat{\pi}$ that can outperform the demonstrations.
* Only assume access to a qualitateive ranking over demonstrations.

### Algorithm

Given a sequence of $m$ demonstrations ranked from worst to best, $\tau_1, ..., \tau_m$, T-REX has two steps:
1. Reward inference
2. Policy optimization

Given the ranked demonstrations, T-REX performs reward inference by approximating the reward at state $s$ using a neural network, $\hat{r}_\theta(s)$, such that  $\sum _{s \in \tau_i} \hat{r} _\theta (s) < \sum _{s \in \tau_j} \hat{r} _\theta(s)$ when $\tau_i \prec \tau_j$.

The parameterized reward function $\hat{r}_\theta$ is trained with the following loss function
$$
\mathcal{L}(\theta) = E _{\tau_i, \tau_j \sim \Pi}[\xi(P(\hat{J} _\theta(\tau_i) < \hat{J} _\theta(\tau_j)), \tau_i \prec \tau_j)]
$$
where
* $\Pi$ is a distribution over demonstrations
* $\xi$ is a binary classification loss function
* $\hat{J}$ is a return defined by a parameterized reward function $\hat{r}_\theta$
The probability $P$ is represented as a softmax-normalized distribution and $\xi$ is instantiated using a cross entropy loss:

![Example image](/img/paper_summaries/brown-goo-2019/eq2-3.png)


### Critiques
* Trained GAIL with all of the demonstrations. I thought they should only have trained from the best expert demonstrations.

### Reason for reading

My reason for reading this paper was to identify its relation to a risk-based IL approach. This relates to risk-based IL because we can rank demonstrations based on their "riskiness". TREX could learn to prioritize low-risk trajectories, thereby learning to optimize both the task reward as well as risk reward. One potential concern is that TREX may not truly "learn" the reward for riskiness on its own. This should be verified in a more thorough review of the TREX algorithm.
