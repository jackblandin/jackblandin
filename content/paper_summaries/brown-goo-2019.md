+++
title = "Extrapolating Beyond Suboptimal Demonstrations via Inverse Reinforcement Learning from Observations"
date = 2020-03-21T11:25:13-05:00
draft = false
summary = "Uses ranked demonstrations to learn a state-based reward function that assigns greater total return to higher-ranked trajectories."

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ["IL", "IRL"]
categories = []

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = ""
+++

[Daniel S. Brown, Wonjoon Goo, Prabhat Nagarajan, Scott Niekum](https://arxiv.org/abs/1904.06387), UT-Austin Submitted 2019/04/12

Overview
* Trajectory-ranked Reward Extrapolation (T-REX).
* Uses ranked demonstrations to learn a state-based reward function that assigns greater total return to higher-ranked trajectories.
* Allows us to identify features that are correlated with rankings, in a manner that can be extrapolated beyond the demonstrations.
