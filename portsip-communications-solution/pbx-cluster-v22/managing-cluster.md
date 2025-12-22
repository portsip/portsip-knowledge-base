# Managing Cluster

After successfully deploying the **PortSIP PBX** cluster, you can manage and maintain the system efficiently using the built-in control scripts. These scripts allow you to start, stop, restart, monitor, remove, and upgrade individual cluster services without impacting the core PBX server.

***

### Managing Cluster Servers

The commands in this article apply to **cluster servers only** (for example, Media, Queue, Meeting, or IVR servers). They must **not** be executed on the main PBX server.

#### Supported Service Types

When running cluster management commands, use the `-s` parameter to specify the service type. The following service names are supported:

* `media-server-only`
* `queue-server-only`
* `meeting-server-only`
* `vr-server-only`

***

#### Starting a Cluster Server

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh start -s media-server-only
```

Replace `media-server-only` with the appropriate service name as required.

***

#### Restarting a Cluster Server

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh restart -s media-server-only
```

***

#### Checking Cluster Server Status

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh status -s media-server-only
```

***

#### Stopping a Cluster Server

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh stop -s media-server-only
```

***

#### Removing a Cluster Server

```bash
cd /opt/portsip
sudo /bin/sh cluster_ctl.sh rm -s media-server-only
```

***

### Managing the Instant Messaging (IM) Server

The IM server is managed independently using a dedicated control script.

#### Start the IM Server

```bash
cd /opt/portsip
sudo /bin/sh im_ctl.sh start
```

#### Restart the IM Server

```bash
cd /opt/portsip
sudo /bin/sh im_ctl.sh restart
```

#### Stop the IM Server

```bash
cd /opt/portsip
sudo /bin/sh im_ctl.sh stop
```

#### Check IM Server Status

```bash
cd /opt/portsip
sudo /bin/sh im_ctl.sh status
```

#### Remove the IM Server

```bash
cd /opt/portsip
sudo /bin/sh im_ctl.sh rm
```

***

### Managing the Data Flow Server

The Data Flow server is managed independently using a dedicated control script.

#### Start the Data Flow Server

```bash
cd /opt/portsip
sudo /bin/sh dataflow_ctl.sh start
```

#### Restart the Data Flow Server

```bash
cd /opt/portsip
sudo /bin/sh dataflow_ctl.sh restart
```

#### Stop the Data Flow Server

```bash
cd /opt/portsip
sudo /bin/sh dataflow_ctl.sh stop
```

#### Check Data Flow Server Status

```bash
cd /opt/portsip
sudo /bin/sh dataflow_ctl.sh status
```

#### Remove the Data Flow Server

```bash
cd /opt/portsip
sudo /bin/sh dataflow_ctl.sh rm
```

***

### Adding Servers to the Cluster

As your business grows, you may need to scale your deployment by adding additional cluster servers.

To add new servers:

1. Follow the guides [Preparing Cluster Servers](preparing-cluster-servers.md) and [Configuring Cluster Servers](configuring-cluster-servers.md) to provision and configure the new nodes.
2. Restart the **resource load balancer** and all **cluster servers**.

> **Note:**\
> The main PBX server does **not** need to be restarted when adding new cluster servers.

#### IM Server Scalability Considerations

* The IM server does **not** currently support clustered deployment.
* It is designed to run as a standalone service.
* With sufficient resources (recommended **16 CPU cores and 16 GB RAM**), a single IM server can support **up to 50,000 concurrent online users**.
* As a result, adding multiple IM servers is not supported at this time.

#### Maintenance Window Recommendation

Restarting the resource load balancer and cluster servers will temporarily impact call processing. To minimize service disruption, it is strongly recommended to perform these operations during off-peak hours, such as late night or scheduled maintenance windows.

***

### Upgrading Cluster Servers

Keeping all cluster components aligned with the latest PortSIP PBX release is essential for system stability, performance, and security.

Whenever a new PortSIP PBX version is released, **all deployed cluster servers must be upgraded accordingly**.

***

#### Upgrading the Main PBX Server

First, follow the guide [Upgrade to the Latest Version Within v22.x ](../portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22.3/upgrade-to-the-latest-version-within-v22.x-on-linux.md)to upgrade the following components:

* Main PBX server
* SBC server
* IM server
* Data Flow server

***

#### Upgrading Cluster Servers

**Step 1: Download the Latest Initialization Script**

> ❗ **Important**\
> This step is **mandatory**, don't skip this step!

```bash
sudo curl \
https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.3/init.sh \
-o init.sh
sudo /bin/sh init.sh
```

***

**Step 2: Upgrade Individual Cluster Services**

Use the `-s` parameter to specify the service type and the `-i` parameter to define the target image version.

**Media Server**

```bash
cd /opt/portsip && sudo /bin/sh cluster_ctl.sh upgrade \
-s media-server-only -i portsip/pbx:22
```

**Queue Server**

```bash
cd /opt/portsip && sudo /bin/sh cluster_ctl.sh upgrade \
-s queue-server-only -i portsip/pbx:22
```

**IVR Server**

```bash
cd /opt/portsip && sudo /bin/sh cluster_ctl.sh upgrade \
-s vr-server-only -i portsip/pbx:22
```

**Meeting Server**

```bash
cd /opt/portsip && sudo /bin/sh cluster_ctl.sh upgrade \
-s meeting-server-only -i portsip/pbx:22
```



