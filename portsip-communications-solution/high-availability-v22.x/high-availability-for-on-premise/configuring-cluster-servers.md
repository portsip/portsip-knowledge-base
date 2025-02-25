# Configuring Cluster Servers

This guide explains how to configure cluster servers for a High Availability (HA) PortSIP PBX deployment, capable of supporting over 1 million users, approximately 50,000 online (registered/signed-in) users, and up to 10,000 simultaneous calls. The setup is also well-suited for high-demand scenarios, including large meetings, IVR, and call queues.

## Setup the PortSIP PBX HA

Before configuring the cluster servers, please ensure that you have completed the PBX HA installation and configuration on the **Main Server** by following the guide [High Availability Installations on Ubuntu](high-availability-installations-on-ubuntu.md).

{% hint style="danger" %}
Note: In this step, just need to install the PBX only, is no need to install the IM server at this stage, as it will be installed later in this guide.
{% endhint %}

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

## Preparing Cluster Servers

We need to prepare the Linux servers for installing the following cluster application servers:

* Media Servers
* Queue Servers
* Meeting Servers
* IVR servers
* IM Server

## **Supported Linux OS**

* Ubuntu 24.04

It only supports 64-bit OS.

{% hint style="danger" %}
All cluster servers must run the same version of the Linux OS as the PBX in High Availability (HA).
{% endhint %}

{% hint style="danger" %}
When setting up the PBX cluster, please ensure there is sufficient network speed and bandwidth between the servers. Insufficient network resources may cause the PBX to not work as expected.
{% endhint %}

{% hint style="warning" %}
We recommend allocating at least 128GB of disk space, with no need for an additional data partition.
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

In this guide, we will assume that the following servers are being installed for the cluster:

* **Main Server**: Install the PBX HA with a **virtual IP address** of **192.168.1.130**.
* **Server 1**: Install a media server with a private IP address of **192.168.1.131** and a static public IP of **104.101.137.60**.
* **Server 2**: Install a queue server with an IP address of **192.168.1.32**.
* **Server 3**: Install a meeting server with an IP address of **192.168.1.33**.
* **Server 4**: Install an IVR server with an IP address of 1**92.168.1.134**.
* **Server 5**: Install an IM server with a static private IP address of **192.168.1.135** and a static public IP address of **104.101.137.61.**

{% hint style="danger" %}
A server can only deploy one type of PortSIP server at a time. For instance, it's not allowed to deploy both the media server and queue server simultaneously on **Server 1**.
{% endhint %}

## **Set password-free login for all three servers**

The following commands provided below should only be executed on the PBX HA node "**pbx01**".

If you are prompted to choose an option (**yes/no**), please enter **yes**.

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.131
```

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.132
```

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.133
```

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub pbx@192.168.1.134
```









