---
layout: post
title:  "Evaluate Reinforcement Learning Algorithm"
date:   2022-03-06
categories: Deep Reinforcement Learning
---

The best approach to learn reinforcement learning is to implement the basic algorithms like VPG, DQN, PPO and DDPG. But how can you make sure that you implemented them correctly. It's not
as easy as with classical algorithm where you just implement a test suit and you if all tests are green you implemented everything correctly.

Thus, in this post we want to discuss how you can analyze your algorithm and verify if it is correct. In this blog we go through the comparison step by step by evaluating the implementation of our DQN algorithm.

One approach is to compare your algorithm to a baseline implementation. A baseline implementation can for example be found at [OpenAI Baseline](https://github.com/openai/baselines)

For discrete action spaces like in(DQN, PPO,..) the atari environments from open ai are a very good basis. [OpenAI Gym](https://gym.openai.com/)






### Baselines
[OpenAI Baselines]()
[Stable Baselines3](https://github.com/DLR-RM/stable-baselines3)
[RL Baselines3 Zoo](https://github.com/DLR-RM/rl-baselines3-zoo)

In the following we will use the Stable Baselines 3 framework for evaluation. More information on it can be found in the following [blog post](https://araffin.github.io/post/sb3/)
The documentation can be found in [Stable Baseline Documentation](https://stable-baselines.readthedocs.io/en/master/index.html)


## RL Zoo to compare different algorithms
git clone --recursive https://github.com/DLR-RM/rl-baselines3-zoo
This command is needed to also have the pre-trained agents inside it. That's what we need later on to compare them to our implementation.
# Environments for Evaluating the Algorithm
### Open AI Gym
This is by fast amount the most popular benchmark for reinforcement learning algorithm. It contains a fast amount of different


### Unity Environment
![Unity Environments](https://github.com/steffenkoerner/steffenkoerner.github.io/tree/master/images/unity-example-envs.png)
Unity aso provide some very interesing scenarios that have higher qualtiy than the Gym ones. I really like to work with them as the quality of the environments is really good. You can check it out
at [untiy repo](https://github.com/Unity-Technologies/ml-agents/blob/main/docs/Learning-Environment-Examples.md)

### Google Research Football (GRF)
This is a very cool environment, where the goal is to learn the most popular game in Europe - Football. Unfortunately, until now I haven't worked with this environment as it seems more complex to learn. But I am a huge football fan and thus definitely want to tackle this environement. Until I am ready I tackle the unity soccer environment that seems to be easier to solve.

More information can be found in the [Google Research Football Github](https://github.com/google-research/football), [Google blog post](https://ai.googleblog.com/2019/06/introducing-google-research-football.html) or in the [Google paper](https://arxiv.org/pdf/1907.11180.pdf)


If you are interested how you compare to other people trying to solve this environment, then you can check out the [Kaggle Competition ](https://www.kaggle.com/c/google-football/overview/training-rl-agents)

### Deepmind Lab
[Deepmind Lab Blog](https://deepmind.com/blog/article/open-sourcing-deepmind-lab) and [Deepmind Lab Github](https://github.com/deepmind/lab)


### Further Resources Evaluation
[Deep Reinforcement Learning that Matters](https://arxiv.org/abs/1709.06560)
[How many seeds should I use](https://openlab-flowers.inria.fr/t/how-many-random-seeds-should-i-use-statistical-power-analysis-in-deep-reinforcement-learning-experiments/457)
[Problem in Evaluation Methodology](https://github.com/hill-a/stable-baselines/issues/199)

### Tips and Tricks for Implementing your own RL algorithm

For evaulating its also good to have a look at the [nuts and bolts of RL research by John Schulman](http://joschu.net/docs/nuts-and-bolts.pdf). It's also available as [video](https://www.youtube.com/watch?v=8EcdaCk9KaQ)

1) Read the original paper
2) Evaluate your algorithm on a toy example (very easy environment like cartpole)
3) Validate your implementation by increasing the complexity of environments
4) Comparing your algorithm against some Baseline Implementation


Some examples of envs in increasing complexity

# discrete
CartPole-v1 
LunarLander
Pong (one of the easiest Atari game)
other Atari games (e.g. Breakout)

# continous
Pendulum


# Reliable Framework for evaluation RLIABLE
[google rliable](https://github.com/google-research/rliable)
Interesting blog post about rliablae [post rliable](https://ai.googleblog.com/2021/11/rliable-towards-reliable-evaluation.html)
Corresponding paper is [Deep Reinforcement Learning at the Edge of the Statistical Precipice](https://arxiv.org/abs/2108.13264)

Nice explanation of rliable [blog post](https://araffin.github.io/post/rliable/)

### Othert topics

Hyperparameter Tuning:
Can also be done via the rl zoo
Example framework [Optuna](https://optuna.org/)