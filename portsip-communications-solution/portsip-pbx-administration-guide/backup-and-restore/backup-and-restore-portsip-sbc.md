# Backup and Restore PortSIP SBC

This article describes how to **back up and restore a PortSIP SBC server on Linux**.\
These procedures are intended to ensure **no data loss** during system upgrades, migrations, or disaster recovery scenarios.

Backup files and data **must be stored on a separate server or external storage device**, not on the SBC used for daily operations.

All procedures are performed **from the command line** and require **root-level access**.

> ❗**Important**
>
> * Linux SBC backup data can only be restored to another **Linux SBC**.
> * Always validate backups by performing periodic test restores in a non-production environment.

***

### Backup Methods

#### Using Virtual Machine or Cloud Snapshots (Recommended)

If your PortSIP SBC is running in a **virtualized environment** (such as VMware, KVM) or on a **cloud platform**, you can create a **snapshot** using the platform’s native tools.

Snapshots capture the complete system state and provide a fast and reliable backup mechanism.

> ❗**Best Practice**\
> Regularly test snapshot restores to ensure the SBC can be successfully recovered in the event of a failure.

***

#### Restoring from a Snapshot

To restore the SBC from a snapshot:

1. Follow the restore procedure provided by your virtualization or cloud platform.
2. Start the SBC services.
3. Verify SIP signaling, media traversal, and network connectivity.

***

### Backing Up SBC Data on Linux

When installing PortSIP SBC on Linux, the `-p` parameter specifies the **parent directory** used to store SBC data.

#### Installation Examples

**SBC v11.x**

```bash
sudo /bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:11
```

**SBC v10.x**

```bash
/bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```

The SBC data is stored in the following subdirectory of the parent path:

```bash
<parent_path>/sbc
```

> ❗**Important**\
> Only the `sbc` subfolder must be backed up.

***

#### Backing Up Using the Default Data Path

If the default parent path `/var/lib/portsip` is used:

**SBC v11.x**

```bash
sudo mkdir -p /back/sbc-data
sudo cp -p -r /var/lib/portsip/sbc /back/sbc-data
```

**SBC v10.x**

```bash
mkdir -p /back/sbc-data
cp -p -r /var/lib/portsip/sbc /back/sbc-data
```

***

#### Backing Up Using a Custom Data Path

If the SBC was installed with a custom parent path, for example:

```bash
-p /portsip/data
```

Back up the following directory instead:

```bash
/portsip/data/sbc
```

```bash
mkdir -p /back/sbc-data
cp -p -r /portsip/data/sbc /back/sbc-data
```

After completing the backup, store the data securely on a separate system.

***

### Restoring Backup Data on Linux (SBC v11.x)

#### Restore to the Same Server

**1. Stop the SBC and Remove Existing Data**

```bash
cd /opt/portsip && sudo /bin/sh sbc_ctl.sh stop
cd /var/lib/portsip/sbc
rm -rf *
```

**2. Restore the Backup Data**

Copy the backup data to the SBC server:

```bash
sudo mkdir -p /var/lib/portsip/sbc
sudo cp -p -r /back/sbc-data/sbc /var/lib/portsip/
```

Ensure that the restored directory, all subdirectories, and files have **UID:GID set to `888:888`**.

**3. Start the SBC with Restored Data**

```bash
sudo /bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:11
```

**Result:** The SBC is successfully restored on the same server.

***

#### Restore to a New Linux Server (SBC v11.x)

> ❗**Note**\
> When restoring to a new server, the SBC will be automatically upgraded to the **latest v11.x release**.

1. Prepare the new Linux server **without installing PortSIP SBC**.
2.  Copy the backup data to the new server:

    ```bash
    sudo mkdir -p /var/lib/portsip/sbc
    sudo cp -p -r /back/sbc-data/sbc /var/lib/portsip/
    ```
3. Ensure permissions are set to `888:888`.
4. Install PortSIP SBC v11.x and specify the restored data path using the `-p` parameter.
5. Sign in to the SBC Web Portal.
6. Navigate to **Settings > Network** and update the SBC IP addresses to match the new server.
7. Save the changes.

***

### Restoring Backup Data on Linux (SBC v10.x)

The restore procedure for SBC v10.x is identical to v11.x, except that the Docker image tag is `portsip/sbc:10`.

#### Restore to the Same Server

```bash
cd /opt/portsip && /bin/sh sbc_ctl.sh stop
cd /var/lib/portsip/sbc
rm -rf *
```

Restore the data and start the SBC:

```bash
/bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```

***

#### Restore to a New Linux Server (SBC v10.x)

> ❗**Note**\
> When restoring to a new server, the SBC will be automatically upgraded to the **latest v10.x release**.

1. Prepare the new Linux server.
2. Copy the backup data to the server.
3. Install PortSIP SBC v10.x using the restored data path (`-p`).
4. Sign in to the SBC Web Portal.
5. Go to **Settings > Network**, update the SBC IP addresses, and save the changes.

***

### Notes and Best Practices

* **Permissions (`888:888`)**\
  Incorrect ownership or permissions may prevent the SBC from starting correctly.
* **IP Address Changes**\
  Always update SBC network settings after restoring to a different server to ensure proper SIP signaling and media routing.
* **Backup Validation**\
  Periodically test restores to confirm backup integrity and recovery readiness.





