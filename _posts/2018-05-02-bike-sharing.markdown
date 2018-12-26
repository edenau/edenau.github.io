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

***Paper coming soon!***

# Abstract

We consider the problem of low service level in bike sharing systems where customers are unable to rent or return bicycles in empty or full docking stations respectively. We construct a generic data-driven simulation model for station based bike sharing systems and we verify that these events are induced by the asymmetric traffic flow in morning and evening peak hours over weekdays. Bicycles are redistributed such that fewer customers encounter no service events. As it becomes a common practice for system operators to incentivize customers in the bike redistribution process, we develop a generic redistribution optimization framework and propose a novel multi-agent consensus-based iterative distributed algorithm that can provide optimal redistribution strategies. We verify the efficacy of our algorithm numerically, for the case where no strict computation convergence of the adopted distributed methodology is guaranteed, that it is able to strike a balance between optimality and runtime for very large systems. The algorithm also produces globally optimal solutions given sufficient computing power. The solutions can easily be disaggregated into individual requests for each user, providing incentivized suggestions of actions need to be taken by every customer. Our novel algorithm is amenable to practical implementation.
