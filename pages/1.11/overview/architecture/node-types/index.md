---
layout: layout.pug
navigationTitle:  Node Types
title: Node Types
menuWeight: 1
excerpt: Understanding DC/OS node types

enterprise: false
---

<!-- The source repository for this topic is https://github.com/dcos/dcos-docs-site -->


A DC/OS node is a virtual or physical machine on which DC/OS components run. DC/OS nodes are networked together to form a DC/OS cluster.

A DC/OS cluster is composed of three types of nodes: master nodes, private agent nodes, and public agent nodes.

It is recommended to use different types of nodes to be set up in different networks (private, public, master) as seen in the below diagram.

Protect your cluster from the outside with firewall settings specific for the node type.

**Note:** You must have the internal communication between the different nodes as open as possible:
- Each node has unfettered IP-to-IP connectivity from itself to all nodes in the DC/OS cluster.
- All ports should be open for communication from the master nodes to the agent nodes and vice versa.

**Important**: Setting firewall rules between the nodes of any types is not recommended.

![DC/OS Node Types](/1.11/img/dcos-node-types.png)


## Master Nodes

A DC/OS master node is a node that works together with other master nodes to manage the rest of the cluster.

Master nodes contain the bulk of the DC/OS components, including a Mesos master process.

### Protected Zone

Depending on distribution, deployment method, and infrastructure configuration, master nodes may be publicly accessible or in a network zone that restricts access to improve security. Common strategies include restricting master node access to the IP range of your offices and requiring [VPN](https://en.wikipedia.org/wiki/Virtual_private_network) access.

### High Availability

Multiple master nodes work together to provide high availability and fault tolerance.

A cluster with only one master node is usable for development, but is not highly available and may not be able to recover from failure.

### Leader Election

Mesos performs [leader election](https://en.wikipedia.org/wiki/Leader_election) and routes incoming traffic to the current leader to ensure consistency.

Like Mesos, several other DC/OS master node components perform independent leader election. This means that the leaders for different components, like Marathon and ZooKeeper, may be on different master nodes.

### Quorum

To maintain consistency, a quorum (half plus one) of master nodes must be connected at all times.

For example, having three master nodes allows one to be down; having five master nodes allows two to be down, allowing for failure during a rolling update. Additional master nodes can be added for additional risk tolerance.

The number of master nodes can only be specified during installation. This is primarily because of the complexity of changing the quorum and configuration of multiple components with leaders on different nodes. This may change in the future.


## Agent Nodes

A DC/OS agent node is a node on which user tasks are run.

Agent nodes contain a few DC/OS components, including a Mesos agent process.

Agent nodes can be public or private, depending on agent and network configuration.

### Public Agent Nodes

A public agent node is an agent node that is on a network that allows ingress from outside of the cluster via the cluster’s [infrastructure networking](/1.11/overview/concepts/#infrastructure-network).

The resources on public agent nodes are, by default, configured to only be allocated to tasks that specify the `slave_public` role. The Mesos agents on public agent nodes also have the `public_ip:true` agent attribute to assist in their discovery.

Public agent nodes are used primarily for externally facing reverse proxy load balancers, like Marathon-LB. This provides a [DMZ](https://en.wikipedia.org/wiki/DMZ_%28computing%29) that decreases the surface area accessible by malicious attackers.

Clusters generally have only a few public agent nodes, because a few load balancers can usually handle proxying to multiple services.

### Private Agent Nodes

A private agent node is an agent node that is on a network that does not have ingress access from outside of the cluster via the cluster’s [infrastructure networking](/1.11/overview/concepts/#infrastructure-network).

The resources on private agent nodes are, by default, configured to allow undifferentiated allocation. More precisely, the resources are given the `*` role, allowing them to be allocated to any task that does not specify a role. For more information, see [Mesos resource roles](http://mesos.apache.org/documentation/latest/roles/).

Because these resources are undifferentiated, most tasks are scheduled on private agent nodes and are inaccessible from outside the cluster, decreasing the surface area accessible by malicious attackers. For this reason, clusters are generally comprised of mostly private agent nodes. Likewise, most [Mesosphere Universe](/1.11/overview/concepts/#mesosphere-universe) packages install by default on private agent nodes.

## More Information

For more on master and agent node components, see [Components](/1.11/overview/architecture/components/).

For more on security, see [Securing Your Cluster](/1.11/administering-clusters/).

For more on scaling your cluster, see [Adding Agent Nodes](/1.11/administering-clusters/add-a-node/).

For more on configuring public nodes, see [Converting Agent Node Types](/1.11/administering-clusters/convert-agent-type/).
