# PortSIP PBX v12.x is EOL

{% hint style="info" %}
The PortSIP PBX v12.x is EOL, please install the [v16.x](../portsip-pbx/portsip-pbx-administration-guide/installation-of-the-portsip-pbx-beta/).
{% endhint %}

## PortSIP PBX High Availability

Make a high-availability cluster using three PortSIP PBX servers. PortSIP PBX can detect a variety of faults on one PBX server and automatically transfer control to the other server, the established calls will be recovered automatically.

**Figure 1-1**   PortSIP PBX HA Architecture

![](../.gitbook/assets/pbx\_ha\_diagram.png)



The Pacemaker is a high availability Cluster Resource Manager (CRM) that can be used to manage resources, and ensure that they remain available in the event of a node failure.

[DRBD ](https://linbit.com/drbd/)is used for High Availability purposes. It is a software product used to replicate data in real time from one server to others. This ensures business continuity even in the event of hardware failure.

The PortSIP PBX HA using the [Pacemaker](http://www.clusterlabs.org/) to do the resource management and monitoring, once the event of PBX node failure, the resources will automatically move to a working node in the cluster.&#x20;

The DRBD is utilized in the PortSIP HA scenario to synchronize data (DB, recording files, log files, and prompt files) between the PBX nodes.

To connect to the PBX service, all SIP clients (IP Phone, Softphone, Mobile App, WebRTC Client) will access the Virtual IP of PortSIP PBX in the HA scenario.

