---
title: >-
  VIA ：Improving Internet Telephony Call Quality Using  Predictive Relay
  Selection
date: 2019-05-14 20:41:50
tags: 
    - VoIP
    - Relay Selection
    - Data-Driven
categories: Papers
---
## Introduction 
- Scenario
The use of Internet for voice call, i.e. Skype, WhatsApp, WeChat, FaceTime
- Introduction
  - VIA uses an approach called prediction-guided explorationto decide which calls to relay and to pick the relay(s).  是否中继以及选择哪个中继
  - It makes these decisions with performance information from call history, which tends to be limited and highly skewed. 利用history信息，但又说history信息非常有限且偏颇
  - even though available performance information from call history may not suffice to accurately predict the best relay for each call, it can nevertheless help identify a small subset of relay choices that contains the best relay.根据 history信息给出一个包含最优解的集合。
  - we use performance information from call history to filter out all but the most promising (top-k) relaying alternatives, from which, we then employ an online exploration-exploitation strategy to identify the optimal relay path, while staying within the relaying budget. Top-k choices，然后寻找optimal relay path, 好的想法。top-k中的k是一个可选参数，当k=1时就是直接给选择了，和我们平时理解采用的学习方法一致，但之前提到根据观察这并不一定是最优解；当k大时，趋于问题的规模又会类似搜索的方法，维度带来的复杂度成为问题。这两种思想的trade off。
- Contributions
  - 分析了net performance对通话质量的影响
  - 量化了a managed overlay network对于通话质量的改进
  - 提出了relay selection algorithm， 性能是close to optimal
## VoIP Performance in the Wild
- Dataset
  - dataset from Skype, consist of a sampled set of 430 million audio calls 好大的数据集，羡慕
  - 数据集中包含路由使用默认路径(e.g., BGP-derived)和使用数据中心的managed relay nodes这两种
  - Each call is associated with three metrics of network performance: (*i*) round-trip time (RTT), (*ii*) loss rate, and (*iii*) jitter。这里没有带宽，作者给出的解释是VoIP是低数据率业务。
  - 对call quality离散的标注，from 1 (worst) to 5 (best). 1 or 2 被认为是 "poor quality"，用来计算"Poor Call Rate (PCR)"
  - 除了PCR，[17] 也提供了一种分析audio call quality的工具， *Mean Opinion Score (MOS)*
- 分析call quality 与  Network performance
   - use-perceived quality对network performance (RTT, loss rate and jitter)敏感。
   - 对数据分析，给出了统计的over 15%的PCR案例各个netwoek performance 参数的门限值。( RTT over 320ms, or loss over 1.2%, or jitter more than 12ms ). 
   - 给出了poor network rate的定义： performance on the metric is worse than the chosen thresholds: RTT  320ms, loss rate  1.2%, jitter  12ms. One of our goals is to reduce PNR of each individual metric
 - Call with poor networks有什么共同的特点？spatial and temporal
   - Spatial patterns: 
      - International 比Domestic 的PNR更高，Inter-AS比Intra-AS的PNR更高。This means that localized solutions that fix a few bad ASes or AS pairs, e.g., informing the AS administrators or the clients directly regarding their ISPs, are not sufficient.
      - This suggests that poor network performance is quite widespread, highlightingthe suitability of a globally deployed overlay network thatprovides high performance inter-connection between overlay nodes.
    - Temporal patterns:
      - we need to dynamically decide if a call should use default Internet routing or be relayed.
  ## VIA
- VIA Architecture
  - consists of relay nodes placed at globally distributed datacenters, such as those run by Amazon, Google, and Microsoft
  - 在caller和callee存在一个controller, 由它来根据historical calls和policy constraints来决定是否中继，如何中继。 
  - 历史测量数据由skype client周期性地提供给controller。controller不需要主动monitor relay nodes，Skype clients会提供end-to-end measurements。
  - relay options: the default (direct) path, a bouncing relay path (one-hop) , or a transit relay path (pair)
  - Need for dynamic relay selection:  The optimal relaying option for 30% of AS pairs lasts for less than 2 days, and only 20% of AS pairs have the same optimal relay option for more than 20 days. 在通信研究中也是 The relay selection should be done dynamically, rather than statically.
- VIA Relay Selection
  - We describe two classes of strawman appro aches— purely predictive and exploration-based — and highlight limitations of both classes. We then present the core intuition behind our relay selection algorithm, called predictionguided exploration and then describe the solution.和之前在Introduction中提到的一样，基于预测一次到位和基于探索的方法结合。
  - Our goal is to assign each call to a particular relaying option.
  - $\mathop{\arg\min}\limits_{Assign\in R^C} \sum\limits_{c\in C}Q(c, Assign(c))$, 全局优化，不是针对单个call优化。
  - Strawman approacher
    - Exploration-based: for every AS-pair and every possible relaying option r, we will explicitly use some of the calls to explore the option and measure the performance, Q(c, r).
    - Prediction-based: we can use suitable prediction algorithms to predict theperformance Q(c, r) for every combination, and select the option that has the best predicted performance.
    - 这两个方法预测*Q(c,r)*竟然都不好， two key reasons:
      - there is a substantial difference in the number of call samples available across different source-destination pairs, both for the direct path and for the various relayed paths.
      - To estimate Q(c, r), we need a significant number of samples before the empirically observed.
- 没有见过的source-destination配对怎么处理
  - Network tomography can help estimate the performance of each network segment. Then, by stitching together the estimates for the individual segments, we can estimate the performance of a path not seen before
- Prediction:
  - $Pred_{mean}(s,d,r)$
  - $Pred_{sem}(s,d,r)$ standard error of mean
  - 95% confidence bounds
    - $Pred_{lower}(s,d,r)=Pred_{mean}(s,d,r)-1.96Pred_{sem}(s,d,r)$
    - $Pred_{upper}(s,d,r)=Pred_{mean}(s,d,r)+1.96Pred_{sem}(s,d,r)$
- Pruning to get top-k choices
  - Instead of using a fixed value of k, VIA dynamically decides k based on the lower and higher confidence bounds for each relay r on the particular source-destination pair s and d.
  - top-k中k的中继的上界比不在top-k中的下界还要小
- Exploration-exploitation step
  - assigns a fraction of calls to explore different relay options ($\epsilon$-greedy) and the rest to exploit the best decision
  - UCB1 algorithm: 数据variance大时直接Normalization会有问题，相同的case会有不同的decision.UB1 Algorithm用来处理exploration.
  - 不太理解怎么得到$C_r\leftarrow\{c^\prime|Assign(c^\prime)=r\}$：是实时获取当前使用中继r的$c\prime$吗？
  - 引入了general exploration来处理dynamic环境
- Budgeted relaying
  - 使用中继的calls所占比例要有限制，e.g., 30%
  - It decides to relay a call only if the benefit of relaying is sufficiently high.
  - It decides to relay a call only if the expected benefit is above the Bth percentile benefit.
## Evaluation
- Evaluation
  - The calls are replayed in the same chronological as in the trace.
  -  the relaying options considered for a call are only those with at least 10 call samples on at least 5 relay options。为了使中继的选择更优信服力
- Benefits of prediction-guided exploration
  - instead of picking a fixed number top candidates, VIA pick top candidates by taking variance of prediction into account
- Practical relaying factors
  - Relaying budget: we define budget as the maximum fraction of calls being relayed. Budget-aware VIA relays a call only when the benefit is larger than a threshold. 没看懂这里Oracle是那个参数如何控制budget的。
  - Relaying decision granularities:First, making decision at granularities coarser than a per AS pair results in a smaller reduction in PNR; Second, making decisions on finer granularities does not help much, though for a different reason.所以最好的空间粒度是AS对
  - Relay usage: Removing 50% of the (least used) relays causes little drop in VIA’s gains.
- Real-world controlled deployment
  - We implemented and deployed a prototype containing the relevant components of VIA at a small scale using modified Skype clients and using Skype’s production relays. The central controller of our prototype (Figure 7), deployed on the public Microsoft Azure cloud, aggregated performance measurements from instrumented Skype clients and implemented the relay selection algorithm. 这也太酷了，真的部署了还对Skype client进行了modification.
## Expressions
- Expressions
  - To bridge this gap, we analyze ...
  -  largely ineffective
  -  pragmatic  practical 务实的，实用的
  -  spanning 135 million users across 126 countries
  -  But contrary to our intuition
  -  envision 预想
  -  whose overhead can be prohibitive at large scale overhead 在大规模问题中的痛点
  -  close-to-optimal
  -  For each network metric, we bin calls based on theirnetwork performance and show the PCR of the calls withineach bin.
  -  Strawman approaches : The strawman is not expected to be the last word; it is refined until a final model or document is obtained that resolves all issues concerning the scope and nature of the project. 中间过渡的方法
- Confusion
  - ASes是啥？ area states?
  - Trace-driven analysis shows that an oracle-based overlay can potentially improve up to 53% of calls whose quality is impacted by poor network performance. oracle performance是怎么来的,oracle是说搜索遍历下的最优情况吗？