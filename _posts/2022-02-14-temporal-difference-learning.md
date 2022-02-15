---
layout: post
title:  "Temporal Difference Learning"
date:   2022-02-14
categories: Reinforcement Learning
---

Temporal Difference Learning (TD Learning) solves the same problem as the Monte Carlo Methods. It also is based on experiences. Where MC needs a full episode, TD methods update their estimates based on estimates of other states. Thus, there is no need to wait until the end of a episode. Updating an estimate without waiting for the end is called bootstrapping. Maybe it still sounds a bit vague. Let's save the our
gridworld with the TD method.

Let's assume that we are in state s, select action a, get reward r and end up in state s'. We will shorten this in the future by (s,a,r, s'), which we call an experience. Can we already use this information to update the estimation of Q(s,a) instead of waiting until the end of the trajectory?

Yes, we can use this information.

## Bellman Equation
The Bellman Equation creates a connection between states that are connected by an action. It states that the expected value of a starting state is equal to the reward plus the expected value of the next state. This makes sense. Imagine a robot that is a state that he has no information about. If he does one step which leads to a reward of +1 and he ends in a state that has an expected reward of +10. Then assuming the policy is not
stochastic he knows that the state before has an expected value of $$+1 + \gamma * +10$$.

This leads us to the Bellman Equation:

$$Q(s,a) = \mathbb{E}[ r(s,a) + \gamma \mathbb{E}[Q(s',a')]]$$

From this equation we can derive an iterative approach to learn from single experiences. The following equation describes this connection. 

$$Q(s,a) = Q(s,a) + \alpha * (r + \gamma * Q(s',a') - Q(s,a))$$

The Bellman Equation states that $$r + \gamma * Q(s',a') - Q(s,a) = 0$$. We call the left-hand side error term, as it is the deviation
between the two states. To reduce the error term we go one small step, which is represented by $$\alpha$$, into the direction of the error.
Thus, reducing the error term.

This approach sounds good, but the main questions is if this method converges. Luckily, it does. We will not
prove this statement in this post. But you can read it in the paper [Convergence Proofs for Value and Policy Iteration](https://arxiv.org/pdf/2009.11403.pdf).

The Bellman Equation is a very fundamental building block in reinforcement learning and many methods are based on them. In the following we will have a look at two well-known algorithms.

## SARSA
The SARSA algorithm is very easy and can be direcly derived from iterative approach formula shown in the section above. The algorithm needs
following information (s,a,r,s',a') for an update of the estimate. The algorithm SARSA is named after this tuple to make the connection obvious.

$$Q(s,a) = Q(s,a) + \alpha * (r + \gamma * Q(s',a') - Q(s,a))$$

SARSA belongs into the category of **on-policy** learning algorithm. This means that the action a' in the next state s' is defined by the current policy.
## Q-Learning
Finally, we come to one of the most famous algorithm in reinforcement learning, which is Q-Learning. The algorithm is defined by this
formula.

$$Q(s,a) = Q(s,a) + \alpha * (r + \gamma * \max_{a'}Q(s',a') - Q(s,a))$$

Q-Learning belongs into the category of **off-policy** learning algorithm. This means that the action a' in the next state s' is not defined by the policy itself. In this case we use the action that has the maximum expected value.

Even though there seems to be only a small difference to SARSA, which is to pick the best action in the next state in contrast to picking the next action based on the current policy, it has a huge influence.

## Summary
In this post we discussed the temporal difference learning approach to solve the reinforcement learning problem. We discussed the similarities and differences to the Monte Carlo approach from the [last post]({% post_url 2022-02-14-monte-carlo-methods %}).

We looked at the SARSA and Q-Learning algorithm and described their fundamental differences.

## Q-Learning

# +++++++++++++++ Unordered Things


Q Learning
Sarsa
SARSA
On Policy Learning 
Off Policy Learning

The connection between Monte Carlo and TD Learning (n-step TD Learning)

++++ Think about how to create a graph by code. Then there is no need to drag and drop and adapt the things

