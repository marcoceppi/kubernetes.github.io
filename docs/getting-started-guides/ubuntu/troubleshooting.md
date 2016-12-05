* TOC
{:toc}

# Troubleshooting

This document with highlighting how to troubleshoot the deployment of a Kubernetes cluster, it will not cover debugging of workloads inside Kubernetes. 

## Debug Action


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