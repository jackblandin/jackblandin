+++
title = "Extrapolating Beyond Suboptimal Demonstrations via Inverse Reinforcement Learning from Observations"
date = 2020-03-21T11:25:13-05:00
draft = false
categories = ["Imitation Learning", "Inverse Reinforcement Learning", "Risk"]
summary = "Uses ranked demonstrations to learn a state-based reward function that assigns greater total return to higher-ranked trajectories."
+++

### Source

[Brown, Daniel S., et al. "Extrapolating beyond suboptimal demonstrations via inverse reinforcement learning from observations." arXiv preprint arXiv:1904.06387 (2019).](https://arxiv.org/abs/1904.06387)

### Key Points

* Attempts to resolve the IL problem where the agent cannot significantly outperform the demonstrator.
* Introduces Trajectory-ranked Reward Extrapolation (T-REX).
* Uses ranked demonstrations to learn a state-based reward function that assigns greater total return to higher-ranked trajectories.
* Identifies features that are correlated with rankings, in a manner that can be extrapolated beyond the demonstrations.

### Reason for reading

My reason for reading this paper was to identify its relation to a risk-based IL approach. This relates to risk-based IL because we can rank demonstrations based on their "riskiness". TREX could learn to prioritize low-risk trajectories, thereby learning to optimize both the task reward as well as risk reward. One potential concern is that TREX may not truly "learn" the reward for riskiness on its own. This should be verified in a more thorough review of the TREX algorithm.
