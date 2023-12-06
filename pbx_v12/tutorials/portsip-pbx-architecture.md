# PortSIP UC Architecture

## Overview

People work together in different ways. And they use a lot of collaboration tools: IP telephony for voice calling, web, and video conferencing, voice mail, mobility, desktop sharing, instant messaging and presence, and more.

PortSIP UC is an IP-based Unified Communications system offers multi-tenant feature, integrating voice, video, data, collaboration, and mobility products and applications. It enables more effective, secure communications and can transform the way in which we communicate. PortSIP UC removes the geographic barriers of effective communications through the use of voice, video, collaboration, and data integration.

PortSIP UC solutions deliver seamless user experiences that help people work together more effectively. Anywhere, on any device. They bring real-time communication from your PBX and conferencing solutions together with messaging and chat, and integrate with everyday business applications using APIs.

PortSIP UC solutions include two types: [**PortSIP PBX**](https://www.portsip.com/portsip-pbx/) and [**PortSIP UCaaS**](https://www.portsip.com/portsip-ucaas/), all are available as on-premises software, partner-hosted solutions, or as a service (UCaaS) from cloud providers.

* **PortSIP PBX**: Designed for the SMB, the deployed size support maximum of 50K users.
* **PortSIP UCaaS**: Designed for the larger service provider, support 1M+ users.



## PortSIP UC Solution Components

The PortSIP UC solutions encompass voice, video, and data traffic within a single network infrastructure. PortSIP UC is capable of managing all three traffic types and interfacing with all standards-based network protocols. It runs virtualized in standard on-premise, private cloud, public cloud environments. It scales both UP and DOWN; it deploys automatically by docker/Kubernetes fits into a minimal resource footprint, and accommodates a wide range of different deployment scenarios and topologies.

**Figure 1-1**   PortSIP Unified Communications Solution Components

![](../../.gitbook/assets/arch\_diagram\_pbx.png)

The components of the standard layers are as follows:

* **Network Infrastructure layer**: The infrastructure consists of routers, switches, virtual IP, public IP, and E1/T1gateways or SIP Trunks. The infrastructure layer carries data, voice, and video between all network devices and applications. This layer also provides high availability, management, quality of service (QoS), and network security.
* **Call control layer**: The call control layer provides for call processing, device control, and administration of the call routes and features. Call control can be provided by PortSIP Call Manager. Call processing is physically independent from the infrastructure layer.
* **Applications layer**: Applications are independent from call-control functions and the physical voice-processing infrastructure. Applications, including those listed here, are integrated through IP, which allows the applications to reside anywhere within the network:
  * Voice Mail, provides the voice mail service that answers calls and saves voice messages in users' Inboxes. It provides greater flexibility than other phone answering systems and increased productivity by allowing users to access their voicemail messages through the interface of their choice: telephone, e-mail client, or Web browser. voicemail messages can be accessed through e-mail clients as a WAVE file attachment.
  * Call Queues/Ring Groups of various sizes can be built with PortSIP UC to provide the Contact Center feature.
  * Conference Server, a medium to the large-scale conferencing server that supports video integration with scalable collaboration and control tools.
  * Presence server, collects information about the availability and communications capabilities of a user and provides this information to watchers of the user as a status indication. The status information includes the user’s communications device availability. For example, the user might be available via phone, video, web collaboration, or videoconferencing.
  * IVR,  manage inbound phone calls by collecting information about the customer inquiry before automatically transferring the call to the right department. It can tailor the conversation even based on the virtual phone number the person dialed.
  * MOH, use of a recording containing music or a spoken message that is played to callers when they are placed on hold (instead of hearing silence).
  * Call Park, lets you place an active call on hold so it can be later picked up by another phone extension.
  * File Server, the scalable file server used to store the recording files, log files and offers the file downloading service.
  * Rest API, provides the APIs that **allow anyone to interact with the PortSIP API in real-time using their own account data**. This is an excellent way to take the API out for a test drive, and a useful tool for developers troubleshooting a particular API issue they are having.
  * WSI Publisher, provides the Pub/Sub mechanism which is based on the WebSocket. The user is able to create the WebSocket in any programming language to subscribe to the PBX events, once the subscribed events occur, PortSIP PBX will push the event message to the subscriber automatically, the message is in the JSON format.
  * Media Server, a powerful media server that relays the media for services such as unified messaging (voicemail), conferencing, auto-attendants, IVR, and call queuing, and also recording audio, video data as a file and stored into the file server.
* **Endpoints layer**: The endpoints layer brings applications to the user, whether the end device is a IP Phone, a PC using a software-based phone,  a SIP-based video conference terminal, Mobile App, WebRTC Client, or a communications client or video terminal.&#x20;

###

## PortSIP PBX Solution Architecture

The PortSIP PBX system delivers fully integrated communications, converging voice, video, and data over a single network infrastructure using standards-based protocols. The PortSIP PBX system delivers unparalleled performance and capabilities to address current and emerging communications needs in the enterprise environment, as illustrated by the network topology in Figure 1-2.

**Figure 1-2**   PortSIP PBX Solution Network

![](../../.gitbook/assets/portsip\_uc\_arch2.png)



The PortSIP UC Solution is designed to optimize functionality, reduce configuration and maintenance requirements, and provide interoperability with a variety of other applications. It provides this capability while maintaining high availability, QoS, and security.

The PortSIP UC solution integrates the following major communications technologies:

* **IP Telephony**: IP telephony refers to technology that transmits voice, video communications over a network using IP standards. PortSIP UC includes a wide array of software products such as call-processing agents, Softphone, Mobile App, WebRTC Client, voice-messaging systems, video conferencing, and many other applications.&#x20;
* **Contact Center**: PortSIP UC solution provides a combination of strategy and architecture to revolutionize call center environments, promotes efficient and effective customer communications across large networks by enabling organizations to draw from a broader range of resources to service customers.&#x20;
* **Video Telephony**: PortSIP UC solution enables real-time video communications and collaboration using the same IP network
* **Rich-media Conferencing**: PortSIP UC solution creates a virtual meeting room with an integrated set of IP-based tools for voice, video, collaboration, and web conferencing.&#x20;
* **Third-party Applications**: PortSIP UC Solution works with leading-edge companies to provide the broadest selection of innovative third-party IP communications applications and products focused on critical business needs such as messaging, customer care, and workforce optimization.
* **WebRTC**, PortSIP has been following the WebRTC technology right from the start and has been investing heavily in WebRTC development, it allows to deliver and accept calls to anyone connected to the internet, without requiring any software to be installed and even anonymously. Customers could call you free of charge at a click of a button, just working likes the normal IP Phone, Softphone, Mobile App.



## PortSIP UC Solution Functions

PortSIP UC extends enterprise telephony features and functions to packet telephony network devices. These packet telephony network devices include IP Phones, media processing devices, VoIP E1/T1 gateways, and multimedia applications. Additional data, voice, and video services, such as converged messaging, multimedia conferencing, collaborative contact centers, and interactive multimedia response systems, interact with the IP telephony solution through the PortSIP VoIP SDK, Rest API, WSI(WebSocket Interface).

PortSIP UC provides these functions:&#x20;

* **Call Processing**: Call processing refers to the complete process of originating, routing, and terminating calls, including any billing and statistical collection processes.
* **Signaling and Device Control**: PortSIP UC sets up all the signaling connections between call endpoints and directs devices such as phones, apps, gateways, and conference terminals to establish and tear down streaming connections. Signaling is also referred to as call control and call setup/call teardown.
* **Call Routing Administration**: The call routing is a set of configurable lists that PortSIP UC uses to perform call routing. PortSIP UC is responsible for digit analysis of all calls.&#x20;
* **Phone Feature Administration**: PortSIP UC extends services such as hold, transfer, forward, conference, speed dial, redial, call park, and many other features to IP phones, Softphones, Apps, and gateways.
* **User Management**: PortSIP UC uses its own database to store user information. User authentication is performed locally, allows for centralized user management.&#x20;
* **Programming Interface to External Applications**: PortSIP provides a programming interface to external applications such as PortSIP Softphone, App, VoIP SDK, WebHook, REST API, and WebScoekt subscribers.



## PortSIP PBX Signaling and Media Paths&#x20;

PortSIP UC uses SIP to communicate with IP Phones/Softphones/Mobile Apps for call setup and teardown and for supplementary service tasks.

&#x20;After a call has been set up, media exchange is relaying by the media server between the IP Phones/Softphones/Mobile Apps across the IP network, using the Real-Time Transport Protocol (RTP) to carry the audio, video.&#x20;

### Example: Basic Call

At the beginning of a call, a user at IP phone A picks up the handset and begins dialing the phone number of phone B. The INVITE message is sent to the PortSIP PBX, where the call manager matches the routes and rewrites the SDP using media server ports before sending the INVITE message to Phone B.

Phone B begins ringing after receiving the INVITE message. When the user on phone B accepts the call, a 200 OK message is sent to the PortSIP PBX, where the call manager rewrites the SDP using the media server ports and sends the 200 OK to Phone A. The call is now established, and the RTP media path between Phone A, media server, and Phone B is established.

**Figure 1-3**   PortSIP PBX Signaling and Media Paths

![](../../.gitbook/assets/basic\_call\_flow.png)



## PortSIP UCaaS

Clustering allows the PortSIP PBX to scale to several thousands of servers, provides redundancy in case of network or server failure, and provides a central point of administration with carrier-grade reliability and security, which is called [PortSIP UCaaS](https://www.portsip.com/portsip-ucaas/).

PortSIP UCaaS is a modern cloud solution. It represents a complete communication and collaboration system, a production environment for real-time services, with all the required applications for voice, video, messaging, presence, conferencing, activity streams, file sharing, and mobility.&#x20;

PortSIP UCaaS is designed for large service providers, which supports 1M+ users by deployed Kubernetes and is easy to scale to support more users by simply adding more servers, to satisfy all the Unified Communications-critical features. PortSIP UCaaS provides our enterprise users and service provider customers with a powerful way to grow revenue and tap new markets.

Benefit from Kubernetes, the PortSIP UCaaS provides you with:

* **Service discovery and load balancing** Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
* **Automated rollouts and rollbacks** You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
* **Automatic bin packing** You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
* **Self-healing** Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
* **Secret and configuration management** Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.

**Figure 1-4**   PortSIP UCaaS Network

![](../../.gitbook/assets/portsip\_ucaas.png)



PortSIP UCaaS contains the following types of servers

* **Edge Proxy Servers**: These are the SIP proxy servers deployed in your perimeter environment. Their role is to send and receive network traffic to the users for the services offered by your cluster servers deployment. To do this successfully, each Edge Server runs:&#x20;
  * Access Edge service: Provides a single, trusted connection point for both outbound and inbound Session Initiation Protocol (SIP) traffic.
  * It is deployed at the edge of the public network and private trusted network (the PortSIP UC Cluster deployment network) to isolate the two networks, prevent unwanted or hostile attacks from entering the trusted networks.
  * Handle SIP Registration: Edge Proxy Server acts as a SIP registrar. It has a shared registration-info storage (Redis). Any endpoint can be reached by any Edge Proxy Server, it also as an authentication server. It processes REGISTER requests and responds 200 OK to the registration initiator.
  * Load balance: Edge Proxy Server also handles load balances inbound traffic by distributing calls to available Call Manager nodes in a data center based on Call Manager loads, the Call Manager load metrics stored in the shared storage (Redis).&#x20;
* **Database Servers**: Given the type of servers stores:
  * Stores the media files (announcements, music on hold, voice mail and etc).
  * Configurations.
  * Call detail records (CDR).
  * User information.
  * Tenant information.
* **Application Servers:** The Applications are described in section  "[PortSIP UC Solutions Components](portsip-pbx-architecture.md#portsip-uc-solution-components)".
* **Message Broker:** The Apache Kafka distributed event streaming platform, all exchange between application servers, call managers, and edge proxy servers is executed via message broker. The message broker is delivering requests and responses between PortSIP UC Cluster components.
* **Redis**: The Redis database is used as a high-performance key/value storage for global system data shared across proxies, call managers, applications servers. This includes registrations, call information, and concurrent calls counters for customers and subscribers, etc...
* **Zookeeper:** For large server deployments that require scalability and linear performance, PortSIP UC Cluster utilizes ZooKeeper Discovery.
* **Prometheus, Loki, and Grafana**: The Prometheus, Loki, and Grafana components are deployed with PortSIP UC Cluster provide WebUI-based monitoring, log collection, and analysis system that makes the cluster easier to manage and maintain.



## PortSIP UCaaS Signaling and Media Paths&#x20;

PortSIP UCaaS Signaling and Media Paths is almost similar to the PortSIP PBX that is described in the section "[PortSIP PBX Signaling and Media Paths](portsip-pbx-architecture.md#portsip-uc-signaling-and-media-paths)".

### Example: Basic Call

Assume Phone A is registered to edge proxy Server 1 and Phone B is registered to edge proxy server 2.

A user on IP phone A picks up the IP Phone and dials the phone number of phone B, the INVITE message is sent to the edge proxy server 1, which receives the call and selects an appropriate call manager based on the call manager server loads, the call manager server process to match the routes, and rewrites the SDP using media server ports, the call manager sends the INVITE to the edge proxy server 2, which Phone B registered, and the edge proxy server 2 sends the INVITE to Phone B.

After receiving the INVITE message, Phone B is ringing. When the user on phone B accepts the call, a 200 OK message is sent to the edge proxy server 2, the server forwards this 200 OK to the call manager server by record-route header, and the call manager server rewrites the SDP using the media server ports, sends the 200 OK to the edge proxy server 1 that Phone A registered, and the call is now established, and the RTP media path is established.

**Figure 1-5**   PortSIP UCaaS Signaling and Media Paths

![](../../.gitbook/assets/basic\_call\_flow\_ucaas.png)



## UCaaS Service: Cloud-Based

Unlike a traditional PBX, the service provider is easy to provide the hosted UCaaS service by deploying PortSIP UCaaS solution in the cloud - AWS, Azure, GCE, or private cloud.



![](../../.gitbook/assets/ucaas\_diagram.png)

Users get all the same features as traditional PBX, phones do not connect directly to an internal server. They connect through VoIP servers securely hosted in the cloud. So on top of typical PBX functionality, you can also integrate top-notch phone service between offices.

A virtual PBX service gives you all the benefits of a PBX without any of the hassles. Furthermore, with the PortSIP UCaaS solution, users also can enjoy the modern unified communications features likes video, video conference, sharing, collaboration, IM, and presence.  It enables more effective, secure communications and can transform the way in which we communicate

