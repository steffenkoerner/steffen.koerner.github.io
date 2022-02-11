---
layout: post
title:  "Solving Reinforcement Learning"
date:   2022-02-11
categories: Reinforcement Learning
---

In the post about reinforcement learning we looked at the key concepts of reinforcement learning and what problem we actually want to solve. Now we want to adress how to solve this problem. Let's get started.


# Value Functions
Before we can start with solving the grid world we need to clarify one more concept. And this is value functions.
A value function $$V: S \to \mathbb{R}$$ maps states to a value. This value represents the expected return the agent gets if
he starts in a state and then acts accoring to its policy.

$$V^{\pi}(s) =  \mathbb{E}_{\pi}[R_t(\tau) | S_t=s] = \mathbb{E}_{\pi} [\sum_{k=0}^{\infty} \gamma^k r_{t+1+k}]$$

How can this value help us? In the end we are interested in the optimal policy $$\pi^*$$ and that is a mapping from state to action.
To 

@Todo missing q value part

## Optimal Value Function
We already talked a few times about the optimal value function and the improved or optimal policy. We intuitively understand what is meant by that. But we missed a formal definition so far. 
A policy $$\pi$$ is better than a policy $$\pi'$$:

($$\pi \geq \pi'$$) if and only if $$V^{\pi}(s) \geq V^{\pi'}(s), \forall s \in S$$

The optimal value function $$V^*(s) = max_{\pi}(V_{\pi}(s))$$.

# Grid World
Welcome back in our 4x4 gridworld we already visited last time. Lets's quickly recap the setting.

Imagine we have a 4x4 grid world and the goal of the agent is to find the star. See in the image below.
This environment has 16 states. The start position of the agent is (0,0) and the goal state is (3,3). The task is episodic, which means that if the agent reaches the goal state, the episode terminates. Now everything is set back, and the agents starts again.

![Grid World Setup](/images/grid_world_setup.png)

We define that the agent has 4 different actions (up, down, left or right). This means that if the agent
executes action up in state (0,0) he will end up in state (1,0). If he evaluates an action that he can't do as there is a border, he will
stay in the same state. This means that action left in state(1,0) will lead to state(1,0).

# Solving the Grid World
The goal of the agent is to reach the start, which is in the top right corner. We define the reward function as +1 for reaching
the star and 0 otherwise. The discount factor $$\gamma = 1$$.

The agent has no idea about the dynamics of the environment and thus moves accoding to it starting policy $$\pi_{0}$$, which normally results in random behaviour. The agent chooses actions according to this policy and tries to learn a better one. This means to achieve a higher expected return. We will define this more formally in a few minutes.

Let's assume this is the first trajectory $$\tau^{\pi_0}=((0,0),up,0,(1,0),right,0,(1,1),...,(3,2),right,1,(3,3))$$ of the agent. It is shown in blue in the image below.

![Grid World Trajectory](/images/grid_world_trajectory.png)

What has the agent learn now? He learned that action right in state(3,2) is good as he got a reward of +1. This
is very obvious. But what about all the other states that are part of this trajectory.

## Value Functions



+++++++++++++++ Unordered Things

Value Function
Value Iteration

Policy Iteration

Q Learning

Bellmann Equation

Temporal Difference Learning
Monte Carlo Methods
SARSA
On Policy Learning 
Off Policy Learning

++++ Think about how to create a graph by code. Then there is no need to drag and drop and adapt the things

