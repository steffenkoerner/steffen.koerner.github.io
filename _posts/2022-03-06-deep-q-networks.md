---
layout: post
title:  "Deep Q-Networks (DQN)"
date:   2022-03-06
categories: Deep Reinforcement Learning
---

In the previous posts we looked at small environments like the grid world. In these environments it easy to
create a table that contains an action for each state. But what happens if the state space is huge, like e.g. in the game Go. Then this tabular approach is not possible anymore.

In this post we will look at some basic implementation 

## Function Approximation
In the case of huge state spaces we need a different approach. One solution is to approximate the complex non-linear function Q(s,a).

There are multiple approaches to approximate this function. Some are described in the excellent book of Richard Sutton, which can be downloaded for for free: [Reinforcement Learning: An Introduction](http://incompleteideas.net/book/the-book.html).

In this post we will focus on using a Deep Neural Network as function approximation. 


TODO: Add Image of neural network. input is a representation of the state and the output is the Q(s,a1), Q(s,a2), Q(s,a3) values
The input representaiton can be a image then we can use a CNN or it actually can be some feature representation. Then we could use a plain Neural Network


How can we train this Neural Network?


TODO: Describe the loss function that is minimised

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
Let's have a look at the Q-Learning formula we discussed in the [Temporal Difference Learning post]({% post_url 2022-02-26-temporal-difference-learning %}).

Unfortunately, the approach is not directly supervised learning as the target depends on the network weights. The target is

 $$y_i = r_i + \gamma \max_{a'}Q(s',a') $$

 The problem is that updating the weights that $$Q(s,a) is closer to y_i also changes y_i as 

Thus we need to update the formula to:

$$Q(s,a) = Q(s,a) + \alpha * (r + \gamma * \max_{a'}Q_T(s',a') - Q(s,a))$$

,where $$Q_T$$ is the target network.

The update of Q(s,a) is based on the estimate Q(s',a'), which means that we are updating an estimation by an estimation. As the states s and s' are close to each other, an update of the weights of the Neural Network to improve the estimate of Q(s,a) can at the same time influence the estimate Q(s',a') and other close states. This can lead to an unstable learning process.

The learning process can be made more stable by using a **target network**. The target network is a copy of the original network and is used to estimate Q(s',a'). The original network is updated during training. 
After some episodes the learned weights are copied to the target network.

The additional target network stabilises the learning process.

# DQN
The algorithm described above is called DQN. More information can be found in the papers from Deepmind.
- [Playing Atari with Deep Reinforcement Learning](https://arxiv.org/abs/1312.5602)
- [Human-level control through deep reinforcement learning](https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf)

### Problems
DQN has problems with continous action spaces. In this case a common solution is to discretize the action spaces.
A natural extension to contious action spaces are algorithms that are based on gradients.

## Summary
In this post we discussed how we can handle environments with a huge state space. We used Neural Networks to approximate Q(s,a). The described algorithm using a Neural Network with replay buffer and target network was firstly
described by DeepMind.

They used it successfully train a DQN for multiple atari games and demonstrated the performance.
More information can be found in their two famous papers.
