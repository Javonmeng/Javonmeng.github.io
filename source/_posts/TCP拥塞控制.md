---
layout: poster
title: TCP拥塞控制
date: 2019-07-05 16:50:07
tags:
    - TCP
categories: Programming
---
- MSS: Maximum Segment Size 最大报文段长度
- TCP拥塞控制算法
  - TCP拥塞控制算法包括慢启动、拥塞避免、快速恢复三个部分。 
  - 1. 慢启动 
    - TCP连接开始时，cwnd的值通常初始置为一个MSS的较小值，这就使得初始发送速率大约为 MSS/RTT；之后，每收到一个报文段的首次被确认cwnd就增加一个MSS。这样，第一个RTT中，cwnd为1 MSS， 第二个RTT中，cwnd为2 MSS， 第三个RTT中，cwnd为4 MSS， 即每个RTT之后，cwnd就翻倍，cwnd呈指数增长。
    - 当发生丢包时，设置一个变量 ssthresh（慢启动阈值） = cwnd / 2, 然后cwnd重新设为1。
  - 2. 拥塞避免 
    - 当出现丢包时，如果（可能会直接进入快速恢复）进入慢启动阶段，ssthresh设为 cwnd/2, cwnd值置为1，然后每个RTT cwnd翻倍，当cwnd值到达 ssthresh之后，进入拥塞避免阶段。 
    -  此时，每个RTT，cwnd值只增加1个MSS，即每收到一个报文的ACK，cwnd = cwnd + MSS/cwnd.直到再次出现丢包后。当出现超时类型的丢包时，进入慢启动阶段（ssthresh = cwnd/2, cwnd=1）；当出现三次冗余ACK（参考[TCP快速重传机制](https://blog.csdn.net/whgtheone/article/details/80983882)）类型的丢包时，进入快速恢复阶段（ssthresh = cwnd/2, cwnd = ssthreash + 3MSS)
  - 3. 快速恢复
    -  当出现三次冗余ACK类型的丢包时，进入快速恢复阶段。此时 ssthresh = cwnd/2, cwnd = ssthreash + 3MSS，之后每个RTT内，cwnd增加1个MSS，直到出现丢包