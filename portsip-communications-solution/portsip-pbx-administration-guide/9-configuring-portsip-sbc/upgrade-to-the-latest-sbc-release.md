# Upgrade to the Latest v11.x Release

This guide is for upgrading your current PortSIP SBC installation to the latest v11.x version. Please follow the steps below to upgrade.

## Back-up

We recommend backing up your SBC data. The data file path is usually `/var/lib/portsip`. You can also back up the entire VM server or take a snapshot of the VM server.

Please follow the article [Backup and Restore: An Essential Guide](../backup-and-restore/).

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

## Prerequisites for Upgrading from v10.x

If you have installed PortSIP SBC 10.x with PortSIP PBX v16.x and wish to upgrade to the latest v11.x for compatibility with PortSIP PBX v22.x, follow the steps below to remove the current installation.

### **1: Stop SBC docker instances**

Perform the following commands to stop the SBC Docker instance:

```sh
cd /opt/portsip
sudo /bin/sh sbc_ctl.sh stop
```

### **2: Delete the SBC docker instances**

Perform the following command to delete the SBC Docker instance:

```sh
sudo /bin/sh sbc_ctl.sh rm
```

### **3: Delete the SBC docker images**

Perform the following command to list the SBC Docker images:

```sh
sudo docker image list
```

You will get a similar result, as shown in the screenshot below.

<figure><img src="../../../.gitbook/assets/sbc_docker.png" alt=""><figcaption></figcaption></figure>

You can use the following command to delete Docker images by specifying the first 4 digits of the IMAGE ID for **SBC**. In this case, the IMAGE ID is **9f51** for **SBC**:

```sh
sudo docker image rm 9f51
```

### **4: Delete the scripts**

Use the below commands to delete the current scripts.

```sh
rm install_pbx_docker.sh
rm install_docker.sh
rm sbc_ctl.sh
```

You are now ready to upgrade to the latest version of PortSIP PBX v11.x.

## Prerequisites for Upgrading from v11.x

If you installed the PortSIP SBC 11.x and wish to upgrade to the latest v11.x, please follow the below steps to remove the current installation.

### **1: Stop SBC docker instances**

Perform the following commands as root to stop the current SBC Docker instance:

```sh
cd /opt/portsip
sudo /bin/sh sbc_ctl.sh stop
```

### **2: Delete the SBC docker instances**

Perform the following command to delete the SBC Docker instance:

```sh
sudo /bin/sh sbc_ctl.sh rm
```

### **3: Delete the SBC docker images**

Perform the following command to list the SBC Docker images:

```sh
docker image list
```

You will get a similar result, as shown in the screenshot below.

<figure><img src="../../../.gitbook/assets/portsip_sbc_v22_docker_image.png" alt=""><figcaption></figcaption></figure>

You can use the following command to delete Docker images by specifying the first 4 digits of the IMAGE ID for **SBC**. In this case, the IMAGE ID is **b6cc** for **SBC**:

```
docker image rm b6cc
```

### **4: Delete the scripts**

```sh
rm install_pbx_docker.sh
rm install_docker.sh
rm sbc_ctl.sh
rm im_ctl.sh
rm init.sh
```

You are now ready to upgrade to the latest version of PortSIP PBX v11.x.

## Upgrade to the Latest PortSIP SBC v11.x <a href="#upgrade-to-the-latest-portsip-pbx-v22.x" id="upgrade-to-the-latest-portsip-pbx-v22.x"></a>

After removing the current installation, proceed with the [Installation of PortSIP SBC v11.x](installation-portsip-sbc-v11.x.md), simply follow the same steps as for a fresh installation, and the installer will automatically manage the data upgrade process.&#x20;

Your SBC is now successfully upgraded to the latest version.

## Upgrading SBC for Windows

If you installed the PortSIP SBC for Windows, please follow the below steps to proceed with the upgrade.

1. We suggest backing up your SBC data. The data file path is usually `c:\programdata\portsip`. You can follow the article [Backup and Restore: An Essential Guide](../backup-and-restore/).&#x20;
2. Download the latest installer from the [PortSIP website](https://www.portsip.com/download-portsip-sbc).&#x20;
3. Double-click the installer to begin the installation process, and the upgrade will be performed automatically.

