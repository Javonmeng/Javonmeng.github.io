---
layout: poster
title: Reinventing Video Streaming for Distributed Vision Analytics
date: 2019-06-09 18:02:59
tags: 
    - video analytics
    - server-driven
    - superposition coding
categories: Papers
---
## Abstract
- in this paper, we call upon this community to similarly develop custom streaming protocols for better analytics quality (accuracy) of vision analytics (deep neural networks).
- We highlight new opportunities to substantially improve the tradeoffs between bandwidth usage and inference accuracy
- the new protocols can directly optimize the inference accuracy while minimizing bandwidth usage

## Introduction
- Video analytics, in contrast, focus onlyon specific video segments with queried objects of interest. Thus, a streaming protocol is optimal as long as the frames with the queried objects of interest are sent at sufficiently high resolution.

## What’s new about video streaming in vision analytics?
- the video needs to be streamed to cloud or edge servers (as shown in Figure 1) to offload the computationally intensive DNN-based vision tasks
- A video streaming protocol can be seen as a function that applies some compression operation p(·) (e.g., resolution downsizing and frame sampling) on the original video v, and sends the compressed video p(v) to the server.
- color changes in background trigger the client to send many frames while the actual objects of interest are static
- This client-driven workflow, however, suffers from the fundamental limitation that, without any input from the server, it is difficult to predict precisely what the server needs, i.e., how much information is needed to encode all objects of interest. 

## A case for a server-driven approach
- maximizes the analytics accuracy by giving the server full (or partial) control over what to send from the client to the server
- The oracle protocol (unrealistically) assumes that the server can access the original video and pick the optimal values for each of the control knobs–frame selection, area cropping, resolution, and compression level. That is, it selects the exact frames where the objects move, crops out only the spatial regions that contain objects, selects the minimal resolution at which the server-side logic can detect the object, etc.
- Challenge
  - the inference result on low-quality video (e.g., low resolution, low frame rate, or aggressive compression), while not accurate, is sufficient to reveal what additional information is needed by the NN model to achieve higher accuracy.
    -  Inference results on low-resolution frames can indicate where objects are likely to appear
    -  Inference results on sparsely sampled frames can indicate which frames are likely to contain objects.

## Towards a practical design
- Formalizing server-driven protocol
  - Iterative workflow using superposition coding
    -  the client first sends a base-quality video (e.g., low resolution or frames sampled sparsely in time) to the server, and if the inference output is not confident, the server decides to get additional information so that it can recover higher-quality video by “adding” the new data to the base-quality video it already has.
    - superposition coding
  - Server-driven protocol as active learning
    - The workflow of a server-driven protocol closely resembles an active learning process 
    - Sending a video segment at a high resolution or a higher sampling rate to the server can be seen as labeling more bits in the region
- A concrete design and its early promise
  - SimpleProto, a simple server-driven protocol that achieves substantial improvement.

## Open issues
- Tighter integration with client/server analytics stack
- Leveraging insights from computer vision literature
## Expressions
- ubiquitously available cameras 无所不在的摄像头
- its high cost prohibits a scalable solution, especially ...prohibits阻止了