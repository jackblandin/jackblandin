+++
title = "Extrapolating Beyond Suboptimal Demonstrations via Inverse Reinforcement Learning from Observations"
date = 2020-03-21T11:25:13-05:00
draft = false
categories = ["Imitation Learning", "Inverse Reinforcement Learning", "Risk"]
summary = "Uses ranked demonstrations to learn a state-based reward function that assigns greater total return to higher-ranked trajectories."
+++

### Source

[Brown, Daniel S., et al. "Extrapolating beyond suboptimal demonstrations via inverse reinforcement learning from observations." arXiv preprint arXiv:1904.06387 (2019).](https://arxiv.org/abs/1904.06387) \\

### Overview

* Trajectory-ranked Reward Extrapolation (T-REX).
* Uses ranked demonstrations to learn a state-based reward function that assigns greater total return to higher-ranked trajectories.
* Allows us to identify features that are correlated with rankings, in a manner that can be extrapolated beyond the demonstrations.
