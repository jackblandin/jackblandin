+++
title = "Learning Deep Robot Controllers by Exploiting Successful and Failed Executions"
date = 2020-03-24T13:29:03-04:00
draft = false
categories = ["Reinforcement Learning"]
summary = "TODO"
+++

### Source
[Esteban, Domingo, Leonel Rozo, and Darwin G. Caldwell. "Learning Deep Robot Controllers by Exploiting Successful and Failed Executions." 2018 IEEE-RAS 18th International Conference on Humanoid Robots (Humanoids). IEEE, 2018.](https://www.researchgate.net/profile/Leonel_Rozo/publication/328477796_Learning_Deep_Robot_Controllers_by_Exploiting_Successful_and_Failed_Executions/links/5bd050254585152b14515e92/Learning-Deep-Robot-Controllers-by-Exploiting-Successful-and-Failed-Executions.pdf)

### Overview
- Guided Policy Search (GPS) methods update the robot policy discarding or giving low probability to unsuccessful trials. In other words, these methods overlook the existence of poorly performing executions, and therefore tend to underestimate the information of these interactions in next iterations.
- In this paper we propose to learn deep neural network controllers with an extension of GPS that considers trajectories optimized with dualist constraints. These constraints are aimed at assisting the policy learning so that the trajectory distributions updated at each iteration are similar to good trajectory distributions (e.g., successful executions) while differing from bad trajectory distributions (e.g. failures).

### Referenced Papers
[Guided Policy Search]({{<ref "/paper_summaries/levine-koltun-2013.md">}})
