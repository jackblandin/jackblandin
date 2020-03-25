+++
title = "Learning good policies from suboptimal demonstrations"
date = 2020-03-22T14:29:03-04:00
draft = false
summary = "Shows how DQfD, which bootstraps RL algorithms with policies from demonstrations, does not perform well when demonstrations are suboptimal."

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []
categories = ["Imitation Learning", "Inverse Reinforcement Learning", "Reinforcement Learning"]

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

[Li, Yuxiang, Ian Kash, and Katja Hofmann. "Learning good policies from suboptimal demonstrations." In 14th European Workshop on Reinforcement Learning (EWRL 2018), vol. 2. 2018.](https://pdfs.semanticscholar.org/754c/dcf031965d49c7b7470db35ff26e4d309da0.pdf)

### Authors
Name | Affiliation
--- | ---
Yuxiang Li | Google, London, UK ; Microsoft Research, Cambridge, UK
Ian A. Kash | University of Illinois at Chicago ; Microsoft Research, Cambridge, UK
Katja Hofmann | Microsoft Research, Cambridge, UK

### Key Points

* Shows how DQfD, which bootstraps RL algorithms with policies from demonstrations, does not perform well when demonstrations are suboptimal.
* Best agents are not necessarily the best teachers.
* Demonstrates the potential to overcome this issue through a performance comparison between learner and demonstrator.
* Explore adapting the weight of the demonstration on a global basis.

### Reason for reading

My reason for reading this paper was to identify its relation to a risk-based IL approach. This paper is concerned primarily with learning from suboptimal demonstrations, where _suboptimal_ refers to the task-based reward function. In order for this to be applicable to risk-based IL, the risk score would need to be built into the reward function, which defeats the purpose. Therefore, I don't think that this paper is worth exploring further for its relation to risk-based RL. Instead, we should look for IL papers that indicate a way of optimizing an additional reward that is not manifested in the observed demonstrations.
