# Upgrade to the Latest Version Within v22.x

### Back Up Before Upgrading

Before performing any upgrade, ensure that you have a complete backup of your PBX and SBC.

Please follow the guide [**Backup and Restore: An Essential Guide**](../../backup-and-restore/) to back up both the PortSIP PBX and PortSIP SBC.

> **Note**\
> If all upgrade steps are followed correctly, your PBX data will remain intact throughout the upgrade process.

#### Attention

* After upgrading from an earlier version to **v22.2.x**, the SBC token is automatically regenerated.\
  To ensure continued operation, you must update the SBC Web Portal with the new token.
* If you upgrade from a version earlier than **v22.3.0** to **v22.3.x**, you must install the Data Flow service after the upgrade.\
  Please follow the guide[ Install Data Flow Service](install-data-flow-service.md) to complete the installation.
* If you upgrade from a version **earlier than v22.3.0** to **v22.3.x**, the extension phone provisioning settings for the BLF keys for Change Status to **Do Not Disturb (DND)** and **Available** will be **cleared**. After the upgrade, you must **reconfigure these BLF key settings**.

<figure><img src="../../../../.gitbook/assets/lost_blf_keys.png" alt=""><figcaption></figcaption></figure>

***

### High Availability Upgrade

If your PortSIP PBX is deployed in High Availability (HA) mode, follow the guide [**Upgrading High Availability Installation**](../../../high-availability-v22.x/high-availability-and-sclability-on-premise/upgrading-high-availability-installation.md).

> ❗ **Important**\
> Do **not** use this standard upgrade procedure for HA deployments.

***

### Upgrading Within v22.x

All commands in this section must be executed in the following directory:

```bash
/opt/portsip
```

#### Update the Installation Scripts

> ❗ **Important**\
> This step is **mandatory**, don't skip this step!

Run the following commands to download and apply the latest upgrade scripts on every server!

```bash
sudo curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.3/init.sh -o init.sh
sudo /bin/sh init.sh
```

***

#### Upgrading the PBX

If you are currently running **PortSIP PBX v22.x** and want to upgrade to the latest v22.x release, run:

```bash
cd /opt/portsip && sudo /bin/sh pbx_ctl.sh upgrade -i portsip/pbx:22
```

***

#### Upgrading the SBC

If you are running **PortSIP SBC** and want to upgrade to the latest version, run:

```bash
cd /opt/portsip && sudo /bin/sh sbc_ctl.sh upgrade -i portsip/sbc:11
```

> ❗ **Important**\
> After upgrading to **v22.2.x or later** from an earlier version, the **SBC token is regenerated automatically**.\
> You must update the **SBC Web Portal** with the new token to restore SBC functionality.

***

#### Upgrading the IM Service

**IM Server Installed on the Same Server as PBX:**

Run the following command:

```bash
cd /opt/portsip && sudo /bin/sh im_ctl.sh upgrade -i portsip/pbx:22
```

***

**IM Service Installed on a Separate Server:**

On the IM server, first update the scripts:

```bash
sudo curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.3/init.sh -o init.sh
sudo /bin/sh init.sh
```

Then run the upgrade command:

```bash
cd /opt/portsip && sudo /bin/sh im_ctl.sh upgrade -i portsip/pbx:22
```

***

#### Install the Data Flow Service

If you upgrade from a version **earlier than v22.3.0** to **v22.3.x**, you must install the **PortSIP Data Flow service** after completing the PBX upgrade.

Please follow the guide [**Install Data Flow Service**](install-data-flow-service.md) to complete the installation.

***

#### Upgrade the Data Flow Service

If you are upgrading from an existing **v22.3.x** release to a **newer v22.x** version, follow the steps below to upgrade the Data Flow service.

On the **Data Flow server**, run the following commands:

```bash
sudo curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.3/init.sh -o init.sh
sudo /bin/sh init.sh
```

Then run the upgrade command:

```shellscript
cd /opt/portsip && sudo /bin/sh dataflow_ctl.sh upgrade -i portsip/pbx:22
```



