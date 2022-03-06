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
Let's have a look at the Q-Learning formula we discussed in the [Temporal Difference Learning post]({% post_url 2022-02-26-temporal-difference-learning %}).

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

## Extensions
Since these two papers appeared there have been many more extension to DQN's that tried to overcome various shortcomings. They dramatically improved the performance. The most famous extensions are listed below.
### Double DQN
DQN is knwo to overestimate action values under certain conditions. This overestimation leads to training problems.

This overestimation comes from the fact that the max operator in the DQN algorithm is used fo selecting the best action and the value.

This problem can be fixed by using two different networks. One for estimating the best action and one to retrieve the value.
As we already have two networks due to the target network this adpation is very easy to implement. We just need to separate the max operation. We use the original network to select the best action and then use the target network to get the value.

Mathematically this means to replace $$ \max_{a'}Q_T(s',a')$$ with $$Q_T(s',\operatorname*{arg\max}_{a' \in A} Q(s',a'))$$

The algorithm is called Double DQN algorithm and is described in detail in the Deepmind paper: [DRL with Double Q Learning](https://arxiv.org/abs/1509.06461)



### Prioritized Experience Replay 
DQN samples uniformly from the replay buffer. Preferrably, we would like to sample interesting experiences more often. Interesting expereinces are the ones that the agent can learn the most from. For this the experiences are prioritized by the training loss.

This extension is called Prioritized Experience Replay and is described in this paper: [Prioritized Experience Replay](https://arxiv.org/abs/1511.05952)
### N-steps DQN
DQN is a one step approach as it collects one reward and then uses the maximum estimation of the next state. Instead of only collecting one reward it is possible to make n-steps until using the estimation of the state. A suitable tuned multi step
approach leads to faster learning.

This extension is called N-steps DQN and is described in this paper: [N-steps DQN](https://arxiv.org/abs/1901.07510)



### Noisy Networks
This extension is called Noisy Networks and is described in this paper: [Noisy Networks for Exploration](https://arxiv.org/abs/1706.10295)



### Dueling Network
This extension is called Dueling Network and is described in this paper: [Dueling Network Architectures for DRL](https://arxiv.org/abs/1511.06581)

### Distributional RL
This extension is called Distributional RL and is described in this paper: [A Distributional Perspective on RL](https://arxiv.org/abs/1707.06887)
## Rainbow
The rainbow algorithm combines all the mentioned extensions. It shows superior performance results. Detailed description and 
analysis can be found in the paper: [Rainbow: Combining Improvements in DRL](https://arxiv.org/abs/1710.02298)


## Summary
In this post we discussed how we can handle environments with a huge state space. We used Neural Networks to approximate Q(s,a). The described algorithm using a Neural Network with replay buffer and target network was firstly
described by DeepMind.

They used it successfully train a DQN for multiple atari games and demonstrated the performance.
More information can be found in their two famous papers.
