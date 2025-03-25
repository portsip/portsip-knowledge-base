# Scaling Servers On-Premise for High Availability

This guide explains how to configure cluster servers for a High Availability (HA) PortSIP PBX deployment, capable of supporting over 1 million users, approximately 50,000 online (registered/signed-in) users, and up to 10,000 simultaneous calls. The setup is also well-suited for high-demand scenarios, including large meetings, IVR, and call queues.

## Prerequisites

Before configuring the cluster servers, please ensure that you have completed the PBX HA installation and configuration on the **Main Server** by following the guide [High Availability Installations on Ubuntu](high-availability-installations-on-ubuntu.md).

{% hint style="danger" %}
Note: In this step, just need to install the PBX only, is no need to install the IM server at this stage, as it will be installed later in this guide.
{% endhint %}

## Preparing Cluster Servers

We need to prepare the Linux servers for installing the following cluster application servers:

* Media Servers
* Queue Servers
* Meeting Servers
* IVR servers

In this guide, we assume that the following servers are being installed for the cluster:

* **Main Server**: Install the PBX HA with a **virtual IP address** of **192.168.1.130**.
* **Server 1**: Install a media server with a private IP address of **192.168.1.21** and a static public IP of **104.101.137.60**.
* **Server 2**: Install a queue server with an IP address of **192.168.1.22**.
* **Server 3**: Install a meeting server with an IP address of **192.168.1.23**.
* **Server 4**: Install an IVR server with an IP address of 1**92.168.1.24**.

{% hint style="danger" %}
A server can only deploy one type of PortSIP server at a time. For instance, it's not allowed to deploy both the media server and queue server simultaneously on **Server 1**.
{% endhint %}

## **Supported Linux OS**

* Ubuntu 24.04, 64bit.
* We recommend allocating at least 128 GB of disk space, with no need for an additional data partition.

{% hint style="danger" %}
All cluster servers must run the same version of the Linux OS as the PBX in High Availability (HA).
{% endhint %}

{% hint style="danger" %}
When setting up the PBX cluster, please ensure there is sufficient network speed and bandwidth between the servers. Insufficient network resources may cause the PBX to not work as expected.
{% endhint %}

## **Preparing the Linux Host Machine for Installation**

Tasks that MUST be completed before installing cluster servers.

* **Ensure the server date-time is synced correctly**.
* If the Linux server is on a LAN, assign a **Static Private IP** address.
* For the **media server cluster**, each media server also needs a **Static Public IP** address if you want the user to call from the Internet.
* Install all available updates and service packs before installing the cluster server.
* Do not install PostgreSQL on the server.
* Ensure that all power-saving options for your system and network adapters are disabled (by setting the system to High-Performance mode).
* Do not install TeamViewer, VPN, or similar software on the host machine.
* The server must not be installed as a DNS or DHCP server.

## **Set password-free login for all these servers**

{% hint style="danger" %}
The following commands provided below should only be executed on the PBX HA node "**pbx01**".
{% endhint %}

The following commands provided below should only be executed on the PBX HA node "**pbx01**".

If you are prompted to choose an option (**yes/no**), please enter **yes**.

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.21
```

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.22
```

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.23
```

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.24
```

## Add the Cluster Servers

To add the cluster servers in the web portal, sign in to the **PBX Web portal** as the system administrator.

### Disable Default Servers <a href="#disable-default-servers" id="disable-default-servers"></a>

The PortSIP PBX installation comes with default media, queue, meeting, and IVR servers. We recommend disabling these default servers so that the **Main Server** only handles SIP signaling, allowing it to support more users and calls.

To do this, please select the **Servers** menu, expand each server type (media servers, queue servers, meeting servers, IVR servers), and turn off the default server as the below screenshot shows up.

**Note:** The IM server does not need to be disabled.

<figure><img src="../../../.gitbook/assets/disable-default-media-server.png" alt=""><figcaption></figcaption></figure>

### Add Media Server <a href="#add-media-server" id="add-media-server"></a>

To add a new media server, please follow the below steps:

1. Select the **Servers > Media Servers** menu and click the **Add** button.
2. Enter the server information as shown in the screenshot, and then click the **OK** button to save it. Please remember the server name "**media-server-1**", we will use it in a later step.
3. If your PBX is deployed for internet users to access, it is **mandatory** to assign a **static public IP** to this extended media server. Enter the static IP address as shown in the screenshot below.

Note: Suggest don't set the maximum of call sessions to more than 5,000.

<figure><img src="../../../.gitbook/assets/media-server-1.png" alt=""><figcaption></figcaption></figure>

4. Run the following command only on the HA PBX node **pbx01**. The process may take some time, so please be patient and wait for it to complete, don't interrupt or reboot.

* **-s media-server-only**: This parameter indicates that only the media server should be installed.
* **-n media-server-1**: This parameter specifies the name of the media server, which must match the name entered in step 2 above.
* **-a 192.168.1.21**: This parameter specifies the private IP address of **Server 1** (the extended media server).

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh run \
-s media-server-only \
-n media-server-1 \
-a 192.168.1.21
```

Once the process is completed, the server will appear as **Online** in the PBX web portal under the menu: **Servers > Media Servers**.

### Add Queue Server <a href="#add-media-server" id="add-media-server"></a>

To add a new queue server, please follow the below steps:

1. Select the **Servers > Queue Servers** menu and click the **Add** button.
2. Enter the server information as shown in the screenshot, and then click the **OK** button to save it. Please remember the server name "**queue-server-1**", we will use it in a later step.

<figure><img src="../../../.gitbook/assets/queue-server-1.png" alt=""><figcaption></figcaption></figure>

3. Run the following command only on the HA PBX node **pbx01**. The process may take some time, so please be patient and wait for it to complete, don't interrupt or reboot.

* **-s queue-server-only**: This parameter indicates that only the queue server should be installed.
* **-n queue-server-1**: This parameter specifies the name of the queue server, which must match the name entered in step 2 above.
* **-a 192.168.1.22**: This parameter specifies the private IP address of **Server 2** (the extended queue server).

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh run \
-s queue-server-only \
-n queue-server-1 \
-a 192.168.1.22
```

Once the process is completed, the server will appear as **Online** in the PBX web portal under the menu: **Servers > Queue Servers**.

### Add Meeting Server <a href="#add-media-server" id="add-media-server"></a>

To add a new meeting server, please follow the below steps:

1. Select the **Servers > Meeting Servers** menu and click the **Add** button.
2. Enter the server information as shown in the screenshot, and then click the **OK** button to save it. Please remember the server name "**meeting-server-1**", we will use it in a later step.

<figure><img src="../../../.gitbook/assets/meeting-server-1.png" alt=""><figcaption></figcaption></figure>

3. Run the following command only on the HA PBX node **pbx01**. The process may take some time, so please be patient and wait for it to complete, don't interrupt or reboot.

* **-s meeting-server-only**: This parameter indicates that only the meeting server should be installed.
* **-n meeting-server-1**: This parameter specifies the name of the meeting server, which must match the name entered in step 2 above.
* **-a 192.168.1.23**: This parameter specifies the private IP address of **Server 3** (the extended meeting server).

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh run \
-s meeting-server-only \
-n meeting-server-1 \
-a 192.168.1.23
```

Once the process is completed, the server will appear as **Online** in the PBX web portal under the menu: **Servers > Meeting Servers**.

### Add IVR Server <a href="#add-media-server" id="add-media-server"></a>

To add a new IVR server, please follow the below steps:

1. Select the **Servers > IVR Servers** menu and click the **Add** button.
2. Enter the server information as shown in the screenshot, and then click the **OK** button to save it. Please remember the server name "**vr-server-1**", we will use it in a later step.

<figure><img src="../../../.gitbook/assets/vr-server-1.png" alt=""><figcaption></figcaption></figure>

3. Run the following command only on the HA PBX node **pbx01**. The process may take some time, so please be patient and wait for it to complete, don't interrupt or reboot.

* **-s vr-server-only**: This parameter indicates that only the IVR server should be installed.
* **-n vr-server-1**: This parameter specifies the name of the IVR server, which must match the name entered in step 2 above.
* **-a 192.168.1.24**: This parameter specifies the private IP address of **Server 4** (the extended IVR server).

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh run \
-s vr-server-only \
-n vr-server-1 \
-a 192.168.1.24
```

Once the process is completed, the server will appear as **Online** in the PBX web portal under the menu: **Servers > IVR Servers**.

**Note**: You can repeat the above steps to set up more IVR servers. If you set up multiple servers, they must not use the same server name or IP address. Especially, you must ensure that the server name specified in the commands matches the one entered on the web portal.

## Managing Extended Servers

{% hint style="danger" %}
**Important:**\
All management commands for extended servers **must be executed on the `pbx01` node**, regardless of whether it is currently the active node.
{% endhint %}

### Available Operations

The following operations are supported for managing extended servers:

* `start` – Start the servers
* `stop` – Stop the servers
* `restart` – Restart the servers
* `rm` – Remove the installed servers

You can optionally target specific server types using the `-s` parameter, with one of the following values:

* `media-server-only` – Manage all Media Servers
* `queue-server-only` – Manage all Queue Servers
* `meeting-server-only` – Manage all Meeting Servers
* `ivr-server-only` – Manage all IVR Servers

To manage a **specific server instance** by its IP address, use the `-a` parameter.

### Managing All Extended Servers

The following commands apply actions to **all** extended servers(Media servers, Queue Servers, Meeting Servers, IVR Servers):

**Start All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh start
```

**Stop All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh stop
```

**Restart All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh restart
```

**Remove All:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh rm
```

### Managing a Specific Type of Extended Servers

To manage only a **specific type** of extended server (e.g., Media Servers), use the `-s` parameter. Replace `media-server-only` with other supported server types as needed.

**Start All Media Servers:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh start -s media-server-only
```

**Stop All Media Servers:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh stop -s media-server-only
```

**Restart All Media Servers:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh restart -s media-server-only
```

**Remove All Media Servers:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh rm -s media-server-only
```

### Managing a Specific Server Instance by IP

To manage a **specific server instance** (e.g., a Media Server at IP `192.168.1.21`), use both `-s` and `-a` parameters:

**Start Specific Media Server:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh start \
-s media-server-only \
-a 192.168.1.21
```

**Stop Specific Media Server:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh start \
-s media-server-only \
-a 192.168.1.21
```

**Restart Specific Media Server:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh start \
-s media-server-only \
-a 192.168.1.21
```

**Remove Specific Media Server:**

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh start \
-s media-server-only \
-a 192.168.1.21
```

{% hint style="info" %}
Replace `media-server-only` with another server type as needed (e.g., `queue-server-only`), and replace `192.168.1.21` with the corresponding server’s IP address.
{% endhint %}

## Upgrading Servers <a href="#upgrade-server" id="upgrade-server"></a>

{% hint style="danger" %}
All the below commands must be performed on the pbx01 node, even if it is not the current active node.
{% endhint %}

Perform the following command only on the HA PBX node **pbx01,** even if it is not the current active node.

First, please ensure you have upgraded the PBX HA as per this guide: [Upgrading High Availability Installation](upgrading-high-availability-installation.md).&#x20;

After that, perform the below command to download the latest HA resources:

```sh
  cd /opt && sudo rm -rf portsip-pbx-ha-guide-22-online.tar.gz && \
  sudo wget -N \
  https://www.portsip.com/downloads/ha/v22/portsip-pbx-ha-guide-22-online.tar.gz \
  && sudo tar xf portsip-pbx-ha-guide-22-online.tar.gz
```

### Upgrading All Extended Servers

If you want to upgrade all extended servers—including the **Media Server**, **Queue Server**, **Meeting Server**, and **IVR Server**—run the following command:

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash extend.sh upgrade
```

### Upgrading Specific Types of Extended Servers

If you want to upgrade only a specific type of extended server, use the `-s` parameter to define the server type. For example, to upgrade **all Media Servers**:

```sh
cd /opt/portsip-pbx-ha-guide/ && \
/bin/bash extend.sh upgrade -s media-server-only
```

**Supported Server Types**

In the above command, you can use one of the following values with the `-s` parameter:

* `media-server-only` – Upgrade all Media Servers
* `queue-server-only` – Upgrade all Queue Servers
* `meeting-server-only` – Upgrade all Meeting Servers
* `ivr-server-only` – Upgrade all IVR Servers

