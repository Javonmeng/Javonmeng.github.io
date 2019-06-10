---
title: Demystifying Deep Learning in Networking
date: 2019-06-10 10:39:20
tags:
    - DNN in networking
    - interpretability
    - domin-specific knowledge
categories: Papers
---
## Abstract
- The problem of model opacity would eventually *impede the adoption of* DNN-based solutions *in practice*
- we call upon this community to similarly develop techniques and leverage domain-specific insights to demystify the DNNs trained in networking settings, and ultimately unleash the potential of DNNs in an explainable and reliable way
- Neural networks, Resource allocation, Interpretability

## Introduction
- Studies from academia and industry have shown remarkably promising improvements by DNN-based solutions (e.g., deep reinforcement learning) in a variety of classic problems, such as cloud resource allocation [10], end-to-end video adaptation [11], routing [17], wireless bandwidth allocation [18], and so forth
- The inability to  understand these deep models lies at the root of a multitude of concerns; e.g., it is difficult to completely trust the  model will perform well in new environments, to debug  why a wrong decision was made, and to defend it against  adversarial manipulation.
- key aspect of DNNs for networking
  - How is a decision made?
  - When will it fail?
  - Can domain-specific insight be integrated?

##  WHY IS INTERPRETABILITY CRUCIAL?
-  Early promises of DNNs
   -  Each problem is formulated as a  reinforcement learning process, in which the key step is to  use a DNN to map an input “state” to a probability distribution over “actions”. Each action triggers a reward (e.g., job  slowdown), and the DNN updates its parameters to maximize expected reward until they converge
- Concerns of treating DNNs as blackboxes
  - Hard to confer causality
  - Hard to trust
  - Incompatible with domain-specific knowledge
    -  the opacity of DNNs makes incorporating domain-specific knowledge difficult. 

## WHY DNN MAKES THE DECISION？
- Important features
  - The input  of a DNN often includes all features that might be useful  in the decision-making
  - To reveal what features a DNN depends on, a popular  technique is to use saliency map [15], which is a heatmap  showing how much impact of each input feature on the  output
  - When a DNN comprises convolutional layers,  the Grad-CAM [14] method can be employed to generate  the saliency map.
  - So if the resource demand  of a job is labeled dark red (or blue) in the saliency map, it  means increasing the resource demand of that job will greatly  increase (or reduce) the chance of making the same decision
- From input to high-level features
  - Maximally activating intermediate neurons:
  - (In)distinguishable input
    - In contrast, by examining the intermediate neurons, we  can actually explain correlation by DNN works internally:  the intermediate neurons are only activated when the job  size is over or below certain thresholds, i.e., the neurons  effectively act as a “filter” of job size. Such interpretation is  more generalizable and reliable
- Research thrusts
  -  domain knowledge is needed   
  -  it is indeed plausible to test domain-specific hypothesis by visualizing/interpreting features and high-level representations of DNNs used in networking/systems settings.

## WILL IT FAIL MISERABLY
- Adversarial input states
  - the generated input state needs to be meaningful
-  Heterogeneous environments
   -  In contrast, machine learning-based algorithms can  optimize performance for workloads drawn from the same  distribution as the training data, but for the algorithm to carry over to a different environment requires the additional transferability
   -  a DNN can fail miserably when  tested against data draw from an unseen distribution, even  when it is simple enough to be handled by simpler alternative models
- Research thrusts

## MACHINE-GENERATED ALGORITHMS TO RULE IT ALL?
- Integrating domain knowledge is the key
  - Better transferability
  - Better robustness
  - Better training efficiency
-  Regulating DNNs with domain knowledge
   -  The basic idea is to use two processes, called “teacher” and “student”, to regulate the training of a DNN f with a domain-specific logic g
   -  Both f and g take input x and output a probability distribution y over the action space
   -   “student” trains f such that it minimizes the difference to the ground truth as well as the difference to the output of g.

## Expressions
- The problem of model opacity would eventually *impede the adoption of* DNN-based solutions *in practice*
- is incompatible with opaque models like DNNs不透明的模型
- which could otherwise achieve the best of both worlds
- causality因果关系
- Without a proper understanding of the causality, we will not be able to trust if DNN works the same way we believe it does
-  rule-based models
-  is amenable to 经得起检验的
-  most techniques are derived from the *aforementioned* formulation.