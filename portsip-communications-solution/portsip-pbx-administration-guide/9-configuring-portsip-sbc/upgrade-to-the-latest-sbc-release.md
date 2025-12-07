# Upgrade to the Latest v11.x Release

This guide is for upgrading your current PortSIP SBC installation to the latest v11.x version. Please follow the steps below to upgrade.

## Back-up

We recommend backing up your SBC data. The data file path is usually `/var/lib/portsip`. You can also back up the entire VM server or take a snapshot of the VM server.

Please follow the article [Backup and Restore: An Essential Guide](../../tutorials/backup-and-restore/).

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

## Upgrading SBC for Linux

### Update the Scripts

Run the following commands to download the latest scripts:

```sh
sudo curl \
https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.x/init.sh  \
-o  init.sh
```

```sh
sudo /bin/sh init.sh
```

### Upgrading SBC

If you are currently running **PortSIP SBC** and wish to upgrade to the latest version, please perform the following command:

```sh
cd /opt/portsip && sudo /bin/sh sbc_ctl.sh upgrade -i portsip/sbc:11
```

## Upgrading SBC for Windows

If you installed the PortSIP SBC for Windows, please follow the below steps to proceed with the upgrade.

1. We suggest backing up your SBC data. The data file path is usually `c:\programdata\portsip`. You can follow the article [Backup and Restore: An Essential Guide](../../tutorials/backup-and-restore/).&#x20;
2. Download the latest installer from the [PortSIP website](https://www.portsip.com/download-portsip-sbc).&#x20;
3. Double-click the installer to begin the installation process, and the upgrade will be performed automatically.

