+++
title = "Pedestrian Collision Avoidance Systems for Scenarios with Occlusions"
date = 2019-05-26T14:17:38-05:00
draft = false
summary = "Minimize AEB system usage by using POMDP policy to adjust driving pattern based on possible road occlusions."

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []
categories = ["POMDPs", "Autonomous vehicles"]

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = ""
+++

### Objective
Determine if a POMDP policy can be learned to reduce unnecessary braking from pure AEB systems.

### Problem formulation
POMDP with pedestrian location as the MDP space distribution.

### Conclusions
Combining AEB with pedestrian location POMDP policy reduces unnecessary braking without increasing the accident rate.
