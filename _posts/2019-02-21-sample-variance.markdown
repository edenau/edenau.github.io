---
title: "️➖ Why Sample Variance is Divided by n-1"
layout: post
date: 2019-02-21 23:00
published: true
headerImage: false
tag:
- statistics
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

While the expected value of ***x_i*** is ***μ***, the expected value of ***x_i²*** is more than ***μ²***. It is because of the non-linear mapping of square function, where the increment of larger numbers is larger than that of smaller numbers. For instance, set (1,2,3,4,5) has mean 3 and variance 2. By squaring every element, we get (1,4,9,16,25) with mean 11=3²+2. **We need this property at a later stage.**

## Estimators
Since we do not know the true population properties, we can try our best to define estimators of those properties from the sample set using a similar construction.

Let’s put a hat (***^***) on ***μ*** and ***σ²*** and call them ‘pseudo-’ mean and variance, and we define it in the following manner:

![1]({{ site.url }}/assets/posts/variance/pseudo@2x.png)

The definitions are a bit arbitrary. You can, in theory, define them in much fancier ways and test them, but let’s try the most straightforward ones. We define pseudo-mean ***^μ*** as the average of all samples ***X***. It feels like this is the best that we can do. A quick check on the pseudo-mean suggested that it is an **unbiased population mean estimator**:

![1]({{ site.url }}/assets/posts/variance/mean_est@2x.png)

Easy. Nevertheless, true sample variance depends on the population mean ***μ***, which is unknown. We, therefore, substitute it with pseudo-mean ***^μ*** as shown above, such that pseudo-variance is dependent on pseudo-mean instead.

<div class="breaker"></div> <a id="1"></a>

# 1. Degree of Freedom
Assume we have a fair dice, but no one knows it is fair, except Jason. He knows the population mean ***μ*** (3.5 pts). Poor William begs for getting the statistical property, but Jason won’t budge. William has to make estimations by sampling, i.e. rolling the dice as many times as he can. He gets tired after rolling it three times, and he got 1 and 3 pts in the first two trials.

Given the true population mean ***μ*** (3.5 pts), you would still have no idea what the third roll was. However, if you knew the sample mean ***^μ*** was 3.33 pts, you would be certain that the third roll was 6, since (1+3+6)/3=3.33 — quick maths.

In other words, the sample mean encapsulates exactly one bit of information from the sample set, while the population mean does not. Thus, the sample mean gives ***one less degree of freedom*** to the sample set.

This is the reasons that we were usually told, but this is not a robust and complete proof of why we have to replace the denominator by ***(n-1)***.

<div class="breaker"></div> <a id="2"></a>

# 2. Source of Bias
Using the same dice example. Jason knows the true mean ***μ***, thus he can calculate the population variance using true population mean (3.5 pts) and gets a true variance of 4.25 pts². William has to take pseudo-mean ***^μ*** (3.33 pts in this case) in calculating the pseudo-variance (a variance estimator we defined), which is 4.22 pts².

In fact, pseudo-variance always ***underestimates*** the true sample variance (unless sample mean coincides with the population mean), as pseudo-mean is the ***minimizer*** of the pseudo-variance function as shown below.

![1]({{ site.url }}/assets/posts/variance/argmin@2x.png)

You can check this statement by the first derivative test, or by inspection based on the convexity of the function.

This suggests that **the usage of pseudo-mean generates bias**. However, this does not give us the value of bias.

<div class="breaker"></div> <a id="3"></a>

# 3. Bessel’s Correction
Our sole goal is to investigate how biased this variance estimator ***^μ*** is. We expect that pseudo-variance is a biased estimator, as it **underestimates** true variance all the time as mentioned earlier. By checking the expected value of our pseudo-variance, we discover that:

![1]({{ site.url }}/assets/posts/variance/proof@2x.png)

One step at a time. The expected value of ***x_j*** ***x_k*** (as shown below) depends on whether you are sampling different (independent) samples where ***j≠k***, or the same (definitely dependent in this case!) sample where ***j=k***. Since we have ***n*** samples, the possibility of getting the same sample is ***1/n***. Therefore,

![1]({{ site.url }}/assets/posts/variance/jk@2x.png)

Remember the expected value of ***x_i²*** mentioned at the start? By expanding ***^μ***, we have

![1]({{ site.url }}/assets/posts/variance/explain@2x.png)

Substitute these formulae back in, and we find out that the *expected value* of pseudo-variance is NOT population variance, but ***(n-1)/n*** of it. Since the scaling factor is smaller than ***1*** for all finite positive ***n***, this again proves that our pseudo-variance underestimates the true population variance.

In order to tune an unbiased variance estimator, we simply apply *Bessel’s correction* that makes the expected value of estimator to be aligned with the true population variance.

![1]({{ site.url }}/assets/posts/variance/s@2x.png)

There you have it. We define ***s²*** in a way such that it is an ***unbiased sample variance***. The ***(n-1)*** denominator arises from Bessel’s correction, which is resulted from the ***1/n*** probability of sampling the same sample (with replacement) in two consecutive trials.

As the number of samples increases to infinity ***n→∞***, the bias goes away ***(n-1)/n→1***, since the probability of sampling the same sample in two trials tends to ***0***.

Thank you for reading!
