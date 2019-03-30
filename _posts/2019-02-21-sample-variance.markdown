---
title: "️➖ Why Sample Variance is Divided by n-1"
layout: post
date: 2019-02-18 23:00
published: true
headerImage: false
tag:
- Statistics
category: blog
author: edenau
description: "Explaining high school statistics that your teachers didn’t teach
"
# jemoji: '<img class="emoji" title=":open_file_folder:" alt=":open_file_folder:" src="https://assets.github.com/images/icons/emoji/unicode/1f5c2.png" height="20" width="20" align="absmiddle">'
---

***This post is now available on <a href="https://towardsdatascience.com/why-sample-variance-is-divided-by-n-1-89821b83ef6d" target="_blank">Towards Data Science — Medium</a>. Check it out!***

![1]({{ site.url }}/assets/posts/variance/intro@2x.png)

If you are reading this article, I assume you have encountered the formula of sample variance, and kind of know what it represents. But it remains a mystery that why the denominator is ***(n-1)***, not ***n***. Here’s why.

## Table of Contents
- [Settings](#settings)
- [1. Degree of Freedom](#1)
- [2. Source of Bias](#2)
- [3. Bessel’s Correction](#3)

## Terminology

*Population*: a set that contains **ALL** members of a group

*Sample*: a set that contains **some** members of a population (technically a multi-subset of a population)

*Independent and identically distributed (i.i.d.) random variables*:
An assumption that all samples (a) are **mutually independent**, and (b) have the same probability distribution.

*Central limit theorem*:
The sampling distribution of i.i.d. random variables tend toward a **normal (Gaussian) distribution** when the sample size is large enough.

*Expected value*:
**Long-run average value** of repetitions of the same experiment.

*Unbiased estimator*:
The unbiased estimator’s **expected value** is equal to the true value of the parameter being estimated. In other words, the distributions of unbiased estimators are centred at the correct value.

<div class="breaker"></div> <a id="settings"></a>

# Settings

Given a large Gaussian population distribution with an **unknown** population mean ***μ*** and population variance ***σ²***, we draw ***n*** i.i.d. samples from the population, such that for each sample ***x_i*** from a set ***X***,

![1]({{ site.url }}/assets/posts/variance/true@2x.png)
