---
title: "Learning Single Camera Depth Estimation using Dual Pixels"
date: 2021-02-08
categories:
  - blog
tags:
  - literature review
  - dual-pixels
---

In this paper, the authors investigate the the use of dual-pixel (DP) sensors to estimate depth from a single camera. 

Illustrations of modern Bayer sensor and DP sensors are shown below.

<!-- ![dual pixel sensor](/assets/sensors.jpg) -->

<img src="/assets/images/learning_depth_estimation_using_DP/sensors.JPG" alt="dual pixel sensors">

A modern Bayer sensor consists of interleaved red, green, and blue pixels underneath a microlens array, while in dual-pixel sensors, pixels under each microlens is split in half, resulting in two images that act as a narrow-baseline stereo camera, much like a reduced light field camera. Hence, the resemblance of a stereo camera of the DP sensors can be exploited to estimate depth maps from a single camera.

DP sensors work by splitting each pixel in half, such that the left half integrates light over the right half aperture and vice versa. This phenomenon can be illustrated in the figure below.

<img src="/assets/images/learning_depth_estimation_using_DP/views.JPG" alt="views">

From (a), we can see that light refracting through the left half of the aperture (dark blue and orange rays) arries at the right half of each dual-pixel.

From both (a) and (b), the authors observed that while different focus distance and different depths, they can still yield the same DP and RGB images. This is caused by an affine transformation on inverse depth. 

Since consumer smartphone cameras are not reliable in recording camera intrinsic metadata, the affine transformation ambiguity could not be resolved. Hence, the authors seek to train a CNN to estimate the inverse depth only up to an affine transformation.

Two different methods of training with view supervision is introduced. The first one (3d assisted loss) requires access to ground truth inverse depth D* and corresponding per-pixel confidence C, then the unknown affine mapping can be found by solving

<img src="/assets/images/learning_depth_estimation_using_DP/3d_assisted_loss.JPG" alt="3d assisted loss">

The second one (folded loss) requires no ground truth depth and the loss function is defined as follows:

<img src="/assets/images/learning_depth_estimation_using_DP/folded_loss.JPG" alt="folded loss">

and then using gradient descent the parameters can be optimize by solving

<img src="/assets/images/learning_depth_estimation_using_DP/folded_loss_opt.JPG" alt="folded loss optimize">
