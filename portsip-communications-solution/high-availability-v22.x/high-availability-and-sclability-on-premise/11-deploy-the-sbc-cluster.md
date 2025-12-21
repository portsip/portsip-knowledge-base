# Scaling SBC On-Premise for High Availability

If your deployment handles a high volume of **WebRTC traffic**, **Microsoft Teams Direct Routing calls**, or if you use a **Session Border Controller (SBC)** to isolate and protect your PBX, all signaling and media traffic will be routed through the SBC layer.

In these scenarios, deploying the SBC in a clustered configuration is strongly recommended. An SBC cluster provides:

* Higher throughput for large call volumes
* Improved scalability as traffic grows
* Enhanced resiliency and fault tolerance
* Better isolation and protection of the PBX core

This architecture is considered a best practice for service providers and enterprises operating at scale.

<figure><img src="../../../.gitbook/assets/sbc_cluster.png" alt=""><figcaption></figcaption></figure>

As illustrated in the architecture diagram, the **PortSIP PBX** is deployed within a secure **VLAN** or **cloud environment**. One or more SBC servers are positioned in front of the PBX, acting as the single ingress and egress point for all external traffic.

Key architectural characteristics:

* End users and external systems **never access the PBX directly**
* All SIP, WebRTC, and media traffic is terminated and secured by the SBC
* The SBC cluster distributes traffic across multiple nodes, ensuring availability and performance
* The PBX remains isolated, protected, and optimized for call control and service logic

This design significantly improves both **security posture** and **operational stability**.

***

### Server Configuration Example

In this deployment example, three SBC servers are configured as a cluster:

* **SBC 1**
  * Private IP: `192.168.1.11`
  * Public IP: `72.247.113.11`
* **SBC 2**
  * Private IP: `192.168.1.12`
  * Public IP: `72.247.113.12`
* **SBC 3**
  * Private IP: `192.168.1.13`
  * Public IP: `72.247.113.13`

Each SBC node has both a **private IP** for internal communication with the PBX and a **public IP** for handling external SIP, WebRTC, and Microsoft Teams traffic. Together, these SBCs operate as a cluster to provide load balancing, redundancy, and seamless failover.

### DNS Configuration

You must create DNS records for each SBC server to ensure proper call routing, redundancy, and failover. The following DNS record types are supported:

* **A records**
* **DNS SRV records**

#### A Records for Individual SBCs

Create the following A records to map each SBC hostname to its corresponding public IP address:

* `sbc1.sbc.com` → `72.247.113.11`
* `sbc2.sbc.com` → `72.247.113.12`
* `sbc3.sbc.com` → `72.247.113.13`

#### A Records for SBC Cluster Access

To enable load distribution and simplify client configuration, you may also create multiple A records for the shared SBC domain:

* `sbc.com` → `72.247.113.11`
* `sbc.com` → `72.247.113.12`
* `sbc.com` → `72.247.113.13`

With this configuration, DNS resolution for `sbc.com` will return all SBC public IP addresses, allowing clients to distribute traffic across the SBC cluster.

***

### TLS Certificates

Purchase and install a **wildcard TLS certificate** for the domain:

```
*.sbc.com
```

This wildcard certificate will secure SIP over TLS, HTTPS, and WebRTC connections across all SBC nodes in the cluster.

For detailed guidance, refer to the document [Certificates for TLS/HTTPS/WebRTC](../../portsip-pbx-administration-guide/certificates-for-tls-https-webrtc/).

***

### Prerequisites

Before configuring the SBC cluster servers, ensure that the following prerequisites are met.

#### PBX High Availability Setup

The PortSIP PBX High Availability (HA) installation and configuration must be completed on the Main Server first by following the guide:  [High Availability Installations on Ubuntu](high-availability-installations-on-ubuntu.md).

***

### Supported Linux Operating System

PortSIP PBX High Availability (HA) and all associated servers require a consistent and compatible Linux environment.

* **Supported OS**: Ubuntu 24.04 (64-bit)
* All servers in the HA cluster, including SBC servers, **must run the exact same OS version** as the PBX server.

***

### User Account Requirements

To ensure consistency and reliable automation across the HA cluster, all SBC servers must meet the following user account requirements:

* All SBC servers must use the **same username and password** as the PBX server
* In this guide, the username `pbx` is used as an example
* The user account **must have sudo privileges** to execute administrative and management commands

***

### Disk Space Recommendations

* **Minimum required disk space**: 128 GB
* No separate data partition is required for SBC servers

This disk size is sufficient for SBC binaries, logs, and operational overhead.

***

### Important Notice

All management and operational commands for **extended servers**, including SBC servers, **must be executed on the `pbx01` node**, regardless of whether it is currently the active or standby node.

This ensures configuration consistency and prevents cluster state conflicts.

***

### Enable Password-Free SSH Login

To allow automated management and deployment, configure **password-free SSH access** from the **`pbx01` node only** to all SBC servers.

If prompted to confirm the connection (yes/no), type **yes**.

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.11
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.12
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.13
```

This step is required before deploying or managing SBC servers from the HA controller.

***

### Deploying the SBC Servers

Run the following command **only on the `pbx01` node** of your PBX HA cluster.

> **Important**\
> The deployment process may take several minutes.\
> **Do not interrupt, reboot, or close the terminal** while the command is running.

```bash
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh run \
-a 192.168.1.11,192.168.1.12,192.168.1.13 \
-i portsip/sbc:11
```

#### Parameters

*   **`-a`**: Specifies the private IP addresses of the SBC servers.\
    Multiple IPs must be separated by commas.

    **Example:**

    ```
    -a 192.168.1.11,192.168.1.12,192.168.1.13
    ```
*   **`-i`**: Specifies the SBC image version to deploy.

    **Example:**

    ```
    -i portsip/sbc:11
    ```

***

### Accessing the SBC Web Portals

After the SBC servers are successfully deployed, you can access their web management portals using the following URLs:

* **SBC 1**: [https://sbc1.sbc.com:8883](https://sbc1.sbc.com:8883)
* **SBC 2**: [https://sbc2.sbc.com:8883](https://sbc2.sbc.com:8883)
* **SBC 3**: [https://sbc3.sbc.com:8883](https://sbc3.sbc.com:8883)

***

### Configuring the SBC Servers

Once deployment is complete, follow the guide [Configuring PortSIP SBC for WebRTC](../../portsip-pbx-administration-guide/9-configuring-portsip-sbc/configuring-sbc-for-webrtc.md) to finalize the SBC configuration.

#### Configuration Notes

Please pay close attention to the following settings during configuration:

* **PBX Address**\
  When configuring PBX connectivity on the SBC, **use the PBX HA Virtual IP address**, not the physical IP of any individual PBX node.
* **TLS Certificate Settings**\
  When uploading TLS certificates:
  * Set **TLS Domain** to `sbc.com`
  * Enable **This is SBC Web Domain Certificate**
*   **Web Domain Configuration**\
    In the **Web Domain** field, enter:

    ```
    sbc.com
    ```

***

### WebRTC Client Access

After configuration is complete, users can access the WebRTC client via the following URL:

```
https://sbc.com:10443/webrtc
```

This URL automatically benefits from SBC clustering and DNS-based load distribution.

***

### Managing SBC Servers

#### Important Notice

All management commands for **extended servers**, including SBC servers, **must be executed on the `pbx01` node**, regardless of whether it is currently the active or standby node.

***

#### Supported Management Operations

The following operations are supported:

* **start** – Start the SBC servers
* **stop** – Stop the SBC servers
* **restart** – Restart the SBC servers
* **upgrade** – Restart the SBC servers
* **rm** – Remove the SBC servers

You may manage all SBC servers at once or target specific servers using the `-a` parameter.

**Start All SBC Servers**

```bash
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh start \
-a 192.168.1.11,192.168.1.12,192.168.1.13
```

**Stop All SBC Servers**

```bash
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh stop \
-a 192.168.1.11,192.168.1.12,192.168.1.13
```

**Restart All SBC Servers**

```bash
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh restart \
-a 192.168.1.11,192.168.1.12,192.168.1.13
```

**Remove All SBC Servers**

```bash
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh rm \
-a 192.168.1.11,192.168.1.12,192.168.1.13
```

***

### Upgrading SBC Servers

All upgrade operations **must be performed on the `pbx01` node**, even if it is not currently the active node.

#### Prerequisite

Before upgrading SBC servers, ensure that the **PBX HA cluster has already been upgraded** by following the guide:  [Upgrading High Availability Installation](upgrading-high-availability-installation.md).&#x20;

***

#### Upgrade All SBC Servers

Run the following command to upgrade all SBC servers:

> **Important**\
> The upgrade process may take some time.\
> **Do not interrupt, reboot, or close the terminal** during execution.

```bash
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh upgrade \
-a 192.168.1.11,192.168.1.12,192.168.1.13 \
-i portsip/sbc:11
```

This command automatically applies the latest updates to all configured SBC server instances in the cluster.

