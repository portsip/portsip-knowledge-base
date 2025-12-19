# Installation of PortSIP PBX v22.x

> ⚠️ **Warning**\
> **PortSIP PBX v22.3 has not yet been released.**\
> Please **do not follow this guide** or begin any upgrade or deployment processes based on this documentation at this time.

***

### Supported Operating Systems

PortSIP PBX can be installed on **Ubuntu** and **Debian** Linux operating systems.

* Debian: 12(recommend)
* Ubuntu: 22.04, 24.04(recommended)

***

### Instant Messaging (IM) Service

Starting with **PortSIP PBX v22.0**, an integrated **Instant Messaging (IM) service** is available, providing modern collaboration features such as one-to-one messaging and group chat.

On Linux deployments, the IM service requires a **separate installation step**. In some scenarios—particularly in large-scale or high-concurrency environments—you may choose to deploy the IM service on a **dedicated server** to achieve optimal performance and scalability.

***

### Data Flow Service (Call Analytics and Real-Time Metrics)

Starting with **PortSIP PBX v22.3**, PortSIP introduces the **Data Flow service**, which delivers advanced call analytics and real-time metrics.

The Data Flow service is built on **ClickHouse**, a high-performance, column-oriented database designed for analytics workloads. Due to its performance characteristics and resource requirements, the Data Flow service **must be installed on a dedicated, high-performance server** and is deployed as a **separate service** from the core PBX.

This architecture ensures:

* High-throughput analytics processing
* Accurate real-time and historical reporting
* Minimal impact on core call processing performance

***

### Upgrading PortSIP PBX

If you are currently running **PortSIP PBX v16.x** and plan to upgrade to the latest **v22.x**, or if you are upgrading from an earlier **v22.x** release to the latest version, follow the upgrade procedures outlined below.

### Upgrade Path

* Deployments currently running **PortSIP PBX v16.x** must first be upgraded to the **latest v16.x release** before proceeding with an upgrade to the latest **v22.x**.
* Deployments currently running earlier **PortSIP PBX v22.x** can upgrade **directly** to the **latest v22.x** release.

Ensure all required services (PBX core, IM service, and Data Flow service, if deployed) are upgraded in the correct order.

> **Recommendation**\
> Always back up configuration files, call data, and databases before performing any upgrade. For production environments, perform upgrades during a maintenance window and validate services after completion

