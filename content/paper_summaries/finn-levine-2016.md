+++
title = "Guided Cost Learning"
date = 2020-03-24T14:13:44-04:00
draft = false
categories = ["Imitation Learning", "Inverse Reinforcement Learning"]
summary = "Presents an algorithm (GCL) that is capable of learning arbitrary nonlinear cost functions. GCL also improves scalability to high-dimensional state and action spaces using an iterative (importance) sample-based method for estimating partition function Z in the MaxEntIRL formulation."
+++

### Source
[Finn, Chelsea, Sergey Levine, and Pieter Abbeel. "Guided cost learning: Deep inverse optimal control via policy optimization." International conference on machine learning. 2016.](http://www.jmlr.org/proceedings/papers/v48/finn16.pdf)

### First Pass
Addreses two key challenges in IRL:
1. The need for informative features and effective regularization to impose structure on the cost.
1. The difficulty of learning the cost function under unknown dynamics for high-dimensional continuous systems.

To address challenge (1):
- Present an algorithm capable of learning arbitrary nonlinear cost functions, such as neural
networks, without meticulous feature engineering.

To address challenge (2):
- Introduces an iterative sample-based method for estimating _Z_ in the `MaxEntIRL` formulation.
- Estimates _Z_ by training a new sampling distribution _q(&#x3C4;)_ and using importance sampling.
- GCL alternates between optimizing cost (reward) function using estimate _q(&#x3C4;),_ and optimizing _q(&#x3C4;)_ to minimize the variance of the importance sampling estimate.
- Can scale to high-dim state and action spaces and nonlinear cost functions.
