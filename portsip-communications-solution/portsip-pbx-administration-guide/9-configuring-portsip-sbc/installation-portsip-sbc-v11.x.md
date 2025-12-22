# Installation PortSIP SBC v11.x

> ❗ **Important**\
> If your PortSIP PBX is running version 22.x, you must install PortSIP SBC version 11.x in order to use the latest WebRTC application.

### Supported Operating Systems

PortSIP SBC supports the following operating systems:

* **Debian:** 112.x
* **Ubuntu:** 22.04, 24.04

***

### Preparing the Server for Installation

The following tasks **must be completed** before installing PortSIP SBC.

#### System Preparation

* Ensure the system date and time are correctly synchronized.
* Linux: Perform installation using a user with `sudo` privileges.
* Windows: Installation must be performed using the Administrator account.
* Assign static IP addresses:
  * If the server is located on a LAN, assign a static private IP address.
  * If the server is on a public network, assign both a static public IP and a static private IP.
* Install all available OS updates and service packs before installation.
* Do not install PostgreSQL on the PortSIP SBC server.
* Disable all power-saving options for the system and network adapters (set the system to High Performance mode).
* Do not install TeamViewer, VPN software, or similar tools on the host machine.
* The PortSIP SBC must not be installed on a server acting as a DNS or DHCP server.

***

### Firewall and Network Requirements

The following ports **must be permitted by the firewall** and **must not be used by other applications**:

* **UDP:** 5066, 25000–34999
* **TCP:** 5065, 5067, 8883, 10443

Additional requirements:

*   All commands must be executed in the directory:

    ```shellscript
    /opt/portsip
    ```
* If deploying on a cloud platform (such as AWS or Azure), you must also open these ports in the cloud provider’s firewall / security group.

***

### Prerequisites

This guide assumes that you have already installed the PortSIP PBX by following the guide:

[Installation of the PortSIP PBX](../1-installation-of-the-portsip-pbx/)

***

### Installing PortSIP SBC and PBX on the Same Server

The PortSIP SBC can be deployed on the same server as the PortSIP PBX.

In this configuration:

* The PBX handles SIP calling directly
* The SBC provides WebRTC services and enables Microsoft Teams Direct Routing

#### Example Server Configuration

* **Private IP:** `192.168.1.72`
* **Public IP:** `66.175.221.120`
* **Domain:** `uc.portsip.cc`, Resolved to: `66.175.221.120`
* A **trusted SSL certificate** (not self-signed) is installed for `uc.portsip.cc`, refer to the guide: [Certificates for TLS / HTTPS / WebRTC](../certificates-for-tls-https-webrtc/)

***

Execute the commands below on the PBX server to install the SBC:

```bash
cd /opt/portsip
sudo /bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:11
```

***

#### Configure the SBC

After installation, follow the guide: [Configure PortSIP SBC on the Same Server as PortSIP PBX](configuring-sbc-for-webrtc.md#configure-portsip-sbc-on-the-same-server-as-portsip-pbx)

***

### Installing PortSIP SBC on a Separate Server

In most deployments, the **PortSIP SBC** is installed on a **separate server** from the PBX.

In this architecture:

* The SBC acts as the front-end edge component
* The PBX remains transparent to end users

***

### Example Server Configuration

* **PBX Server (Private IP):** `192.168.1.72`
* **SBC Server (Private IP):** `192.168.1.73`
* **SBC Server (Public IP):** `66.175.221.120`

#### Domain Configuration

* **Domain:** `sbc.portsip.cc`, resolved to: `66.175.221.120`
* A **trusted wildcard SSL certificate** (not self-signed) is installed for `portsip.cc`, refer to: [Certificates for TLS / HTTPS / WebRTC](../certificates-for-tls-https-webrtc/)

***

### Install PortSIP SBC on Linux (Separate Server)

#### Step 1: Initialize the Environment

On the dedicated server used to install the PortSIP SBC, execute the following commands:

```bash
sudo mkdir -p /opt/portsip
sudo curl \
https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.x/init.sh \
-o init.sh
sudo /bin/sh init.sh
```

***

#### Step 2: Install the Docker-Compose Environment

```bash
cd /opt/portsip
sudo /bin/sh install_docker.sh
```

If prompted with:

```
cloud.cfg (Y/I/N/O/D/Z) [default=N] ?
```

Enter **Y** and press **Enter**.

***

#### Step 3: Create and Run the SBC Docker Instance

```bash
sudo /bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:11
```

***

#### Configure the SBC

After installation, follow the guide: [Configure PortSIP SBC on a Separate Server](configuring-sbc-for-webrtc.md#configure-portsip-sbc-on-a-separate-server)

***

### Managing the PortSIP SBC Docker Instance

After successfully installing the SBC, you can manage the Docker instance using the following commands.

```bash
cd /opt/portsip
```

#### Show SBC Status

```bash
sudo /bin/sh sbc_ctl.sh status
```

#### Start the SBC

```bash
sudo /bin/sh sbc_ctl.sh start
```

#### Stop the SBC

```bash
sudo /bin/sh sbc_ctl.sh stop
```

#### Restart the SBC

```bash
sudo /bin/sh sbc_ctl.sh restart
```

#### Remove the SBC Container

> This command **does not delete SBC data**.

```bash
sudo /bin/sh sbc_ctl.sh rm
```



