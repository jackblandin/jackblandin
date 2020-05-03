+++
title = "Adversarial Inverse Optimal Control for General Imitation Learning Losses and Embodiment Transfer"
date = 2020-04-20T15:43:55-04:00
draft = false
summary = "TODO"
categories = ["Imitation Learning"]
math = true
+++

### Summary

### Source
[Chen, Xiangli, et al. "Adversarial Inverse Optimal Control for General Imitation Learning Losses and Embodiment Transfer." UAI. 2016.](http://www.auai.org/uai2016/proceedings/papers/106.pdf)

### Key References
[Grünwald, Peter D., and A. Philip Dawid. "Game theory, maximum entropy, minimum discrepancy and robust Bayesian decision theory." the Annals of Statistics 32.4 (2004): 1367-1433.](https://projecteuclid.org/euclid.aos/1091626173)

### Open Questions
* How is the min-max different than the one used in MMP?
* How does the adversary "approximate" the demonstrated examples?

### Key Points

* Develop an IOC framework that distinguishes between rationalizing demonstrated behavior and imitating inductively inferred behavior. I.e. since the learner may not have the same state/action space as the demonstrator, _inferring_ the demonstrator's behavior should not necessarily be intertwined with _imitating_ the demonstrator.
* Formulation takes the form of a zero-sum game between
    * A learner seeking a policy to minimize imitative loss.
    * An adversary seeking a control policy that adequately characterizes the demonstrations, but maximizes the learner's loss. The adversary "approximates" the demonstrated examples in limited ways.
* Key philosophy of their approach is that unknown properties of _how the demonstrator would behave in new situations_ should be treated as pessimistically as possible, since any unwarranted assumptions could lead to substancial errors when behavior is evaluated under more general loss functions or transferred across embodiments.
* Key challenge is that the learner must still estimate the control policy of the demonstrator to be able to generalize to new situations, while also constructing its own control policy to overcome its differences in embodiment. An example is shown in Figure 1 below:
![Figure 1](/img/paper_summaries/chen-monfort-2016/figure-1.png)
* Refernce a useful definition: An IOC algorithm is considered **Fisher consistent** if the learned policy is the optimal policy for *any set of dynamics*, given enough demonstrations and a sufficiently expressive feature representation for policies.
* Combines rationalizing demonstrated behavior from IOC with a game-theoretic perspective that incorporates different imitative losses. The approach assumes that the demonstrator's policy is the worst-case possible for the learner. **This avoids generalizing from available demonstrations in an optimistic manner that may be unrealistic and ultimately detrimental to the learner.** [^1]
[^1]: This same approach should also be effective in minimizing the amount of risk for a learned policy. How would this approach need to be changed in order to minimize risk instead of just differences in embodiment?
* Although the demonstrator's true policy is unlikely to be maximally detrimental to the learner, considering it as such leads to
    * Fisher consistency
    * Provides strong generalization guarantees
    * Avoids making any unwarranted assumptions

### Adversarial Approach

##### Definition 3
The adversarial IOC learner for the joint demonstrator/learner transition dynamics $(\tau, \hat{\tau})$ is defined as a zero-sum game in which each player chooses a stochastic control policy, $\hat{\pi}$ or $\check{\pi}$, optimizing

$$
\min_{\hat{\pi}} \max _{\check{\pi} \in \tilde{\Xi}} \left[ \sum _{t=1} ^{T} loss(\hat{S}_t, \check{S}_t) \bigg| \check{\pi}, \tau, \hat{\pi}, \hat{\tau} \right] \tag{2}
$$

* The $\min_{\hat{\pi}}$ is the learner trying to minimize the imitative loss measure.
* The $\max_{\check{\pi} \in \tilde{\Xi}}$ is the adversary trying to maximize the imitative loss yet matching feautre expectations.

##### Theorem 2
An additional desirable property of this approach is that if the set $\tilde{\Xi}$ can be defined to include the demonstrator's true policy, $\pi$, then generalization performance will be upper bounded by the expected adversarial training loss.

##### Theorem 3
An equilibrium for the game of Definition 3 is obtained by solving an unconstrained zero-sum game parameterized by a vector of Lagrange multipliers:

$$
\min _{w} \min _{\hat{\pi}} \max _{\check{\pi}} \mathbb{E} \left[ \sum _{t=1} ^{T} loss(\hat{S}_t, \check{S}_t) + \mathbf{w}\cdot\phi(\check{S}_t) \bigg| \check{\pi}, \tau, \hat{\pi}, \hat{\tau} \right] - \mathbf{w}\cdot\tilde{c}
$$
* ${loss}$ is the predefined imitative loss - how different two states are.
* I believe that $\tilde{c}$ represents the feature expectations of the demonstrator.

Note that this is the same as Equation 2, except that the feature matching constraints are moved to an outer minimization.

### Game representation
The stochastic policy of each player $\check{\pi}$, $\tilde{\pi}$ is formed as a mixture of deterministic policies: $\check{\delta}$ and $\hat{\delta}$. Conceptually, the payoff matrix of the zero-sum game can be constructed by specifying each combination of deterministic policies having payoff: $\mathbb{E} [ \sum _{t=1} ^{T} loss(\check{S}_t, \hat{S}_t) + \mathbf{w} \cdot \phi(\check{S}_t) | \check{\delta}, \tau, \hat{\delta}, \hat{\tau} ]$. Note that the $\tau$'s represent the dynamics, not trajectories. An example payoff matrix is shown in Table 1 with the adversary choosing a distribution over columns, and the learner choosing a distribution over rows:

![Table 1](/img/paper_summaries/chen-monfort-2016/table-1.png)

This leads to a payoff matrix with a size that is exponential in terms of the actions in the decision process. This cannot be explicitly constructed for practical problems so the authors employ the double oracle method [McMahan et al., 2003] to construct a smaller sub-portion of the matrix that supports the Nash equilibrium strategy for the game. The basic strategy, outlined in Algorithm 1, iteratively computes a Nash equilibrium for a payoff sub-matrix and *augments* the payoff matrix with an additional column and row that provide the most improvement for each player.

Finding the best response for each player:
$$
\underset{\hat{\delta}}{\text{argmin }} \mathbb{E} \left[ \sum _{t=1} ^{T} loss(\hat{S}_t, \check{S}_t) \bigg| \check{\pi}, \tau, \hat{\delta}, \hat{\tau} \right] \tag{learner}
$$
$$
\underset{\check{\delta}}{\text{argmax }} \mathbb{E} \left[ \sum _{t=1} ^{T} loss(\hat{S}_t, \check{S}_t) + \mathbf{w} \cdot \phi(\check{S}_t) \bigg| \check{\delta}, \tau, \hat{\pi}, \hat{\tau} \right] \tag{adversary}
$$

reduces to a time-varying optimal control problem.

# Algorithm
```python
def learn_reward_weights(demo, tau_dem, tau_lrn, loss, lr, c_dem):
    """
    Learns reward weights w which, when combined with the loss, give the
    optimal reward function.

    Algorithm 2 in paper.

    Parameters
    ----------
    demo : array-like
        Array of state-action pairs which represent a single demo trajectory.
    tau_dem : function
        Demonstrator dynamics.
    tau_lrn : function
        Learner dynamics.
    loss : function
        Imitation loss.
    lr : function or array-like
        Learning rate schedule.
    c_dem : array-like
        Demonstrator feautre expectations. "c' stands for constraints.

    Returns
    -------
    array-like
        Reward parameters (lagrange multipliers) w.
    """
    w = 0
    pi_adv = initialize_pi()
    pi_lrn = initialize_pi()

    while w not converged:
        pi_adv, _ = _double_oracle(tau_dem, tau_lrn, loss, pi_adv, pi_lrn, phi, w)
        adv_feat_exp = _feature_expectations(pi_adv, phi, tau_adv)
        w = w - lr*(adv_feat_exp - c)

    return w


def _double_oracle(tau_dem, tau_lrn, loss, pi_adv, pi_lrn, phi, w):
    """
    Computes a Nash equilibrium (two stochastic policies) for learner and adversary.

    Algorithm 1 in paper.

    Parameters
    ----------
    tau_dem : function
        Demonstrator dynamics.
    tau_lrn : function
        Learner dynamics.
    loss : function
        Imitation loss.
    pi_adv : function
        Adversary's  initial policy.
    pi_lrn : function
        Learner's initial policy.
    phi : array-like
        Feature functions.
    w : array-like
        Reward weights (lagrange multipliers).

    Returns
    -------
    function
        Adversary policy of Nash Equilibrium.
    function
        Learner policy of Nash Equilibrium.
    """
    while True:
        # Compute nash eq and game value
        nashpol_dem, nashpol_lrn, v_dem = _nash_eq(pi_adv, pi_lrn, loss, phi, w)
        # Compute adversary's best response and its value to learner's nash eq policy
        bestresp_adv, v_bestresp_adv = _best_response_adv(nashpol_dem, tau_dem,
                                                          nashpol_lrn, tau_lrn)

        # Add adversary best response to payoff matrix
        if v_dem != v_bestresp_adv:
            pi_adv.append(bestresp_adv)

        # Compute nash eq and game value
        nashpol_adv, nashpol_lrn, v_lrn = _nash_eq(pi_adv, pi_lrn, loss, phi, w)
        # Compute leanrer's best response and its value to adversary's nash eq policy
        bestresp_lrn, v_bestresp_lrn = _best_response_lrn(nashpol_adv, tau_dem,
                                                          nashpol_lrn, tau_lrn)

        # Add learner best response to payoff matrix
        if v_lrn != v_bestresp_lrn:
            pi_lrn.append(bestresp_lrn)

        # Convergence check
        if (v_dem == v_bestresp_adv == v_lrn == v_bestresp_lrn):
            break

    return nashpol_dem, nashpol_lrn


def _best_response_adv(nashpol_dem, tau_dem, nashpol_lrn, tau_lrn, loss):
    total_loss = loss + w.dot(phi)
    best_resp = argmax(total_loss)

def _best_response_lrn(nashpol_dem, tau_dem, nashpol_lrn, tau_lrn, loss):
    total_loss = loss
    best_resp = argmax(total_loss)

def _feature_expectations(pi_adv, phi, tau_adv)
    """Not implemented."""
    pass


def _nash_eq(pi_adv, pi_lrn, loss, phi, w):
    """Not implemented."""
    pass

```

### Existing Method Relationships

Adversarial IOC can be viewed as combining the mixing behavior of Abbeel & Ng's feature-matching algorithm [Abbeel and Ng, 2004] with MMP's margin-like [Ratliff et al., 2006] selection of policies to mix, while avoiding the integration over all policies required by maximum (causal) entropy
