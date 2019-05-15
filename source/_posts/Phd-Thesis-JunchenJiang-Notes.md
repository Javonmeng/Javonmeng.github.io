---
title: Phd_Thesis_JunchenJiang_Notes
date: 2019-05-15 17:10:57
tags:
    - QoE, DDN
categories: Papers
---
## Abstract

- Keywords: Internet applications, Quality of Experience, Data-Driven Networking, user-perceived
- The key contribution of this dissertation is to bridge the long-standing gap between the visibility to user-perceived QoE and the visibility to network conditions by a data-driven approach.
-  improve QoE by maintaining a global view of up-to-date network conditions based on the QoE information collected from many endpoints.
-  session-level
-  our solutions can yield substantial QoE improvement and consequently higher user engagement for video streaming and Internet telephony than existing solutions as well as many standard machine learning solutions.

## Introduction about DDN

- two broadly defined classes of prior approaches to optimize QoE
  - In-network approaches: better designs of in-network devices (e.g., routers, switches, and middleboxes) and routing schemes. lack of visibility to userperceived QoE
  - Endpoint approaches: using intelligent logic running at individual endpoints to react to changes in network conditions. limited by individual endpoints’ local visibility to network conditions
- DDN's features
  - Compared to in-network approaches, DDN can monitor client-side applications and thus can directly optimize user-perceived QoE, rather than indirect low-level metrics. Compared to endpoint-based approaches, DDN compensates the lack of visibility of network conditions at one endpoint by a real-time, **global view of QoE observed from many endpoints**, thus addressing the key limitation of the endpoint adaptation.
  - improve QoE by using **a logically centralized controller** (as illustrated in Figure 1.1) which maintains a global view of real-time network conditions by gathering QoE measured from many application sessions and uses this global view to make optimal decisions regarding the adaptation of individual sessions
  - ![DNN's features](Phd-Thesis-JunchenJiang-Notes/DDN1.PNG)
  - use realworld deployment and large-scale emulation to demonstrate that our solutions can substantially improve the QoE of video streaming and Internet telephony.我觉得我们这里根本做不到
  - 几种方法： 
    - CFA: a video QoE prediction system that can accurately predict the quality of a video client if it uses certain CDN and bitrate
    - DDA：a throughput prediction system to accurately predict end-to-end throughput at the beginning of a video session to help determine the highestyet-sustainable initial bitrate
    - group-based exploration-exploitation：decomposes the global exploration-exploitation process of all sessions into subprocesses
    - guided exploration: instead of exploring the whole decision space of all possible relay choices, learns a small set of promising relays for each AS pair based on long-term (e.g., daily) historical data, and explores these promising relays using most calls in near real time.
  -  video QoE is measured by buffering time, start-up delay, and average bitrate, each of which has been shown to have strong correlation with user engagement in multiple studies.While subjective metrics can directly reflect user satisfaction, we chooseto use these objectively measurable metrics as a proxy for real user satisfaction for two reasons: (1) they can be passively collected en masse by instrumentation code running in client devices without any user input, and (2) they are less noisy than subjective metrics which can be affected by factors (e.g., content or personal preference) beyond the scope of this dissertation. QoE和选择objectively measurable metrics判定用户满意度的原因。

## QoE

## Expressions
- Expressions
  -  it has become of paramount importance that ...
  -  Despite the intense research towards better Internet QoE ...
  -  We elaborate on these quality problems in Chapter 2.