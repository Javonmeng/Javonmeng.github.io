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
- Video QoE metrics: buffering ratio; join time; average bitrate; join failures.
  - Dataset: based on client-side measurements of video quality from over 300 million sessions over a duration of two weeks. The unique feature of our dataset is that it is collected over 379 distinct content providers spanning diverse genres, both live and video-on-demand content, different content delivery platforms, different types of bitrate adaptation algorithms, and device/browser platforms.
- VoIP QoE metrics: poor call rate (PCR)
  - dataset: The dataset from Skype consists of a sampled set of 430 million audio calls drawn from a seven month period.
  - VoIP QoE distribution: The results show that a significant fraction of calls (over 15%) occur on paths with RTT over 320ms, or loss over 1.2%, or jitter more than 12ms, which we pick as our thresholds for poor performance.
  
## Internet Video
- 播放速率控制
  - Early Internet video technologies: Apple QuickTime, Adobe Flash RTMP (based on connection-oriented video transport protocols)
  - new generation of Internet video technologies: Microsoft Smooth Streaming, Apple's HLS, Adobe's HDS (全部都是HTTP-based adaptive streaming protocols)
- HTTP-based  adaptive streaming protocols' advantages: three points

## Internet Telephony
- managed overlay approach enjoys several practical benefits: three points

## Improve QoE
- Prior Work on Quality Optimization
- Taxonomy
  - Where in the network? endpoint-based solutions (clients, servers, caches)and in-network solutions (switches, routers).
  - Which level in the protocol stack? Lower levels and Application
- Design trade-offs
  - More visibility to user-perceived QoE
  - More visibility to network conditions
- In-Network Solutions
  - IP-layer support for QoS: novel QoS service architecture
  - Router-assist congestion control
  - SDN-based approach
- Endpoint Solutions
  - Overlay routing
  - Congestion control
  - Application-level adaptation
  - Network performance prediction:
    - to use packet-level probes to estimate end-to-end performance;
    - to build an “Internet performance map” based on active probes from a selective set of “vantage points”;
    - to leverage the history of the same client-server pair 

## evaluation of performance
- QoE metrics - we need to optimize user-perceived QoE
- Video measurements

## Prior Work on Data-Driven Optimization in Networking
- Better Settings of Parameters: While these efforts are in entirely different areas, they have largely exploited the same inefficacy in traditional control logic that the parameters are
statically configured based on assumptions that are ill-matched with the dynamic workload in
runtime.Such problem can be fixed by dynamically training these parameters with recent measurements or synthetically simulated data.
- Better Run-Time Decisions: 
  - In  other word, rather than inferring the underlying network states and finding best decisions analytically, it builds a model that directly associate quality feedback with decisions (configurations).  Research has shown that this more direct data-driven approach can improve the performance of  routing [181], TCP throughput [89], server selection in web performance [146], as well as bitrate  adaptation in video streaming [150]. (这不就机器学习吗)
  -  exploration-exploitation strategies (multi-armed techniques [215]), or Bayesian optimization
## Expressions
- Expressions
  -  it has become of paramount importance that ...
  -  Despite the intense research towards better Internet QoE ...
  -  We elaborate on these quality problems in Chapter 2.
  -  other details are omitted for clarity.
  -  leverage 利用
  -  well-provisioned  精心调配的
  -  our ultimate objective is to improve
  -  As we will see, our solution strikes a better balance between the visibility to user-perceived QoE and the visibility to network conditions through taking a more data-driven approach to endpoint solutions