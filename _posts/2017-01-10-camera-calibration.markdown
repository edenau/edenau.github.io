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

A known planar object is used for calibration. A Tsai Tile calibration pattern, which consists of multiple identical tiles, is used. Tile corners are <img src="https://latex.codecogs.com/svg.latex?[x \ y]'" /> in object coordinate system. By taking a single picture, the corresponding coordinates of tile corners [■(u&v)]' on the image frame can then be measured. Thus, we have a set of ‘Correspondence’ [■(u@v@x@y)] which has its homography H where




<div class="breaker"></div> <a id="alg"></a>

# Algorithm Performance

<div class="breaker"></div> <a id="discussion"></a>

# Discussion

<div class="breaker"></div> <a id="conclusion"></a>

# Conclusion
