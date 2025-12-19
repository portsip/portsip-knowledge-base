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

Please follow the guide [Upgrade to the Latest Version Within v22.x on Linux](/broken/pages/HYNilqLl52W9urfWCLid) to upgrade the main PBX server, SBC server, and IM server.

### Upgrading Cluster Servers

Perform the below commands to download the latest scripts.

```sh
sudo curl \
https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.x/init.sh  \
-o  init.sh
```

```sh
sudo /bin/sh init.sh
```

Use the below commands to upgrade the servers. We use the **-s** parameter to specify the server name.

#### Media Server

```sh
cd /opt/portsip && sudo /bin/sh cluster_ctl.sh upgrade \
-s media-server-only -i portsip/pbx:22
```

#### Queue Server

```sh
cd /opt/portsip && sudo /bin/sh cluster_ctl.sh upgrade \
-s queue-server-only -i portsip/pbx:22
```

#### IVR Server

```sh
cd /opt/portsip && sudo /bin/sh cluster_ctl.sh upgrade \
-s vr-server-only -i portsip/pbx:22
```

#### Meeting Server

```sh
cd /opt/portsip && sudo /bin/sh cluster_ctl.sh upgrade \
-s meeting-server-only -i portsip/pbx:22
```



