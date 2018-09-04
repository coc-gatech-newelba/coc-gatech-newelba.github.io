---
permalink: /docs/latency-long-tail-problem/
title: Latency Long Tail Problem
---

### Latency Long Tail Problem





	
  * At moderate CPU utilization levels (about 60% at 9000 users), 4% of requests take several seconds, instead of milliseconds


![Screen Shot 2018-04-10 at 12.29.02 PM](/img/screen-shot-2018-04-10-at-12-29-02-pm.png)



	
  * The graphs above show frequency of requests by their response times at two representative workloads. The system is at moderate utilization, but the latency long tail problem can be clearly seen.

	
  * No system resource is near saturation​

	
    * Very Long Response Time (VLRT) requests start to appear at moderate utilization levels (often at 50% or lower)​




	
  * VLRT requests themselves are not bugs:​

	
    * They only take milliseconds when run by themselves​

	
    * Each run presents different VLRT requests​




	
  * VLRT requests appear and disappear too quickly for most monitoring tools​


