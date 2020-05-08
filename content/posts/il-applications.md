+++
title = "Real-world Applications of Imitation Learning"
date = 2020-05-07T13:57:28-05:00
draft = false
tags = []
categories = ["Imitation Learning"]
summary="In this post, I enumerate potential real-world applications of Imitation Learning and Inverse Reinforcement Learning."
+++

In this post, I enumerate potential real-world applications of Imitation Learning (IL) and Inverse Reinforcement Learning (IRL).

### Use-case 1: Learning when reward cannot be specified

Sometimes, it is easier to get access to an expert demonstrating a task than it is to define the reward. This may be because the reward is too complex to encode. Alternatlivey, it might be because the reward is too specific for a task, and isn't worth a robot manufacturer spending the time to encode it. Consider Roombas, for example. Roombas come with a base policy that randomly changes directions in order to work in all home environments. However, we could improve this policy for the Roomba if we enabled the Roomba to learn from a human demonstrating the correct path to take for a particular household.

The key point here is that robots are not adept at generalizing their knowledge to unseen environments, but humans are. Therefore, we can leverage this ability in humans by enabling them to correct robots' behavior through demonstration. A related topic is improving policies through constructive feedback.

**Criteria for this use-case**

* Reward is too difficult to specify.
* Demonstrator is easily available.
* Demonstrator is willing to demonstrate enough trajectories to cover the state space expected by the agent.
* Embodiments are similar between demonstrator and learner.

**Examples**

* Factory workers teaching robots to do complex assembly tasks.
* Household chores
    * Load the dishwasher.
    * Set the table.
    * Put dishes away in cabinets.

**Relevant papers**

* [Knox, W. B., & Stone, P. (2009, September). Interactively shaping agents via human reinforcement: The TAMER framework. In Proceedings of the fifth international conference on Knowledge capture (pp. 9-16).](https://dl.acm.org/doi/abs/10.1145/1597735.1597738)

### Use-case 2: Bootstrapping RL agents with strong prior policies

For situations where a simulator is not provided, and an RL agent needs to learn from real-world exploration, starting to learn a policy from scratch is not likely to be a safe. If the agent learns a policy from an expert demonstration, then the agent will learn a safe baseline policy.

Similarly, learning any RL policy from scratch is an inefficient process. Bootstrapping the agent with a policy learned from demonstrations can serve as a catalyst the learning process. This is true regardless of whether or not a simulator is provided.

**Criteria for this use-case**

* Sample inefficiency is a problem, such as in long-horizon planning problems where the reward is sparse.

### Use-case 3: Forecasting future behavior

If we can learn to imitate an agent, be it human or robot, then we can predict its behavior in any particular setting. For example, suppose we used security videos to learn to imitate human movement preferences in a shopping mall. We could then simulate a scenario where there was a fire or an active shooter, and we could determine where people would try to go or exit the building. This could expose safety issues with the mall, and could enable the mall to take proactive steps to reduce safety hazards at a low cost.

**Criteria for this use-case**

* Observations of agents to forecast are available.
* Changes to environment can be simulated.

**Examples**

* Any multiplayer game, e.g. chess, Atari. This works for cooperative or adversarial games.

**Relevant papers**

* [Kitani, K. M., Ziebart, B. D., Bagnell, J. A., & Hebert, M. (2012, October). Activity forecasting. In European Conference on Computer Vision (pp. 201-214). Springer, Berlin, Heidelberg.](http://courses.cs.washington.edu/courses/cse590v/13au/Activity%20Forecasting.pdf)

### Use-case 4: Robotic teleoperation

In situations where a human is remotely operating a robot, it can be difficult for the human to control the robot. This is particularly true when the action spaces are different for the human and robot. It would be better if the robot could *infer* what the human operator was trying to do, and then correct any mistakes that the human made.

**Relevant papers**

* [Chen, Xiangli, et al. "Adversarial Inverse Optimal Control for General Imitation Learning Losses and Embodiment Transfer." UAI. 2016.](http://www.auai.org/uai2016/proceedings/papers/106.pdf)
