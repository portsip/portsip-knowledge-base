# Scaling SBC On-Premise for High Availability

If your business handles a high volume of WebRTC and MS Teams calls, or if you use an SBC to isolate your PBX, all calls will be routed through the SBC. In such cases, it is recommended to deploy the SBC in a cluster configuration to efficiently manage the call traffic.

## Deployment Architecture

<figure><img src="../../../.gitbook/assets/sbc_cluster.png" alt=""><figcaption></figcaption></figure>

As illustrated in the diagram, the PBX is deployed on either a VLAN or a cloud platform. Multiple SBC servers are positioned in front of the PBX to prevent direct access by users.

Servers

Assume we have the three servers with the below IP addresses:

* **SBC 1**: Private IP **192.168.1.11**, public IP is: **72.247.113.11**
* **SBC 2**: Private IP **192.168.1.12**, public IP is: **72.247.113.12**
* **SBC 3**: Private IP **192.168.13**, public IP is: **72.247.113.13**

## DNS Configuration

You will need to create DNS records for each SBC server. The DNS records can be of the following types:

* **A Record**
* **DNS SRV Record**

You can create the following **A Records** for the SBC servers:

* Resolve **sbc1.sbc.com** to **72.247.113.11**
* Resolve **sbc2.sbc.com** to **72.247.113.12**
* Resolve **sbc3.sbc.com** to **72.247.113.13**

Additionally, you may create records to resolve **sbc.com** to all three SBC IPs:

* Resolve **sbc.com** to **72.247.113.11**
* Resolve **sbc.com** to **72.247.113.12**
* Resolve **sbc.com** to **72.247.113.13**

## Certificates

Please purchase a wildcard TLS certificate for the domain **sbc.com** as per the article[ Certificates for TLS/HTTPS/WebRTC](../../portsip-pbx-administration-guide/certificates-for-tls-https-webrtc/).

## Prerequisites

Before configuring the cluster servers, please ensure that you have completed the PBX HA installation and configuration on the **Main Server** by following the guide [High Availability Installations on Ubuntu](high-availability-installations-on-ubuntu.md).

## **Supported Linux OS** <a href="#supported-linux-os" id="supported-linux-os"></a>

* Ubuntu 24.04, 64-bit.
* We recommend allocating at least 128 GB of disk space, with no need for an additional data partition.
* All cluster servers must run the same version of the Linux OS as the PBX in High Availability (HA).
* When setting up the PBX cluster, please ensure there is sufficient network speed and bandwidth between the servers. Insufficient network resources may cause the PBX to not work as expected.

{% hint style="danger" %}
**Important:** All management commands for extended servers **must be executed on the `pbx01` node**, regardless of whether it is currently the active node.
{% endhint %}

## **Set Password-Free Login for All These Servers** <a href="#set-password-free-login-for-all-these-servers" id="set-password-free-login-for-all-these-servers"></a>

If you are prompted to choose an option (**yes/no**), please enter **yes**.

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.11
```

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.12
```

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.13
```

## Deploying the SBC Servers

Run the following command only on the **pbx01** node of your HA PBX cluster. The process may take some time—**do not interrupt, reboot, or close the terminal** during execution.

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh run \
-a 192.168.1.11,192.168.1.12,192.168.1.13 \
-i portsip/sbc:11
```

#### Parameters

* `-a` – Specifies the IP addresses of the SBC servers. Use commas to separate multiple IPs.\
  **Example:** `-a 192.168.1.11,192.168.1.12,192.168.1.13`
* `-i` – Specifies the SBC image version to deploy.\
  **Example:** `-i portsip/sbc:11`

## Accessing the SBC Servers

After successfully deploying the SBC servers, you can access their web portals using the URLs below:

* **SBC 1:** [https://sbc1.sbc.com:8883](https://sbc1.sbc.com:8883)
* **SBC 2:** [https://sbc2.sbc.com:8883](https://sbc2.sbc.com:8883)
* **SBC 3:** [https://sbc3.sbc.com:8883](https://sbc3.sbc.com:8883)

## Configuring SBC Servers

Once the SBC servers have been successfully deployed, follow the [Configuring PortSIP SBC for WebRTC ](../../portsip-pbx-administration-guide/9-configuring-portsip-sbc/configuring-sbc-for-webrtc.md)guide to complete the setup.

Please keep the following points in mind during configuration:

* **PBX Address:** When specifying the PBX information in the SBC configuration, use the **PBX HA Virtual IP address** as the PBX IP.
* **TLS Certificate Settings:** When uploading TLS certificates:
  * Set the **TLS Domain** to `sbc.com`.
  * Enable the **This is SBC Web Domain Certificate** option.
* **Web Domain Configuration:** In the **Web Domain** field, enter: `sbc.com`.

### WebRTC Client App

You can access the WebRTC client using this URL: [https://sbc.com:10443/webrtc](https://sbc.com:10443/webrtc).

## Managing SBC Servers

{% hint style="danger" %}
**Important:**\
All management commands for extended servers **must be executed on the `pbx01` node**, regardless of whether it is currently the active node.
{% endhint %}

### Available Operations

The following operations are supported for managing SBC servers:

* `start` – Start the servers
* `stop` – Stop the servers
* `restart` – Restart the servers
* `rm` – Remove the installed servers

To manage a **specific SBC server instance** by its IP address, use the `-a` parameter.

The following commands apply actions to **all** SBC servers:

**Start All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh start \
-a 192.168.1.11,192.168.1.12,192.168.1.13
```

**Stop All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh stop \
-a 192.168.1.11,192.168.1.12,192.168.1.13
```

**Restart All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh restart \
-a 192.168.1.11,192.168.1.12,192.168.1.13
```

**Remove All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh rm \
-a 192.168.1.11,192.168.1.12,192.168.1.13
```

## Upgrading SBC Servers <a href="#upgrade-server" id="upgrade-server"></a>

{% hint style="danger" %}
All the below commands must be performed on the pbx01 node, even if it is not the current active node.
{% endhint %}

First, please ensure you have upgraded the PBX HA as per this guide: [Upgrading High Availability Installation](upgrading-high-availability-installation.md).&#x20;

To upgrade all SBC servers, run the following command on the `pbx01` node:

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh upgrade \
-a 192.168.1.11,192.168.1.12,192.168.1.13 \
-i portsip/sbc:11
```

This command will automatically apply the latest updates to all configured SBC server instances.

