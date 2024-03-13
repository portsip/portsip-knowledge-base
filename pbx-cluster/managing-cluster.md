# Managing Cluster

After successfully setup the PortSIP PBX cluster, you can manage it in an easy way.

## Managing Servers

On each cluster server (**not the PBX server**), you can use the following commands to manage it.

In the commands, use the parameter **-s** to specify the service name, PortSIP PBX supports these services:

* media-server-only
* queue-server-only
* meeting-server-only
* vr-server-only

### Start Server

```
cd /opt/portsip
/bin/sh cluster_ctl.sh start -s media-server-only
```

You can replace the **media-server-only** with another service name such as mentioned above.

### Restart Server

```
cd /opt/portsip
/bin/sh cluster_ctl.sh restart -s media-server-only
```

### Check Status

```
cd /opt/portsip
/bin/sh cluster_ctl.sh status -s media-server-only
```

### Stop Server

```
cd /opt/portsip
/bin/sh cluster_ctl.sh stop -s media-server-only
```

### Remove Server

```
cd /opt/portsip
/bin/sh cluster_ctl.sh rm -s media-server-only
```

## Adding Servers

As the scale of your business expands, you may need to add more servers to the existing cluster.&#x20;

To do this, simply follow the topics on [Preparing Cluster Servers](preparing-cluster-servers.md) and [Configuring Cluster Servers](configuring-cluster-servers.md) to add servers. Then, restart the resource load balancer and all cluster servers - there is no need to restart the PBX server.&#x20;

Please note that restarting the resource load balancer and servers will affect the calling service, so it is recommended to do this during the middle night.

## Upgrading Servers

Every time the PortSIP PBX releases a new version, you must also upgrade these installed cluster servers. The steps are:

1. Remove the current running server by the commands:

```
cd /opt/portsip
/bin/sh cluster_ctl.sh stop -s media-server-only
/bin/sh cluster_ctl.sh rm -s media-server-only
```

2. Install the latest version server as the guide [Configuring Cluster Servers](configuring-cluster-servers-for-portsip-pbx-ha.md).

