# Configuring Cluster Servers

In this guide, we will assume that the following servers are being installed for the cluster:

* **Main Server**: Install the PBX with an IP address of **192.168.1.20**.
* **Server 1**: Install a media server with a private IP address of **192.168.1.21** and a static public IP of **104.101.137.60**.
* **Server 2**: Install a queue server with an IP address of **192.168.1.22**.
* **Server 3**: Install a meeting server with an IP address of **192.168.1.23**.
* **Server 4**: Install an IVR server with an IP address of 1**92.168.1.24**.

{% hint style="danger" %}
A server can only deploy one type of PortSIP server at a time. For instance, it's not allowed to deploy both the media server and queue server simultaneously on **Server 1**.
{% endhint %}

## Setup the PortSIP PBX

Before configuring the cluster servers, please ensure that you have completed the PBX installation and configuration on the **Main Server** by following the guide for [Installation of PortSIP PBX](../portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v16/) and [Configuring the PortSIP PBX](/broken/pages/6uo0BsKGLXFqs7Cz40HY).

## Configure the Firewall

It is necessary to create firewall rules on the **Main Server** that allow the cluster servers to access the PBX server (**Main Server**).&#x20;

To configure these firewall rules, please perform the following commands on PBX server (**Main Server**).

```
firewall-cmd --permanent --zone=trusted --add-source=192.168.1.21
firewall-cmd --permanent --zone=trusted --add-source=192.168.1.22
firewall-cmd --permanent --zone=trusted --add-source=192.168.1.23
firewall-cmd --permanent --zone=trusted --add-source=192.168.1.24
firewall-cmd --reload
```

To verify that the rule has been created correctly, you can use the following command:

```
firewall-cmd --zone=trusted --list-all
```

The correct output should be like below:

```
[root@localhost ~]# firewall-cmd --zone=trusted --list-all
trusted (active)
  target: ACCEPT
  icmp-block-inversion: no
  interfaces: 
  sources: 192.168.1.21 192.168.1.22 192.168.1.23 192.168.1.24
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

To allow the entire C class address to access the PBX server (**Main Server**), you can use the following commands:

```
firewall-cmd --permanent --zone=trusted --add-source=192.168.1.0/24  
firewall-cmd --reload
```

In the future, if you add more servers to the existing cluster, you will need to create firewall rules to allow those servers’ IP addresses to access the PBX server (**Main Server**), just like the rules mentioned above.

## Configuring the IP Address Whitelist

{% hint style="danger" %}
This step is mandatory; without it, the service will not work.
{% endhint %}

To prevent the PBX from limiting the cluster servers' request rate, we need to add the cluster servers' IPs to the whitelist in the PBX.&#x20;

To do this, please follow the below steps:

1. Sign in the PBX web portal as the System Administrator
2. Select the menu **IP Blacklist** > **Add**.&#x20;
3. Enter the cluster server IP as shown in the screenshot below and choose a long **expiration date.**
4. Repeat the above steps for each cluster server.

<figure><img src="../../.gitbook/assets/cluster_ip_whitelist.png" alt=""><figcaption></figcaption></figure>

## Add the Cluster Servers

To add the cluster servers in the web portal, sign in to the PBX Web portal as the system administrator.

### Disable Default Servers

The PortSIP PBX installation comes with default media, queue, meeting, and IVR servers. We recommend disabling these default servers so that the **Main Server** only handles SIP signaling, allowing it to support more users and calls.&#x20;

To do this, please select the **Servers** menu, expand each server type(media servers, queue servers, meeting servers, IVR servers), and turn off the default server as the below screenshot shows up.

<figure><img src="../../.gitbook/assets/disable-default-media-server.png" alt=""><figcaption></figcaption></figure>

### Add Media Server

To add a new media server, select the **Servers > Media Servers** menu, click the **Add** button, enter the server information as shown in the screenshot then click the **OK** button to save it. Please remember the server name "**media-server-1**", we will use it in a later step.

Suggest don't set the Maximum of call sessions to more than 5,000.

<figure><img src="../../.gitbook/assets/media-server-1.png" alt=""><figcaption></figcaption></figure>

### Install Media Server

Please go to **Server 1** (whose IP address is **192.168.1.21**) to install the media server.&#x20;

Make sure you have followed the guide for [Preparing Cluster Servers](preparing-cluster-servers.md) on **Server 1**.

To install the media server, execute the following commands and pay close attention to the parameters:

* `-a 192.168.1.21`: This specifies the private IP of **Server 1**.
* `-x 192.168.1.20`: This specifies the private IP of the PBX Server (**Main Server**).
* `-s media-server-only`: This indicates that only the media server should be installed.
* `-n media-server-1`: This specifies the name of the media server, which must be the same as the name entered on the PBX Web portal in the previous step.

```
cd /opt/portsip
/bin/sh cluster_ctl.sh \
run -p /var/lib/portsip \
-a 192.168.1.21 \
-x 192.168.1.20 \
-i portsip/pbx:16 \
-s media-server-only \
-n media-server-1
```

{% hint style="danger" %}
If your media server is hosted on a cloud platform such as AWS or Azure, you will need to create a network rule in the cloud platform to allow the following ports:

* `35000 - 65000`: UDP
{% endhint %}

You can repeat the above steps to set up more media servers. Just make sure to use a different IP address and server name for each one.

{% hint style="danger" %}
If you set up multiple media servers, they must not use the same server name or IP address. Especially, you must ensure that the server name specified in the commands matches the one entered on the web portal.
{% endhint %}

### Add Queue Server

To add a new queue server, select the **Servers > Queue Servers** menu, click the **Add** button, enter the server information as shown in the screenshot then click the **OK** button to save it. Please remember the server name "**queue-server-1**", we will use it in a later step.

<figure><img src="../../.gitbook/assets/queue-server-1.png" alt=""><figcaption></figcaption></figure>

### Install Queue Server

Please go to **Server 2** (whose IP address is **192.168.1.22**) to install the queue server.&#x20;

Make sure you have followed the guide for [Preparing Cluster Servers](preparing-cluster-servers.md) for **Server 2**.

&#x20;To install the queue server, execute the following commands and pay close attention to the parameters:

* `-a 192.168.1.22`: This specifies the private IP on **Server 2**.
* `-x 192.168.1.20`: This specifies the private IP of the PBX Server (**Main Server**).
* `-s queue-server-only`: This indicates that only the queue server should be installed.
* `-n queue-server-1`: This specifies the name of the queue server, which must be the same as the name entered on the PBX Web portal in the previous step.

```
cd /opt/portsip
/bin/sh cluster_ctl.sh \
run -p /var/lib/portsip \
-a 192.168.1.22 \
-x 192.168.1.20 \
-i portsip/pbx:16 \
-s queue-server-only \
-n queue-server-1
```

You can repeat the above steps to set up more queue servers. Just make sure to use a different IP address and server name for each one.

{% hint style="danger" %}
If you set up multiple queue servers, they must not use the same server name or IP address. Especially, you must ensure that the server name specified in the commands matches the one entered on the web portal.
{% endhint %}

### Add Meeting Server

To add a new meeting server, select the **Servers > Meeting Servers** menu, click the **Add** button, enter the server information as shown in the screenshot then click the **OK** button to save it. Please remember the server name "**meeting-server-1**", we will use it in a later step.

<figure><img src="../../.gitbook/assets/meeting-server-1.png" alt=""><figcaption></figcaption></figure>

### Install Meeting Server

Please go to **Server 3** (whose IP address is **192.168.1.23**) to install the meeting server.&#x20;

Make sure you have followed the guide for [Preparing Cluster Servers](preparing-cluster-servers.md) on **Server 3**.

&#x20;To install the meeting server, execute the following commands and pay close attention to the parameters:

* `-a 192.168.1.23`: This specifies the private IP of **Server 3**.
* `-x 192.168.1.20`: This specifies the private IP of the PBX Server (**Main Server**).
* `-s meeting-server-only`: This indicates that only the meeting server should be installed.
* `-n meeting-server-1`: This specifies the name of the meeting server, which must be the same as the name entered on the PBX Web portal in the previous step.

```
cd /opt/portsip
/bin/sh cluster_ctl.sh \
run -p /var/lib/portsip \
-a 192.168.1.23 \
-x 192.168.1.20 \
-i portsip/pbx:16 \
-s meeting-server-only \
-n meeting-server-1
```

You can repeat the above steps to set up more meeting servers. Just make sure to use a different IP address and server name for each one.

{% hint style="danger" %}
If you set up multiple meeting servers, they must not use the same server name or IP address. Especially, you must ensure that the server name specified in the commands matches the one entered on the web portal.
{% endhint %}

### Add IVR Server

To add a new IVR server, select the **Servers > IVR Servers** menu, click the **Add** button, enter the server information as shown in the screenshot then click the **OK** button to save it. Please remember the server name "**vr-server-1**", we will use it in a later step.

<figure><img src="../../.gitbook/assets/vr-server-1.png" alt=""><figcaption></figcaption></figure>

### Install IVR Server

Please go to **Server 4** (whose IP address is **192.168.1.24**) to install the IVR server.&#x20;

Make sure you have followed the guide for [Preparing Cluster Servers](preparing-cluster-servers.md) on **Server 4**.

To install the IVR server, execute the following commands and pay close attention to the parameters:

* `-a 192.168.1.24`: This specifies the private IP of **Server 4**.
* `-x 192.168.1.20`: This specifies the private IP of the PBX Server (**Main Server**).
* `-s vr-server-only`: This indicates that only the meeting server should be installed.
* `-n vr-server-1`: This specifies the name of the IVR server, which must be the same as the name entered on the PBX Web portal in the previous step.

```
cd /opt/portsip
/bin/sh cluster_ctl.sh \
run -p /var/lib/portsip \
-a 192.168.1.24 \
-x 192.168.1.20 \
-i portsip/pbx:16 \
-s vr-server-only \
-n vr-server-1
```

You can repeat the above steps to set up more IVR servers. Just make sure to use a different IP address and server name for each one.

{% hint style="danger" %}
If you set up multiple IVR servers, they must not use the same server name or IP address. Especially, you must ensure that the server name specified in the commands matches the one entered on the web portal.
{% endhint %}

## Restarting Servers

After completing adding the cluster servers, now go to restart the servers to make them up.

### Restart the Resource Load Balancer

Perform the following command on the PBX Server (**Main Server**) to restart the resource load balancer.

```
cd /opt/portsip
/bin/sh pbx_ctl.sh restart -s loadbalancer
```

### Restart Media Servers

Go to each media server, and perform the below commands to restart the server.

```
cd /opt/portsip
/bin/sh cluster_ctl.sh restart -s media-server-only
```

### Restart Queue Servers

Go to each queue server, and perform the below commands to restart the server.

```
cd /opt/portsip
/bin/sh cluster_ctl.sh restart -s queue-server-only
```

### Restart Meeting Servers

Go to each meeting server, and perform the below commands to restart the server.

```
cd /opt/portsip
/bin/sh cluster_ctl.sh restart -s meeting-server-only
```

### Restart IVR Servers

Go to each IVR server, and perform the below commands to restart the server.

```
cd /opt/portsip
/bin/sh cluster_ctl.sh restart -s vr-server-only
```

Now all cluster servers have been successfully up, you can check their status on the PBX Web Portal by going to the **Servers** menu,  if everything has been set up correctly, the server status will be displayed as "**Online**".

## Upgrading Servers

{% hint style="danger" %}
It’s crucial to keep your cluster servers updated in line with the latest PortSIP PBX releases. This ensures that all features function as expected and that your system maintains optimal performance and security.
{% endhint %}

Whenever a new version of PortSIP PBX is released, it’s essential to upgrade your installed cluster servers as well. Follow the steps below to ensure a successful upgrade.

We use the media server as an example, you will need to replace the media server with other servers as your actual environment.

1. Remove the current running server by the commands:

```
cd /opt/portsip
/bin/sh cluster_ctl.sh stop -s media-server-only
/bin/sh cluster_ctl.sh rm -s media-server-only
```

2. Install the latest version of the server. Follow the instructions provided in the [Add Cluster Servers](configuring-cluster-servers.md#add-the-cluster-servers) guide to complete this step.

## SBC Cluster

To deploy the SBC cluster, please follow this topic [Deploy the SBC Cluster](../portsip-pbx-administration-guide/11-deploy-the-sbc-cluster.md).

