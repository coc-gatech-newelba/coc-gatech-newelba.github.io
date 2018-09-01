---
permalink: /docs/nsfile-example/
title: .NSFile Resources
---

#### NSFile Example


Copy and paste this into a text editor and upload to the form that appears when you select begin new experiment on _Emulab.net. _

Sample NSFile for describing a topology with the following:



	
  * 1 Controller -- controls the setup and tear down of the other nodes

	
  * 1 Benchmark, 1 Client -- generate requests to the system

	
  * 1-1-1-1 Apache-Tomcat-CJDBC-MySQL system topology

	
  * All nodes deployed across 3 LANs

    
    <code>  set ns [new Simulator]
      source tb_compat.tcl
      
      set node1 [$ns node]
      tb-set-node-os $node1 FC15RubOral
      tb-set-hardware $node1 d710
      
      set node2 [$ns node]
      tb-set-node-os $node2 FC15RubOral
      tb-set-hardware $node2 d710
      
      set node3 [$ns node]
      tb-set-node-os $node3 FC15RubOral
      tb-set-hardware $node3 d710
      
      set node4 [$ns node]
      tb-set-node-os $node4 FC15RubOral
      tb-set-hardware $node4 d710
      
      set node5 [$ns node]
      tb-set-node-os $node5 FC15RubOral
      tb-set-hardware $node5 d710
      
      set node6 [$ns node]
      tb-set-node-os $node6 FC15RubOral
      tb-set-hardware $node6 d710
      
      set node7 [$ns node]
      tb-set-node-os $node7 FC15RubOral
      tb-set-hardware $node7 pc3000
    
      set node8 [$ns node]
      tb-set-node-os $node8 FC15RubOral
      tb-set-hardware $node8 pc3000
    
      set node9 [$ns node]
      tb-set-node-os $node9 FC15RubOral
      tb-set-hardware $node9 pc3000
    
      set node10 [$ns node]
      tb-set-node-os $node10 FC15RubOral
      tb-set-hardware $node10 pc3000
    
      set lan1 [$ns make-lan "$node1 $node2 $node3 $node4 $node5 $node6 $node7" 100Mb 0ms]
      set lan2 [$ns make-lan "$node7 $node8 $node9" 100Mb 0ms]
      set lan3 [$ns make-lan "$node9 $node10" 100Mb 0ms]		
    
      $ns rtproto Static
      $ns run</code>







#### Emulab FAQ: Using the Testbed: Resources Temporary Unavailable


An error of this type is likely because there are insufficient free nodes of the right type at this time. To see how many nodes are available right now, or when nodes will be available in the future, check out the: [cluster status](https://www.emulab.net/portal/resinfo.php) page.

It is also possible that currently available nodes do not meet the criteria you specified, for example, you need 10Gb network interfaces but no nodes with such interfaces are available. The [hardware overview page](https://wiki.emulab.net/wiki/wiki/UtahHardware) is a good place to start in order to determine the capabilities of the available nodes.

Finally, even if there are nodes currently available, meeting your requirements, you may still get the message if the free nodes are [reserved](http://docs.emulab.net/reservations.html) to another project. In this case, you might try making a [reservation](https://www.emulab.net/portal/reserve.php).

Not sure how to proceed or have further questions about this error? Join the Emulab users group (emulab-users@grooglegroups.com) and ask a question. Be sure to include the error message in your question, and the name of your project and experiment.
