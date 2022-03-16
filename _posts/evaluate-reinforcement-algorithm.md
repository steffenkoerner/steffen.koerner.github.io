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


### Open AI Gym
This is by fast amount the most popular benchmark for reinforcement learning algorithm. It contains a fast amount of different


### Unity Environment
Unity aso provide some very interesing scenarios that have higher qualtiy than the Gym ones. I really like to work with them

### Google Research Football (GRF)
This is a very cool environment, where the goal is to learn the most popular game in Europe - Football. Unfortunately, until now I haven't worked with this environment as it seems more complex to learn. But I am a huge football fan and thus definitely want to tackle this environement. Until I am ready I tackle the unity soccer environment that seems to be easier to solve.

More information can be found in the [Google Research Football Github](https://github.com/google-research/football), [Google blog post](https://ai.googleblog.com/2019/06/introducing-google-research-football.html) or in the [Google paper](https://arxiv.org/pdf/1907.11180.pdf)


If you are interested how you compare to other people trying to solve this environment, then you can check out the [Kaggle Competition ](https://www.kaggle.com/c/google-football/overview/training-rl-agents)


