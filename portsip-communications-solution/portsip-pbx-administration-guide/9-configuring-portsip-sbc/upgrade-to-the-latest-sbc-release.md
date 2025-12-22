# Upgrade to the Latest v11.x Release

This guide explains how to upgrade your existing **PortSIP SBC** installation to the **latest v11.x release**.\
Please follow the steps below carefully to complete the upgrade.

### Back Up Before Upgrading

Before upgrading, we strongly recommend backing up your SBC data.

#### Linux

*   Default data directory:

    ```shellscript
    /var/lib/portsip
    ```
* You may also back up the **entire virtual machine** or take a **VM snapshot**.

For detailed backup instructions, refer to: [Backup and Restore: An Essential Guide](../backup-and-restore/)

> ❗**Important**\
> All commands must be executed in the following directory:
>
> ```
> /opt/portsip
> ```

***

### Upgrading PortSIP SBC on Linux

#### Step 1: Update the Management Scripts

Run the following commands to download and apply the latest PortSIP management scripts:

```bash
sudo curl \
https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.x/init.sh \
-o init.sh
sudo /bin/sh init.sh
```

***

#### Step 2: Upgrade the SBC

If PortSIP SBC is currently running and you want to upgrade it to the latest v11.x version, execute:

```bash
cd /opt/portsip && sudo /bin/sh sbc_ctl.sh upgrade -i portsip/sbc:11
```

The upgrade process may take some time.\
**Do not interrupt the process, close the terminal, or reboot the server during the upgrade.**



