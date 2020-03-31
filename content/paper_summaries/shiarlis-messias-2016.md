+++
title = "Inverse Reinforcement Learning from Failure"
date = 2020-03-22T14:50:12-04:00
draft = false
summary = "Learns from both successful and failed demonstrations."
math = true
categories = ["Imitation Learning", "Inverse Reinforcement Learning"]

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = ""
+++

### Source

[Shiarlis, Kyriacos, Joao Messias, and S. A. Whiteson. "Inverse reinforcement learning from failure." (2016).](https://ora.ox.ac.uk/objects/uuid:8593deb6-f16c-4545-a732-472625eaffb3/download_file?file_format=pdf&safe_filename=LearningFromFailure.pdf&type_of_work=Conference)

### Authors
Name | Affiliation
--- | ---
Kyriacos Shiarlis | Informatics Institute University of Amsterdam
Joao Messias | Informatics Institute University of Amsterdam
Shimon Whiteson | Dept. of Computer Science University of Oxford

### Summary

- First attempt at making use of failed demonstrations in IRL.
- Proposes IRL from Failure (IRLF) that exploits both successful and failed demonstrations.
- Use MaxCausalEntIRL as a base, and propose a new constrained optimization formulation that accommodates both successful and failed demonstrations while remaining convex.
- Evaluate algorithm using social navigation for a mobile robot, as well as the _Factory_ problem.

### Method
*  Apprentice has access to a dataset $\mathcal{F}$ failed demonstrations, in addition to a dataset $\mathcal{D}$ of successful ones.
A challenge with learning from failed demonstrations is that it is not obvious how to interpret failed demonstrations. A failed trajectory is ambiguous because it is unclear what about it is wrong:
    * Was the entire trajectory incorrect?
    * Was only it almost correct but wrong for a specific feature?

The authors propse a constrained optimization problem similar to MaxCausalEntIRL, but with an additional constraint:

MaxCausalEntIRL | IRLF
--- | ---
![MaxCausalEntIRL COP](/img/paper_summaries/shiarlis-messias-2016/maxcausalent-cop.png) | ![IRLF COP](/img/paper_summaries/shiarlis-messias-2016/irlf-cop.png)

* The first term in the objective seeks to maximize $\pi$'s causal entropy.
* The second term balances the first term by maximizing dissimilarity between $\pi$'s feature expectations and the empirical expectations in $\mathcal{F}$.
* The third term regularises to discourage large values of $\theta$.

The main advantage of this formulation is that $\pi$ and $\theta$ become decoupled for a given $z$, making maximization of the Lagrangian feasible while preserving convexity. In other words, $\theta$ and $\mu$ never appear together in the same term in the new Lagrangian:

![IRLF COP](/img/paper_summaries/shiarlis-messias-2016/new-lagrangian.png)

The final reward function becomes

$$
\sum _{k=1} ^K (w _k ^D + {w} ^{\mathcal{F}}) \phi _k (s, a)
$$

This shows that the value of $\pi$ now depends on _both_ Lagrangian multipliers $w^\mathcal{D}$ and $w^\mathcal{F}$.


A key characteristic of IRLF is that it also handles cases where the failed trajectories are only ‘partial’ failures, i.e., when the failed trajectories are similar to the successful ones with respect to some features and dissimilar with respect to others. E.g. if a feature $\mu _1 ^\mathcal{D}$ is similar to $\mu _1 ^\mathcal{F}$, it might seem that the updates to $w _k ^\mathcal{D}$ and $w _k ^\mathcal{F}$ would cancel each other out, which would prevent IRLF from imitating successful trajectories with respect to that feature. This is not the case, however.

Another useful characterstic of IRL is that it does not require the feature sets for successful and failed demonstrations to be the same.

![IRLF COP](/img/paper_summaries/shiarlis-messias-2016/irlf-algorithm.png)
