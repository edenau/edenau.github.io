---
title: "️🗺️ Visualizing bike mobility in London using interactive maps and animations"
layout: post
date: 2019-01-07 23:00
published: true
headerImage: false
tag:
- visualization
- bike
- Python
- DataScience
category: blog
author: edenau
description: "visualizing bike sharing systems"
# jemoji: '<img class="emoji" title=":open_file_folder:" alt=":open_file_folder:" src="https://assets.github.com/images/icons/emoji/unicode/1f5c2.png" height="20" width="20" align="absmiddle">'
---

# Introduction

Bike sharing systems have become popular means of travel in recent years, providing a green and flexible transportation scheme to citizens in metropolitan areas. Many governments in the world have seen this as an innovative strategy that could potentially bring a number of societal benefits. For instance, it could reduce the use of automobiles and hence reduce greenhouse gas emission and alleviate traffic congestion in city centres.

> <a href="http://content.tfl.gov.uk/attitudes-to-cycling-2016.pdf" target="_blank">Reports</a> have shown that 77% of Londoners agree that cycling is the fastest way to make short-distance journeys. In the long run, it might also help increase the life expectancy in the city.

I have been working on a data-driven cost-effective algorithm for optimizing (re-balancing) the system of Santander Cycles, a public bicycle hire scheme in London, which I should make a post about it once my paper is published (more info here). Before really working on the algorithm, I had to delve into loads of data and it would really help if I could somehow ***visualize*** them.

>> TL;DR - Let's see how we can visualize a bike sharing system using graphs, maps, and animations.

*You can find web maps in this <a href="https://edenau.github.io/maps/" target="_blank">page</a>. Most maps, animations, and source codes are available on <a href="https://github.com/edenau/maps" target="_blank">GitHub</a>. My work is inspired by <a href="https://blog.prototypr.io/interactive-maps-with-python-part-1-aa1563dbe5a9" target="_blank">Vincent Lonij</a> and <a href="https://towardsdatascience.com/master-python-through-building-real-world-applications-part-2-60d707955aa3" target="_blank">Dhrumil Patel</a>. Check out their posts!*

## Table of Contents
- [More about Data - the Boring Bit](#data)
- [Bar charts](#bar)
- [Interactive Maps](#interactive)
- [Density Maps](#density)
- [Connection Maps](#connection)
- [Animation](#animations)
- [Conclusions](#conclusions)
- [Remarks](#remarks)

<div class="breaker"></div> <a id="data"></a>







<a href="https://edenau.github.io/maps/static" target="_blank">Static Map</a>

https://edenau.github.io/maps/morning

https://edenau.github.io/maps/evening
