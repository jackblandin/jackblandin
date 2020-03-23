+++
title = "IPOMDP Lite: Towards Practical Planning to Predict and Exploit Intentions for Interacting with Self-Interested Agents"
date = 2019-06-15T11:25:13-05:00
draft = false
summary = "Investigate how intention prediction can be efficiently exploited and made practical in planning, leading to efficient intention-awareplanning frameworks."

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
categories = ["IPOMDP", "POMDP", "Practical AI", "Autonomous Vehicles"]
tags = []

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = ""
+++

[Hoang, et al](https://arxiv.org/abs/1304.5159), Submitted 2013/04/18

### Problems with existing attempts

Existing attempts at planning for Multi Agent Environments (MAEs) is being undermined due to either

* the restrictive assumptions of the other agents' behavior
* the failure in accounting for their rationality
* the prohibitively expensive cost of modeling and predicting their intensions.

### Objective

Investigate how intention prediction can be efficiently exploited and made practical in planning, leading to efficient intention-awareplanning frameworks.

### Results

Performance losses from new planning policies are linearly bounded by the error of intention prediction. New policies achieve better and more robust performance than existing algorithms.
