---
author: gtelbaproject
date: 2018-04-09 19:43:24+00:00
layout: post
title: "Data Processing and Management"
---

#### milliScope Data Flow and Architecture


![Screen Shot 2018-03-28 at 7.05.35 PM](https://gtelbatutorial.files.wordpress.com/2018/03/screen-shot-2018-03-28-at-7-05-35-pm.png)

The data transformation flow of milliScope. The event mScopeMonitors capture
timestamps, as shown in Figure 4, in the component logs, while the resource mScopeMonitors record the system resource utilization. mScopeDataTranformer converts these unstructured data to structured tuples and loads them into a dynamic data warehouse, mScopeDB, for advanced analysis.


###### mScopeData Transformer Design


Data pipeline transforms highly variable monitoring data into aggregates that are helpful in millibottleneck isolation and root cause analysis​

These aggregates are:​



	
  * Point-in-Time Response Time​

	
  * Queue Length​

	
  * Downstream Service Time​

	
  * Resource Consumption Descriptive Statistics​




###### Other Parsers


Parsers – extract request-level data out of component logs.



	
  * ServiceTime – reads request-level data and calculates dst​

	
  * Pointintime – reads request-level data (Apache) and calculated pit​

	
  * Multifile – basically joins and aggregates data in uniform entities (dst_table, resource_table); pit is independent​


How to reconcile not ingesting XML into mScopeDataTransformer?​

	
  * Component parsers were supposed to output XML​

	
  * Even if they were, we still need to calculate aggregates like queue lengths and dst​

	
    * Probably in database; the issue with this is trying to do the integration and calculation when the schemas change…​

	
    * Or, compute aggregates externally and import​





