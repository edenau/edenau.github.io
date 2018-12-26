---
title: "📷 Camera calibration"
layout: post
date: 2017-01-10 12:00
tag:
- camera
- MATLAB
projects: true
hidden: true # don't count this post in blog pagination
description: "A simple pinhole camera calibration model"
category: project
author: edenau
---

***Source code is now available on <a href="https://github.com/edenau/Camera-Calibration" target="_blank">Github</a>. Check it out!***

# Introduction

Camera is a very common tool to capture our daily lives. Pinhole camera was invented that provided an economical option compare to other expensive ones. However, due to its design, the images acquired are distorted. To eliminate this inherent deficiency, one can use calibration tool to obtain a relatively accurate representation of the world captured.

## Table of Contents
- [Objectives](#objectives)
- [Outline](#outline)
- [Algorithm Performance](#alg)
- [Discussion](#discussion)
- [Conclusion](#conclusion)

<div class="breaker"></div> <a id="objectives"></a>

# Objectives

The main point of camera calibration is to reconstruct the world by estimating the parameters of the camera such as focal length, skewness, or in general, intrinsic and extrinsic parameters. By doing so, we can achieve:
-	Image reconstruction
-	Distance and position measurement

<div class="breaker"></div> <a id="outline"></a>

# Outline

A known planar object is used for calibration. A Tsai Tile calibration pattern, which consists of multiple identical tiles, is used. Tile corners are <img src="https://latex.codecogs.com/svg.latex?[x \ y]'" /> in object coordinate system. By taking a single picture, the corresponding coordinates of tile corners <img src="https://latex.codecogs.com/svg.latex?[u \ v]'" /> on the image frame can then be measured. Thus, we have a set of ‘Correspondence’ <img src="https://latex.codecogs.com/svg.latex?[u \ v \ x \ y]'" /> which has its homography **H** where

<img src="https://latex.codecogs.com/svg.latex?S\ [u \ v \ 1]'=\mathbf{H}\ [x \ y \ 1]'" />

However, homography **H** is not exactly the same for every Correspondence due to the presence of noise and outliers in <img src="https://latex.codecogs.com/svg.latex?[u \ v]'" />. Despite having an inconsistent solution, a universal homography H can be obtained by minimising a cost function, or in other words, an error function.
The intrinsic model of the camera (i.e. K-matrix) can be estimated using the property of rotation matrix, since

<img src="https://latex.codecogs.com/svg.latex?\mathbf{H}=\lambda\ \mathbf{K}\ [\mathbf{r_1} \ \mathbf{r_2} \ \mathbf{t}]" />

Multiple views are needed, and **K** can be estimated using a similar cost-minimising technique. Lastly, optimize K, which will be discussed later.

<div class="breaker"></div> <a id="alg"></a>

# Algorithm Performance

## Camera Simulation
### Camera model


<div class="breaker"></div> <a id="discussion"></a>

# Discussion

Real cameras deviate from the simple pinhole model. Non-linear lens distortion is present due to the limitations in manufacturing lens. Straight lines in the real world are curved through real lens. Fortunately, this distortion is always radially symmetric with radius *r*. Debevec (1996, pp.31-37) demonstrates that the centre of distortion <img src="https://latex.codecogs.com/svg.latex?(c_x, c_y)" /> can easily be observed by using a simple edge detector and by shrinking the image in both directions. The radial transformation function is modelled in the form of

<img src="https://latex.codecogs.com/svg.latex?\mathcal{F}(r)=r(1+k_1 r^2+k_2 r^4+\dots)" />

where <img src="https://latex.codecogs.com/svg.latex?(k_1,k_2)" /> can be estimated by fitting in a polynomial with least-squares minimisation. By applying it to images, they can all be undistorted before putting them into calibration algorithm.

<div class="breaker"></div> <a id="conclusion"></a>

# Conclusion
