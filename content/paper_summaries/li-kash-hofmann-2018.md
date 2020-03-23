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

### Summary

* Shows how DQfD, which bootstraps RL algorithms with policies from demonstrations, does not perform well when demonstrations are suboptimal.
* Potential area of future research: Investigate potential of adaptively trading off between RL and IL.
* Best agents are not necessarily the best teachers.
