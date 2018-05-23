---
author: gtelbaproject
comments: false
date: 2018-03-25 23:56:54+00:00
layout: page
link: https://gtelbatutorial.wordpress.com/faqs/
slug: faqs
title: FAQs
wordpress_id: 56
---





	
  1. There are two primary ways of distributing required software for experiments:

	
    * custom images

	
      * create a custom image with the appropriate configuration and software pre-loaded

	
      * use the location of the pre-loaded software in the image as the value for the SOFTWARE_HOME paramter in the Experiment XML




	
    * use the `\proj` mounted file share on _emulab_ to copy files to and from experimental nodes. The following [link](https://wiki.emulab.net/wiki/kb81) contains additional information about this share.

	
    * Note: Several of the images on _emulab_ and _PRObE (cmu)_ associated with the _Infosphere_ project already have fully mounted filesystems




	
  2. Some users use package managers like `yum` to install software like _collectl_; however, not all enviornments provide sufficient network access for package managers like `yum`

	
  3. Ensure tarball versions specified in the Experiment XML are available at the location SOFTWARE_HOME references. For example, some of the typical follies include:

	
    * Mysql 5.0.x instead of 5.5.x

	
    * systat 7.x instead of systat 10.x

	
    * Instrumted vs. Uninstrumented Rubbos benchmark files. Instrumented versions are prefixed with "CA-."




	
  4. Some versions of operating systems do not support certain versions of monitoring tools (sysstat). For example, Fedora 15 does not support _systat 7.x_

	
  5. Some of the software requires specific versions of Java

	
    * Asynchronous version of Rubbos benchmark in addition to our tracing infrastructure requires at least Java 7

	
    * Previous versions of the JVM have been shown to introduce performance penalties. As such, an uninstrumented version of the benchmark _**should not**_ be executed with any version prior to Java 6.






