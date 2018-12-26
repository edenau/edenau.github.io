---
title: "🚲 Optimizing station-based bike sharing systems"
layout: post
date: 2018-05-02 12:00
tag:
- optimization
- MATLAB
- system
projects: true
hidden: true # don't count this post in blog pagination
description: "A generic model and a distributed algorithm for optimization of station-based bike sharing systems"
category: project
author: edenau
---

***Source code is now available on <a href="https://github.com/edenau/Bike-Sharing-Systems-Optimization" target="_blank">Github</a>. Check it out!***

# Introduction

A growing number of public bike sharing systems have been installed around the globe, offering an alternative carbon-free mode of transport in already congested metropolitan areas. These systems often suffer from low service level where customers are unable to rent/return bicycles in empty/full stations. These events occur mainly due to (i) asymmetric traffic flow in peak hours, (ii) the intrinsic stochastic nature of the system where service demand fluctuates for different reasons, and (iii) lack of resources including bikes and station docks. The common solution would be deploying trucks to relocate bikes in the system using static/dynamic rebalancing algorithms, which contradicts the 'green' bike sharing principle.

One of the most promising and popular alternatives is to involve customers in the bike redistribution process. It involves (i) developing an *ideal* optimal redistribution strategy, and (ii) deploying a cost effective model incentivizing users. While the latter reward scheme has gained increasing attention in the field, this thesis deals with developing an optimal bike redistribution tactic with the assumption of 100% customer co-operation (thus ideal).

While greedy and centralized methods perform well in small and moderate sized systems respectively, they suffer from extremely poor optimality and runtime performance respectively in large-scale systems. We therefore propose a novel truncated distributed-greedy algorithm that leverages distributed computing techniques. It is verified numerically, for the case where no strict computation convergence of the adopted distributed methodology is guaranteed, that it is able to strike a balance between optimality and runtime for large systems. Privacy is also ingrained in our approach. Whenever users send a request to the peer-to-peer network, only their targeted destination node will receive the command and process it, without explicitly sharing the information among other nodes in network. The efficacy of the developed algorithms are all verified numerically via extensive simulation study.
