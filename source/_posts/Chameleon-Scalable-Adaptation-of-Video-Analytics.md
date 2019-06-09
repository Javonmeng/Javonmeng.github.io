---
title: 'Chameleon: Scalable Adaptation of Video Analytics'
date: 2019-06-09 23:09:30
tags:
    - video analytics
    - configuration
    - top-k
    -  Persistent characteristics over time
    - Cross-camera similarities
    - Independence of configuration knobs
categories: Papers
---
## Abstract
- to balance resource and accuracy by selecting a suitable NN configuration (e.g., the resolution and frame rate of the input video)
- We present Chameleon, a controller that dynamically picks the best configurations for existing NN-based video analytics pipelines.
- tradeoff: adapting configurations frequently can reduce resource consumption with little degradation in accuracy, but searching a large space of configurations periodically incurs an overwhelming resource overhead that negates the gains of adaptation.
- keywords: vedio analytics, DNN, object detection
  
## Introduction
- The pipeline has several “knobs” such as frame resolution, frame sampling rate, and detector model (e.g., Yolo, VGG or AlexNet). We refer to a particular combinations of knob values as a configuration.
- The “best” configuration is the one with the lowest resource demand whose accuracy is over a desired threshold
- While prior video analytics systems [16,32, 33] profile the processing pipeline to minimize cost, they only do so once, at the beginning of the video。忽略了内在的动态性
- In fact, in our early experiments, the cost of periodic profiling  often exceeded any resource savings gained by adapting the  configurations. The main challenge, thus, is to significantly  reduce the resource cost of periodic configuration profiling.
- leverages domain-specific insights on the temporal and spatial correlations of these configurations.
- Temporal correlation: 
  -  The top-k best configurations (cheapest k configurations with accuracy above the desired threshold) tend to be relatively stable over time, for a small value of k.
  -   configurations that are very bad—very inaccurate and/or very expensive— remain so over long time periods.
  -  we can significantly prune the search space during profiling by learning which configurations are promising and which are unhelpful.
- Cross-camera correlation:
  - Once we identify a good set of configurations for one video feed, we can reuse it on similar feeds
- Independence of configurations:
  - for a given configuration  knob, the relationship between its resource and accuracy is  largely independent of the values of the other configuration  knobs.
- Contributions
  - We do a cost-benefit analysis for continuously adapting NN configurations compared to one-time tuning
  - We identify and quantify the impact of spatial and temporal correlations on resource-accuracy tradeoffs
  - We present a suite of techniques to dramatically reduce the cost of periodic profiling by leveraging the spatial/temporal correlations.

## RECOURCE-ACCURACY PROFILES
-  Object detection pipelines
   -  Pipelines
   - Configurations
     -  Pipeline A has 150 configurations and Pipeline B has 20.
- Performance of configurations
  - Accuracy
    - we compute accuracy of a single frame by comparing the detected objects with the objects detected by the most expensive configuration, which we call the golden configuration, using the F1 score
  - Cost
    - We use average GPU processing time (with 100% utilization) per frame as the metric of resource consumption
  - Performance impact
  - we show that the relationship between configuration and accuracy has great temporal variability, so dynamically adapting the configuration can lead to better resource-accuracy tradeoffs
-  Prohibitive profiling cost
   -  A sizable component of this profiling cost comes from running the golden configuration.
- Challenges in reducing profiling cost
  - profiling cost includes the cost of running the golden configuration, which itself can be prohibitively expensive.
  - simply increasing the update interval T does not help in practice.

## POTENTIAL OF ADAPTATION
- Quantifying potential
  - One-time update
  - Periodic update
  
## KEY IDEAS INCHAMELEON
-  if the NN configurations’ resource-accuracy tradeoff is affected by some persistent characteristics of the video, we can learn these temporal correlations to reuse configurations over time
   -   More generally, even though the best configuration, i.e., the one with the lowest cost meeting the accuracy threshold α, might change frequently, the set of top-k best configurations (top-k cheapest configurations with accuracy ≥ α) tend to remain stable over time. Thus, we can dramatically reduce the search space by focusing on these top-k configurations.
- if two video feeds share similar characteristics, it is likely they will also share the same best configurations. Such cross-camera correlations provide an opportunity to amortize profiling cost across multiple camera feeds
  - we can leverage the fact that similar videos tend to have similar distributions of best configurations. Then, we can use the most promising configurations from one camera—e.g., the top-k best configurations—to  guide the profiling of a spatially-related camera.
- we have experimentally observed that many of the configuration knobs independently impact accuracy, allowing us to avoid an exponential search这点很重要啊 说明参数的独立性
  - it lets us tune the resolution knob independent of the frame rate; this prunes a large part of the configuration space
  - it lets us estimate a configuration’s accuracy by combining its perknob accuracies; in particular, we can do this withoutrunning the expensive golden configuration.

## CHAMELEON TECHNIQUES
-  Overview
   -  Chameleon uses a solution inspired by greedy hill climbing  that exploits the independence of NN configuration knobs  to reduce the search space from exponential to linear (§5.4).
   -  Using this profiling method, it leverages the temporal persistence of configurations to learn their properties over time and amortizes the cost of profiling across multiple cameras by leveraging cross-video similarities
- Temporal incremental updates
- Cross-video inference
  - Grouping related videos
- Profiling a video segment
  - Chameleon leverages the empirically-driven assumption from §4.3 that the knobs of our NN configurations can be treated independently.
  - greedy hill climbing, where each knob is tuned while all other knobs are held fixed, reducing the search space from $n^m$ to O(mn).
  -  since our knobs exhibit monotonically increasing/decreasing performance, we can stop the loop when performance is good enough (or bad enough, depending on the search direction)

## EVALUATION
- dataset: a dataset of video streams from five traffic video cameras deployed in different intersections in a metropolitan area (Bellevue, WA).

## FUTURE WORK
- Network bandwidth
- Profiling on the edge
- Triggering the profiling

## Expressions
- improving inference accuracy often requires a prohibitive cost in computational resources
- factory floor 工厂车间，工人工作的地方