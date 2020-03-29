+++
title = "Learning Deep Robot Controllers by Exploiting Successful and Failed Executions"
date = 2020-03-24T13:29:03-04:00
draft = false
categories = ["Reinforcement Learning"]
summary = "TODO"
+++

### Source
[Esteban, Domingo, Leonel Rozo, and Darwin G. Caldwell. "Learning Deep Robot Controllers by Exploiting Successful and Failed Executions." 2018 IEEE-RAS 18th International Conference on Humanoid Robots (Humanoids). IEEE, 2018.](https://www.researchgate.net/profile/Leonel_Rozo/publication/328477796_Learning_Deep_Robot_Controllers_by_Exploiting_Successful_and_Failed_Executions/links/5bd050254585152b14515e92/Learning-Deep-Robot-Controllers-by-Exploiting-Successful-and-Failed-Executions.pdf)

### Reason for reading

My reason for reading this paper was to identify its relation to a risk-based IL approach.

### First Pass
- Guided Policy Search (GPS) methods update the robot policy discarding or giving low probability to unsuccessful trials. In other words, these methods overlook the existence of poorly performing executions, and therefore tend to underestimate the information of these interactions in next iterations.
- In this paper we propose to learn deep neural network controllers with an extension of GPS that considers trajectories optimized with dualist constraints. These constraints are aimed at assisting the policy learning so that the trajectory distributions updated at each iteration are similar to good trajectory distributions (e.g., successful executions) while differing from bad trajectory distributions (e.g. failures).

### Second Pass
* The problem of ignoring failed demonstrations can be even more critical in GPS, because the guiding policies seek to minimize an expected surrogatecost that includes not only the task-related cost, but also a term that encourages the guiding policies to resemble the complex global policy.
* Propose a model-based trajectory-centricRL algorithm that explicitly exploits good and bad experi-ences in the policy optimization step.
* Good and bad trajectory distributions are defined and updated by samples with low and high cost, respectively.
* These trajectories are explicitly considered in the policy update by including the following dualist constraints:
    * _upper_ bounds on the KL divergences between new and _good_ trajectory distributions
    * _lower_ bounds on the KL divergences between new and _bad_ trajectory distributions
* The proposed framework was evaluated in two reaching tasks with collision avoidance. Both a simulated 3-DoF planar manipulator and a simulated humanoid robot were required to reach a desired Cartesian pose while avoiding an obstacle that is halfway.

### Referenced Papers
[Guided Policy Search]({{<ref "/paper_summaries/levine-koltun-2013.md">}})
