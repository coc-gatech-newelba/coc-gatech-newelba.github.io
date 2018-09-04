---
author: gtelbaproject
comments: true
date: 2018-04-09 19:43:05+00:00
layout: post
title: "Methodology and Data Analysis"
---

#### Analysis Outline





	
  * Explain your approach to data transformation, integration, and management.

	
  * Detail how you use the data gathered to isolate millibottlenecks and diagnose their root cause.

	
  * Describe the steps needed to analyze your experiment:

	
    * Copy the tarball archive containing the experiment data to a location proximate to the cloned Elba Github repository.

	
    * Unzip the copied archive.

	
    * Navigate to the parsers directory of the cloned Elba Github repository.

	
    * Execute "runparsers.sh" by providing the following:

	
      1. Path to the directory containing the unzipped archive

	
      2. Target location to store the processing results










#### milliAnalyst





	
  * Uniform data interface consisting of three entities:​

	
    * PointinTime ​

	
    * DownstreamServiceTime (DST)​

	
    * ResourceObservations​




	
  * Each of these entities can have a variable number of number of attributes ​

	
  * Experiment specification impacts this, i.e. the number of nodes determines the number of DST attributes​

	
  * Number of attributes can also depend on characteristics that might not be known prior to running an experiment, such as a node’s number of CPUs, cores, NICs or disks​

	
  * Method needs to handle this variable number of attributes, i.e. no fixed schemas​

	
  * Method needs to be able to integrate data across time and space​

	
  * Representation needs to be able to handle data anomalies and support efficient filtering and retrieval of small data subsets across all of the measurements​

	
  * Support graph-based reasoning​




#### milliBottlenecks





	
  * Millibottlenecks happen in different system layers​

	
    * System software: Java garbage collection​

	
    * Processor architecture: DVFS​

	
    * Application Virtual Machine consolidation​




	
  * Though short-lived, millibottlenecks have big impact** **on n-tier application performance​

	
    * VLRT requests​

	
    * Queue amplification from n-tier system component dependencies​





![Screen Shot 2018-03-28 at 7.07.24 PM](/img/screen-shot-2018-03-28-at-7-07-24-pm.png)

Above is the work flow of milliBottleneck discovery. Users define the configuration file at first, and the script generator generates scripts which set up the experiment environment and deploy milliScope as well as other softwares. mScopeDataTransformer converts these unstructured data to structured tuples in mScopeDB as described in Figure 3 for advanced analysis.

![Screen Shot 2018-04-10 at 12.39.26 PM](/img/screen-shot-2018-04-10-at-12-39-26-pm.png)



	
  * VLRT requests (see (a)) caused by queue peaks in Apache (see (b)) when the system is at workload 9000 clients.


![Screen Shot 2018-04-10 at 12.39.49 PM](/img/screen-shot-2018-04-10-at-12-39-49-pm.png)



	
  * Queue peaks in Apache (a) due to very short bottlenecks caused by Java GC in Tomcat (d).




#### Solutions





	
  * Three kinds of solutions for Latency Long Tail Problem​

	
    * Bug-fix, specific solutions for each cause (many causes, not all can be fixed)​

	
    * General solutions for transient bottlenecks (primarily improved queue management)​

	
    * Last-resort solution​




	
  * Bug-Fix for Specific Solutions

	
    * Some cases can be “fixed”​

	
      * Java GC in JVM 1.5 was “fixed” in JVM 1.6​

	
      * DVFS anti-synchrony can be “fixed” by changing control periods (some complications)​




	
    * Other cases are harder to fix​

	
      * VM consolidation case (noisy neighbor) is really non-deterministic​




	
    * Kernel daemon processes (many)​




	
  * General Solutions

	
    * Approaches that address transient bottlenecks (instead of specific “bugs”)​

	
      * 3 stages: (1) transient bottleneck formation, (2) queue amplification, (3) packet retransmission​




	
    * (1) Transient bottleneck detection and remedial action (e.g., disruption)​

	
      * Difficult, due to the short lifespan of transient bottlenecks​








