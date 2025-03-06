# Upgrade to the Latest Version Within v22.x on Linux

## Back-Up

Please follow the article [Backup and Restore: An Essential Guide](https://support.portsip.com/portsip-pbx/portsip-pbx-administration-guide/backup-and-restore) to back up the PBX and SBC.

{% hint style="info" %}
Rest assured, if all steps are followed correctly, your PBX data will remain intact throughout the upgrade process.
{% endhint %}

## Upgrading within v22.x

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

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

### Upgrading PBX

{% hint style="danger" %}
**Important**: If you installed the PBX High Availability, please follow the guide [Upgrading High Availability Installation](../../../high-availability-v22.x/) to upgrade.
{% endhint %}

If you are currently running **PortSIP PBX v22.x** and want to upgrade to the latest version, run the following command:

```sh
cd /opt/portsip && sudo /bin/sh pbx_ctl.sh upgrade -i portsip/pbx:22
```

### Upgrading SBC

If you are currently running **PortSIP SBC** and wish to upgrade to the latest version, please perform the below command:

```sh
cd /opt/portsip && sudo /bin/sh sbc_ctl.sh upgrade -i portsip/sbc:11
```

### Upgrading IM Server

If your IM server is installed on the same server as the PBX, run the following command to upgrade:

```sh
cd /opt/portsip && sudo /bin/sh im_ctl.sh upgrade -i portsip/pbx:22
```

If your IM server is installed on a separate server, run the following commands to upgrade:

```sh
sudo curl \
https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.x/init.sh  \
-o  init.sh
```

```sh
sudo /bin/sh init.sh
```

```sh
cd /opt/portsip && sudo /bin/sh im_ctl.sh upgrade -i portsip/pbx:22
```

