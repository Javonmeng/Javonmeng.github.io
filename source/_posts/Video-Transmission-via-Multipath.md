---
title: Video Transmission via Multipath
date: 2019-06-20 15:25:19
tags:
    - Video
    - Multipath
categories: Papers
---
# Video transmission via Multipath  
## Cross-Layer Scheduler for Video Streaming over MPTCP

### Abstract
- Our objective is to maximize the amount of video data that is received in time at the client
- Keyword:  Packet scheduling, Cross-Layer Scheduler

### Introduction
-  MPTCP is expected to enable quicker and more stable communication by concurrently exploiting several network paths
-  head-of-line 排头阻塞 出现在缓存式通信网络交换中的一种现象,这就如同你在只有一条行车路线的马路上右转，但你前面有直行车，虽然这时右行线已经空闲，但你也只能等待。
-  We focus on the multipath streaming of a video.
-   Our objective is to increase the quality of experience (QoE) of the viewers by maximizing the reception of decodable video data in difficult conditions (video bit-rate close to the available bandwidth and small application buffer).
- Our proposal applies to both live and on-demand services
- Contributions
  - cross-layer scheduler
    - leverages information from both application and transport layers
  - theoretical analysis
    - Integer Linear Program 
  - These traces in dataset include the timestamps of both sending and arrival times of each packet in each path.
  - video-aware

### Backgroud

#### Multi-Path Networking
- Application Sending Buffer
- Multi-Path Sending Buffer
- TCP Sending Buffer
- Receiving Buffer
  - MPTCP implements only one  receiving buffer, which gets all incoming packets from all  subflows before the delivery to the application.
- Application Receiving Buffer
- The recent research activities related to  multi-path transport protocols, such as Concurrent  Multipath Transfer SCTP (CMT-SCTP) or MPTCP, have  dealt with the scheduling of packets at the Multi-Path  Sending Buffer (which packets to send to which TCP  Sending Buffer) [2, 22, 25] and the management of  retransmission in case of packet losses
- CMT-SCTP features the same coupled congestion control as  MPTCP (Opportunistic Linked-Increases Algorithm  (OLIA) [20, 26]) but it has no default options, so parameter  setting is hard, and it suffers from deployment issue since  it is based on SCTP

#### Multimedia Stream Structure
- The bit-stream  is cut into frames, which have temporal dependencies with  regards to their types: Intra (I), Predicted (P) or  Bidirectional (B) pictures
  
#### Existing Work on Carrying out Video Through Multiple Paths
- In the multimedia  community, papers have mostly focused on the application  level (a seminal work is [19]). 
- In the network community, a  research axis is to deal with streaming in specific wireless  environment (for example [8]). 
- Our paper is between these  approaches (without any assumption on the physical layer).
- The scheduler is identified as one key component driving  the performance of MPTCP [4] but some other approaches  have also been considered.
- These proposals are in line with  the services the TAPS working group at the IETF aim to  provide: smart interactions between the multi-path  transport and the video generation to improve QoE.

### Video-content aware scheduler over MPTCP
- Particularly, data are transmitted from the  MPTCP Sending Buffer in the same order as they arrived  to this buffer, even if some data should no longer be sent,  for example an obsolete video unit whose playback deadline  has already expired. Such an event is likely to occur in  transport protocols that guarantee in-order delivery, such  as TCP and MPTCP because the loss of one data packet  can delay the delivery of multiple following packets.
  
###  Passing Information between the Different Layers
- Video content awareness
  - The cross-layer layer knows all those  dependencies and so is able to infer whether the client can  decode a video unit at a specific time or not. Each video  unit is associated with a specific video frame (even for  tiles)
- Network awareness
  - The cross-layer scheduler knows the  status of the network, including the smoothed Round-Trip  Time (sRTT), the global moving average bandwidth and  the loss probability of each path
  - With the sRTT, the  cross-layer scheduler can estimate the arrival time of each  packets, and choose to send a video unit close to its  deadline on the path with the shortest sRTT.
  - From the  moving average bandwidth, the scheduler can choose to  drop some less important video units to favor others. 
  - From  the loss probability, it can choose to send highly important  video units on the path with the smallest loss probability
  - the cross-layer scheduler needs to  implement a feedback loop from the MPTCP socket.  Although such feedback does not exist in open-source  implementations yet, the development is not challenging  since the server hosts both the MPTCP implementation  and the application.
- Application awareness
  - The cross-layer scheduler knows the  status of the video player on the client. It can estimate the  lag at the client side. 
  - From the lag, the start  timestamp and the POC, the cross-layer scheduler can  estimate the deadline of each video unit.
  - The client can add extra information in its HTTP requests or use Real-Time  Transport Control Protocol (RTCP) to implement this  feedback in practice. The lag is a fixed value, which can be  communicated to the server. The hardest thing is to  estimate the timestamp when the client starts playing the  decoded video.
- Algorithm
  - The goal of our algorithm is to prioritize the video units that are the most likely to be received in time
  - The crosslayer scheduler is content-aware (it knows how to extract  video units) and application-aware (it is able to estimate  their playback deadline from an applicative feedback loop).  It is also network-aware (it knows which path the MPTCP  stack selects to send the next packet as well as the sRTT of  this path)
- 
### MPTCP DATASET
- for each packet, the transmission  timestamp, the reception timestamp, the size of the  payload and the path on which this packet has been  carried.
  
## Expressions
- The term video unit can *be interpreted as either frame or tile*