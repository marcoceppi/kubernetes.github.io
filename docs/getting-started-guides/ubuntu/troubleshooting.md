* TOC
{:toc}

# Troubleshooting

This document with highlighting how to troubleshoot the deployment of a Kubernetes cluster, it will not cover debugging of workloads inside Kubernetes. 

## Debug Action

## Logging

The `juju debug-log` will show all of the consolidated logs of all the Juju agents running on each node of the cluster. This can be useful for finding out why a specific node hasn't deployed or is in an error state. These agent logs are located in `/var/lib/juju/agents` on every node. 

See the [Juju documentation](https://jujucharms.com/docs/stable/troubleshooting-logs) for more information.

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