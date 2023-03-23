---
layout: post
title:  "DQN Extensions"
date:   2023-03-23
categories: Deep Reinforcement Learning
---

Since the two famous papers of Deepmind about DQN appeared, there have been many extensions to DQN's that tried to overcome various shortcomings. They dramatically improved the performance. The most famous extensions are listed below. 

This post just gives a quick overview to get a feeling which tweaks can be applied. For each extension, the corresponding paper is linked for detailed explanations.

### Double DQN
DQN is known to overestimate action values under certain conditions. This overestimation leads to training problems.

This overestimation comes from the fact that the max operator in the DQN algorithm is used for selecting the best action and the value.

It can be fixed by using two different networks. One for estimating the best action and one to retrieve the value.

As we already have two networks due to the target network, this adaption is very easy to implement. We just need to separate the max operation. We use the original network to select the best action and then use the target network to get the value.

Mathematically this means to replace $$ \max_{a'}Q_T(s',a')$$ with $$Q_T(s',\operatorname*{arg\max}_{a' \in A} Q(s',a'))$$.

This algorithm is called Double DQN and is described in detail in the Deepmind paper: [DRL with Double Q Learning](https://arxiv.org/abs/1509.06461).



### Prioritized Experience Replay 
DQN samples uniformly from the replay buffer. Preferrably, we would like to sample interesting experiences more often. Interesting experiences are the ones that the agent can learn the most from. This can be achieved by prioritizing the experiences based on the training loss.

This extension is called Prioritized Experience Replay and is described in this paper: [Prioritized Experience Replay](https://arxiv.org/abs/1511.05952).


### N-steps DQN
DQN is a one-step approach, as it collects one reward and then uses the maximum estimation of the next state. Instead of only collecting one reward, it is possible to make n-steps before using the estimation of the state. A suitable tuned multi-step approach leads to faster learning.

This extension is called N-steps DQN and is described in this paper: [N-steps DQN](https://arxiv.org/abs/1901.07510).



### Noisy Networks
It's common to to achieve expoloration of the environment by taking random actions. This is usally done by introducing some hyperparameter $$\epsilon$$, which is decreased during learning.

Many paper have shwon that this only works in very simple environments with short episodes. It also needs some good tuning to make the training process efficient.

Another approach is to add random noise to the fully-connected layers and adjust it during training.

This extension is called Noisy Networks and is described in this paper: [Noisy Networks for Exploration](https://arxiv.org/abs/1706.10295).


### Dueling Network
This extension is called Dueling Network and is described in this paper: [Dueling Network Architectures for DRL](https://arxiv.org/abs/1511.06581).

### Distributional RL
This extension is called Distributional RL and is described in this paper: [A Distributional Perspective on RL](https://arxiv.org/abs/1707.06887).

### Rainbow
The rainbow algorithm combines all the mentioned extensions. It shows superior performance results. A detailed description and 
results can be found in the paper: [Rainbow: Combining Improvements in DRL](https://arxiv.org/abs/1710.02298)


### Summary
This article gives an overview of the most common extensions for the DQN.The post is kept short to just give an idea what kind of tweaks are possible. Detailed information and evaluations can be found in the corresponding papers.