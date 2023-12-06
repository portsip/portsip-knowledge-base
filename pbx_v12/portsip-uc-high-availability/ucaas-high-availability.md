# UCaaS High Availability

## PortSIP UCaaS High Availability

Today, PortSIP UCaaS is deployed and managed as a [Kubernetes ](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)Cluster; in this part, we'll focus on two topics: high availability of the Kubernetes Cluster and high availability of SIP Service Discovery.

### Kubernetes Cluster High Availability

A Kubernetes cluster is a set of node machines for running containerized applications. If you’re running Kubernetes, you’re running a cluster.

Kubernetes will automatically manage your cluster to match the desired state. As a simple example, suppose you deploy an application with a desired state of "3," meaning 3 replicas of the application should be running. If 1 of those containers crashes, Kubernetes will see that only 2 replicas are running, so it will add 1 more to satisfy the desired state.

Benefit from the Kubernetes Cluster, the PortSIP UCaaS will take the below Advantages:

#### Avoid Single Points of Failure

Kubernetes helps improve reliability by providing redundant components, and making it possible to schedule application containers across multiple nodes and multiple availability zones (AZs) in the cloud. Use anti-affinity or node selection to help spread your applications across the Kubernetes cluster for high availability.

Node selection allows you to define which nodes in your cluster are eligible to run your application based on labels. The labels typically represent node characteristics like bandwidth or special resources like CPUs.

Anti-affinity allows you to further constrain nodes where your application should not be allowed to run, based on the presence of labels. This keeps your application containers from running on the same node, or from running on the same node with other components of the same application.

#### Set Resource Requests and Limits&#x20;

Resource requests and limits for CPU and memory are at the heart of what allows the Kubernetes scheduler to do its job. If a single pod is allowed to consume all of the node CPU and memory, then resources will be starved from other pods and potentially Kubernetes components. Setting limits on a pods consumption will increase reliability by keeping pods from consuming all of the available resources on a node (this is referred to as the “noisy neighbor problem”).



### SIP Service Discovery Using DNS SRV Records

The Domain Name System(DNS) distributes the responsibility for assigning domain names and mapping them to IP networks by allowing an authoritative server for each domain to keep track of its own changes, avoiding the need for a central registrar to be continually consulted and updated. An SRV record or Service record is a category of data in the Domain Name System specifying information on available services. It is defined in RFC 2782. Newer internet protocols, such as SIP, often require SRV support from clients.

An example SRV record might look like this using bind syntax:&#x20;

```
_sip._udp.carrier.com. 86400 IN SRV 0 5 5060 sip.carrier.com
```

This points to a server named **carrier.com** listening on UDP port 5060 for SIP protocol connections. The priority given here is 0, and the weight is 5. With SIP telephony, a SIP call might start as **1001@carrier.com** for the call to proceed, the phone needs to locate **sip.carrier.com** and it will use DNS to do this. In case the sip.carrier.com is the edge proxy server of PortSIP UCaaS.

In the following example, a DNS SRV record query to **uc.portsip.com** with both the priority and weight fields each with 50% of the traffic load is used to provide a load balancing and backup service and would yield:

```
 _sip._udp.uc.portsip.com 60 IN SRV 10 50 5060 proxy1.portsip.com
 _sip._udp.uc.portsip.com 60 IN SRV 10 50 5060 proxy2.portsip.com
 _sip._udp.uc.portsip.com 60 IN SRV 20 0 5060  proxy3.portsip.com
```

If one edge proxy server becomes unavailable, the remaining machine takes the load.

The two records share a priority of 50, so the weight field’s value will be used by clients to determine which server (host and port combination) to contact. The sum both values is 100, so **proxy1.portsip.com** will be used 50% of the time and **proxy2.portsip.com** will be used 50% of the time.

DNS A Record Query yields:

```
proxy1.portsip.com = 66.175.222.56
proxy2.portsip.com = 66.175.222.57
proxy3.portsip.com = 66.175.222.58
```

If both edge proxy servers with priority 10 are unavailable, the record with the next highest priority value will be chosen, which is **proxy3.portsip.com**. This might be a machine in another physical location, presumably not vulnerable to anything that would cause the first two hosts to become unavailable. Several implementations relying on DNS also automatically update the DNS servers to optimize the load of servers or to remove servers due to service failures.

**Figure 1-1**   PortSIP UCasS DNS SRV

![](../../.gitbook/assets/ucaas\_ha\_diagram.png)
