# Preparing Cluster Servers

This section describes how to prepare Linux servers for deploying the following PortSIP PBX cluster application servers:

* Media Servers
* Queue Servers (ACD)
* Meeting Servers
* IVR Servers
* Instant Messaging (IM) Server
* DataFlow Server

Proper system preparation is critical to ensure system stability, performance, and predictable behavior in clustered PBX deployments.

***

### Supported Linux Operating Systems

The following **64-bit Linux distributions** are officially supported:

* **Ubuntu 22.04**&#x20;
* **Ubuntu 24.04**
* **Debian 12**

> ⚠️ Only 64-bit operating systems are supported.

***

### Network Requirements

When deploying a PBX cluster, ensure **sufficient network bandwidth and low latency** between all cluster nodes.

* High-quality, low-latency network connectivity between servers is mandatory
* Insufficient bandwidth or unstable network conditions may cause:
  * Call drops
  * Media quality issues
  * Queue and meeting instability
  * Unexpected PBX behavior

For production environments, PortSIP strongly recommends deploying all cluster servers within the same data center or high-speed private network.

***

### Preparing the Linux Host Machine

The following tasks must be completed before installing any PortSIP PBX cluster server.

#### System and Network Configuration

* Ensure the system date and time are correctly synchronized (NTP enabled)
* If the server is deployed on a LAN, configure a static private IP address
* For Media Servers handling Internet calls, configure a static public IP address for each media server
* Verify proper DNS resolution and hostname configuration

***

#### Operating System Preparation

* Install **all available OS updates and security patches** before proceeding
* Do **not** install PostgreSQL on the server\
  (PortSIP PBX includes and manages its own database services)
*   Disable all **power-saving features** for:

    * CPU
    * Disk
    * Network adapters

    \
    Set the system to **High-Performance** mode to avoid real-time media issues

***

#### Software and Role Restrictions

To prevent conflicts and performance issues:

* ❌ Do not install **TeamViewer**, VPN clients, or similar remote-access software
* ❌ Do not configure the server as a **DNS** or **DHCP** server
* ❌ Avoid installing unnecessary background services or monitoring agents

These restrictions ensure deterministic performance and reduce latency for real-time voice and media workloads.

***

### Setting Up the Docker Environment

PortSIP PBX cluster services are deployed using Docker. The Docker environment must be prepared on **each cluster server**.

Run the following steps on each server.

#### Step 1: Download the Initialization Script

Run the following commands on each server:

```bash
sudo curl \
https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.x/init.sh \
-o init.sh
sudo /bin/sh init.sh
```

This script prepares the system for Docker installation and performs prerequisite checks.

***

#### Step 2: Install Docker and Docker Compose

Run the following commands on each server:

```bash
cd /opt/portsip
sudo /bin/sh install_docker.sh
```

If you are prompted with a message similar to:

```
*** cloud.cfg (Y/I/N/O/D/Z) [default=N] ?
```

* Enter **Y**
* Press **Enter** to continue

This installs Docker and Docker Compose using PortSIP-validated settings.



