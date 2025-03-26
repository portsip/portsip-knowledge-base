# Scaling IM Server On-Premise for High Availability

With the PortSIP PBX HA deployment, we need to deploy the IM service on a separate server.

Before proceeding with this guide, ensure that you have already completed the [High Availability Installations on Ubuntu](high-availability-installations-on-ubuntu.md) deployment.

## Install IM Service on a Separate Server

For optimal performance, it’s recommended to install the IM service on a separate server, especially when handling a large number of users for chat and file-sharing activities (including files and pictures). The following hardware specifications are suitable for supporting up to 50,000 users online, with messaging and file sharing:

* **CPU**: 20 cores or higher
* **Memory**: 16 GB
* **Disk**: High I/O performance required (SSD recommended, at least 256 GB)
* **Network Bandwidth**: 1000 Mbps or higher, particularly if handling high volumes of messages and file sharing.
* **Static private IP**: You must configure a static private IP for this IM server. In this case, we assume it's **192.168.1.25.**
* **Static public IP:** If your PBX and IM server are located in the cloud for the internet users to access, you must have a static public IP for this IM service. In this case, we assume it's **104.18.36.110.**

## Supported Linux OS

PortSIP PBX High Availability (HA) and all associated servers require a consistent and compatible Linux environment.

### Operating System Requirements

* **Supported OS:** Ubuntu 24.04 (64-bit)
* All servers in the HA cluster **must run the exact same OS version** as the PBX server.

### User Account Requirements

To ensure consistency and seamless operation across the HA cluster, IM server must meet the following user account requirements:

* IM server ust use the **same username and password** as the PBX server.
* In this guide, the username **`pbx`** is used as an example. The user account **must have `sudo` privileges** to execute administrative commands.

### Disk Space Recommendations

* A minimum of **128 GB** of disk space is required. No additional data partition is necessary. If you have massive users to send & receive messages, and share the files in the chat, please allocate an appropriate disks size.

### Network Settings

For this setup, we assume the **PortSIP PBX High Availability** is installed on a server:

* PBX Server High Availability **Virtual IP** address: **192.168.1.130**
* PBX Server static public IP address: **104.18.36.119**.

## Step 1: **Preparing the Linux server for IM Installation**

Tasks that MUST be completed before installing cluster servers.

* **Ensure the server date-time is synced correctly**.
* If the Linux server is on a LAN, assign a **static private IP** address, in this case, **192.168.1.25**.
* Assign/route the static public IP address to this server, in this case, **104.18.36.110.**
* Install all available updates and service packs before installing the cluster server.
* Do not install PostgreSQL on the server.
* Ensure that all power-saving options for your system and network adapters are disabled (by setting the system to High-Performance mode).
* Do not install TeamViewer, VPN, or similar software on the host machine.
* The server must not be installed as a DNS or DHCP server.

## Step 2: Configuring the IP Address Whitelist <a href="#configuring-the-ip-address-whitelist" id="configuring-the-ip-address-whitelist"></a>

This step is mandatory; the service will not function without it.&#x20;

To prevent the PBX from restricting the request rate to the IM servers, you must add the IM servers' IP addresses to the PBX whitelist. Follow the steps below to complete this process:

1. Sign in to the PBX web portal as the System Administrator
2. Select the menu **IP Blacklist** > **Add**.
3. Enter the IM server IP as shown in the screenshot below, and choose a long **expiration date.**

<figure><img src="../../../.gitbook/assets/im_server_whitelist.png" alt="" width="563"><figcaption></figcaption></figure>

## Step 3: Generate Token for the IM Server

1. Log in as the **System Administrator** to the PortSIP PBX HA Web portal.
2. Navigate to **Servers > IM Servers**.
3. Select the default server and click the **Generate Token** button.

<figure><img src="../../../.gitbook/assets/im_server_update_address_new_token.png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
**Important:** All below commands for extended servers **must be executed on the `pbx01` node**, regardless of whether it is currently the active node.
{% endhint %}

## **Step 4: Set Password-Free Login** <a href="#set-password-free-login-for-all-these-servers" id="set-password-free-login-for-all-these-servers"></a>

If you are prompted to choose an option (**yes/no**), please enter **yes**.

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.25
```

## **Step 5: Deploy the IM Service**

Run the following command only on the **pbx01 node** of the HA PBX cluster.

The setup process may take several minutes—**do not interrupt, reboot, or close the terminal** until it completes.

Use the following command to deploy the IM server, specifying both the static private and static public IP addresses:

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash im.sh run \
-a 192.168.1.25 \
-A 104.18.36.110
```

If you want to define a custom path to store IM chat files, use the `-f` parameter. For example:

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash im.sh run \
-a 192.168.1.25 \
-A 104.18.36.110 \
-f /data/im
```

#### Parameter Reference

* `-a` – The **static private IP address** of the IM server
* `-A` – The **static public IP address** of the IM server
* `-f` – _(Optional)_ Custom file path to store chat messages

If everything is set up correctly, the PBX web portal will display the IM server's IP address, as shown in the screenshot below.

<figure><img src="../../../.gitbook/assets/im_server_update_address.png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
If you have extra firewall in your network infrastructure, you must ensure that TCP port 8887 is open in the firewall rules. The client application requires access to this port in order to send and receive messages.
{% endhint %}

## Managing the IM Server

### Available Operations

The following operations are supported for managing IM servers:

* `start` – Start the servers
* `stop` – Stop the servers
* `restart` – Restart the servers
* `rm` – Remove the installed servers

The following commands apply actions to **the IM** server:

**Start:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash im.sh start
```

**Stop All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash im.sh stop
```

**Restart All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash im.sh restart
```

**Remove All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash im.sh rm
```

## Upgrading the IM Server <a href="#upgrade-server" id="upgrade-server"></a>

Please follow the below steps to upgrade the IM server.

1. Please ensure you have upgraded the PBX HA as per this guide: [Upgrading High Availability Installation](upgrading-high-availability-installation.md).&#x20;
2. Log in as the **System Administrator** to the PortSIP PBX HA Web portal. Navigate to **Servers > IM Servers, sele**ct the default server, and click the **Generate Token** button to generate the new token.
3. Now upgrade the IM server and run the following command on the `pbx01` node. The process may take some time—**do not interrupt, reboot, or close the terminal** during execution.

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash sbc.sh upgrade
```

