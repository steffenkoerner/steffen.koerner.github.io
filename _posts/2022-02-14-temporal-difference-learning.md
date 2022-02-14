---
layout: post
title:  "Temporal Difference Learning"
date:   2022-02-14
categories: Reinforcement Learning
---

# Temporal Difference Learning (TD Learning)
Temporal Difference Learning (TD Learning) solves the same problem as the Monte Carlo Methods. It also is based on experiences. Where MC needs a full episode, TD methods update their estimates based on estimates of other states. Thus, there is no need to wait until the end of a episode. Updating an estimate without waiting for the end is called bootstrapping. Maybe it still sounds a bit vague. Let's save the our
gridworld with the TD method.

Let's assume that we are in state s, select action a, get reward r and end up in state s'. We will shorten this in the future by (s,a,r, s'). Can we use this information to update the estimation of Q(s,a)?

Yes, we can update our estimation with the following formula:

$$Q(s,a) = Q(s,a) + \alpha * (r + \gamma * Q(s',a') - Q(s,a))$$

Let's analyze this equation in more detail.



The question we always need to ask for these kind of algorithms is if it converges. 


# +++++++++++++++ Unordered Things


Q Learning
Sarsa
SARSA
On Policy Learning 
Off Policy Learning

The connection between Monte Carlo and TD Learning (n-step TD Learning)

++++ Think about how to create a graph by code. Then there is no need to drag and drop and adapt the things

