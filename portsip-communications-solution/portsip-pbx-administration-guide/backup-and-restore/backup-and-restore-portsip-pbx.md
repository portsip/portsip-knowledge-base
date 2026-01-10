# Backup and Restore PortSIP PBX

This article describes how to **back up and restore a PortSIP PBX server on Linux** to support upgrades, migrations, and disaster recovery scenarios with **no data loss**.

Backups must be stored on a **separate server or external storage device**, not on the same system used for daily operations.

All procedures in this article are performed **from the command line** and require **root-level access**.

> ❗**Important**
>
> * Backup data from a Linux PBX **can only be restored to another Linux PBX**.
> * Always validate backups by performing periodic test restores in a non-production environment.

***

### Backup Methods

#### Using Virtual Machine or Cloud Snapshots (Recommended)

If your PortSIP PBX is running in a **virtualized environment** (for example, VMware, KVM) or on a **cloud platform**, you can create a **server snapshot** using the platform’s native snapshot feature.

Snapshots provide a fast and reliable way to capture the full system state, including:

* PBX configuration
* Database data
* Application runtime state

> ❗**Best Practice**\
> Regularly test snapshot restores to ensure the PBX can be successfully recovered when required.

***

#### Restoring from a Snapshot

To restore the PBX from a snapshot, follow the restore procedure provided by your virtualization or cloud platform.

After restoring:

1. Start the PBX services.
2. Verify extension registration and call routing.
3. Confirm inbound and outbound calling behavior.

***

### Backing Up PBX Data on Linux

When installing PortSIP PBX on Linux, the `-p` parameter specifies the **parent directory** used to store PBX data.

#### Installation Examples

**PBX v22.x**

```bash
sudo /bin/sh pbx_ctl.sh \
run -p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:22
```

**PBX v16.x**

```bash
/bin/sh pbx_ctl.sh \
run -p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:16
```

The following directories must be backed up:

* `pbx` – PBX configuration and runtime data
* `postgresql` – PBX database data

***

#### Backing Up Using the Default Data Path

**PBX v22.x**

```bash
sudo mkdir -p /back/pbx-data
sudo cp -p -r /var/lib/portsip/pbx /back/pbx-data

sudo mkdir -p /back/pbx-db
sudo cp -p -r /var/lib/portsip/postgresql /back/pbx-db
```

**PBX v16.x**

```bash
mkdir -p /back/pbx-data
cp -p -r /var/lib/portsip/pbx /back/pbx-data

mkdir -p /back/pbx-db
cp -p -r /var/lib/portsip/postgresql /back/pbx-db
```

***

#### Backing Up Using a Custom Data Path

If you installed the PBX using a custom parent path such as:

```bash
-p /portsip/data
```

Back up the following directories instead:

```bash
/portsip/data/pbx
/portsip/data/postgresql
```

After completing the backup, store the data securely on a different server or external storage device.

***

### Restoring Backup Data on Linux (PBX v22.x)

#### Restore to the Same Server

**1. Stop and Remove the Existing PBX**

```bash
cd /opt/portsip && sudo /bin/sh pbx_ctl.sh stop
sudo /bin/sh pbx_ctl.sh rm

sudo rm -rf /var/lib/portsip/pbx/*
sudo rm -rf /var/lib/portsip/postgresql/*
```

**2. Restore the Backup Data**

Copy the backup data to the PBX server:

```bash
sudo mkdir -p /var/lib/portsip/pbx
sudo cp -p -r /back/pbx-data/pbx /var/lib/portsip/

sudo mkdir -p /var/lib/portsip/postgresql
sudo cp -p -r /back/pbx-db/postgresql /var/lib/portsip/
```

Ensure the restored directories and files have **UID:GID set to `888:888`**, including all subdirectories.

**3. Start the PBX with Restored Data**

```bash
sudo /bin/sh pbx_ctl.sh \
run -p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:22
```

> ❗**Note**
>
> If the server does not have a public IP address, replace `66.175.221.120` with the server’s private LAN IP.

**Result:** The PBX is fully restored on the same server.

***

#### Restore to a New Linux Server (PBX v22.x)

> ❗**Note**\
> When restoring to a new server, the PBX will be automatically upgraded to the **latest v22.x release**.

1. Prepare the new Linux server **without installing PortSIP PBX**.
2.  Copy the backup data to the new server:

    ```bash
    sudo mkdir -p /var/lib/portsip/pbx
    sudo cp -p -r /back/pbx-data/pbx /var/lib/portsip/

    sudo mkdir -p /var/lib/portsip/postgresql
    sudo cp -p -r /back/pbx-db/postgresql /var/lib/portsip/
    ```
3. Ensure permissions are set to `888:888`.
4. Install PortSIP PBX and specify the restored data path using the `-p` parameter.
5. Sign in to the PBX Web Portal and launch the **Setup Wizard**.
6. In **Step 1**, update the PBX IP address to match the new server’s IP address.

***

### Restoring Backup Data on Linux (PBX v16.x)

The restore procedure for PBX v16.x is the same as for v22.x, except that the Docker image tag is `portsip/pbx:16`.

#### Restore to the Same Server

```bash
cd /opt/portsip && /bin/sh pbx_ctl.sh stop
/bin/sh pbx_ctl.sh rm

rm -rf /var/lib/portsip/pbx/*
rm -rf /var/lib/portsip/postgresql/*
```

Restore the data and start the PBX:

```bash
/bin/sh pbx_ctl.sh \
run -p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:16
```

***

#### Restore to a New Linux Server (PBX v16.x)

> ❗**Note**\
> When restoring to a new server, the PBX will be automatically upgraded to the **latest v16.x release**.

1. Prepare the new Linux server.
2. Copy the backup data to the server.
3. Install PortSIP PBX v16.x using the restored data path (`-p`).
4. Sign in to the PBX Web Portal.
5. Launch the **Setup Wizard** and update the PBX IP address in **Step 1**.

***

### Notes and Best Practices

* **Permissions**\
  Incorrect ownership or permissions can prevent the PBX or database from starting.
* **IP Address Changes**\
  Always update the PBX IP address after restoring to a different server to avoid SIP signaling issues.
* **Backup Verification**\
  Regularly test restores to ensure backups are valid and usable.







