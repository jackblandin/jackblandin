+++
title = "Connecting Generative Adversarial Networks and Actor-Critic Methods"
date = 2020-03-24T13:37:11-04:00
draft = false
categories = ["Reinforcement Learning"]
tags = ["Generative Adversarial Networks"]
summary = "Highlights connection between GANs and AC as bilevel optimization problems, where one model is optimized with respect to the optimum of another model. By highlighting the connection, the authors aim to encourage collaboration across communities."
+++

### Source
[Pfau, David, and Oriol Vinyals. "Connecting generative adversarial networks and actor-critic methods." arXiv preprint arXiv:1610.01945 (2016).](https://arxiv.org/abs/1610.01945)

### Overview
-  A number of problems in machine learning lack a single unified cost, and instead consist of a hybrid of several models, each of which passes information to other models but tries to minimize its own private loss function. This upsets many of the assumptions behind most learning algorithms, and applying ordinary methods like gradient descent often leads to pathological behavior such as oscillations or collapse onto degenerate solutions.
- Both GANs and AC can be seen as bilevel or two-time-scale optimization problems, where one
model is optimized with respect to the optimum of another model
- Actor-critic methods (AC) [2, 3] and generative adversarial networks (GANs) [4] are two such classes of multilevel optimization problems which have close parallels.
- Both of these models suffer from stability issues, and techniques for stabilizing training have been developed largely independently by the two communities.
- While most RL algorithms either focus on learning a value function, like value iteration and TD-learning, or learning a policy directly, as in policy gradient methods, AC methods learn both simultaneously - the actor being the policy and the critic being the value function.
