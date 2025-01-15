# Managing Cluster

After successfully setting up the PortSIP PBX cluster, you can manage it easily.

## Managing Servers

On each cluster server (**not the PBX server**), you can use the following commands to manage it.

In the commands, use the parameter **-s** to specify the service name, PortSIP PBX supports these services:

* media-server-only
* queue-server-only
* meeting-server-only
* vr-server-only

### Start Server

```sh
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh start -s media-server-only
```

You can replace the **media-server-only** with another service name such as mentioned above.

### Restart Server

<pre class="language-sh"><code class="lang-sh">cd /opt/portsip
<strong>sudo /bin/sh cluster_ctl.sh restart -s media-server-only
</strong></code></pre>

### Check Status

```sh
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh status -s media-server-only
```

### Stop Server

```sh
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh stop -s media-server-only
```

### Remove Server

```sh
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh rm -s media-server-only
```

## Managing IM Server

### Start Server

```sh
cd /opt/portsip
sudo /bin/sh im_ctl.sh start
```

### Restart Server

```sh
cd /opt/portsip
sudo /bin/sh im_ctl.sh restart
```

### Stop Server

```sh
cd /opt/portsip
sudo /bin/sh im_ctl.sh stop
```

### Check Status

```sh
cd /opt/portsip
sudo /bin/sh im_ctl.sh status
```

### Remove Server

```sh
cd /opt/portsip
sudo /bin/sh im_ctl.sh rm
```

## Adding Servers

As the scale of your business expands, you may need to add more servers to the existing cluster.&#x20;

To do this, simply follow the topics on [Preparing Cluster Servers](preparing-cluster-servers.md) and [Configuring Cluster Servers](configuring-cluster-servers.md) to add servers. Then, restart the resource load balancer and all cluster servers - there is no need to restart the PBX server.&#x20;

{% hint style="info" %}
Currently, the IM server does not support cluster installations; it can be deployed as a standalone server. It can support up to 50,000 online users with a powerful CPU and memory(16 cores, 16GB). So there is no support to add more IM servers.
{% endhint %}

Please note that restarting the resource load balancer and servers will affect the calling service, so it is recommended to do this in the middle night.

## Upgrading Servers

{% hint style="danger" %}
It’s crucial to keep your cluster servers updated in line with the latest PortSIP PBX releases. This ensures that all features function as expected and that your system maintains optimal performance and security.
{% endhint %}

Whenever a new version of PortSIP PBX is released, it’s essential to upgrade your installed cluster servers as well. Follow the steps below to ensure a successful upgrade.

### Upgrading the Main PBX Server

1. Remove the currently running PBX Main server by the commands on the server 192.168.1.20:

```sh
cd /opt/portsip
sudo /bin/sh pbx_ctl.sh stop
sudo /bin/sh pbx_ctl.sh rm
```

2. Install the latest version of the PBX server. Follow the instructions provided in the [Installation of the PortSIP PBX](../portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22/install-portsip-pbx-on-linux.md) guide to complete this step with the latest version.

### Upgrading the IM Server

1. Remove the currently running server by the commands:

```sh
cd /opt/portsip
sudo /bin/sh im_ctl.sh stop
sudo /bin/sh im_ctl.sh rm
```

2. Install the latest version of the IM server. Follow the instructions provided in the [Installing the IM Server](configuring-cluster-servers.md#installing-the-im-server) guide to complete this step with the latest version.

### Upgrading Cluster Servers

We use the **media server** as an example, you will need to replace the media server with other servers as your actual environment.

1. Remove the current running server by the commands:

```sh
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh stop -s media-server-only
sudo /bin/sh cluster_ctl.sh rm -s media-server-only
```

2. Install the latest version of the server. Follow the instructions provided in the [Install Media server](../pbx-cluster/configuring-cluster-servers.md#install-media-server) guide to complete this step with the latest version.

