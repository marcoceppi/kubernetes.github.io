* TOC
{:toc}

# Monitoring

## Connecting Prometheus

Prometheus monitoring is not yet available, but is a work in progress. 

## Connecting Nagios

Nagios utilizes the Nagions Remote Execution Protocol (NRPE) as an agent on each node to derive machine level details of the health and applications. To make initial setup easier, a pre-built bundle exists with these integrations

[![

### New install of Nagios

To start, deploy the latest version of the Nagios and NRPE charms from the store:

```
juju deploy nagios
juju deploy nrpe
```

Connect Nagios to NRPE

```
juju add-relation nagios nrpe
```

Finally, add NRPE to all applications deployed that you wish to monitor, for example `kubernetes-master`, `kubernetes-worker`, `etcd`, `easyrsa`, and `kubeapi-load-balancer`.

```
juju add-relation nrpe kubernetes-master
juju add-relation nrpe kubernetes-worker
juju add-relation nrpe etcd
juju add-relation nrpe easyrsa
juju add-relation nrpe kubeapi-load-balancer
```

### Existing install of Nagios

If you already have an exisiting Nagios installation, the `nrpe-external-master` charm can be used instead. This will allow you to supply configuration options mapping your exisiting external Nagios installation to NRPE.


## Connecting Elastic stack
## Connecting Datadog
## Connecting Sysdig
