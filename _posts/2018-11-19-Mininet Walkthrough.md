---
layout:     post                    # 使用的布局（不需要改）
title:      Mininet Walkthrough               # 标题 
subtitle:    #副标题
date:       2018-11-19              # 时间
author:     Aaron Mao   # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Mininet
---

> **This is a simple note about Mininet Walkthrough**

> **for complete source documentation please click here:http://mininet.org/walkthrough/#run-a-regression-test**


basic mininet workflow and additional details are available in the Mininet walkthrough

the OpenFlow tutorial and Mininet documentation

# Mininet Sample Workflow
### creating a Network
> sudo mn --switch ovs --controller ref --topo tree,depth=2,fanout=8 --test pingall

**interacting with a network**
> mininet> h1 ping h2

start a web server on one host and make an HTTP request from another:
> mininet> h2 python -m SimpleHTTPServer 80 >& /tmp/http.log &

> mininet> h3 wget -O -h2

**customizing a network**

> from mininet.net import Mininet

> from mininet.topolib import TreeTopo

> tree4 = TreeTopo(depth=2,fanout=2)

> net = Mininet(topo = tree4)

> net.start()

> h1, h4 = net.hosts[0], net.hosts[3]

> print h1.cmd('ping -c1 %s ' % h4.IP())

> net.stop()

> sharing and running on hardware

# Mininet Walkthrough

this walkthrough demonstrates most Mininet commands ,as well as its typical usage in concert with the wireshark dissector

## Part 1: Everyday Mininet Usage

**Display startup options**
> sudo mn -h

### start wireshark

###  interact with hosts and switches

> mininet> h1 ifconfig -a

> mininet> s1 ifconfig -a

this will show the interfaces 

print the process list from a host process:
> mininet> h1 ps -a

**Test connectivity between hosts**

> mininet> h1 ping -c 1 h2

**Openflow control traffic:**

the first host ARPs for MAC of the second, cause a  **packet_in** message to go to the controller

the controller sends a **packet_out** message to flood the broadcast packet to other ports on the switch

the second host sees the ARp request and sends a **reply**.

this **reply** goes to the controller , which sends it to the first host and pusher down a flow entry

## Part 2: Advanced Startup Options

**Run a Regression Test**

Mininet can boot a default OpenFlow reference controller if u don't entry the parameter
and we can test something directly

**Changing Topology size and type**

default topo is a single switch connected to two hosts

--topo can used to change this

> --topo linear,4

> --topo single,3

> --topo tree = depth,4 fanout,2

**Link variations**

set link parameters

> sudo mn --link tc,bw = 10, delay = 10ms

**Adjustable Verbosity**

prints what mininet is doing during startup and teardown

> sudo mn -v debug

> sudo mn -v output

**Custom Topologies**

using simple Python API

> from mininet.topo import Topo

class Mytopo( Topo ):

    def __init__(self):
    
        Topo.__init__(self)

        LeftHost = self.addHost('h1')

        ...   
        self.addLink(leftHost,rightHost)
    
    topos = {‘mytopo’: ( lamba:Mytopo() ) }


**other Switch Types**

> --switch user --test iperf

ping test hava a much higher delay

user-space switch can be a great starting point for implementing new functionality,especially where software performance is not critical
another switch type is **Open vSwitch OVS** which comes preinstalled on the mininet VM
>  sudo mn --switch ovsk --test iperf

**Mininet Benchmark 基准**

to record the time to set up and tear down a topo,use test 'none'
> sudo mn --test none

**Everything in its own Namespace(user swtich only)**

by default, the hosts are put in their own namespace

switched and controller are in the **root namespace**

to put switcher in their own namespace,pass **--innamespace** option
> sudo mn --innamespace --switch user


## Part 3:Mininet Command-Line interface (CLI) Commands
display options

**python interpreter**

print the accessible local variables:
> mininet> py locals()

see the methods and properties avaiable for a node
> mininet> py dir(s1)

evaluate methods of variables:

> mininet> py h1.IP()

**Link up or down**

> mininet> link s1 h1 down

mininet> link s1 h1 up

## Part 4: Python API Examples

**examples directoyu** in the mininet source code includes many examples of how to user mininet's api

links: https://github.com/mininet/mininet/tree/master/examples

## Part 5: Walkthrough Complete 

**OpenFlow tutorial:** https://github.com/mininet/openflow-tutorial/wiki

**The introduction to Mininet** provides an introduction t Mininet and its python API

and u can also learn to user **remoter controller**(running outside Mininet's Control)
> sudo mn --controller=remote,ip =[ ],port=[ ]















