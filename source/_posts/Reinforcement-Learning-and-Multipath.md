---
title: Reinforcement Learning  and Multipath
date: 2019-06-24 14:40:43
tags:
    - RL
    - Multipath
categories: Papers
---
# Reinforcement Learning  and Multipath

## A Reinforcement Learning Approach for Multipath TCP Data Scheduling

### Abstract
- the performance of MPTCP is seriously affected by the path environment
- In  order to solve this problem, we propose a new data scheduling  algorithm based on reinforcement learning(RL) with the newly  introduced Deep Q Network (DQN) framework to enhance the  MPTCP data scheduling performance in the asymmetric path.  
- The reinforcement learning algorithm gets the information of  every path and adaptively choose the most suitable path by  the artificial intelligence.

### Introduction
- For instance, paths have huge differences in the round-trip time(RTT), bandwidth resources which may cause a serious decline in performance because the disordered data will fill buffer if the packet cannot deliver to the in time under the limitation of resources in sender and receiver.
- in this paper, we assume  the data scheduling problem and the path selection problem  can be formulated as a MDP problem which can be calculated  as the reinforcement learning problem.

## ReLeS: A Neural Adaptive Multipath Scheduler based on DRL

### Abstract
- ReLeS  uses modern deep reinforcement learning (DRL) techniques  to learn a neural network to generate the control policy for  packet scheduling. 
- It adopts a comprehensive reward function  that takes diverse QoS characteristics into consideration to  optimize packet scheduling. 
- To support real-time scheduling,  we propose an asynchronous training algorithm that enables  parallel execution of packet scheduling, data collecting, and  neural network training. 
- We implement ReLeS in the Linux  kernel and evaluate it over both emulated and real network  conditions.
  
### Introduction
- Packet scheduling is a unique and fundamental mechanism for the design and implementation of MPTCP.
  - A scheduler is  responsible for determining the amount of data packets to be  distributed onto the subflows, which has a significant impact  on the performance of MPTCP
- head-of-line blocking
- related work
  - The ReMP scheduler [12] duplicates  packets over all subflows in exchange for reliability
  - MinRTT, the  default scheduler of MPTCP, attempts to fill the congestion  window of the subflow with the lowest RTT before advancing  to other subflows
  - BLEST  [13], a blocking estimation based scheduler, which takes a  proactive stand towards minimising HoL-blocking
  - **The DEMS  scheduler [14] aims at reducing the data chunk download time  over multiple paths, whose benefits are maximized when the  file size is small or medium**
- In this paper
  - To support online scheduling, the model training process runs offline using the real network traces and updates the online scheduler iteratively. 
  - To speedup the training process, we further introduce an asynchronous deep reinforcement learning algorithm that decouples data collection and model training to enable parallel execution of packet scheduling, data collecting, and neural network training.
- Challenges
  - Multipath packet scheduling should take network heterogeneity (e.g., delay and capacity) into consideration in order to effectively leverage multipath scenarios. 
  - Packet scheduling algorithms must balance a variety of QoS goals such as maximizing average throughput, reducing the overall data transfer time, minimizing self-inflicted latency or out-of-order buffer size, and maintaining jitter smoothness
  - Multipath scheduler should adapt to the dynamic of network environments.

### PROBLEM DESCRIPTION AND SOLUTION FRAMEWORK

#### Problem Description
- A typical value of SI (scheduling intervals) is 200ms, which is about 3∼4 RTTs
-  An episode is defined as the duration of an MPTCP connection, starting from the initiation of the MPTCP session until its teardown, which may contain several SIs.
- The scheduler’s goal is to find the best set of split ratios (p1, p2, ··· , pn) for the subflows that lead to the optimal performance
  
#### Solution Framework
- Online scheduling:
- Offline training:
- The online scheduling and offline training process run  asynchronously and iteratively. After each episode the trainer  synchronizes the scheduler with the trained neural network in  adaption to the environment changes, driving the scheduling  policy towards the optimum

### Training Algorithm

#### The deep reinforcement learning task
- State, At the beginning of each SI, the agent observes the system state
  - Assume in the *t*-th SI, the system state is represented by $s_t=(s_{t,1},s_{t,2},...,s_{t,n})$, where $s_{t,i}$ is the observed state of the i-th subflow which can be represented by a tuple $s_{t, i}=\left(x_{t, i}, w_{t, i}, d_{t, i}, u_{t, i}, v_{t, i}\right)$.
  - $x_{t,i}$ is the subflow throughput measurement in SI t
  - $w_{t,i}$ is the average congestion window sizes in SI t
  - $d_{t,i}$ is the subflow mean RTT in SI t
  - $u_{t,i}$ is the number of unacked packets in SI t
  - $v_{t,i}$ is the number of retransmitted packets in SI t
- Action
  - an action is a scheduling decision, which determines how to distribute the current traffic over multiple paths
- Policy
- Reward
  - After applying the action, the state of the environment transitions to st+1 and the agent receives a reward by taking the action.
  - A reward function is used to evaluate the long-term performance of MPTCP by for a particular packet scheduling policy.
#### Reward Function
- $R\left(s_{t}, a_{t}\right)=V_{t}^{\text { throughput }}-\alpha V_{t}^{\mathrm{RTT}}-\beta V_{t}^{\text { lost }}$lling

#### Asynchronous Training Algorithm
- asynchronous training algorithm, which decouples data collection and model learning
to enable parallel execution of real-time scheduling and neural network training.
- Normalized Advantage Function (NAF) [18] framework
  - LSTM.  extract features 
    - With LSTM, ReLeS can incorporate a large  amount of network measurement history into its state space to  capture the temporal feature of actual network characteristics.
  - Softmax is used for the final layer activation function in the neural network to form a probability expression of the packet scheduling policy.
- The asynchronous learning algorithm is summarized in  Algorithm 1. The trainer thread takes samples from the  replay buffer to update the deep neural network Q-function  approximator using stochastic gradient descent. This thread  runs on a central server, and dispatches updated neural network  parameters to the scheduler thread. The collector thread run  on the MPTCP connection sender, and sends the observation,  action, and reward in each SI to the central server to append  to the replay buffer.
-  asynchronously
   - The trainer keeps training the neural network using the recent experience from the replay buffer
   - the scheduler/collector executes packet scheduling actions on the sender side
   - synchronizes its neural network parameters with the trainer iteratively, and pushes the experience into the shared replay buffer.
- While $y_t$ is also dependent on $\theta^Q$, directly implementing  Q-learning (Eq. 5) with neural network has been proved to  be unstable in many environments
  - Because the network Q(s, a|$\theta^Q$) being updated is also used in calculating the target
value $y_t$, the Q update is prone to divergence.

### SYSTEM DESIGN AND IMPLEMENTATION
- At the sender, the chunk data coming from the application (APP) is  stored in the meta buffer, and is then fetched, split, scheduled,  and transmitted by the MPTCP scheduler. 
- The policy network  generates actions that determine the split ratio of each subflow  for packet scheduling. 
- The network states, actions, and rewards  in each SI are passed to the collector and stored in a  replay buffer, which will be used by the trainer to train the  reinforcement learning model and update the policy network.  
- The receiver side logic is much simpler. As shown in Fig. 5,  it passively receives or acknowledges the data, reassembles it  in the receiver-side meta buffer, and delivers the in-order data  to the APP.
- **In the implementation of the system, it requires interaction between the kernel and userspace**
- getsockopt( ) setsockopt( )

### PERFORMANCE EVALUATION
- we implement ReLeS, BLEST [13], and DEMS (the basic DEMS that does not perform reinjection) [14] in the Linux kernel base on MPTCP v0.92
- For emulation, we use Linux tc to throttle the bandwidth and to add extra delay on the sender.
- Linux操作系统中的流量控制器TC（Traffic Control）用于Linux内核的流量控制，主要是通过在输出端口处建立一个队列来实现流量控制。Linux操作系统中的流量控制器TC（Traffic Control）用于Linux内核的流量控制，主要是通过在输出端口处建立一个队列来实现流量控制。
- We consider different downloading files sizes varying from 64KB to 256MB to test the performance of the schedulers under different traffic classes.
-  Performance Metrics
   -  Application goodput
   -  Application delay
   -  OFO queue
   -  Download time
- finally make further evaluation under real-world settings
-  Performance Analysis
   - Taking MinRTT as  a baseline, the outperformance ratio is defined as the ratio  of the number of episodes where ReLeS’s download time  is smaller than MinRTT to the total number of episodes
   - Additionally, increasing the number of collectors makes it converge significantly faster
   - The training time  is within 0.2 hour for 104 updates, which is efficient. The data  collecting consumes much more time than the neural network  training, thus, we can achieve significant speedup in overall  training time from simultaneously collecting experience across  multiple collectors
   -  Performance under real-world settings (five locations)
      -  office, library, supermarket, dormitory and canteen
      -   Google Nexus 5 smartphone - Android 4.4, based on MPTCP v0.86.x

### Related Work
- Two widely used scheduling algorithms in MPTCP are MinRTT (default) and Round-Robin, which are simple and heuristic
- ReMP sends the duplicated data to all subflows to improve reliability in the cost of high redundancy[12]
-  Frommgen et al. [28] proposed a programming model for MPTCP scheduling. 
-  Guo et al. [14] developed theoretical analyses to show that having all subflows complete at the same time at the receiver side is a necessary condition for achieving the optimal  performance and proposed DEMS
-  ReLeS is a learning-based and experience-driven multipath scheduling approach which is self-adaptive to various kinds of network environments
## Epressions
- many efforts have been made in this research [3] put forward
- what is more, 
- is prone to divergence
- Different from the existing solutions, the proposed ReLeS is