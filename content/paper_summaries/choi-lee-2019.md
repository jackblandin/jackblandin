+++
title = "Robust Learning From Demonstrations With Mixed Qualities Using Leveraged Gaussian Processes"
date = 2020-03-22T14:42:37-04:00
draft = false
summary="Learning from demonstrations with different unlabeled expert proficiencies, allowing for suboptimal demonstrations."
categories = ["Imitation Learning"]
+++

### Summary
Learning from demonstrations with different unlabeled expert proficiencies, allowing for suboptimal demonstrations.

### Source

[Choi, Sungjoon, Kyungjae Lee, and Songhwai Oh. "Robust learning from demonstrations with mixed qualities using leveraged gaussian processes." IEEE Transactions on Robotics 35, no. 3 (2019): 564-576.](http://rllab.snu.ac.kr/publications/papers/2016_icra_levopt.pdf)

### Reason for reading

My reason for reading this paper was to identify its relation to a risk-based IL approach. Similar to [Learning good policies from suboptimal demonstrations]({{<ref "/paper_summaries/li-kash-hofmann-2018.md">}}), this paper only focuses on learning from suboptimal demonstrations with respect to the task-reward function. Therefore, this paper does not appear useful in determining a risk-based IL approach.

### Key Points

* Learning from demonstrations with different unlabeled expert proficiencies, allowing for suboptimal demonstrations.
* Propose a novel method for robust learning from demonstration using leveraged Gaussian process regression.
* Model multiple sources of demonstrations, i.e., demonstrations from *experts* and *novices*, as a correlated random process under the leveraged Gaussian process framework.
* Present a sparse constrained leverage optimization method by combining a model selection method for Gaussian process regression with proximal linearized minimization, a sparse optimization method.
* Proposed to use a Gaussian process to represent a non-linear reward function whose kernel moved the prediction close to positive samples and drift it away from negative ones.

### Limitations
* Robust to small numbers of unlabeled suboptimal demonstrations, but require a majority of expert demonstrations in order to correctly identify which demonstrations are anomalous (Brown, Goo 2019).
