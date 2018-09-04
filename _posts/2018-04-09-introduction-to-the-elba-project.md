---
author: gtelbaproject
date: 2018-04-09 19:45:23+00:00
layout: post
title: "Introduction to the Elba Project"
---

[Tutorial](https://gtelbatutorial.wordpress.com/linear-tutorial/)   [FAQs](https://gtelbatutorial.wordpress.com/faqs/)


**Background.** One of the main research challenges in the Adaptive Enterprise vision is the automation of large application system management, encompassing design, deployment, to production use, and capturing application monitoring, evaluation, and evolution. Current approaches to enterprise system evaluation and tuning happen on production systems where the real workload to the deployed system is analyzed on-line and corresponding measurements are taken. In addition, many of these systems go through a detailed staging process that is mostly manual, complex and time-consuming. During the staging process the system to be produced is subjected to workloads to determine whether it will meet the production workloads. Finally, data gleaned from the staging process can be re-used to guide future designs and for management of system during operations.


**Project Motivation.** We want to verify and test an application system deployment plan in a staging environment before committing it to a production environment. Manual verification of a deployment is cumbersome, time consuming, and error prone. This problem will grow in importance in the deployment of increasingly larger and more sophisticated applications. Therefore, it will be increasingly important to have an automatic method for executing a benchmark on the deployment plan to validate the deployment during staging, instead of debugging a deployment during production use.

![ELBA_ARCH](https://gtelbatutorial.files.wordpress.com/2018/02/elba_arch.jpg)

**Approach.** In our project we intend to automate the staging process thus reducing the time and manual labor involved in the process, increase confidence, and extract predictive performance data. Further, the automation will support a more thorough application test and validation in a larger state space, since we plan to automate the monitoring and analysis steps to speed up the refinement of application deployment. Our tools will translate a high-level specification of performance and availability (e.g., SLA requirements) into executable deployment, test, evaluation, and analysis code for the staging phase. This work builds on our experience and technology previously developed such as evaluation of SmartFrog and translation of Quartermaster design specifications into SmartFrog deployment programs.

**System Architecture.** The overall architecture of our project is shown in the figure above, where we achieve full automation in system deployment, evaluation, and evolution, by creating code generation tools to link the different steps of deployment, evaluation, reconfiguration, and redesign in the application deployment lifecycle.



**Research Contributions.** Currently, there is no reliable way to predict the performance of complex applications (e.g., N-Tier distributed application such as Rubis and TPC-App) in a complex environment(e.g., data centers). The limitations of analytical methods are due to the strong assumptions needed for solving the analytical models (e.g., based on queuing theory) that are valid only for relatively simple environments. The limitations of experimental measurements are due to the complexity of managing the many configuration combinations in practice. Our work leverages the Elba infrastructure (particularly, the Mulini generator) to generate and manage the experiments, and then use automated analysis techniques and tools to digest the information and create a Performance Map. The Performance Map is a reliable indicator of complex system performance, since it reflects actually measured experiments on the Performance Terrain (modulo tuning and other complications).


### Importance of N-Tier Systems


[Latency Long Tail Problem](https://gtelbatutorial.wordpress.com/latency-long-tail-problem)

Scalable distributed architecture​



	
  * Division of labor for low-latency tasks​

	
  * Web servers for parsing/HTML handling​

	
  * App servers for business logic handling​

	
  * DB servers for consistent data management​


Separation of stateless from stateful​

	
  * DB servers handle the difficult state part​

	
  * Web and App servers are “stateless” so more instances can be easily added, if needed​.


