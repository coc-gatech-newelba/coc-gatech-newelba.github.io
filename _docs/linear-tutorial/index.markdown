---
author: gtelbaproject
comments: false
date: 2018-03-23 16:35:23+00:00
layout: page
link: https://gtelbatutorial.wordpress.com/linear-tutorial/
slug: linear-tutorial
title: Linear Tutorial
wordpress_id: 19
---

**Experiment Step-by-Step Set-up**

[Home](https://gtelbatutorial.wordpress.com/2018/02/22/introduction-to-elba)



	
  1.  Create account on the Emulab cloud infrastructure at [_emulab.net_](http://www.emulab.net/index.php3) and wait for approval![EmulabElba](https://gtelbatutorial.files.wordpress.com/2018/03/emulabelba.png)

	
  2. Once approved for an account fill out your information- make note of the following two points:

	
    1. When filling out information for your account ensure that you put “Infosphere” for both the group AND the project

	
    2. Select **bash **under the shell portion

	
      1. Please note: if you forget, this can be set after logging into Emulab and clicking the “Edit Profile” link







	
  3. Create a public RSA (SSH) key on your local ‘nix machine under a **bash** shell. 


[SSH Generation](https://gtelbatutorial.wordpress.com/2018/02/22/ssh)

4. Add the SSH key to your account by selecting “My Account” and then “Edit SSH keys” on the left hand side of the page.  This is where you would paste your SSH key from the prior step.

5. Email Josh Kimball with your GitHub username to gain access into the GitHub repository



	
  1. Josh Kimball email address: [Elba@cc.gatech.edu](mailto:Elba@cc.gatech.edu)


	
  2. If you are unfamiliar with GitHub, please create an account at GitHub.com


6. Using your computer terminal, create a directory on Emulab’s file share, users.emulab.net, by executing the following commands:



	
  1. 

	
    1. Please note: substitute for your Emulab account username

	
      1. Example: [JaneDoe@users.emulab.net](mailto:JaneDoe@users.emulab.net)




	
    2. ssh JaneDoe@users.emulab.net  
 cd /proj/Infosphere  
 mkdir JaneDoe![Screenshot2LinearTutorial](https://gtelbatutorial.files.wordpress.com/2018/03/screenshot2lineartutorial.png)





7. Use the following webpage to clone the ‘Alpha release of Project NewElba’ into your directory: [https://github.com/coc-gatech-newelba/NewElbaAlpha.git](https://github.com/coc-gatech-newelba/NewElbaAlpha.git)

_NOTE: if you do not gain permission from Josh Kimball, you will not be able to access this page._



	
  1. In your terminal, enter the directory where you would like to copy the repository

	
    1. cd ~/




	
  2. Clone the repository by executing the following code

	
    1. git clone [https://github.com/coc-gatech-newelba/NewElbaAlpha.git](https://github.com/coc-gatech-newelba/NewElbaAlpha.git)

	
      1. After executing this command, you will be asked to provide your GitHub username and password, so please have this information on hand when going through this step

	
      2. (insert March 1.1 screenshot)







	
  3. Navigate into the directory of the repository you just created and replace with the repository’s name (NewElbaAlpha)

	
    1. cd NewElbaAlpha




	
  4. Type:

	
    1. git status

	
    2. You should see this:![Screenshot3LinearTutorial](https://gtelbatutorial.files.wordpress.com/2018/03/screenshot3lineartutorial.png)






	
  1. Next, execute the following commands:

	
    1. cd /proj/Infosphere/  

	
    2. cp -r ./NewElbaAlpha/generator/common /proj/Infosphere/  

	
    3. mkdir -p /proj/Infosphere//rubbos/rubbos_yasu  

	
    4. tar -zxvf ./NewElbaAlpha/generator/shared_files.tar.gz --directory=/proj/Infosphere//rubbos/rubbos_yasu  

	
    5. mkdir -p /proj/Infosphere//softwares/data  

	
    6. cp -r ./NewElbaAlpha/mScopeEventMonitors /proj/Infosphere//softwares  

	
    7. cp -r ./NewElbaAlpha/mScopeResourceMonitors /proj/Infosphere//softwares   

	
    8. cp /proj/Infosphere/shared_software/rubbos_orig_data.tar.gz /proj/Infosphere//softwares/data  

	
    9. Your terminal should look like the following:





![Screenshot4Tutorial](https://gtelbatutorial.files.wordpress.com/2018/03/screenshot4tutorial.png)


#### ![Screenshot5Tutorial](https://gtelbatutorial.files.wordpress.com/2018/03/screenshot5tutorial.png)




#### [Experiment Description](https://gtelbatutorial.wordpress.com/experiment-customization/)


Follow this [link](https://gtelbatutorial.wordpress.com/experiment-customization/) if you want to execute an experiment other than the topology 1-1-1-1.

The example experiment uses an n-tier application benchmark, RUBBoS. RUBBoS is modeled after a commercial web-based bulletin board system, SlashDot. The benchmark natively supports Read-only and Read-Write workloads, and it enables users to specify the workload in terms of the number of concurrent client requests. For this tutorial, we will use workloads ranging from 1000 to 7000. This example experiment features a system topology composed of:



	
  * 1 Apache HTTP server

	
  * 1 Tomcat Application server

	
  * 1 CJDBC Middleware server

	
  * 1 MySQL DB server


Note: we generally refer to this as a [1-1-1-1 topology](https://gtelbatutorial.wordpress.com/experiment-topology-1-1-1-1/) where each number corresponds to the number of deployed Web, Application, Middleware and Database servers in that order.

[googleapps domain="drive" dir="file/d/0ByvytIe8I-qpbDhDV1ZPU0RjcVU/preview" query="" width="640" height="480" /]

_Note: The video above takes you through executing an experiment. However, the most up to date information is listed below so put precedence on the below instructions and use the video as support._


#### [](https://github.com/coc-gatech-newelba/coc-gatech-newelba.github.io/wiki/Tutorial:-Bootstrap-&-Experiment-Execution-(alpha)#experiment-creation)Experiment Creation


To increase the efficiency and speed of cloud-based systems research, we rely on code generation techniques to create experiments. Before generating artifacts such as installation scripts and configuration, a "reservation" needs to be created on one of the supported public clouds. In addition, the experiment configuration needs to be supplied to the generator. The following two steps accomplish these tasks while the final step outputs the necessary experimental artifacts for running the desired experiment on the public cloud.


#### [](https://github.com/coc-gatech-newelba/coc-gatech-newelba.github.io/wiki/Tutorial:-Bootstrap-&-Experiment-Execution-(alpha)#step-1-create-experiment-on-emulab-cluster)Step 1: Create experiment on Emulab cluster





	
  1. Create a new experiment on Emulab by selecting the _Experimentation_ drop down at the top of the website and select _Begin an Experiment_

	
  2. Fill out the the form accordingly.

	
    * Make note of the _**Experiment Name**_ you provide; this value will be needed later to generate the experiment's artifacts

	
    * _Naming Convention_ for _**Experiment Name**_ : use lowercase letters and/or numbers in the name, no spaces or special characters, including underscore ("_")




	
  3. This form also asks you to provide an _.NSFile_[.NSFile Generation](https://gtelbatutorial.wordpress.com/nsfile)

	
    * This input file specifies the network topology, machine types and OS images for the experiment

	
    * Note: a sample .NSfile can be found [here](https://gtelbatutorial.wordpress.com/nsfile/).

	
      * To execute the 1-1-1-1 topology you can copy and paste the code given [here](https://gtelbatutorial.wordpress.com/nsfile/) into a text editor. _Note: if you are using a Mac then use Sublime, Vim, or TextWrangler- do NOT use TextEditor. _







	
  4. For this example, the _.NSFile_ needs to include 10 nodes, corresponding to: 1 Control node, 1 Benchmark node, 4 Client node, 1 HTTPD node, 1 Tomcat node, 1 C-JDBC node, and 1 MySQL node.


Comment: In this example, all of the nodes are on one local area network (LAN). This can be changed by including more LAN's in the specification and connecting the desired nodes to a particular LAN.


#### [](https://github.com/coc-gatech-newelba/coc-gatech-newelba.github.io/wiki/Tutorial:-Bootstrap-&-Experiment-Execution-(alpha)#step-2-describing-the-experiment-by-editingcompleting-experiment-xml)Step 2: Describing the Experiment by editing/completing Experiment XML


We use an XML file as a basis for generating all experiment artifacts. This XML file contains various environmental and experimental parameters, for example:



	
  1. username and groupname for executing the experiment

	
  2. workload, i.e. number of concurrent user requests

	
  3. number of nodes (servers) and the component software (application software) to install on each

	
  4. monitoring to activate


In short, these parameters control the software, topology, workload and monitors used during the execution of a system experiment. As such, it functions as a de facto experiment specification.

To begin, perform the following commands:

    
    <code>cd /proj/Infosphere//common/experiment_xml  
    cp RUBBOS-1111-EMULAB-DEFAULT.xml RUBBOS-1111-EMULAB-.xml  
    </code>


Several default parameters need to be changed to execute our example:



	
  * _EMULAB_EXPERIMENT_NAME_
Complete this value using the **_Experiment Name_** value from above (step 1) using the following format: _Experiment Name.infosphere.emulab.net_

	
    * For example, if the supplied value for Experiment Name during Step 1 was jkimball1112, then the value for this parameter would be: jkimball1112.infosphere.emulab.net




	
  * Output Directories

	
    * The following group of parameters direct where the experiment's scripts and output are copied.

	
    * Complete these by providing the _Experiment Name_ value from Step 1 and your Emulab account _username_






    
    <code> /rubbos/rubbos_yasu"/>/rubbos/ rubbos_yasu/"/>@users.emulab.net"/>/results"/>  </code>





	
  * _Username: _Complete using your Emulab account username


`  "/>  `



	
  * Monitors: Set the following monitor parameters to _false_



	
  * Workloads

	
    * This example experiment uses workloads ranging from 1000 to 7000 concurrent requests.



	
    * Lines like the one below can be modified by either changing the corresponding value or commenting out the line entirely





_Important Notes_



	
  * mysqlReponseTime parameter should remain "false." The screencast accompanying these instructions sets this parameter incorrectly.

	
  * This file contains many additional parameters. A more complete description for most of them can be found at the end of the tutorial in the [XML Configuration Parameters](https://gtelbatutorial.wordpress.com/xml-configuration-parameters/).




#### [](https://github.com/coc-gatech-newelba/coc-gatech-newelba.github.io/wiki/Tutorial:-Bootstrap-&-Experiment-Execution-(alpha)#step-3-generate-experiment-scripts-and-configuration-using-experiment-xml)Step 3: Generate Experiment Scripts and Configuration using Experiment XML



Experimental artifacts like the scripts and software configuration are created dynamically by the code generation pipeline. This pipeline can be executed using the following commands, substituting the values for and as provided in Step 2:



	
  * `ssh node1.`

	
  * `cd /proj/Infosphere//common/rubbosMulini6
`


Finally, open _runRubbosExperiment.sh_ and substituting with the specified values



	
  * `BUILD_DIR = /proj/Infosphere/`

	
  * ` OUTPUT_HOME =  from Step2
`


The script can now be run with the following command where corresponds to the name of the Experiment XML file from Step 2:

	
  * `./runRubbosExperiment.sh `


In the case of our running example, this should be:

	
  * `./runRubbosExperiment.sh RUBBOS-1111-EMULAB-.xml
`


_Important Notes_:



	
  * Activating the experiment on _emulab_, ("swapping it in") and logging into node1 is not a _strict_ dependency. Since the image and _emulab_ environment already have the proper environment variables set, this step ensures consistency during experiment generation.

	
  * The script should be run from a location that has the environment variable, JAVA_HOME, set. Nodes using the FC15RubOral image already have JAVA_HOME set. So, we recommend connecting to the Control node with **ssh** to execute this step.

	
  * [jk] Double check whether this can run with a different JAVA version or if it needs to be Java 1.4

	
  * Sample Experiment XML files are available in the `/examples` directory in our [Github repository.](https://github.com/coc-gatech-newelba/NewElbaAlpha.git)




#### Code Generator Description


In general, our code generator is orchestrated by way of the `runRubbosExperiment.sh` script. This script calls a Java program, DeployScriptGenerator, which generates all of the necessary scripts and artifacts for executing an experiment in one of the supported infrastructures. The following describes the script more generally and the organization of the code generator source code:



	
  1. Code Generator Organization:

	
    * Base directory, `../common/rubbosMulini6`, contains the aforementioned script. It also acts as a reference point for executing the code generator and pipeline.

	
    * Two sub-directories house the code generator source and base artifacts:

	
      * `../rubbosMulini6/templates` - contains the XSL-based code templates that serve as a basis for the experiment scripts

	
      * `../rubbosMulini6/source` - contains the Java source code for the code generator







	
  2. Parameter Descriptions. The `runRubbosExperiment.sh` script uses the following parameters in addition to those mentioned at the beginning of this Step. These might need to be altered depending on the environment used for generating experiments:

	
    * MULINI_DIR: specifies the base directory of the code generator, i.e. `../rubbosMulini6`from above

	
    * EXP_DIR: specifies the path to Experiment XML files, i.e. the location for the filename specified as a command line parameter




	
  3. _Dependencies_:

	
    * Directories, `/templates` and `/source` need to be located in the same directory as the aforementioned script. Currently, all of these appear under `/common` in the source code repository under `/code_generator/`

	
    * Value of `OUTPUT_HOME` parameter used in this script needs to be the same as the value of the `OUTPUT_HOME` parameter specified in the referenced Experiment XML file







#### [](https://github.com/coc-gatech-newelba/coc-gatech-newelba.github.io/wiki/Tutorial:-Bootstrap-&-Experiment-Execution-(alpha)#step-4-executing-the-experiment-on-emulab-cluster)Step 4: Executing the Experiment on Emulab cluster


Continuing with our example, perform the following preliminaries by substituting the values for parameters, and , as provided in Step 2:



	
  * `ssh node1. `

	
  * `
cd /scripts
`


Finally, the experiment can be run by executing the ‘run.sh’ script with the following command:

	
  * `./run.sh
`


While executing an experiment in this manner works, there a few downsides to executing an experiment this way. These limitations are:

	
  * Error messages or standard output will not be captured for future analysis. This makes the verification that the experiment ran "correctly" rather difficult as debugging would require examining terminal-based output.

	
  * Secondly, since this command is executed over an SSH session, it could timeout if the experiment execution time exceeds the default session timeout, causing an abrupt termination of the experiment prior to its natural completion. These problems are resolved by using the following, alternative method.


Running experiments under a `screen` session mitigates these problems. [Ensure the Linux program _screen_ is installed prior to executing an experiment with this method.] The Linux program, _screen_, enables scripts to be executed independent of an active **ssh** session. To run an experiment under _screen_, use the following:



	
  * `./run_screen.sh
`


This script creates a new session, navigates to the experiment at the provided path, runs the experiment while storing the output, and detaches the screen from your current shell. The script also saves all output to a log file in the specified directory.

To re-attach to the screen session, a user can run the following command and replacing with the _Experiment Name_ value from Step 1:



	
  * `screen –r
`




#### [](https://github.com/coc-gatech-newelba/coc-gatech-newelba.github.io/wiki/Tutorial:-Bootstrap-&-Experiment-Execution-(alpha)#experiment-organization)Experiment Organization


In general, every generated experiment adheres to a canonical structure.

    
    <code>/proj/infosphere//rubbos/rubbos_yasu/  
    </code>





	
  * is the top-level directory that holds all of the generated experiment artifacts. In fact, this line corresponds to the value supplied for the _OUTPUT_HOME_ parameter in the Experiment XML

	
  * This acts as a de facto reference point for many other scripts during the installation and configuration of the environment's nodes and benchmark

	
  * It must be accessible to the environment where the experiment will be run, so the associated scripts and configuration can be executed

	
  * _Shared Structures._ There are several _shared directories_, i.e. all experiments reference these directories and must exist under `../rubbos_yasu`, i.e. in the context of OUTPUT_HOME, these structures must be at the same level in the path as the . These directories are:

	
    * emulab_files: file system configuration for `fdisk`

	
    * tomcat_files: standard startup and shutdown scripts

	
    * apache_files: static html files for the benchmark

	
    * rubbos_files: benchmark source code for the Client generator and Application logic (Servlets)

	
    * _Note: There can be others such as postgres_files and a directory for marmot; however, these are only necessary for running experiments with postgres DBMS and the PRObE cloud respectively._







#### [FAQ and Best Practices](https://gtelbatutorial.wordpress.com/faqs/)



