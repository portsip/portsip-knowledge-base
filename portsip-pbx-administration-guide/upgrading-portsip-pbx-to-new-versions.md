# Upgrading PortSIP PBX to New Versions

This guide is for upgrading your current PortSIP PBX **v16.x** installation to the latest version. Please follow the steps below to upgrade.

{% hint style="info" %}
Please ensure that your current PortSIP PBX installation is version **16. x.**
{% endhint %}

## Upgrading SBC

{% hint style="danger" %}
If you upgraded the PBX to **v16.1.0**, then you must also upgrade the SBC to **v10.0.6**, and update the PBX access token as the guide.
{% endhint %}

If you installed the PortSIP SBC, please follow this article to [upgrade the PortSIP SBC](../portsip-sbc-administration-guide/upgrading-portsip-sbc-to-new-versions.md).

## Upgrading PBX for Windows

1. We suggest backing up your PBX data. The data file path is usually `c:\programdata\portsip`. You can follow the article [Backup and Restore: An Essential Guide](../backup-and-restore/).
2. Download the latest installer from the [PortSIP website](https://www.portsip.com/download-portsip-pbx/).&#x20;
3. Double-click the installer to install it and the upgrade will be performed automatically.

## **Upgrading PBX for Linux**

We recommend backing up your PBX data. The data file path is usually `/var/lib/portsip`. You can also back up the entire VM server or take a snapshot of the VM server.

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

### Step 1 Stop PBX Docker Instance

Perform the following commands as root to stop the current PBX Docker instance:

```
cd /opt/portsip && /bin/sh pbx_ctl.sh stop
```

### Step 2 Delete the PBX Docker Instance

Perform the following command to delete the PBX Docker instance:

```
/bin/sh pbx_ctl.sh rm
```

### Step 3 List the PBX Docker Images

Perform the following command to list the PBX Docker images:

```
docker image list
```

You will get the result shown in the below screenshot.

<figure><img src="../.gitbook/assets/docker_image.png" alt=""><figcaption></figcaption></figure>

#### Delete the PBX Docker Images

You can use the following command to delete Docker images using the first 4 digits of the IMAGE ID for PBX and Postgresql.

```
docker image rm 03b8 d569 
```

### Step 4 Delete the PBX Scripts

```
rm install_pbx_docker.sh
rm install_docker.sh
rm pbx_ctl.sh
```

### Step 5 **Download the  Latest Installation Scripts**

```
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/install_docker.sh \
-o install_docker.sh
```

```
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/pbx_ctl.sh \
-o pbx_ctl.sh
```

### Step 6 **Setup the Docker Environment**

Execute the below command to install the `Docker-Compose` environment. If you get the prompt likes`*** cloud.cfg (Y/I/N/O/D/Z) [default=N] ?`, enter the **Y** and then press the **Enter** button.

```
/bin/sh install_docker.sh
```

### Step 7 Create and Run the PortSIP PBX Docker Container Instance

The below command is used to create and run the PBX on a server whose IP is`66.175.221.120`. If running the PBX in a LAN without the public IP, just replace the IP `66.175.221.120` with the PBX server's LAN private IP.

{% hint style="danger" %}
Please replace the IP address 66.175.221.120 in the command below with your own PBX server’s IP address.
{% endhint %}

```
/bin/sh pbx_ctl.sh \
run -p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:16
```

Your PBX has now been successfully upgraded to the latest version.

##
