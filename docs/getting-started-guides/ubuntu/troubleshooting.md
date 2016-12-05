* TOC
{:toc}

# Troubleshooting

This document with highlighting how to troubleshoot the deployment of a Kubernetes cluster, it will not cover debugging of workloads inside Kubernetes. 

## Debug Action

## Understanding Cluster Status

Using `juju status` can give you some insight as to what's happening in a cluster:


```
Model  Controller  Cloud/Region   Version
kubes  work-multi  aws/us-east-2  2.0.2.1

App                Version  Status  Scale  Charm              Store       Rev  OS      Notes
easyrsa            3.0.1    active      1  easyrsa            jujucharms    3  ubuntu  
etcd               2.2.5    active      1  etcd               jujucharms   17  ubuntu  
flannel            0.6.1    active      2  flannel            jujucharms    6  ubuntu  
kubernetes-master  1.4.5    active      1  kubernetes-master  jujucharms    8  ubuntu  exposed
kubernetes-worker  1.4.5    active      1  kubernetes-worker  jujucharms   11  ubuntu  exposed

Unit                  Workload  Agent  Machine  Public address  Ports           Message
easyrsa/0*            active    idle   0/lxd/0  10.0.0.55                       Certificate Authority connected.
etcd/0*               active    idle   0        52.15.47.228    2379/tcp        Healthy with 1 known peers.
kubernetes-master/0*  active    idle   0        52.15.47.228    6443/tcp        Kubernetes master services ready.
  flannel/1           active    idle            52.15.47.228                    Flannel subnet 10.1.75.1/24
kubernetes-worker/0*  active    idle   1        52.15.177.233   80/tcp,443/tcp  Kubernetes worker running.
  flannel/0*          active    idle            52.15.177.233                   Flannel subnet 10.1.63.1/24

Machine  State    DNS            Inst id              Series  AZ
0        started  52.15.47.228   i-0bb211a18be691473  xenial  us-east-2a
0/lxd/0  started  10.0.0.55      juju-153b74-0-lxd-0  xenial  
1        started  52.15.177.233  i-0502d7de733be31bb  xenial  us-east-2b
```

In this example we can glean some information. The `Workload` column will show the status of a given service. The `Message` section will show you the health of a given service in the cluster. During deployment and maintenance these workload statuses will update to reflect what a given node is doing. For example the workload my say `maintenance` while message will describe this maintenance as `Installing docker`.

During normal oprtation the Workload should read `active`, the Agent column (which reflects what the Juju agent is doing) should read `idle`, and the messages will either say `Ready` or another descriptive term. `juju status --color` will also return all green results when a cluster's deployment is healthy. 

Status can become unweildly for large clusters, it is then recommended to check status on individual services, for example to check the status on the workers only:

    juju status kubernetes-workers

or just on the etcd cluster:

    juju status etcd

Errors will have an obvious message, and will return a red result when used with `juju status --color`. Nodes that come up in this manner should be investigated.   

## SSHing to units.

You can ssh to individual units easily with the following convention, `juju ssh <servicename>/<unit#>:

    juju ssh kubernetes-worker/3

Will automatically ssh you to the 3rd worker unit.

    juju ssh easyrsa/0 

This will automatically ssh you to the easyrsa unit. 

## Common Problems

## etcd

## Kubernetes

By default there is no log aggregation of the Kubernetes nodes, each node logs locally. It is recommended to deploy the Elastic Stack for log aggregation if you desire centralized logging. 