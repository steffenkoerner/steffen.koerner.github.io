---
layout: post
title:  "Solving Reinforcement Learning"
date:   2022-02-11
categories: Reinforcement Learning
---

In the post about reinforcement learning we looked at the key concepts of reinforcement learning and what problem we actually want to solve. Now we want to adress how to solve this problem.

Before we can combine the faszinating part of understanding how we can sove the problem we need to clarify one more concept. And this is state vaue and action value functions. Let's get started.
 

# State Value Function and Action Value Function
A state value function $$V: S \to \mathbb{R}$$ maps states to a value. This value represents the expected return the agent gets if he starts in a state and then acts accoring to its policy. 

$$V^{\pi}(s) =  \mathbb{E}_{\pi}[R_t(\tau) | S_t=s] = \mathbb{E}_{\pi} [\sum_{k=0}^{\infty} \gamma^k r_{t+1+k}]$$

How can this value help us? In the end we are interested in a policy $$\pi$$ and that is a mapping from state to action.

One approach is to evaluate for each possible action in state s what the reward belongs to this action and what is
the expected return of the state s' we end up by this action.

$$a = \operatorname*{arg\max}_{a \in A}\mathbb{E}_{\pi}[R_a(s,s') + V(s')]$$

This is a valuation solution in a MDP, as there we know the state transition function, meaning that we know in which 
state S' we end if we execute action a in state s. In reinforcement learning we unfortunately don't know this.

Thus, instead of storing only the expected return for a state we store the expected return for each action we can do in
that state. This function is called action value function.

$$Q^{\pi}(s,a)= \mathbb{E}_{\pi}[R_t(\tau) | S_t=s, A_t=a]$$

Having the q-values stored for each state we can easily chose the action that returns the highest expected return.

$$Q(s,a)=\operatorname*{arg\max}_{a \in A}Q(s,a)$$


## Optimal Value Function
We already talked a few times about the optimal value function and the improved or optimal policy. We intuitively understand what is meant by that. But we missed a formal definition so far. 
A policy $$\pi$$ is better than a policy $$\pi'$$:

$$\pi \geq \pi' \text { if and only if } V^{\pi}(s) \geq V^{\pi'}(s), \forall s \in S$$

The optimal value function is defined as:

$$V^*(s) =  \operatorname*{max}_{\pi} V^{\pi}(s)$$

Analogusly, we can define the optimal action value function:

$$Q^*(s,a) = \operatorname*{max}_{\pi} Q^{\pi}(s,a)$$

# Solving the Grid World
Finally, we are ready to solve our first reinforcement learning problem. This will be amazing. At first we will define the problem we need to specifiy the environment we want to solve.

## Environment - Grid World
Welcome back in our 4x4 gridworld we already visited last time. Lets's quickly recap the setting.

Imagine we have a 4x4 grid world and the goal of the agent is to find the star. See in the image below.
This environment has 16 states. The start position of the agent is (0,0) and the goal state is (3,3). The task is episodic, which means that if the agent reaches the goal state, the episode terminates. Now everything is set back, and the agents starts again.

![Grid World Setup](/images/grid_world_setup.png)

We define that the agent has 4 different actions (up, down, left or right). This means that if the agent
executes action up in state (0,0) he will end up in state (1,0). If he evaluates an action that he can't do as there is a border, he will
stay in the same state. This means that action left in state(1,0) will lead to state(1,0).

## How does the Agent Learn
The goal of the agent is to reach the start, which is in the top right corner. We define the reward function as +1 for reaching the star and 0 otherwise. The discount factor $$\gamma = 0.9$$. The influence of the reward function to the
behaviour of the agent we already discussed in the last post.

The agent has no idea about the dynamics of the environment and thus moves accoding to it starting policy $$\pi_{0}$$, which normally results in random behaviour. The agent chooses actions according to this policy and tries to learn a better one. This means to achieve a higher expected return. We will define this more formally in a few minutes.

Let's assume this is the first trajectory $$\tau^{\pi_0}=((0,0),up,0,(1,0),right,0,(1,1),...,(3,2),right,1,(3,3))$$ of the agent. It is shown in blue in the image below.

![Grid World Trajectory](/images/grid_world_trajectory.png)

What has the agent learn now? He learned that action right in state (3,2) is good as he got a reward of +1. This
is very obvious. But what about all the other states that are part of this trajectory.



These method is called Monte Carlo.

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

