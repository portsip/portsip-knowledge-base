# Preparing Cluster Servers

We need to prepare the Linux servers for installing the following cluster application servers:

* Media Servers
* Queue Servers
* Meeting Servers
* IVR servers

## **Supported Linux OS**

* Ubuntu 20.04, 22.04, 24.04
* Debian 11.x, 12.x

It only supports 64-bit OS.

{% hint style="danger" %}
When setting up the PBX cluster, please ensure there is sufficient network speed and bandwidth between the servers. Insufficient network resources may cause the PBX to not work as expected.
{% endhint %}

## **Preparing the Linux Host Machine for Installation**

Tasks that MUST be completed before installing cluster servers.

* **Ensure the server date-time is synced correctly**.
* If the Linux server is on a LAN, assign a **Static Private IP** address.
* For the **media server cluster**, each media server also needs a **Static Public IP** address if you want the user to call from the Internet.
* Install all available updates and service packs before installing the cluster server.
* Do not install PostgreSQL on the Server.
* Ensure that all power-saving options for your system and network adapters are disabled (by setting the system to High-Performance mode).
* Do not install TeamViewer, VPN, or similar software on the host machine.
* The server must not be installed as a DNS or DHCP server.

## **Download the  Installation Scripts**

{% hint style="info" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

Execute the below commands to download the installation scripts.

```shell
mkdir -p /opt/portsip
```

```sh
cd /opt/portsip
```

```
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/install_docker.sh \
-o install_docker.sh
```

<pre><code><strong>curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/cluster_ctl.sh \
</strong>-o cluster_ctl.sh
</code></pre>

## **Setup the Docker Environment**

Execute the below command to install the `Docker-Compose` environment. If you get the prompt likes`*** cloud.cfg (Y/I/N/O/D/Z) [default=N] ?`, enter the **Y,** and then press the **Enter** button.

```shell
/bin/sh install_docker.sh
```

