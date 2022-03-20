---
layout: post
title:  "Policy Gradient"
date:   2022-03-11
categories: Deep Reinforcement Learning
---

Policy based methods are a class of algorithms that directly search for the optimal policy without using estimmates of state values.

Policy gradient methods are a subset of policy based methods. They are using gradient ascent. The main idea is to increase the probability of actions that lead to a high return and decrease
the probability of actions that lead to a low return.

Why policy based methods 

Simplicity: Policy-based methods directly get to the problem at hand (estimating the optimal policy), without having to store a bunch of additional data (i.e., the action values) that may not be useful.
Stochastic policies: Unlike value-based methods, policy-based methods can learn true stochastic policies.
Continuous action spaces: Policy-based methods are well-suited for continuous action spaces.

See stochastic policy png as example where a value based approach is not very good.


How do the policy based approaches work?
Instead of mapping a state to an actions state value we now map states to actions. The problem is that we now can't use TD Learning anymore. Before we compared the state action value of the current state with the reward plus the maximum action state value of the next state. The goal was to minimize this error. But now we don't have these estimated values. Thus we need to collect the trjactory for the agent.

$$U(\phi) = P(\tau| \phi) R(\tau)$$

Our goal is to maximize the expected return U. One way to do that is by gradient ascent. This is ver closely related to the more famous gradient descent. There are only two differences 

Gradient ascent tries to find the maximum while gradient descent is searching for the minimum.
Gradient ascent steps in the direction of the gradient while gradient descent steps in the direction of the negative gradient

Thus, our parameter update looks as following:

$$\phi \leftarrow \phi + \alpha \nabla_{\phi} U(\phi)$$

where $$\alpha$$ is the step size. The step size decreases and it should be chosen such that $$\lim\limits_{\alpha \to \infty} = 0$$

TODO: Add formular for delta U(phi)

TODO: Describe the use case in a neural network.
If we implement the algorithm in pytorch we do not care about the gradient. We just calculate U(phi). As we want to maximize this value with gradient descent, but the backprogagtion algorithm uses gradient descent for the loss function. We use -U(phi) as the loss. THe whole weights are then automatically updated by pytorch.

What does this mean? It means that actions that are resulting in a positive return are made more likely. Meaning it reinforces the action.

If the action is in a trajectory that leads to a negative return less likely.

No clear credit assignment
The problem with this approach is that it updates the weight of actions on the trajectory based on the return. But this doesn't really say something about this action being good or bad.

Update process is very inefficient. Use a trajcetory only once.

Noisy trajectory. Due to the noise we can end up with trajectories that are not very representative for this policy.

Show reinforce with the cartpole example. This example shows that reinforce algorithm can solve environments with a discrete state space. But it also can handle contious action spaces.

In this case instead of having the actions in the output layer we can use the output layer to represent continous probabilitiy distributions. The output can for example be a normal distribution that is reprsented by the mean and variance. Then the action can be sampled from this distribution. 

While theoretically this approach is possible it doesn't show good results. Is there a better way to handle continous action spaces?

Sure, there are some approaches to handle these cases. We will talk about them in a one of the next blogs. If you can't wait for it, then you can google for the Proximal Policy Optimization (PPO) algorithm.

### Proximal Policy Optimization (PPO)


### Importance Sampling
to enable reuse of collected trajectories to be more resourceful.


$$\sum_{\tau} P(\tau | \phi) R(\tau) $$

under the new policy it looks lik

$$\sum_{\tau} P(\tau | \phi') R(\tau) $$

which can be re-written as

$$\sum_{\tau} P(\tau | \phi) \frac{P(\tau | \phi')} {P(\tau | \phi)} R(\tau) $$

The term $$ \frac{P(\tau | \phi')} {P(\tau | \phi)}$$ is called
re-weighting factor