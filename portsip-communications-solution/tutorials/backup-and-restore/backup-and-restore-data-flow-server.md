# Backup and Restore Data Flow Server

This guide outlines the steps to back up and restore a PortSIP Data Flow server. These procedures are designed to ensure data integrity during system upgrades, migrations, or disaster recovery.

**Important Notes:**

* **Backup Storage**: Backup files and data must be stored on a separate server or external storage device, not on the Data Flow server used for daily operations.
* **Backup Validation**: Regularly test your backups by performing restores in a non-production environment to ensure their reliability.

***

### Backup Methods

#### 1. Using Virtual Machine or Cloud Snapshots (Recommended)

If your PortSIP Data Flow Server is running in a virtualized environment (e.g., VMware, KVM) or on a cloud platform, you can use the platform's native tools to create a snapshot. Snapshots capture the entire system state, providing a fast and reliable backup solution.

**Best Practice**:

* Regularly test snapshot restores to ensure the Data Flow server can be successfully recovered in case of failure.

**Restoring from a Snapshot**:

1. Follow the restore procedure provided by your virtualization or cloud platform.
2. Start the Data Flow services.
3. Verify that the Data Flow features work as expected.

***

#### 2. Backing Up Data

When you install PortSIP Data Flow, the `-p` parameter specifies the parent directory for Data Flow data storage.

#### **Installation Example**

```shell
sudo /bin/sh dataflow_ctl.sh run \
-p /var/lib/portsip/ \
-a 192.168.1.35 \
-i portsip/pbx:22 \
-x 192.168.1.20 \
-d portsip/clickhouse:25.8
```

#### **Backup Procedure** (Default Parent Path: `/var/lib/portsip`)

1.  Stop the Data Flow Server:

    ```shell
    cd /opt/portsip && sudo /bin/sh dataflow_ctl.sh stop
    ```
2.  Create the Backup Directory and Copy Data:

    ```shell
    sudo mkdir -p /back
    sudo cp -p -r /var/lib/portsip/dataflow /back/df-data
    sudo cp -p -r /var/lib/portsip/clickhouse /back/df-db
    ```
3.  Restart the Data Flow Server (if necessary):

    ```shell
    cd /opt/portsip && sudo /bin/sh dataflow_ctl.sh start
    ```

***

### Restoring Backup Data

#### 1. Restoring to the Same Server

*   Step 1: Stop the Data Flow Server and Remove Existing Data:

    ```shell
    cd /opt/portsip && sudo /bin/sh dataflow_ctl.sh stop
    sudo rm -rf /var/lib/portsip/{clickhouse,dataflow}
    ```
*   Step 2: Restore the Backup Data:

    ```shell
    sudo cp -p -r /back/df-data /var/lib/portsip/dataflow
    sudo cp -p -r /back/df-db /var/lib/portsip/clickhouse
    sudo chmod 755 /var/lib/portsip/{dataflow,clickhouse}
    sudo chown -R 888:888 /var/lib/portsip/dataflow
    sudo chown -R 101:101 /var/lib/portsip/clickhouse/*
    ```
*   Step 3: Start the Data Flow Server with Restored Data:

    ```shell
    cd /opt/portsip && sudo /bin/sh dataflow_ctl.sh start
    ```

**Expected Result**: The Data Flow server is successfully restored on the same server.

***

#### 2. Restoring to a New Server

**Note**: When restoring to a new server, the Data Flow server will be automatically upgraded to the latest version.

1. **Prepare the New Linux Server**: Ensure the new server has no previous installation of the PortSIP Data Flow Server.
2.  **Copy the Backup Data to the New Server**:

    ```shell
    sudo mkdir /var/lib/portsip
    sudo cp -p -r /back/df-data /var/lib/portsip/dataflow
    sudo cp -p -r /back/df-db /var/lib/portsip/clickhouse
    sudo chmod 755 /var/lib/portsip/{dataflow,clickhouse}
    sudo chown -R 888:888 /var/lib/portsip/dataflow
    sudo chown -R 101:101 /var/lib/portsip/clickhouse/*
    ```
3. Follow the "[Install Data Flow Service](../../portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22.3/install-data-flow-service.md)" guide to install the Data Flow server on the new server, using the restored file paths and the new IP address.

**Expected Result**: The newly installed Data Flow server successfully restores the backup data.

***

### Notes and Best Practices

1. **Permissions (888:888)**:\
   Incorrect ownership or permissions on the data folders may prevent the Data Flow server from starting correctly. Ensure that permissions are set as shown in the restore steps.
2. **IP Address Changes**:\
   Always use the new IP address when restoring the Data Flow server to a new server to avoid network issues.
3. **Backup Validation**:\
   Periodically test restores to confirm the integrity of your backups and ensure the system can be recovered when needed.
