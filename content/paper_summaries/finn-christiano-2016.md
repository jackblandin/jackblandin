+++
title = "A Connection Between Generative Adversarial Networks, Inverse Reinforcement Learning, and Energy-Based Models"
date = 2020-03-23T15:08:31-04:00
draft = false
categories = ["Inverse Reinforcement Learning", "Generative Adversarial Networks"]
summary="Certain IRL methods are in fact mathematically equivalent to GANs. Discuss the interpretation of GANs as an algorithm for training energy-based models."
+++

### Source
[Finn, Chelsea, et al. "A connection between generative adversarial networks, inverse reinforcement learning, and energy-based models." arXiv preprint arXiv:1611.03852 (2016).](https://arxiv.org/pdf/1611.03852.pdf)

### Authors
Name | Affiliation
--- | ---
Chelsea Finn | University of California, Berkeley
Paul Christiano | University of California, Berkeley
Peter Abbeel | University of California, Berkeley
Sergey Levine | University of California, Berkeley

### Summary

* Show that the gradient updates for the cost and the policy in [MaxEntIRL] methods can be viewed as the updates for the discriminator and generator in GANs, under a specific form of the discriminator where the generator density can be evaluated.
* GAN training can significantly improve the quality of samples even when the generator density can[not] be exactly evaluated. This is analogous to the ability of IRL to imitate behaviors that cannot be successfully learned through behavioral cloning which attempts direct maximum likelihood regression to the demonstrated behavior.
* MaxEntIRL is a special case of an energy-based model (EBM). The learned cost in MaxEntIRL corresponds to the energy function, and is trained via maximum likelihood. Authors show how a particular form of GANs can be used to train EBMs.
