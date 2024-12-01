# Upgrade to the Latest v16.x Release

This guide provides step-by-step instructions for upgrading your current PortSIP PBX **v16.x** installation to the latest release within the **v16.x** series. Before beginning, please ensure that your existing installation is version **16.x**.

## Important

> This guide is specifically for upgrades within the **v16.x** series. If you need to upgrade from v16.x to v22.x, please refer to the [Upgrading to the Latest v22.x Release](../installation-of-portsip-pbx-v22/upgrade-portsip-pbx-to-v22.x.md).

If you installed the PortSIP SBC, please follow this article to [upgrade the PortSIP SBC](broken-reference).

## **Upgrading PBX for Linux**

We recommend backing up your PBX data. The data file path is usually `/var/lib/portsip`. You can also back up the entire VM server or take a snapshot of the VM server.

Please follow the article [Backup and Restore: An Essential Guide](../../backup-and-restore/).

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

### Step 1 Stop PBX Docker Instance

Perform the following commands as root to stop the current PBX Docker instance:

{% hint style="danger" %}
You must use the su - rather than su root
{% endhint %}

```sh
su -
cd /opt/portsip && /bin/sh pbx_ctl.sh stop
```

### Step 2 Delete the PBX Docker Instance

Perform the following command to delete the PBX Docker instance:

```sh
/bin/sh pbx_ctl.sh rm
```

### Step 3 List the PBX Docker Images

Perform the following command to list the PBX Docker images:

```sh
docker image list
```

You will get a similar result shown in the below screenshot.

<figure><img src="../../../../.gitbook/assets/docker_image.png" alt=""><figcaption></figcaption></figure>

#### Delete the PBX Docker Images

You can use the following command to delete Docker images using the first 4 digits of the IMAGE ID for `PBX` and `Postgresql`, in this case, is `03b8` and `d569`.

```sh
docker image rm 03b8 d569 
```

### Step 4 Delete the PBX Scripts

```sh
rm install_pbx_docker.sh
rm install_docker.sh
rm pbx_ctl.sh
```

### Step 5 **Download the  Latest Installation Scripts**

```sh
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/install_docker.sh \
-o install_docker.sh
```

```sh
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/pbx_ctl.sh \
-o pbx_ctl.sh
```

### Step 6 **Setup the Docker Environment**

Execute the below command to install the `Docker-Compose` environment. If you get the prompt likes`*** cloud.cfg (Y/I/N/O/D/Z) [default=N] ?`, enter the **Y** and then press the **Enter** button.

```sh
/bin/sh install_docker.sh
```

### Step 7 Create and Run the PortSIP PBX Docker Container Instance

The below command is used to create and run the PBX on a server whose IP is`66.175.221.120`. If running the PBX in a LAN without the public IP, replace the IP `66.175.221.120` with the PBX server's LAN private IP.

{% hint style="danger" %}
Please replace the IP address 66.175.221.120 in the command below with your own PBX server’s IP address.
{% endhint %}

{% hint style="info" %}
Please be patient during this step, especially if your PBX contains a large amount of data. Do not cancel the process.
{% endhint %}

```sh
/bin/sh pbx_ctl.sh \
run -p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:16
```

Your PBX has now been successfully upgraded to the latest version.

## Upgrading Cluster Servers

{% hint style="danger" %}
It’s crucial to keep your cluster servers updated in line with the latest PortSIP PBX releases. This ensures that all features function as expected and that your system maintains optimal performance and security.
{% endhint %}

If you have set up your PBX as a cluster following [this guide](../../../pbx-cluster/), it’s mandatory to upgrade those servers whenever the PBX is updated. Please refer to the article [Managing Cluster](../../../pbx-cluster/managing-cluster.md#upgrading-servers) for the upgrade process.

## Upgrading PBX for Windows

1. We suggest backing up your PBX data. The data file path is usually `c:\programdata\portsip`. You can follow the article [Backup and Restore: An Essential Guide](../../backup-and-restore/).
2. Download the latest installer from the [PortSIP website](https://www.portsip.com/download-portsip-pbx/).&#x20;
3. Double-click the installer to install it and the upgrade will be performed automatically.



