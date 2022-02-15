---
layout: post
title:  "Deep Q-Networks"
date:   2022-02-15
categories: Deep Reinforcement Learning
---

In the previous posts we looked at small environments like the grid world. In these environments it easy to
create a table that contains a action for each state. But what happens if the state space is huge, like e.g. in the game Go. Then this tabular approach is not possible anymore.

In this post we want to tackle the challenge of huge state spaces. To handle these cases we will approximate the states. There are multiple ways to do that. Some are described in the excellent book of Richard Sutton which can be downloaded for for free: [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/the-book.html).

We will focus on Deep Q-Learning which uses a Deep Neural Network as function approximation and builds on the Q-Learning algorithm we talked about in the [Temporal Difference Learning post]({% post_url 2022-02-14-temporal-difference-learning %})



