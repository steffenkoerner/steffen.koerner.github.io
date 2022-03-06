---
layout: post
title:  "Deep Q-Networks"
date:   2022-03-06
categories: Deep Reinforcement Learning
---

In the previous posts we looked at small environments like the grid world. In these environments it easy to
create a table that contains an action for each state. But what happens if the state space is huge, like e.g. in the game Go. Then this tabular approach is not possible anymore.

In this post we will look at some basic implementation 

## Function Approximation
In the case of huge state spaces we need a different approach. One solution is to approximate the complex non-linear function Q(s,a).

There are multiple approaches to approximate this function. Some are described in the excellent book of Richard Sutton, which can be downloaded for for free: [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/the-book.html).

In this post we will focus on using a Deep Neural Network as function approximation. How can we train this Neural Network?

Actually, we can use a classical supervised learning approach. Our target (label) can be retrieved from the
Bellman Equation we discussed in the [Temporal Difference Learning post]({% post_url 2022-02-26-temporal-difference-learning %}).

The Bellman Equation:

$$Q(s,a) = \mathbb{E}[ r(s,a) + \gamma \mathbb{E}[Q(s',a')]]$$

Unfortunately, it's not possible to directly use the data that is coming from the environment. We will discuss why this is the case and how we can solve these problems.


### Experience Replay
Training a Neural Network uses stochastic gradient descent. This algorithm assumes that the training data is independent and identically distributed (i.i.d). Unfortunately, our training data doesn't fulfill this assumption.

It is not independent as the data is received is coming from an agent that is following a policy. In other words, the next state and the current state is coupled by the current policy. Thus, the agent can't learn directly from the observations. 

This problem can be adressed with a buffer. The buffer stores the data collected through many episodes. The stored experiences are (s,a,r, s'). During the learning process a subset is sampled randomly from the buffer. This approach leads to the fulfillment of i.i.d experiences and thus we can train our Neural Network.

A simple implementation of a buffer is a ringbuffer of a fixed size. Mainly in the order of at least 100,000. This means that if the buffer is full a newly added experience pushes out the oldes one.

The buffer in this context is called **replay buffer** as you replay transistions that you have recorded.

The process of sampling a subset from the replay buffer to learn from it is called **experience replay**.

### Target Networks

## Summary
Let's have a look at the Q-Learning formula again:

$$Q(s,a) = Q(s,a) + \alpha * (r + \gamma * \max_{a'}Q(s',a') - Q(s,a))$$

Instead of looking up Q(s,a) and Q(s',a') in a table we now approximate it with a Deep Q-Network.

For this we use a neural network with the state s as input and the action a as output. 

The training of the Neural Networks happens by interaction

The implementation of the Neural Network can be found in [Github Implementation](https://github.com/steffenkoerner/deep_reinforcement_learning/tree/main/library)   


# Stochastic Gradient Descent (SGD)
Stochastic Gradient 
=> Replay Buffer

# Correlation

The algorithm described in this post was used from Deepmind to successfully train a DQN for multiple atari games and demonstrated the performance.

[Playing Atari with Deep Reinforcement Learning](https://arxiv.org/abs/1312.5602)
[Human-level control through deep reinforcement learning](https://www.semanticscholar.org/paper/Human-level-control-through-deep-reinforcement-Mnih-Kavukcuoglu/e0e9a94c4a6ba219e768b4e59f72c18f0a22e23d)


Since these two papers appeared there have been many more extension to DQN's that tried to overcome various shortcomings. Let's shortly have a look at them. If I find time I will implement these extension and create a more detailed post.


- [Rainbow: Combining Improvements in DRL](https://arxiv.org/abs/1710.02298)
- [DRL with Double Q Learning](https://arxiv.org/abs/1509.06461)
- [Noisy Networks for Exploration](https://arxiv.org/abs/1706.10295)
- [Prioritized Experience Replay](https://arxiv.org/abs/1511.05952)
- [Dueling Network Architectures for DRL](https://arxiv.org/abs/1511.06581)
- [A Distributional Perspective on RL](https://arxiv.org/abs/1707.06887)
- [N-steps DQN](https://arxiv.org/abs/1901.07510)