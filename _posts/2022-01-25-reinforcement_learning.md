---
layout: post
title:  "Reinforcement Learning"
date:   2022-01-28 Reinforcement Learning
categories: Reinforcement Learning
---

# <span style="color: red;">---------------------- DRAFT ----------------------------- </span>
Reinforcement Learning (RL) is a subfield of machine learning and focuses on learning by trial and error. This is very amazing. Just imagine a chess engine that learns to play chess on it's own, without any expert knowledge (except the allowed moves of the single pieces). The chess engine just plays a game and in the end receives a reward (+1 for winning, 0 for draw and -1 for losing). Only by this feedback the engine is able to learn to play chess extremely good. Isn't that amazing.

Reinforcement learning gets a lot of popularity since 2016 when the strongest Go Player of the world Lee Sedol was beaten by AlphaGo in the game Go. AlphaGo is an algorithm that was developed by Google Deepmind. This was a huge milestone as Go was not expected to be beaten by an algorithm in the near feature. This assumption was based on the fact that the game has a very huge amount of possible states. 

# Problems that have been solved by reinforcement learning
- LearningChess
- Learing Go
- Learning Atari Games

### Key concepts
The agent is in a state s<sub>t</sub> and interacts with the environment by executing action a<sub>t</sub>. By this interaction the environment changes into a new state s<sub>t+1</sub>. At the same time the agent receives a reward r<sub>t+1</sub> that contains information on how good this action was. This feedback is used by the agent to learn which action is good in which state. The agent choses it's actions to maximize the rewards he can collect.

Let's have first a look at the terminology and afterwards at a concrete example.

# Agent
The agent interacts with the environnent by executing actions. It's goal is to receive as much reward as possible.

# Environment, State and Observation Space
The environment is completely described by the the state s. In contrast to that an observation o is just a partial description of the state. 

Imagine a vacuum cleaner (agent) that has the task to clean the rooms of a house. The state s contains information about all rooms in the 
house. The vaccum cleaner itself only has partial information about the room it is. This is due to the limiting range of the sensor it
is equipped with.

The reinforcement learning literature doesn't really differentiate between these two. They often talk about states even though the correct
term would be observation. Thus, I also use this notation in this blog.

The State Space S constists of the set of valid states and the Observation Space O of the set of valid observations.

# Discrete and Continous Action Spaces
The action space A contains all the actions a that an agent can execute. These are highly dependent on the environment.

That makes sense. If you play the game monopoly you can e.g. buy a property, construct a hotel or throw the dices. In crontast to that if you play soccer you can e.g. pass the ball, dribble or shoot.

The action space could either be discrete or continous. Discrete means to have a finite amount of actions. Imagine a easy computer game where
you can only go left or right.
In contrast to that continous action spaces have an infinite amount of possible actions. Imagine soccer where you don't only have the shoot action, but the possibility to vary the intensity of the shoot.


# Return and Reward
The reward is a very important part of reinforcement learning. This actually tells the agent which behaviour is good (high reward) or which is bad (low reward). Thus, it defines what behaviour the agent learns at the end. We will look at some examples how the reward influences the behaviour of the agent at the agent of this page.

The reward is what the agent gets from the environment after he executes action a<sub>t</sub> in state s<sub>t</sub> and ends up in a new
state s<sub>t+1</sub>. This means that the reward r can be seen as a function $$R : S x A x S ->\mathbb{R} $$.

The goal of the agent is to maximize the cumulative rewards during some trajectory $$\tau$$. A trajectory is simply a sequence of states and
actions. $$\tau = (s_t, a_t,s_{t+1},a_{t+1},...)$$. The cummulative reward is called return G. This means the agent want to maximize the return until the final time stamp T.

$$G(\tau) = \sum_{t=0}^T r_{t}$$

How is this final time step defined? In games the final time step is obviously the winning/draw/losing state. Here the trajectory $$\tau$$ has a defined length and is called an episode. The task is called episodic.

But this is very different for other tasks that are continuing over a long period of time. This is e.g. the case in controling the heat of a room. The trajectory is infinite and we call it an continuing task.

In a continuing task ($$T = \infty $$), the return is not converging to an finite value. To prevent this we add a discount factor $$\gamma\in[0,1]$$. This factor gurantees a convergence. 

$$G(\tau) = \sum_{t=0}^{\infty} \gamma^{t} r_{t}$$

The disount factor is also useful for episodic tasks as it encodes that a reward now is more worth than in the future. This forces the agent to achieve its goal as fast as possible and that's what we want. We will look on the influence on the behaviour of an agent for different reward functions at the end of the page.

# Policy
The policy $$\pi$$ is the brain of the agent and selects the action it should take in each state. It is a mapping from states to a probability distribution over the actions.  

$$a_{t} \sim \pi(s_{t})$$

The policy is called deterministic if in each state one action has probability 1 and thus the others have 0. Intuitevely, this means that
you exactly know which action a the agent picks in state s.

Otherwise the policy is called stochastic. In this case you don't know exactly which action is picked. You just now the probability
distributioin. An example for state $$s_{1}$$ could be $$\pi(a_{1}|s_{1})= 0.3$$,  $$\pi(a_{2}|s_{1})= 0.5$$ and  $$\pi(a_{3}|s_{1})= 0.2$$.

## Markov Decision Process
This actually sounds like a Markov Decision Process and we know how to solve this. What is then the problem? The problem is that we don't know
anything about the transitisons. The agent needs to disover the dynamics of the environment by trial and error. Trial and error means do try
different actions in different states and see which rewards he gets.


## Example
Imagine we have a 4x4 grid world and the goal of the agent is to find the star. See in the image below.
This enviornment has 16 states and we assume that the agent has 4 different actions (move up, move down, move left and move right). We model the reward tp

In this example we create a environment that is a 4x4 grid world. Each square is a state that sums up to 16 unique states.
Each square

 Thus, we have 16 different states our agent can be in which consists of 16 separate fields. In this example these are the states the agent can
be in. 




 and a reward rt is given to the agent. The agent now executes another action at+1 and so on. The goal of the agent is accumulate as much rewards as possible.

The two main concepts are agent and environment. The agent is 

Agent: The agent (e.g. a algorithm, robot) that interacts with the environment
Environment: The surrounding the agent interacts with
Policy: The policy is what in the reinforcement learning process should be attained. It defines the behaviour of an agent in a given state.
Reward: The reward defines the goal of reinforcement learning. In every time step the agent receives a single number from the environment, called the reward. It is an important feedback to the agent on how good the action was. The goal of the agent is to maximize the expected reward.




# Example

**Value Function**:
The value functions contains for every state the expected long term reward an agent can achieve starting in this state. It is an accumulation of the rewards of possible future states. It can be used to evaluate the goodness/badness of states. In contrast to that, the reward only tells how good an immediate action is. An action with a low reward can end up in a state that has a high value or vice versa.

# The mathematical interpretation



Imagine the agent being a robot that is in a room, which is represented as a grid. The goal is to reach the top right corner. Let's assume that the robot has 4 different actions in each step.
He can go left, right, front or back by one step.



$$mean = \frac{\displaystyle\sum_{i=1}^{n} x_{i}}{n}$$


Environment: The environment describes the state the agent is in.

Value Function: The value functions contains for every state the expected long term reward an agent can achieve starting in this state. It is an accumulation of the rewards of possible future states. It can be used to evaluate the goodness/badness of states. In contrast to that, the reward only tells how good an immediate action is. An action with a low reward can end up in a state that has a high value or vice versa.

# Influence of the reward on the behaviour of the agent
The chosen reward function is very important and often not so easy as in this toy example. Let's see how it influences the behaviour of the agent.

The most easies reward function for the grid world is to give the robot +1 if he reaches the goal state and 0 in all other cases. The robot learns to reach the goal. Is it the fastest way he learns? No, he doesn't. There is no difference in reward for the agent if he reaches the goal after timestep t1 or tx. Normally, future rewards are discounted by gamma as shown abvoe. This
forces the agent to learn the quickest route as rewards at an earlier timestep are worth more than later ones.

Another way to reach the goal faster with a gamma=1, would be to reward the agent with +1 if he reaches the goal. And a reward of -0.1 for each timestep. To maximize the expected reward the 
agent would try to go to the goal as fast as possible.

What would happen if we set the reward for each timestep to an value >0 and the reward for the goal state to +1. What strategy would he learn? Actually, in this case the agent would learn
to never reach the goal state. That's because he gets a positive reward for each step. If the agent reaches the goal state this reward would stop as the episode is finished. Thus, he tries
to avoid it.