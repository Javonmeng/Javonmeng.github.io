---
title: a summary of recent 4 blogs of paper
date: 2019-06-11 15:38:41
tags:
    - summary
categories: Papers
---
## 数据中心的数据拷贝路径的选择(BDS: A Centralized Near-Optimal Overlay Network for Inter-Datacenter Data Replication) EuroSys 2018 
### Abstract
- pair-wise inter-DC data transfer is good, but insufficient to optimize bulk-data multicast. this is due to fail to explore the capability of servers to store-and-forward data as well as the rich inter-DC overlay paths that exist in geo-distributed DCs.
- fully centralized architecture: a central controller to maintain an up-to-date global view of data delivery status of intermediate servers
- control algorithm: selection of overlay paths and scheduling of data transfers

### Overview of BDS
- Centralized control: At a high level, BDS uses a centralized controller that periodically pulls information (e.g.,data delivery status) from all servers, updates the decisions regarding overlay routing, and pushes them to agents running locally on servers
- The key to realizing centralized control: the design of BDS performs a trade-off between incurring a small update delay in return for the near-optimal decisions brought by a centralized system.
- Tricks:
  - Decoupling scheduling and routing
  - it groups the blocks with the same source and destination pair to reduce the problem size
  - the routing step is reduced to a mixed-integer LP problem
  - it uses the improved fully polynomial-time approximation schemes (FPTAS) to optimize the dual problem of the original problem and works out an ε-optimal solution.

## 视频监控分析中视频传输参数(码率)的选择 (Reinventing Video Streaming for Distributed Vision Analytics) HotCloud 2018 

### Abstract
- we call upon this community to similarly develop custom streaming protocols for better analytics quality (accuracy) of vision analytics (deep neural networks).
- improve the tradeoffs between bandwidth usage and inference accuracy
- the new protocols can directly optimize the inference accuracy while minimizing bandwidth usage

### Practical Design
- Server-driven protocol as active learning:
  - The workflow of a server-driven protocol closely resembles an active learning process
  - Sending a video segment at a high resolution or a higher sampling rate to the server can be seen as labeling more bits in the region
- Iterative workflow using superposition coding
  - the client first sends a base-quality video (e.g., low resolution or frames sampled sparsely in time) to the server
  - if the inference output is not confident, the server decides to get additional information so that it can recover higher-quality video by “adding” the new data to the base-quality video it already has.
  - superposition coding

## 视频监控分析中神经网络架构的选择 (Chameleon: Scalable Adaptation of Video Analytics) SIGCOMM 2018 
### Abstract
- to balance resource and accuracy by selecting a suitable NN configuration (e.g., the resolution and frame rate of the input video)
- dynamically picks the best configurations for existing NN-based video analytics pipelines.
- tradeoff: adapting configurations frequently can reduce resource consumption with little degradation in accuracy, but searching a large space of configurations periodically incurs an overwhelming resource overhead that negates the gains of adaptation.

### Overview of Chameleon
- quantify the impact of spatial and temporal correlations on resource-accuracy tradeoffs
- present a suite of techniques to dramatically reduce the cost of periodic profiling by leveraging the spatial/temporal correlations.
- 处理large action space（利用action的时间和地理相关性聚类做决定；利用独立性将参数的指数空间$n^m$降低到O(nm)）
  - temporal correlations
    - the one with the lowest cost meeting the accuracy threshold α, might change frequently, the set of top-k best configurations (top-k cheapest configurations with accuracy ≥ α) tend to remain stable over time. Thus, we can dramatically reduce the search space by focusing on these top-k configurations.
  - spatial correlations
    - if two video feeds share similar characteristics, it is likely they will also share the same best configurations. Such cross-camera correlations provide an opportunity to amortize profiling cost across multiple camera feeds
  - the configuration knobs independently impact accuracy:
    - it lets us tune the resolution knob independent of the frame rate; this prunes a large part of the configuration space
    - it lets us estimate a configuration’s accuracy by combining its perknob accuracies; in particular, we can do this withoutrunning the expensive golden configuration.

## 在网络中应用DL待解决的问题 (Demystifying Deep Learning in Networking) APNet 2018 

### Abstract
- we call upon this community to similarly develop techniques and leverage domain-specific insights to demystify the DNNs trained in networking settings
- unleash the potential of DNNs in an explainable and reliable way

### Interpretability and Integrating Domain Knowledge
- Interpretability
  - saliency map
    -  a heatmap showing how much impact of each input feature on the output
    - When a DNN comprises convolutional layers, the Grad-CAM [14] method can be employed to generate the saliency map
- From input to high-level features
  - distinguishable input
    - we can actually explain correlation by DNN works internally: the intermediate neurons are only activated when the job size is over or below certain thresholds, i.e., the neurons effectively act as a “filter” of job size. 
  - Maximally activating intermediate neurons
- Integrating domain knowledge
  - key ideas
    - Better transferability
    - Better robustness
    - Better training efficiency
- Regulating DNNs with domain knowledge
  - The basic idea is to use two processes, called “teacher” and “student”, to regulate the training of a DNN f with a domain-specific logic g
  - Both f and g take input x and output a probability distribution y over the action space
  - student” trains f such that it minimizes the difference to the ground truth as well as the difference to the output of g.