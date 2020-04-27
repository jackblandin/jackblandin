+++
title = "Pedestrian Collision Avoidance Systems for Scenarios with Occlusions"
date = 2019-05-26T14:17:38-05:00
draft = false
summary = "Minimize AEB system usage by using POMDP policy to adjust driving pattern based on possible road occlusions."
categories = ["POMDPs"]
+++

### Summary
Minimize AEB system usage by using POMDP policy to adjust driving pattern based on possible road occlusions.

[Schratter, et al](https://arxiv.org/abs/1904.11566) \\
Submitted 2019/04/25

### Objective
Determine if a POMDP policy can be learned to reduce unnecessary braking from pure AEB systems.

### Problem formulation
POMDP with pedestrian location as the MDP space distribution.

### Conclusions
Combining AEB with pedestrian location POMDP policy reduces unnecessary braking without increasing the accident rate.
