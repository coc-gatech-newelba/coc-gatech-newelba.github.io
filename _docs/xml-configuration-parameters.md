---
permalink: /docs/xml-configuration-parameters/
title: XML Configuration Parameters
---

#### Experiment XML Configuration Parameters





[Home](https://gtelbatutorial.wordpress.com/2018/02/22/introduction-to-elba)
	
  * Experiment Configuration Parameters:

	
    * RUBBOS-CONF

	
      * controls the operation of the Rubbos benchmark

	
      * _connection_pool_size_ controls the Tomcat connection pool size

	
      * _dist_of_requests_ determines the distribution of the request types that are generated

	
      * _think_time_ mimics the time human users take to make their next request

	
      * _removeBinFiles_ controls whether binary file outputs are kept or not

	
      * monitoring parameters:
* collectl
* iostatMonitor
* psMonitor * sarMonitor
* XML_ENABLED




	
    * APACHE-CONF

	
      * contains standard parameters for Apache's configuration

	
      * consult Apache configuration file for more information




	
    * TOMCAT-CONF

	
      * contains standard parameters for Tomcat's configuration

	
      * consult Tomcat configuration file for more information




	
    * MYSQL-CONF

	
      * _COME BACK TO THIS_

	
      * can contain parameters specifically for the clustered mysql configuration; however, if a cluster node is not present then it won't affect the experiment's execution

	
      * _mysql_tarball_rt_ is a deprecated parameter; was originally tied to logging and sampling




	
    * LOGGING

	
      * contains parameters for turning on/off particular monitoring tools

	
      * does contain a special parameter for mysql to log slow running queries

	
      * also drives sampling frequency, which is largely deprecated at this point given our experimental approach

	
      * much of this was formerly implemented by Yasu

	
      * mysqlResponseTime: _deprecated_ this should remain _false_




	
    * QINGYANG

	
      * implemented for testing the effects of (bursty) workloads when nodes are physically consolidated

	
      * _qyclient_ parameter modifies the Rubbos benchmark's client generator to output specific types of requests




	
    * _Dependencies_:

	
      * Name of the experiment in this XML file must match the name of the experiment as input into the web console of the cloud environment







	
  * Cloud Environment Parameters:

	
    * The following concerns the identifiers given to nodes in the Experiment XML with `` tag

	
    * _CMU/Marmot Cluster_: Nodes in this cluster have multiple networking interfaces. So, the hostnames for each of the nodes must include the appropriate device i.e. "_hostname_.eth1"

	
    * _ElbaCluster_: Nodes can only be addressed using their IP address. As such, these nodes can only be accessed using their physical IP address. In addition, nodes can only be accessed using "root@_ipaddress_"

	
    * _Emulab_ : Nodes do not need to specifiy specific device or IP addresses; they can just be logical names like "node1, etc"




	
  * Permissions and Images:

	
    * If using the FC15RubOral image, then make sure the "USE_EXISTING_IMAGE" flag is set to "Yes."

	
    * This ensures the generator outputs a script to change the default owner and permissions of the directories of interest for the experiment

	
    * There have been instances where a permissions error might appear for `/mnt/elba`directory. However, this does not impact the installation of software in this directory or the overall operation of the experiment.

	
    * Failure to change the permissions associated these directories can result in installation failures

	
    * Operating system images need to have file system permissions updated with the userid executing the experiment. Two options for handling:

	
      * run script to change permissions after mounting filesystem on each node

	
      * create a new image that already contains the modified permissions








