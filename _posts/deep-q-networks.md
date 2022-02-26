---
layout: post
title:  "Deep Q-Networks"
date:   2022-02-26
categories: Deep Reinforcement Learning
---

In the previous posts we looked at small environments like the grid world. In these environments it easy to
create a table that contains an action for each state. But what happens if the state space is huge, like e.g. in the game Go. Then this tabular approach is not possible anymore.

## Function Approximation
In Q-Learning we store for each state and action combination a value that states how good it is to take action a in state s. This is only feasible with a small amount of states. If we look at the Atari games the size of the state space is very huge. It is not possible
to solve these environments with a tabular method like in classical Q-Learning.

To handle these cases we will approximate the states. There are multiple ways to do that. Some are described in the excellent book of Richard Sutton, which can be downloaded for for free: [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/the-book.html).

In this post we will focus on Deep Q-Learning which uses a Deep Neural Network as function approximation and builds on the Q-Learning algorithm we talked about in the [Temporal Difference Learning post]({% post_url 2022-02-14-temporal-difference-learning %})
    
$$Q(s,a) = Q(s,a) + \alpha * (r + \gamma * \max_{a'}Q(s',a') - Q(s,a))$$