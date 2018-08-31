---
author: gtelbaproject
comments: false
date: 2018-03-26 14:35:33+00:00
layout: page
link: https://gtelbatutorial.wordpress.com/ns-commands/
slug: ns-commands
title: NS Commands
wordpress_id: 67
---

Testbed NS Command Extensions

In order to use the testbed specific commands you must include the following line near the top of your NS file (before any testbed commands are used):

    
        source tb_compat.tcl


If you wish to use your file under NS you can use download [tb_compat.tcl](https://wiki.emulab.net/Emulab/wiki/attachment/Tutorial/tb_compat.tcl). Place it in the same directory as your NS file. When run in this way under NS the testbed commands will have no effect, but NS will be able to parse your file.


#### TCL, NS, and Node Names


In your file you will be creating nodes with something like:

    
        set node1 [$ns node]
    


What is really going on is that the simulator, represented by $ns is creating a new node, involving a bunch of internal data changes, and returning a reference to it which is stored in the variable node1. In almost all cases, when you need to refer to a node you will do it as $node1, the $ indicating that you want the value of the variable node1, i.e. the reference to the node. Thus you will be issuing commands like:

    
        $ns duplex-link $node1 $node2 100Mb 150ms DropTail
        tb-set-ip $node1 10.1.0.2
    


(Note the $'s)

You will notice that when your experiment is setup the node names and such will be node1. This happens because the parser detects what variable you are using to store the node reference and uses that as the node name. In the case that you do something like:

    
        set node1 [$ns node2]
        set A $node1
    


The node will still be called node1 as that was the first variable to contain the reference.

If you are dealing with many nodes you may store them in array, perhaps like this:

    
        for {set i 0} {$i < 4} {incr i} {
           set nodes($i) [$ns node]
        }
    


In this case the names of the node will be nodes-0, nodes-1, nodes-2, nodes-3. (In other words, the ( character is replaced with -, and ) is removed.) This slightly different syntax is used to avoid any problems that () may cause later in the process. For example, the () characters cannot appear in DNS entries.

As a final note, everything said above for nodes applies equally to lans. I.e.:

    
        set lan0 [$ns make-lan "$node0 $node1" 100Mb 0ms]
        tb-set-lan-loss $lan0 .02
    


(Again, note the $'s)

Links can also be named just like nodes and lans. The names can then be used to set loss rates or IP addresses. This technique is the only way to set such attributes when there are multiple links between two nodes.

    
        set link1 [$ns duplex-link $node0 $node1 100Mb 0ms DropTail]
        tb-set-link-loss $link1 0.05
        tb-set-ip-link $node0 $link1 10.1.0.128




#### Captured NS file parameters


A common convention when writing NS files is to place any parameters in an array named "opt" at the beginning of the file. For example:

    
        set opt(CLIENT_COUNT) 5
        set opt(BW) 10mb;    Link bandwidth
        set opt(LAT) 10ms;   Link latency
    
       ...
    
        $ns duplex-link $server $router $opt(BW) $opt(LAT) DropTail
    
        for {set i 0} {$i < $opt(CLIENT_COUNT)} {incr i} {
            set nodes($i) [$ns node]
    	...
        }
        set serverprog [$server program-agent -command "starter.sh"]
    


Normally, this convention is only used to help organize the parameters. In Emulab, however, the contents of the "opt" array are captured and made available to the emulated environment. For instance, the parameters are added as environment variables to any commands run by [program-agents](https://wiki.emulab.net/Emulab/wiki/eventsystem#PROGRAM) (only available on recent FBSD410-STD and RHL90-STD images). So, in the above example of NS code, the "starter.sh" script will be able to reference parameters by name, like so:

    
        #! /bin/sh
    
        echo "Testing with $CLIENT_COUNT clients."
        ...
    


Note that the contents of the "opt" array are not ordered, so you should not reference other parameters and expect the shell to expand them appropriately:

    
        set opt(prefix) "/foo/bar"
        set opt(BINDIR) '$prefix/bin'; # BAD
    
        set opt(prefix) "/foo/bar"
        set opt(BINDIR) "$opt(prefix)/bin"; # Good




#### Ordering Issues


tb- commands have the same status as all other Tcl and NS commands. Thus the order matters not only relative to each other but also relative to other commands. One common example of this is that IP commands must be issued after the links or lans are created.


#### Hardware Commands




#### 1. tb-set-hardware




tb-set-hardware _node_ _type_ [_args_]




tb-set-hardware $node3 pc
tb-set-hardware $node4 shark


_node_ - The name of the node.
_type_ - The type of the node.

Notes:



	
  * Please see the [Node Status](https://www.emulab.net/nodecontrol_list.php3) page for a list of available types. pc is the default type.

	
  * No current types have any additional arguments.




#### IP Address Commands


Each node will be assigned an IP address for each interface that is in use. The following commands will allow you to explicitly set those IP addresses. IP addresses will be automatically generated for all nodes that you do not explicitly set IP addresses.

In the common case the IP addresses on either side of a link must be in the same subnet. Likewise, all IP addresses on a LAN should be in the same subnet. Generally the same subnet should not be used for more than one link or LAN in a given experiment, nor should one node have multiple interfaces in the same subnet. Automatically generated IP addresses will conform to these requirements. If part of a link or lan is explicitly specified with the commands below then the remainder will be automatically generated under the same subnet.

IP address assignment is deterministic and tries to fill lower IP's first, starting at 2. Except in the partial specification case (see above), all automatic IP addresses are in the network 10.


#### 1. tb-set-ip




tb-set-ip _node_ _ip_




tb-set-ip $node1 142.3.4.5


_node_ - The node to assign the IP address to
_ip_ - The IP address.

Notes:



	
  * This command should only be used for nodes that have a single link. For nodes with multiple links the following commands should be used. Mixing tb-set-ip and any other IP command on the same node will result in an error.




#### 2. tb-set-ip-link




tb-set-ip-link _node_ _link_ _ip_




tb-set-ip-link $node0 $link0 142.3.4.6


_node_ - The node to set the IP for.
_link_ - The link to set the IP for.
_ip_ - The IP address.

Notes:



	
  * One way to think of the arguments is a link with the node specifying which side of the link to set the IP for.

	
  * This command can not be mixed with tb-set-ip on the same node.




#### 3. tb-set-ip-lan




tb-set-ip-lan _node_ _lan_ _ip_
tb-set-ip-lan $node1 $lan0 142.3.4.6


_node_ - The node to set the IP for.
_lan_ - The lan the IP is on.
_ip_ - The IP address.

Notes:



	
  * One way to think of the arguments is a node with the LAN specifying which port to set the IP address for.

	
  * This command can not be mixed with tb-set-ip on the same node.




#### 4. tb-set-ip-interface




tb-set-ip-interface _node_ _dst_ _ip_
tb-set-ip-interface $node2 $node1 142.3.4.6


_node_ - The node to set the IP for.
_dst_ - The destination of the link to set the IP for.
_IP_ - The IP address.

Notes:



	
  * This command can not be mixed on the same node with tb-set-ip. (See above)

	
  * In the case of multiple links between the same pair of nodes there is no way to distinguish which link to the set the IP for. This should be fixed soon.

	
  * This command is converted internally to either tb-set-ip-link or tb-set-ip-lan. It is possible that error messages will report either of those commands instead of tb-set-ip-interface.




#### 5. tb-set-netmask




tb-set-netmask _lanlink_ _netmask_




tb-set-netmask $link0 "255.255.255.248"


_lanlink_ - The lan or link to set the netmask for.
_netmask_ - The netmask in dotted notation.

Notes:



	
  * This command sets the netmask for a lan or link. The mask must be big enough to support all of the nodes on the lan or link!

	
  * You may play with the bottom three octets (0xFFFFFXXX) of the mask; attempts to change the upper octets will cause an error.




#### 6. tb-set-node-routable-ip




tb-set-node-routable-ip _node_ _onoff_




tb-set-node-routable-ip $node0 1


_node_ - The node to which a public (routable) IP address should be allocated.
_onoff_ - 1 if you want a public address.

Notes:



	
  * This command indicates that the designated node should be allocated a public IP address on its control network interface when _onoff_ is set to 1.

	
  * It is only meaningful for virtual nodes (e.g. for one you have used tb-set-hardware to choose a type pcvm). Physical nodes already have public control addresses by default; this command has no effect for them.




#### OS Commands




#### 1. tb-set-node-os




tb-set-node-os _node_ _os_




tb-set-node-os $node1 FBSD-STD
tb-set-node-os $node1 MY_OS


_node_ - The node to set the OS for.
_os_ - The id of the OS for that node.

Notes:



	
  * The OSID can either by one of the standard OS's we provide or a custom OSID, created via the web interface.

	
  * If no OS is specified for a node a default OS is chosen based on the nodes type. This is currently RHL-STD for PCs.

	
  * The currently available standard OS types are: FBSD-STD, RHL-STD, [WINXP-UPDATE](https://wiki.emulab.net/Emulab/wiki/Windows), NBSD14-STD (should not be used on PC nodes), and NETBOOT-STD (oskit netboot kernel for loading other operating systems over the network). [Windows 2000](http://www.emulab.net/kb-show.php3?xref_tag=SWS-WIN2K) is not supported. Click [List ImageIDs and OSIDs](https://www.emulab.net/showosid_list.php3) in the Emulab web interface "Interaction" pane to see the current list of Emulab-supplied OSs.




#### 2. tb-set-node-rpms




tb-set-node-rpms _node_ _rpms..._




tb-set-node-rpms $node0 rpm1 rpm2 rpm3


Notes:



	
  * This command sets which rpms are to be installed on the node when it first boots after being assigned to an experiment.

	
  * Each rpm can be either a path to a file or a URL. Paths must be to files that reside in /proj or /groups. You are not allowed to place your rpms in your home directory. http(s):// and ftp:// URLs will be fetched into the experiment's directory, and re-distributed from there.

	
  * See the [tutorial](https://wiki.emulab.net/Emulab/wiki/Tutorial) for more information.




#### 3. tb-set-node-startcmd




tb-set-node-startcmd _node_ _startupcmd_




tb-set-node-startcmd $node0 "mystart.sh -a >& /tmp/node0.log"


Notes:



	
  * Specify a script or program to be run when the node is booted.

	
  * See the [tutorial](https://wiki.emulab.net/Emulab/wiki/Tutorial) for more information.




#### 4. tb-set-node-cmdline




tb-set-node-cmdline _node_ _cmdline_




tb-set-node-cmdline $node0 {???}


Notes:



	
  * Set the command line, to be passed to the _kernel_ when it is booted.

	
  * Currently, this is supported on OSKit kernels only.




#### 5. tb-set-node-tarfiles




tb-set-node-tarfiles _node_ _install-dir1_ _tarfile1_ ...


The tb-set-node-tarfiles command is used to install one or more tar files onto a node's local disk. This command is useful for installing files that are used frequently, but will change very little during the course of your experiments. For example, if your software depends on a third-party library not provided in the standard disk images, you can produce a tarball and have the library ready for use on all the experimental nodes. Another example would be the data sets for your software. The benefit of installing files using this method is that they will reside on the node's local disk, so your experimental runs will not be disrupted by NFS traffic. Finally, you will want to avoid using this command if the files are changing frequently because the tars are only (re)installed when the nodes boot.

Installing individual tar files or RPMs is a midpoint in the spectrum of getting software onto the experimental nodes. At one extreme, you can read everything over NFS, which works well if the files are changing constantly, but can generate a great deal of strain on the control network and disrupt your experiment. The tar files and RPMs are also read over NFS when the nodes initially boot, however, there won't be any extra NFS traffic while you are running your experiment. Finally, if you need a lot of software installed on a large number of nodes, say greater than 20, it might be best to create a [custom disk image](https://wiki.emulab.net/Emulab/wiki/Tutorial). Using a disk image is easier on the control network since it is transferred using multicast, thus greatly reducing the amount of NFS traffic when the experiment is swapped in.

_Required Parameters:_



	
  * _node_ - The node where the files should be installed. Each node has its own tar file list, which may or may not be different from the others.

	
  * One or more _install-dir_ and _tarfile_ pairs are then listed in the order you wish them to be installed:

	
    * _install-dir_ - An existing directory on the node where the tar file should be unarchived (e.g. '/', '/usr', '/usr/local'). The "tar" command will be run as "root" [Note1](https://wiki.emulab.net/wiki/nscommands#tb-set-node-tarfiles), so all of the node's directories will be accessible to you. If the directory does not exist on the image or was not created by the unarchiving of a previous tar file, the installation will fail [Note2](https://wiki.emulab.net/wiki/nscommands#tb-set-node-tarfiles).

	
    * _tarfile_ - An existing tar file located in a project directory (e.g. '/proj' or '/groups') or an http, https, or ftp URL. In the case of URLs, they are downloaded when the experiment is swapped in and cached in the experiment's directory for future use. In either case, the tar file name is _required_ to have one of the following extensions: .tar, .tar.Z, .tar.gz, or .tgz. Note that the tar file could have been created anywhere, however, if you want the unarchived files to have valid Emulab user and group id's, you should create the tar file on ops or an experimental node.





_Example usage:_

    
    # Overwrite files in /bin and /sbin.
    tb-set-node-tarfiles $node0 /bin /proj/foo/mybinmods.tar /sbin /proj/foo/mysbinmods.tar
    
    # Programmatically generate the list of tarballs.
    set tb [list]
    # Add a tarball located on a web site.
    lappend tb / http://foo.bar/bazzer.tgz
    # Add a tarball located in the Emulab NFS space.
    lappend tb /usr/local /proj/foo/tarfiles/bar.tar.gz
    # Use 'eval' to expand the 'tb' list into individual
    # arguments to the tb-set-node-tarfiles command.
    eval tb-set-node-tarfiles $node1 $tb
    


_See also:_



	
  * [`tb-set-node-rpms`](https://wiki.emulab.net/wiki/nscommands#tb-set-node-rpms)

	
  * [Custom disk images](https://wiki.emulab.net/Emulab/wiki/Tutorial)


_Notes:_



	
  1. Because the files are installed as root, care must be taken to protect the tar file so it cannot be replaced with a trojan that allowed less privileged users to become root.

	
  2. Currently, you can only tell how/why an installation failed by examining the node's console log on bootup.




#### 6. tb-set-tarfiles




tb-set-tarfiles _install-dir1_ _tarfile1_ ...




or




tb-set-tarfiles _install-dir1_ _tarfile1_
tb-set-tarfiles _install-dir2_ _tarfile2_
...


Like tb-set-node-tarfiles but installs the tarfiles to all pcs in the experiment.

It uses multicast (frisbee) to distribute the tarfiles to all nodes simultaneously so it currently scales better than tb-set-node-tarfiles. Hence, it is practical to use this with large tarfiles, unlike tb-set-node-tarfiles.

_Notes:_



	
  1. This feature requires a fairly recent version of the testbed client-side software to be installed. Currently the following images are supported: FBSD73-STD.

	
  2. Tarfiles specified with tb-set-tarfiles will only get installed once on experiment first swapin. To force a reinstall remove the file "/var/emulab/boot/rc.blobs-ran". on the node.

	
  3. tb-set-tarfiles will refuse to install tarballs when the target directory is an NFS-mounted filesystem.

	
  4. If the target directory starts with "/local", each node will automatically create a large local filesystem (using mkextrafs) mounted on "/local" prior to insatlling the tarballs.




#### Link Loss Commands


This is the NS syntax for creating a link:

    
    $ns duplex-link $node1 $node2 100Mb 150ms DropTail
    


Note that it does not allow for specifying link loss rates. Emulab does, however, support link loss. The following commands can be used to specify link loss rates.


#### 1. tb-set-link-loss




tb-set-link-loss _src_ _dst_ _loss_
tb-set-link-loss _link_ _loss_




tb-set-link-loss $node1 $node2 0.05
tb-set-link-loss $link1 0.02


_src_, _dst_ - Two nodes to describe the link.
_link_ - The link to set the rate for.
_loss_ - The loss rate (between 0 and 1).

Notes:



	
  * There are two syntax's available. The first specifies a link by a source/destination pair. The second explicitly specifies the link.

	
  * The source/destination pair is incapable of describing an individual link in the case of multiple links between two nodes. Use the second syntax for this case.




#### 2. tb-set-lan-loss




tb-set-lan-loss _lan_ _loss_




tb-set-lan-loss $lan1 0.3


_lan_ - The lan to set the loss rate for.
_loss_ - The loss rate (between 0 and 1).

Notes:



	
  * This command sets the loss rate for the entire LAN.




#### 3. tb-set-node-lan-delay




tb-set-node-lan-delay _node_ _lan_ _delay_




tb-set-node-lan-delay $node0 $lan0 40ms


_node_ - The node we are modifying the delay for.
_lan_ - Which LAN the node is in that we are affecting.
_delay_ - The new node to switch delay (see below).

Notes:



	
  * This command changes the delay between the node and the switch of the LAN. This is only half of the trip a packet must take. The packet will also traverse the switch to the destination node, possibly incurring additional latency from any delay parameters there.

	
  * If this command is not used to overwrite the delay, then the delay for a given node to switch link is taken as one half of the delay passed to make-lan. Thus in a LAN where no tb-set-node-delay calls are made the node to node latency will be the latency passed to make-lan.

	
  * The behavior of this command is not defined when used with nodes that are in the same LAN multiple times.

	
  * Delays of less than 2ms (per trip) are too small to be accurately modeled at this time, and will be silently ignored. As a convenience, a delay of 0ms can be used to indicate that you do not want added delay; the two interfaces will be "directly" connected to each other.




#### 4. tb-set-node-lan-bandwidth




tb-set-node-lan-bandwidth _node_ _lan_ _bandwidth_




tb-set-node-lan-bandwidth $node0 $lan0 20Mb


_node_ - The node we are modifying the bandwidth for.
_lan_ - Which LAN the node is in that we are affecting.
_bandwidth_ - The new node to switch bandwidth (see below).

Notes:



	
  * This command changes the bandwidth between the node and the switch of the LAN. This is only half of the trip a packet must take. The packet will also traverse the switch to the destination node which may have a lower bandwidth.

	
  * If this command is not used to overwrite the bandwidth, then the bandwidth for a given node to switch link is taken directly from the bandwidth passed to make-lan.

	
  * The behavior of this command is not defined when used with nodes that are in the same LAN multiple times.




#### 5. tb-set-node-lan-loss




tb-set-node-lan-loss _node_ _lan_ _loss_




tb-set-node-lan-loss $node0 $lan0 0.05


_node_ - The node we are modifying the loss for.
_lan_ - Which LAN the node is in that we are affecting.
_loss_ - The new node to switch loss (see below).

Notes:



	
  * This command changes the loss probability between the node and the switch of the LAN. This is only half of the trip a packet must take. The packet will also traverse the switch to the destination node which may also have a loss chance. Thus for packet going to switch with loss chance A and then going on the destination with loss chance B the node to node loss chance is (1-(1-A)(1-B)).

	
  * If this command is not used to overwrite the loss, then the loss for a given node to switch link is taken from the loss rate passed to the make-lan command. If a loss rate of L is passed to make-lan then the node to switch loss rate for each node is set to (1-sqrt(1-L)). Thus as each packet will have two such chances to be lost the node to loss rate comes out as the desired L.

	
  * The behavior of this command is not defined when used with nodes that are in the same LAN multiple times.




#### 6. tb-set-node-lan-params




tb-set-node-lan-params _node_ _lan_ _delay_ _bandwidth_ _loss_




tb-set-node-lan-params $node0 $lan0 40ms 20Mb 0.05


_node_ - The node we are modifying the loss for.
_lan_ - Which LAN the node is in that we are affecting.
_delay_ - The new node to switch delay.
_bandwidth_ - The new node to switch bandwidth.
_loss_ - The new node to switch loss.

Notes:



	
  * This command is exactly equivalent to calling each of the above three commands appropriately. See above for more information.




#### 7. tb-set-link-simplex-params




tb-set-link-simplex-params _link_ _src_ _delay_ _bw_ _loss_




tb-set-link-simplex-params $link1 $srcnode 100ms 50Mb 0.2


_link_ - The link we are modifying.
_src_ - The source, defining which direction we are modifying.
_delay_ - The source to destination delay.
_bw_ - The source to destination bandwidth.
_loss_ - The source to destination loss.

Notes:



	
  * This commands modifies the delay characteristics of a link in a single direction. The other direction is unchanged.

	
  * This command only applies to links. Use tb-set-lan-simplex-params below for LANs.




#### 8. tb-set-lan-simplex-params




tb-set-lan-simplex-params _lan_ _node_ _todelay_ _tobw_ _toloss_ _fromdelay_ _frombw_ _fromloss_




tb-set-lan-simplex-params $lan1 $node1 100ms 10Mb 0.1 5ms 100Mb 0


_lan_ - The lan we are modifying.
_node_ - The member of the lan we are modifying.
_todelay_ - Node to lan delay.
_tobw_ - Node to lan bandwidth.
_toloss_ - Node to lan loss.
_fromdelay_ - Lan to node delay.
_frombw_ - Lan to node bandwidth.
_fromloss_ - Lan to node loss.

Notes:



	
  * This command is exactly like tb-set-node-lan-params except that it allows the characteristics in each direction to be chosen separately. See all the notes for tb-set-node-lan-params.




#### 9. tb-set-endnodeshaping




tb-set-endnodeshaping _link-or-lan_ _enable_




tb-set-endnodeshaping $link1 1
tb-set-endnodeshaping $lan1 1


_link-or-lan_ - The link or LAN we are modifying.
_enable_ - Set to 1 to enable, 0 to disable.

Notes:



	
  * This command specifies whether [end node shaping](https://wiki.emulab.net/Emulab/wiki/linkdelays) is used on the specified link or LAN (instead of a delay node).

	
  * Disabled by default for all links and LANs.

	
  * Only available when running the standard Emulab FreeBSD or Linux kernels.

	
  * See [Node Traffic Shaping](https://wiki.emulab.net/Emulab/wiki/linkdelays) for more details.




#### 10. tb-set-noshaping




tb-set-noshaping _link-or-lan_ _enable_




tb-set-noshaping $link1 1
tb-set-noshaping $lan1 1


_link-or-lan_ - The link or LAN we are modifying.
_enable_ - Set to 1 to disable bandwidth shaping, 0 to enable.

Notes:



	
  * This command specifies whether link bandwidth shaping should be enforced on the specified link or LAN. When enabled, bandwidth limits indicated for a link or LAN will not be enforced.

	
  * Disabled by default for all links and LANs. That is, link bandwidth shaping _is_ enforced on all links and LANs by default.

	
  * If the delay and loss values for a tb-set-noshaping link are zero (the default), then no delay node or end-node delay pipe will be associated with the link or LAN.

	
  * **This command is a hack and may go away at any time! **The primary purpose for this command is to subvert the topology mapper, assign). Assign always observes the physical bandwidth constraints of the testbed. By using tb-set-noshaping, you can convince assignthat links are low-bandwidth and thus get your topology mapped, but then not actually have the links shaped.




#### 11. tb-use-endnodeshaping




tb-use-endnodeshaping _enable_




tb-use-endnodeshaping 1


_enable_ - Set to 1 to enable end-node traffic shaping on all links and LANs.

Notes:



	
  * This command allows you to use end-node traffic shaping globally, without having to specify per link or LAN with tb-set-endnodeshaping.

	
  * See [Node Traffic Shaping](https://wiki.emulab.net/Emulab/wiki/linkdelays) for more details.




#### 12. tb-force-endnodeshaping




tb-force-endnodeshaping _enable_




tb-force-endnodeshaping 1


_enable_ - Set to 1 to force end-node traffic shaping on all links and LANs.

Notes:



	
  * This command allows you to specify non-shaped links and LANs at creation time, but still control the shaping parameters later (e.g., increase delay, decrease bandwidth) after the experiment is swapped in.

	
  * This command forces allocation of end-node shaping infrastructure for all links. There is no equivalent to force delay node allocation.

	
  * See [Node Traffic Shaping](https://wiki.emulab.net/Emulab/wiki/linkdelays) for more details.




#### 13. tb-set-multiplexed




tb-set-multiplexed _link_ _allow_




tb-set-multiplexed $link1 1


_link_ - The link we are modifying.
'allow_ - Set to 1 to allow multiplexing of the link, 0 to disallow._

Notes:



	
  * This command allows a link to be multiplexed over a physical link along with other links.

	
  * Disabled by default for all links.

	
  * Only available when running the standard Emulab FreeBSD (not Linux) and only for links (not LANs).

	
  * See [Multiplexed Links](https://wiki.emulab.net/Emulab/wiki/linkdelays) for more details.




#### Virtual Type Commands


Virtual Types are a method of defining fuzzy types. I.e. types that can be fulfilled by multiple different physical types. The advantage of virtual types (vtypes) is that all nodes of the same vtype will usually be the same physical type of node. In this way, vtypes allows logical grouping of nodes.

As an example, imagine we have a network with internal routers connecting leaf nodes. We want the routers to all have the same hardware, and the leaf nodes to all have the same hardware, but the specifics do not matter. We have the following fragment in our NS file:

    
    ...
    tb-make-soft-vtype router {pc600 pc850}
    tb-make-soft-vtype leaf {pc600 pc850}
    
    tb-set-hardware $router1 router
    tb-set-hardware $router2 router
    tb-set-hardware $leaf1 leaf
    tb-set-hardware $leaf2 leaf
    


Here we have set up two soft (see below) vtypes, router and leaf. Our router nodes are then specified to be of type _router_, and the leaf nodes of type _leaf_. When the experiment is swapped in the testbed will attempt to make router1 and router2 be of the same type, and similarly, leaf1 and leaf2 of the same type. However, the routers/leafs may be pc600s or they may be pc850s, whichever is easier to fit in to the available resources.

As a basic use, vtypes can be used to request nodes that are all the same type, but can be of any available type:

    
    ...
    tb-make-soft-vtype N {pc600 pc850}
    
    tb-set-hardware $node1 N
    tb-set-hardware $node2 N
    


Vtypes come in two varieties, hard and soft. With soft vtypes, the testbed will try to make all nodes of that vtype the same physical type, but may do otherwise if resources are tight. Hard vtypes behave just like soft vtypes except that the testbed will give higher priority to vtype consistency and swapping in will fail if the vtypes can not be satisfied. So, if you use soft vtypes you are more likely to swap in but there is a chance your node of a specific vtype will not all be the same. If you use hard vtypes all nodes of a given vtype will be the same, but swapping in may fail.

Finally, you can have weighted soft vtypes. Here you assign a weight from 0 to 1 exclusive to your vtype. The testbed will give higher priority to consistency in the higher weighted vtypes. The primary use of this is to rank multiple vtypes by importance of consistency. Soft vtypes have a weight of 0.5 by default.

As a final note, when specifying the types of a vtype, use the most specific type possible. For example: tb-make-soft-vtype router {pc pc600}, is not very useful, as pc600 is a sub type of pc. You may very well end up with two routers as type pc with different hardware, as pc covers multiple types of hardware.


#### 1. tb-make-soft-vtype




tb-make-soft-vtype _vtype_ {_types_}
tb-make-hard-vtype _vtype_ {_types_}
tb-make-weighted-vtype _vtype_ _weight_ {_types_}




tb-make-soft-vtype router {pc600 pc850}
tb-make-hard-vtype leaf {pc600 pc850}
tb-make-weighted-vtype A 0.1 {pc600 pc850}


_vtype_ - The name of the vtype to create.
_types_ - One or more physical types.
_weight_ - The weight of the vtype, 0 < _weight_ < 1.

Notes:



	
  * These commands create vtypes. See notes above for description of vtypes and the difference between soft and hard.

	
  * tb-make-soft-vtype creates vtypes with weight 0.5.

	
  * vtype commands must appear before tb-set-hardware commands that use them.

	
  * Do not used tb-fix-node with nodes that have a vtype.




#### Misc. Commands




#### 1. tb-fix-node




tb-fix-node _vnode_ _pnode_




tb-fix-node $node0 pc42


_vnode_ - The node we are fixing.
_pnode_ - The physical node we want used.

Notes:



	
  * This command forces the virtual node to be mapped to the specified physical node. Swap in will fail if this can not be done.

	
  * Do not use this command on nodes that are a virtual type.




#### 2. tb-fix-interface




tb-fix-interface _vnode_ _vlink_ _iface_




tb-fix-interface $node0 $link0 "eth0"


_vnode_ - The node we are fixing.
_vlink_ - The link connecting to that node that we want to set.
_iface_ - The Emulab name for the interface that is to be used.

Notes:



	
  * The interface names used are the ones in the Emulab database - we can make no guarantee that the OS image that boots on the node assigns the same name.

	
  * Different types of nodes have different sets of interfaces, so this command is most useful if you are also using tb-fix-node and/or tb-set-hardware on the vnode.




#### 3. tb-set-uselatestwadata




tb-set-uselatestwadata 0
tb-set-uselatestwadata 1


Notes:



	
  * This command indicates which widearea data to use when mapping widearea nodes to links. The default is 0, which says to use the aged data. Setting it to 1 says to use the most recent data.




#### 4. tb-set-wasolver-weights




tb-set-wasolver-weights _delay bw plr_




tb-set-wasolver-weights 1 10 500


_delay_ - The weight to give delay when solving.
_bw_ - The weight to give bandwidth when solving.
_plr_ - The weight to give lossrate when solving.

Notes:



	
  * This command sets the relative weights to use when assigning widearea nodes to links. Specifying a zero says to ignore that particular metric when doing the assignment. Setting all three to zero results in an essentially random selection.




#### 5. tb-set-node-failure-action




tb-set-node-failure-action _node action_




tb-set-node-failure-action $nodeA "fatal"
tb-set-node-failure-action $nodeB "nonfatal"


_node_ - The node name.
_action_ - One of "fatal" or "nonfatal"

Notes:



	
  * This command sets the failure mode for a node. When an experiment is swapped in, the default action is to abort the swapin if any nodes fail to come up normally. This is the "fatal" mode. You may also set a node to "nonfatal" which will cause node bootup failures to be reported, but otherwise ignored during swapin. Note that this can result in your experiment not working properly if a dependent node fails, but typically you can arrange your software to deal with this.


