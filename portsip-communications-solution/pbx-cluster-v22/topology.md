# Topology

Traditional PBX systems place heavy demands on server resources: CPU, memory, disk I/O, and network bandwidth when handling large volumes of concurrent calls, meetings, queue traffic, IVR interactions, and real-time media processing. As concurrency grows, single-node deployments quickly become a bottleneck, making a clustered architecture essential for scalability, performance, and reliability.

PortSIP PBX is designed from the ground up with a **distributed, service-oriented architecture**. At the core is a centralized **Call Manager**, dedicated exclusively to SIP signaling and call control, while specialized functional servers—such as Media, Meeting, Queue (ACD), IVR, Instant Messaging, and DataFlow—operate independently and scale horizontally as a cluster.

This architecture cleanly separates signaling from media and application workloads, enabling efficient resource utilization, predictable performance, and seamless horizontal scaling across large deployments.

***

### Scalability and Capacity

The PortSIP PBX cluster architecture is engineered for carrier-grade environments and can support:

* Over 1 million users
* Approximately 100,000 concurrent online users (registered or signed in)
* Up to 10,000 simultaneous calls

***

### Cluster Architecture Components

The diagram below illustrates the overall PortSIP PBX cluster topology. Each component plays a distinct role in ensuring scalability, performance, and operational resilience.

<figure><img src="../../.gitbook/assets/pbx_cluster.png" alt=""><figcaption></figcaption></figure>

#### Call Manager Server

The Call Manager is the central control plane of the system. It is deployed together with the PBX core services, database, and load-balancing logic, and is responsible for:

* SIP signaling and call control
* Registration and authentication
* Routing logic and policy enforcement
* Coordination of all backend service nodes

By isolating signaling from media processing, the Call Manager ensures stable call control even under heavy load.

***

#### Media Servers

Media Servers handle real-time media processing, including RTP streams, transcoding, recording, and tone generation.

* Can be deployed as a single server or as a scalable cluster
* Calls are dynamically load-balanced by the Call Manager
* Additional Media Servers can be added at any time to increase capacity

This design allows media workloads to scale independently of signaling.

***

#### IVR Servers

IVR Servers process interactive voice response logic, prompts, and DTMF input.

* Can be deployed standalone or as a clustered service
* IVR sessions are load-balanced across available servers
* Capacity can be expanded by adding more IVR Servers as required

***

#### Queue Servers (ACD)

Queue Servers manage contact center queue logic, including call distribution, agent state handling, and queue metrics.

* Support single-server or clustered deployment
* Load balancing ensures queues are evenly distributed
* Additional servers can be added seamlessly as traffic grows

***

#### Meeting Servers

Meeting Servers are responsible for audio and video conferencing workloads.

* Meetings are dynamically distributed across the cluster
* Capacity scales horizontally by adding more Meeting Servers
* Ensures consistent performance for large or concurrent meetings

***

#### Instant Messaging (IM) Servers

IM Servers provide real-time messaging and collaboration services.

* Typically deployed on a single high-performance server
* Can support up to **50,000 concurrent online users**
* Optimized for high-throughput messaging workloads

***

#### DataFlow Servers

DataFlow Servers power analytics, reporting, dashboards, and real-time metrics.

* Usually deployed on a dedicated, high-performance server
* Designed for high-volume data ingestion and fast query performance
* Provides real-time visibility into call activity and system performance

***

### Cluster Architecture with High Availability (HA)

For customers requiring maximum uptime and fault tolerance, PortSIP PBX supports a **High Availability (HA) cluster architecture**.

<figure><img src="../../.gitbook/assets/pbx_ha_cluster_diagram.png" alt=""><figcaption></figcaption></figure>

#### Call Manager Servers (HA)

In an HA deployment:

* **Three Call Manager servers** are deployed in an HA configuration
* A **Virtual IP (VIP)** is exposed to client apps, IP phones, and backend services
* SIP signaling continues seamlessly even if one Call Manager fails

This ensures continuous service availability and eliminates single points of failure in the control plane.

Detailed design and deployment guidance is available in the **PortSIP PBX High Availability Architecture** documentation.

***

#### Media Servers (HA)

* Media Servers are continuously monitored by the Call Manager
* If a Media Server fails, active calls are automatically reassigned to available servers
* No manual intervention is required for call recovery

***

<figure><img src="../../.gitbook/assets/ha_callmanager.png" alt=""><figcaption></figcaption></figure>

#### IVR Servers (HA)

* If an IVR Server becomes unavailable, administrators receive an email alert
* The failed server can be disabled through the PBX web portal
* IVR traffic is automatically redistributed to remaining servers

***

#### Queue Servers (HA)

Queue Servers follow the same high-availability behavior as IVR Servers:

* Automatic failure detection
* Email alerts to administrators
* Manual disablement via the web portal
* Automatic reassignment of queue workloads

***

#### Meeting Servers (HA)

* Meeting Server failures trigger administrator notifications
* The failed server can be disabled through the PBX web portal
* Active and new meetings are redirected to healthy servers

***

#### IM Servers (HA)

* IM Servers support up to 50,000 online users per high-performance node
* In HA deployments, multiple IM Servers ensure uninterrupted messaging services
* Failures do not disrupt user messaging sessions

***

#### DataFlow Servers (HA)

* DataFlow Servers provide analytics and real-time metrics
* Typically deployed on powerful dedicated hardware
* Designed for continuous availability and high data throughput

***

### Summary

PortSIP PBX’s cluster and HA architecture delivers:

* True horizontal scalability
* Clear separation of signaling, media, and application workloads
* Carrier-grade reliability and fault tolerance
* Simple, incremental expansion without service disruption

This architecture makes PortSIP PBX an ideal platform for service providers, enterprises, and UCaaS operators building large-scale, high-performance communication services.

***

If you’d like, I can next:





Typically, PBX systems face significant hardware demands—including CPU, memory, and bandwidth—when managing a high volume of simultaneous calls, meetings, queue calls, and IVR interactions. To handle these scenarios effectively, PBX systems often require deployment in a clustered architecture.

PortSIP streamlines this process by leveraging a centralized Call Manager server dedicated exclusively to SIP signaling, while specialized media servers, meeting servers, queue servers, Instant Messaging servers, and IVR servers operate as a cluster. This distributed architecture efficiently manages media, recording, ACD, messaging, and other call processing tasks, ensuring optimal resource utilization and effortless scalability.

The PortSIP PBX Cluster topology is built to support over 1 million users, approximately 50,000 concurrent online users (registered or signed-in), and up to 10,000 simultaneous calls. For service providers requiring the capacity to serve more than 50,000 concurrent users, we recommend exploring our advanced PortSIP UCaaS solution for unmatched scalability and performance.

## Cluster Architecture Overview

The diagram below illustrates the PBX cluster architecture. Here’s how each component works:

<figure><img src="../../.gitbook/assets/pbx_cluster.png" alt=""><figcaption></figcaption></figure>

* **Call Manager Server**\
  The main server is deployed with the PBX Call Manager, Database, and Load Balancer. It primarily handles SIP signaling and database operations.
* **Media Servers**\
  Media Servers can be deployed as a single server or in a cluster. The Call Manager dynamically load-balances calls across the available media servers. As your business scales, you can easily add more media servers.
* **IVR Servers**\
  IVR Servers function similarly to Media Servers. They can be deployed individually or as a cluster. The load balancer assigns IVRs to the available servers. Additional servers can be added as needed.
* **Queue Servers**\
  Queue Servers, like Media and IVR servers, can be deployed as a single server or a cluster. The load balancer ensures that call queues are managed across the available servers. More servers can be added as your needs grow.
* **Meeting Servers**\
  Meeting Servers handle virtual meetings, and like the other servers, they can be scaled by adding more servers to the cluster. The load balancer ensures meetings are distributed effectively.
* **IM Servers**\
  IM Servers, while typically deployed on a single server, can support up to 50,000 online users with powerful server hardware.
* **Data Flow Servers**\
  DataFlow Servers, while typically deployed on a single server, provide data analytics and real-time metrics with powerful server hardware.

## Cluster with High Availability (HA) Architecture

For customers requiring enhanced reliability, the **HA architecture** provides high availability by deploying multiple Call Manager Servers and ensuring fault tolerance.

<figure><img src="../../.gitbook/assets/pbx_ha_cluster_diagram.png" alt=""><figcaption></figcaption></figure>

* **Call Manager Server**\
  In this architecture, three Call Manager servers are deployed with high availability (HA) support. A **Virtual IP** is exposed for access by other server components, client apps, and IP phones. This setup provides seamless SIP signaling management and guarantees continuous service in case of server failure. \
  The diagram below provides detailed information about the **Call Manager HA** component, as referenced in the above **PortSIP PBX** **HA Cluster** diagram.&#x20;

Further details can be found in the [PortSIP PBX High Availability Architecture](../high-availability-v22.x/) documentation.

* **Media Servers**\
  Media Servers in the HA cluster are managed by the Call Manager, which dynamically reassigns calls from any failed media server to available ones. If a media server becomes unavailable, the call manager will automatically reassign the calls of this server to another available media server.
* **IVR Servers**\
  IVR Servers are deployed similarly to Media Servers. If an IVR server is down, the system administrator is notified via email and can disable that server through the PBX web portal. The load balancer will then reassign IVR tasks to other active servers.
* **Queue Servers**\
  Queue Servers follow the same behavior as IVR Servers. In case of a server is down, the system sends an email alert to the administrator. The PBX web portal allows administrators to disable the failed server, with the load balancer reassigning tasks to another available server.
* **Meeting Servers**\
  For Meeting Servers, it operates similarly. If a Meeting Server fails, the administrator receives an alert and can disable it via the PBX web portal. Meetings are automatically reassigned to other active servers in the cluster.
* **IM Servers**\
  The IM Server can handle up to 50,000 online users when deployed on high-performance hardware. In an HA configuration, IM servers ensure that messaging services remain uninterrupted, even in the event of server failure.
* **Data Flow Servers**\
  DataFlow Servers, while typically deployed on a single server, provide data analytics and real-time metrics with powerful server hardware.

