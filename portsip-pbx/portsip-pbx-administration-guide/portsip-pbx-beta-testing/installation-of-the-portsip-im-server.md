# Installation of the PortSIP IM Server

The installation of the IM Server is a separate step, required only when PortSIP PBX is installed on a Linux server.

Before proceeding with this guide, ensure that you have already completed **steps 1–4** of the [Installation of the PortSIP PBX v22.x](../1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22.md).

You have two options for deploying the PortSIP IM Server:

1. **Same server as PortSIP PBX**: If you have a smaller number of users, you can install the IM Service on the same server as your PortSIP PBX. This setup is simpler but may not provide optimal performance for larger user bases.
2. **Separate server**: For better performance, especially when dealing with a large number of users who require chat and file-sharing capabilities, it is recommended to install the IM Server on a separate, more powerful server.

## Install IM Service on the Same Server as PortSIP PBX

Follow these steps to install the IM server on the same server as PortSIP PBX.

### Step 1: Generate Token for the IM Server

1. Log in as the **System Administrator** to the PortSIP PBX Web portal.
2. Navigate to **Servers > IM Servers**.
3. Select the default server and click the **Generate Token** button.
4. Copy the generated token.

<figure><img src="../../../.gitbook/assets/im_server_update_address_new_token.png" alt=""><figcaption></figcaption></figure>

### Step 2: Create and Run Instant Messaging Docker Instance

Follow these steps to create the IM service Docker instance:

1. Navigate to the **/opt/portsip** directory by running the following command:

```sh
cd /opt/portsip
```

2. Use the command below to create the Instant Messaging service Docker instance. Replace the placeholders with your actual values:

* **-p**: Specifies the path for storing the IM service data.
* **-a**: Specifies the IP address of the server.
* **-i**: Specifies the PBX Docker image version.
* **-t**: Specifies the token generated in the previous step.

{% code overflow="wrap" %}
```sh
sudo /bin/sh im_ctl.sh run -p /var/lib/portsip/ -i portsip/pbx:22 \
-t OWMWYWJKZJYTMWM2NI0ZNZJMLWJJZDKTMGVMZDYXNZU1NWI1
```
{% endcode %}

If everything is set up correctly, the PBX web portal will display the IM server's IP address, as shown in the screenshot below.

<figure><img src="../../../.gitbook/assets/im_server_update_address.png" alt=""><figcaption></figcaption></figure>

The Instant Messaging (IM) server has been successfully installed. We can now proceed with the next steps in the [PortSIP PBX installation step 6](../1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22.md#step-6-reboot-to-apply-the-certificate).

## Install IM Service on a Separate Server

For optimal performance, it’s recommended to install the IM service on a separate server, especially when handling a large number of users for chat and file-sharing activities (including files and pictures). The following hardware specifications are suitable for supporting up to 50,000 users online, with messaging and file sharing:

* **CPU**: 20 cores or higher
* **Memory**: 16 GB
* **Disk**: High I/O performance required (SSD recommended, at least 256 GB)
* **Network Bandwidth**: 1000 Mbps or higher, particularly if handling high volumes of messages and file sharing.

### **Supported Linux OS** <a href="#supported-linux-os" id="supported-linux-os"></a>

It only supports 64-bit OS.

* Ubuntu 22.04, 24.04
* Debian 11.x, 12.x

For this setup, we assume:

* The PortSIP PBX is installed on a server, which has the static private IP address 192.168.1.20
* The Instant Messaging (IM) service on a server with a static IP address of **192.168.1.25**.

### Step 1: **Preparing the Linux server for Installation**

Tasks that MUST be completed before installing cluster servers.

* **Ensure the server date-time is synced correctly**.
* If the Linux server is on a LAN, assign a **Static Private IP** address, in this case **192.168.1.25**.
* Install all available updates and service packs before installing the cluster server.
* Do not install PostgreSQL on the Server.
* Ensure that all power-saving options for your system and network adapters are disabled (by setting the system to High-Performance mode).
* Do not install TeamViewer, VPN, or similar software on the host machine.
* The server must not be installed as a DNS or DHCP server.

### Step 2: Configure the Firewall on the PBX Server <a href="#configure-the-firewall" id="configure-the-firewall"></a>

To allow the separate Instant Messaging (IM) server (IP: **192.168.1.25**) to access the PBX server (IP: **192.168.1.20**), it is necessary to create appropriate firewall rules on the PBX server.

Please execute the following commands on the PBX server (IP: **192.168.1.20**) to configure these firewall rules.

```sh
sudo firewall-cmd --permanent --zone=trusted --add-source=192.168.1.25
sudo firewall-cmd --reload
```

To verify that the rule has been created correctly, you can use the following command:

```sh
sudo firewall-cmd --zone=trusted --list-all
```

The correct output should be like below:

```sh
[ubuntu@localhost ~]$ sudo firewall-cmd --zone=trusted --list-all
trusted (active)
  target: ACCEPT
  icmp-block-inversion: no
  interfaces: 
  sources: 192.168.1.25
  services: 
  ports: 
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules:
```

### Step 3: Configuring the IP Address Whitelist <a href="#configuring-the-ip-address-whitelist" id="configuring-the-ip-address-whitelist"></a>

This step is mandatory; without it, the service will not work.

To prevent the PBX from limiting the IM servers' request rate, we need to add the IM servers' IPs to the whitelist in the PBX.

To do this, please follow the below steps:

1. Sign in to the PBX web portal as the System Administrator
2. Select the menu **IP Blacklist** > **Add**.
3. Enter the IM server IP as shown in the screenshot below and choose a long **expiration date.**

<figure><img src="../../../.gitbook/assets/im_server_whitelist.png" alt="" width="563"><figcaption></figcaption></figure>

### Step 4: Generate Token for the IM Server

1. Log in as the **System Administrator** to the PortSIP PBX Web portal.
2. Navigate to **Servers > IM Servers**.
3. Select the default server and click the **Generate Token** button.
4. Copy the generated token.

<figure><img src="../../../.gitbook/assets/im_server_update_address_new_token.png" alt=""><figcaption></figcaption></figure>

### **Step 5: Create and Run Instant Messaging Docker Instance**

Use the following command to create the Instant Messaging (IM) service Docker instance on the IM server (IP **192.168.1.25)**. Replace each parameter with your actual values:

* **-E**: Specifies that the IM server is installed in extended mode (required).
* **-p**: Specifies the path for storing IM service data (required).
* **-a**: Specifies the private IP address of this IM server. If this parameter is omitted, the **-A** parameter must be specified.
* **-A**: Specifies the public IP address of this IM server. If this parameter is omitted, the **-a** parameter must be specified.
* **-i**: Specifies the PBX Docker image version (required).
* **-x**: Indicates the main PBX server's IP address (typically the private IP of the main PBX server) (required).
* **-t**: Provides the token generated and copied in the previous step (required).
* **-f**: Specifies the path for storing files sent in chats. This path must differ from the one specified with **-p**. If omitted, chat files will be stored in the path specified by **-p**.

```sh
sudo /bin/sh im_ctl.sh run -E \
-p /var/lib/portsip/ \
-a 192.168.1.25 \
-i portsip/pbx:22 \
-x 192.168.1.20 \
-t OWMWYWJKZJYTMWM2NI0ZNZJMLWJJZDKTMGVMZDYXNZU1NWI1
```

Perform the below commands to restart the service (**please ensure the Main PBX service is started**).

```sh
cd /opt/portsip
sudo /bin/sh im_ctl.sh restart
```

If everything is set up correctly, the PBX web portal will display the IM server's IP address, as shown in the screenshot below.

<figure><img src="../../../.gitbook/assets/im_server_update_address.png" alt=""><figcaption></figcaption></figure>

The Instant Messaging (IM) server has been successfully installed. We can now proceed with the next steps in the [PortSIP PBX installation step 6](../1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22.md#step-6-reboot-to-apply-the-certificate).

