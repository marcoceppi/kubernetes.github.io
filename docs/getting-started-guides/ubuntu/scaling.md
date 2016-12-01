* TOC
{:toc}

# Scaling

Any of the applications can be scaled out post-deployment. The charms
update the status messages with progress, so it is recommended to run.

```
watch -c juju status --color
```

## Kubernetes masters

## Kubernetes workers

The kubernetes-worker nodes are the load-bearing units of a Kubernetes cluster.

By default pods are automatically spread throughout the kubernetes-worker units
that you have deployed.

To add more kubernetes-worker units to the cluster:

```
juju add-unit kubernetes-worker
```

or specify machine constraints to create larger nodes:

```
juju set-constraints kubernetes-worker "cpu-cores=8 mem=32G"
juju add-unit kubernetes-worker
```

Refer to the
[machine constraints documentation](https://jujucharms.com/docs/stable/charms-constraints)
for other machine constraints that might be useful for the kubernetes-worker units.

## etcd

Etcd is used as a key-value store for the Kubernetes cluster. The bundle
defaults to one instance in this cluster.

For reliability and more scalability, we recommend between 3 and 9 etcd nodes.
If you want to add more nodes:

```
juju add-unit etcd
```

The CoreOS etcd documentation has a chart for the
[optimal cluster size](https://coreos.com/etcd/docs/latest/admin_guide.html#optimal-cluster-size)
to determine fault tolerance.

## Juju Controller

A single node is responsible for coordinating with all the Juju agents on each machine that manage Kubernetes, it is called the controller node. For production deployments it is recommended to enable HA of the controller node:

    juju enable-ha
    
Enabling HA results in 3 controller nodes, this should be sufficient for most use cases. 5 and 7 controller nodes are also supported for extra large deployments. 
    
Refer to the [Juju HA controller documentation](https://jujucharms.com/docs/2.0/controllers-ha) for more information. 






## Scale up cluster

Want larger Kubernetes nodes? It is easy to request different sizes of cloud
resources from Juju by using **constraints**. You can increase the amount of
CPU or memory (RAM) in any of the systems requested by Juju. This allows you
to fine tune th Kubernetes cluster to fit your workload. Use flags on the
bootstrap command or as a separate `juju constraints` command. Look to the
[Juju documentation for machine](https://jujucharms.com/docs/2.0/charms-constraints)
details.

## Scale out cluster

Need more workers? We just add more units:    

```shell
juju add-unit kubernetes-worker
```

Or multiple units at one time:  

```shell
juju add-unit -n3 kubernetes-worker
```
You can also ask for specific instance types or other machine-specific constraints. See the [constraints documentation](https://jujucharms.com/docs/stable/reference-constraints) for more information. Here are some examples, note that generic constraints such as `cores` and `mem` are more portable between clouds. In this case we'll ask for a specific instance type from AWS: 

```shell
juju set-constraints kubernetes-worker instance-type=c4.large
juju add-unit kubernetes-worker
```

You can also scale the etcd charm for more fault tolerant key/value storage:  

```shell
juju add-unit -n3 etcd
```
It is strongly recommended to run an odd number of units for quorum. 
