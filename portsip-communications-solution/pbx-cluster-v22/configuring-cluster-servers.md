# Configuring Cluster Servers

In this guide, we assume the following servers are deployed for the PortSIP PBX cluster:

* **Main Server (PBX Call Manager):** PBX installed on `192.168.1.20`
* **Server 1 (Media Server):** Private IP `192.168.1.21`, Static Public IP `104.101.137.60`
* **Server 2 (Queue Server):** `192.168.1.22`
* **Server 3 (Meeting Server):** `192.168.1.23`
* **Server 4 (IVR Server):** `192.168.1.24`
* **Server 5 (IM Server):** Static Private IP `192.168.1.25`, Static Public IP `104.18.36.110`
* **Server 6 (DataFlow Server):** Static Private IP `192.168.1.26`

> **Important**\
> A single Linux host can deploy **only one PortSIP server role** at a time.\
> For example, you cannot deploy both a Media Server and a Queue Server on Server 1.

***

### Set Up the PortSIP PBX on the Main Server

Before configuring any cluster application servers, complete the PBX installation and initial configuration on the **Main Server** by following the [Installation of the PortSIP PBX](../portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/).

> **Note**\
> In this step, install **only the PBX**.\
> Do **not** install the IM server yet, it will be installed later in this guide.

> **Command Location**\
> Unless otherwise specified, run all commands from:\
> `/opt/portsip`

***

### Configure the Firewall on the Main Server

You must allow all cluster nodes to access the **PBX server (Main Server)**.

To configure these firewall rules, please execute the following commands on the PBX server (**Main Server**).

```bash
sudo firewall-cmd --permanent --zone=trusted --add-source=192.168.1.21
sudo firewall-cmd --permanent --zone=trusted --add-source=192.168.1.22
sudo firewall-cmd --permanent --zone=trusted --add-source=192.168.1.23
sudo firewall-cmd --permanent --zone=trusted --add-source=192.168.1.24
sudo firewall-cmd --permanent --zone=trusted --add-source=192.168.1.25
sudo firewall-cmd --permanent --zone=trusted --add-source=192.168.1.26
sudo firewall-cmd --reload
```

#### Verify Firewall Rules

```bash
sudo firewall-cmd --zone=trusted --list-all
```

Expected output example:

```shellscript
[ubuntu@localhost ~]$ sudo firewall-cmd --zone=trusted --list-all
trusted (active)
  target: ACCEPT
  icmp-block-inversion: no
  interfaces: 
  sources: 192.168.1.21 192.168.1.22 192.168.1.23 192.168.1.24 192.168.1.25 192.168.1.26
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

#### Optional: Allow the Entire /24 Subnet

If you want to allow the entire `192.168.1.0/24` subnet:

```bash
sudo firewall-cmd --permanent --zone=trusted --add-source=192.168.1.0/24
sudo firewall-cmd --reload
```

> **Note**\
> When you add new servers to the cluster later, you must update firewall rules to allow the new server IP addresses to access the PBX server.

***

### Configure the IP Whitelist in the PBX

This step is **mandatory**. Without it, cluster services may fail due to request rate limiting.

To prevent the PBX from throttling internal cluster traffic, add **each cluster server IP address** to the PBX whitelist.

<figure><img src="../../.gitbook/assets/cluster_ip_whitelist.png" alt=""><figcaption></figcaption></figure>



1. Sign in to the PBX Web Portal as **System Administrator**
2. Go to **IP Blacklist** > **Add**
3. Enter the cluster server IP address and select a long expiration date
4. Repeat for **each cluster server IP address**

***

### Add the Cluster Servers in the PBX Web Portal

Sign in to the PBX Web Portal as a System Administrator, then proceed with the following steps.

#### Disable Default Servers on the Main Server

The PBX installation includes default **Media**, **Queue**, **Meeting**, and **IVR** servers running on the Main Server. For production cluster deployments, PortSIP recommends disabling them so the Main Server focuses on SIP signaling and call control.

1. Go to menu **Servers**
2. Click on each server type:
   * Media Servers
   * Queue Servers
   * Meeting Servers
   * IVR Servers
3. Turn off the default server for each type

<figure><img src="../../.gitbook/assets/disable-default-media-server.png" alt=""><figcaption></figcaption></figure>

> **Note**\
> The **default IM and Data Flow server** does not need to be disabled.

***

### Media Server

#### Add Media Server in the Web Portal

1. Go to **Servers** > **Media Servers**
2. Click **Add**
3. Enter the server information with the server name `media-server-1`, as shown in the screenshot
4. Click **OK** to save

If your PBX is deployed for internet users to access, it is essential to assign a **static public IP** to the extended media server. Enter the static IP address as shown in the screenshot below.

<figure><img src="../../.gitbook/assets/media-server-1.png" alt=""><figcaption></figcaption></figure>

> **Recommendation**\
> Do not set **Maximum Call Sessions** higher than **5,000** per media server unless advised by PortSIP support based on sizing validation.

#### Install Media Server (Server 1)

Go to **Server 1** (whose IP address is **192.168.1.21**) to install the media server.

Make sure you have followed the guide for [Preparing Cluster Servers](https://support.portsip.com/portsip-communications-solution/pbx-cluster-v22/preparing-cluster-servers) on **Server 1**.

To install the media server, execute the following commands and pay close attention to the parameters:

* `-a 192.168.1.21`: This specifies the private IP of **Server 1**.
* `-x 192.168.1.20`: This specifies the private IP of the PBX Server (**Main Server**).
* `-s media-server-only`: This indicates that only the media server should be installed.
* `-n media-server-1` This specifies the name of the media server, which **must match** the name entered on the PBX Web portal in the previous step.

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh \
run -p /var/lib/portsip \
-a 192.168.1.21 \
-x 192.168.1.20 \
-i portsip/pbx:22 \
-s media-server-only \
-n media-server-1
```

**Cloud Firewall Requirement**

If the Media Server is hosted in the cloud, such as AWS or Azure, allow: `35000–65000/UDP`

**Scaling Tip**

To add more media servers, repeat the process with a unique **server name** and **IP address**.\
The server name used in the command **must exactly match** the name created in the Web Portal.

{% hint style="danger" %}
If you set up multiple media servers, they must not use the same server name or IP address. Especially, you must ensure that the server name specified in the commands matches the one entered on the web portal.
{% endhint %}

***

### Queue Server

#### Add Queue Server in the Web Portal

1. Go to **Servers** > **Queue Servers**
2. Click **Add**
3. Enter the server information with the server name `queue-server-1`, as shown in the screenshot
4. Click **OK** to save

<figure><img src="../../.gitbook/assets/queue-server-1.png" alt=""><figcaption></figcaption></figure>

#### Install Queue Server (Server 2)

Go to **Server 2** (whose IP address is **192.168.1.22**) to install the queue server.

Make sure you have followed the guide for [Preparing Cluster Servers](https://support.portsip.com/portsip-communications-solution/pbx-cluster-v22/preparing-cluster-servers) for **Server 2**.

On **Server 2** (`192.168.1.22`), execute the command below and pay close attention to the parameters:

* `-a 192.168.1.22`: This specifies the private IP on **Server 2**.
* `-x 192.168.1.20`: This specifies the private IP of the PBX Server (**Main Server**).
* `-s queue-server-only`: This indicates that only the queue server should be installed.
* `-n queue-server-1`: This specifies the name of the queue server, which **must match** the name entered on the PBX Web portal in the previous step.

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh \
run -p /var/lib/portsip \
-a 192.168.1.22 \
-x 192.168.1.20 \
-i portsip/pbx:22 \
-s queue-server-only \
-n queue-server-1
```

**Scaling Tip**

To add more queue servers, repeat the process with a unique **server name** and **IP address**.\
The server name used in the command **must exactly match** the name created in the Web Portal.

{% hint style="danger" %}
If you set up multiple queue servers, they must not use the same server name or IP address. Especially, you must ensure that the server name specified in the commands matches the one entered on the web portal.
{% endhint %}

***

### Meeting Server

#### Add Meeting Server in the Web Portal

1. Go to **Servers** > **Meeting Servers**
2. Click **Add**
3. Enter the server information with the server name `meeting-server-1`, as shown in the screenshot
4. Click **OK** to save

<figure><img src="../../.gitbook/assets/meeting-server-1.png" alt=""><figcaption></figcaption></figure>

#### Install Meeting Server (Server 3)

Go to **Server 3** (whose IP address is **192.168.1.23**) to install the meeting server.

Make sure you have followed the guide for [Preparing Cluster Servers](https://support.portsip.com/portsip-communications-solution/pbx-cluster-v22/preparing-cluster-servers) on **Server 3**.

To install the meeting server, execute the following commands and pay close attention to the parameters:

* `-a 192.168.1.23`: This specifies the private IP of **Server 3**.
* `-x 192.168.1.20`: This specifies the private IP of the PBX Server (**Main Server**).
* `-s meeting-server-only`: This indicates that only the meeting server should be installed.
* `-n meeting-server-1`: This specifies the name of the meeting server, which **must match** the name entered on the PBX Web portal in the previous step.

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh \
run -p /var/lib/portsip \
-a 192.168.1.23 \
-x 192.168.1.20 \
-i portsip/pbx:22 \
-s meeting-server-only \
-n meeting-server-1
```

**Scaling Tip**

To add more meeting servers, repeat the process with a unique **server name** and **IP address**.\
The server name used in the command **must exactly match** the name created in the Web Portal.

{% hint style="danger" %}
If you set up multiple meeting servers, they must not use the same server name or IP address. Especially, you must ensure that the server name specified in the commands matches the one entered on the web portal.
{% endhint %}

***

### IVR (Virtual Receptionist) Server

#### Add IVR Server in the Web Portal

1. Go to **Servers** > **IVR Servers**
2. Click **Add**
3. Enter the server information with the server name `vr-server-1`, as shown in the screenshot
4. Click **OK** to save

<figure><img src="../../.gitbook/assets/vr-server-1.png" alt=""><figcaption></figcaption></figure>

#### Install IVR Server (Server 4)

Make sure you have followed the guide for [Preparing Cluster Servers](https://support.portsip.com/portsip-communications-solution/pbx-cluster-v22/preparing-cluster-servers) on **Server 4**.

To install the IVR server, execute the following commands and pay close attention to the parameters:

* `-a 192.168.1.24`: This specifies the private IP of **Server 4**.
* `-x 192.168.1.20`: This specifies the private IP of the PBX Server (**Main Server**).
* `-s vr-server-only`: This indicates that only the meeting server should be installed.
* `-n vr-server-1`: This specifies the name of the IVR server, which must match the name entered on the PBX Web portal in the previous step.

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh \
run -p /var/lib/portsip \
-a 192.168.1.24 \
-x 192.168.1.20 \
-i portsip/pbx:22 \
-s vr-server-only \
-n ivr-server-1
```

**Scaling Tip**

To add more IVR servers, repeat the process with a unique **server name** and **IP address**.\
The server name used in the command **must exactly match** the name created in the Web Portal.

{% hint style="danger" %}
If you set up multiple IVR servers, they must not use the same server name or IP address. Especially, you must ensure that the server name specified in the commands matches the one entered on the web portal.
{% endhint %}

***

### Installing the IM Server

At this time, the IM server **does not support clustered deployment** and must be deployed as a standalone server.

With a properly sized server, IM can support up to **50,000 concurrent online users** for messaging and file sharing.

#### Recommended Hardware (Up to 50,000 Online Users)

* **CPU**: 20 cores or higher
* **Memory**: 16 GB
* **Disk**: High I/O performance required (SSD recommended, at least 256 GB)
* **Network Bandwidth**: 1000 Mbps or higher, particularly if handling high volumes of messages and file sharing.
* **Static private IP**: You must configure a static private IP for this IM server. In this case, we assume it's **192.168.1.25.**
* **Static public IP:** If your PBX and IM server are located in the cloud for the internet users to access, you must have a static public IP for this IM service. In this case, we assume it's **104.18.36.110.**

***

#### Generate an IM Token

1. Sign in to the PBX Web Portal as **System Administrator**
2. Go to **Servers** > **IM Servers**
3. Select the default server and click **Generate Token**
4. Copy the token for later use

<figure><img src="../../.gitbook/assets/portsip-pbx-v22-im-token.png" alt=""><figcaption></figcaption></figure>

***

#### Create and Run the IM Docker Instance (Server 5)

**Parameter reference**

Use the following command to create the Instant Messaging (IM) service Docker instance in the server that has the IP **192.168.1.25**. Replace each parameter with your actual values:

* **-E**: Specifies that the IM server is installed in extended mode (required).
* **-p**: Specifies the path for storing IM service data (required).
* **-a**: Specifies the private IP address of this IM server. If this parameter is omitted, the **-A** parameter must be specified.
* **-A**: Specifies the public IP address of this IM server. If this parameter is omitted, the **-a** parameter must be specified. If you install the IM server on a **separate server in the cloud, this parameter must be specified**. Otherwise, it can be ignored. In this case is **104.18.36.110**.
* **-i**: Specifies the PBX Docker image version (required).
* **-x**: Indicates the main PBX server's IP address (typically the private IP of the main PBX server) (required).&#x20;
* **-t**: Provides the token generated and copied in the previous step (required).
* **-f**: Specifies the path for storing files sent in chats. This path must differ from the one specified with **-p**. If omitted, chat files will be stored in the path specified by **-p**.

Example (no separate chat file path):

```bash
cd /opt/portsip
sudo /bin/sh im_ctl.sh run -E \
-p /var/lib/portsip/ \
-a 192.168.1.25 \
-A 104.18.36.110 \
-i portsip/pbx:22 \
-x 192.168.1.20 \
-t MJC4NZBLYTGTZTJJNS0ZMWZHLWIXZDCTZJLLMDEWZJHKZTAY
```

Example (store chat files under `/cha`For example, if you want to store the chat files in the path `/chat/files`, please ensure this path already exists, then use the below command (please pay attention to the **-f** parameter) to create the Instant Messaging (IM) service Docker instance on the IM server (IP **192.168.1.25)**. Replace each parameter with your actual values:

```bash
cd /opt/portsip
sudo /bin/sh im_ctl.sh run -E \
-p /var/lib/portsip/ \
-f /chat/files/ \
-a 192.168.1.25 \
-A 104.18.36.110 \
-i portsip/pbx:22 \
-x 192.168.1.20 \
-t MJC4NZBLYTGTZTJJNS0ZMWZHLWIXZDCTZJLLMDEWZJHKZTAY
```

**Cloud Firewall Requirement (IM)**

{% hint style="danger" %}
If your Instant Messaging (IM) server is hosted in the cloud (e.g., AWS), you must ensure that TCP port 8887 is open in the cloud firewall rules. The client application requires access to this port in order to send and receive messages.
{% endhint %}

***

### Installing the DataFlow Server

Currently, the DataFlow server **does not support clustered deployment** and must be deployed as a standalone server.

DataFlow is built on [ClickHouse](https://www.clickhouse.com). For best performance and stability, follow ClickHouse sizing best practices.

#### Minimum Requirements

* **vCPU:** 4 cores
* **Memory:** 8 GB
* **Disk:** 128 GB SSD

#### Recommended Requirements

* **vCPU:** 8 cores
* **Memory:** 16 -32 GB
* **Disk:** 256 GB+ (NVMe SSD preferred)

#### Sizing Guideline (Large Deployments)

* **vCPU:** ≥ 8
* **Memory:** `vCPU × 4 GB`
* **Disk:** Based on expected CDR volume and retention policy

***

#### Generate a DataFlow Token

1. Sign in to the PBX Web Portal as **System Administrator**
2. Go to **Servers** > **Data Flow**
3. Select the default server and click **Generate Token**
4. Copy and securely store the token

<figure><img src="../../.gitbook/assets/data-flow-1.png" alt=""><figcaption></figcaption></figure>

***

#### Create the DataFlow Docker Instance (Server 6)

**Command parameters**

* `-p` : Path for storing Data Flow and ClickHouse data (required)
* `-d` : ClickHouse Docker image (`portsip/clickhouse:25.8`)
* `-a` : Private IP address of the Data Flow server
* `-A` : Public IP address (use if private IP is not available)
* `-i` : PortSIP PBX Docker image version (required)
* `-x` : PBX server static private IP address

Example:

```bash
cd /opt/portsip
sudo /bin/sh dataflow_ctl.sh run \
-p /var/lib/portsip/ \
-a 192.168.1.26 \
-i portsip/pbx:22 \
-x 192.168.1.20 \
-d portsip/clickhouse:25.8
```

***

#### Operational Notes (DataFlow)

* If the **PBX IP address changes**, you must delete and recreate the existing Data Flow Docker instance.
* If a **new authentication token** is generated, the Data Flow Docker instance must be deleted and recreated.
* After upgrading the **PBX to a new version**, you must remove and recreate the Data Flow Docker instance to ensure compatibility.

The above operations **do not affect or erase existing analytics data** stored in ClickHouse.

***

### Restart Services

After adding and installing all cluster servers, restart services in the following order.

#### Restart PBX (On Main Server)

```bash
cd /opt/portsip
sudo /bin/sh pbx_ctl.sh restart
```

#### Restart Resource Load Balancer (On Main Server)

```bash
cd /opt/portsip
sudo /bin/sh pbx_ctl.sh restart -s loadbalancer
```

#### Restart Media Servers

On each media server:

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh restart -s media-server-only
```

#### Restart Queue Servers

On each queue server:

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh restart -s queue-server-only
```

#### Restart Meeting Servers

On each meeting server:

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh restart -s meeting-server-only
```

#### Restart IVR Servers

On each IVR server:

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh restart -s vr-server-only
```

#### Restart the IM Server

On the IM server (ensure PBX is already running):

```bash
cd /opt/portsip
sudo /bin/sh im_ctl.sh restart
```

#### Restart the Data Flow Server

On the Data Flow server (ensure PBX is already running):

```bash
cd /opt/portsip
sudo /bin/sh dataflow_ctl.sh restart
```

***

### SBC Cluster

To deploy the SBC cluster, follow the instructions in [Deploy the SBC Cluster](../portsip-pbx-administration-guide/11-deploy-the-sbc-cluster.md).



