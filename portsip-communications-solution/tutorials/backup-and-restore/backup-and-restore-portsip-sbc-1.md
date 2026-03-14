# Backup and Restore IM Server

This guide describes the steps for backing up and restoring a PortSIP IM server. These procedures are designed to ensure data integrity during system upgrades, migrations, or disaster recovery scenarios.

**Important**

* Backup files and data must be stored on a separate server or external storage device, not on the IM server used for daily operations.
* This guide is intended for cases where the IM server is installed on a separate server.
* Always validate backups by performing periodic test restores in a non-production environment.

***

### **Backup Methods**

#### **1. Using Virtual Machine or Cloud Snapshots (Recommended)**

If your PortSIP IM Server is running in a virtualized environment (e.g., VMware, KVM) or on a cloud platform, you can use the platform’s native tools to create a snapshot. Snapshots capture the entire system state, providing a fast and reliable backup solution.

**Best Practice**

* Regularly test snapshot restores to ensure the IM server can be successfully recovered in the event of a failure.

**Restoring from a Snapshot**

To restore the IM Server from a snapshot:

1. Follow the restore procedure provided by your virtualization or cloud platform.
2. Start the IM services.
3. Verify IM message delivery and network connectivity.

***

### **Backing Up IM Data**

When you install PortSIP IM, the `-p` parameter specifies the parent directory for IM data storage.

#### **Installation Example**

```bash
sudo /bin/sh im_ctl.sh run -E \
  -p /var/lib/portsip/ \
  -a 192.168.1.25 \
  -A 104.18.36.110 \
  -i portsip/pbx:22 \
  -x 192.168.1.20 \
  -t OWMWYWJKZJYTMWM2NI0ZNZJMLWJJZDKTMGVMZDYXNZU1NWI1
```

If you specify a chat file storage directory with the `-f` parameter, such as:

```bash
sudo /bin/sh im_ctl.sh run -E \
  -p /var/lib/portsip/ \
  -f /chat/files/ \
  -a 192.168.1.25 \
  -A 104.18.36.110 \
  -i portsip/pbx:22 \
  -x 192.168.1.20 \
  -t OWMWYWJKZJYTMWM2NI0ZNZJMLWJJZDKTMGVMZDYXNZU1NWI1
```

When backing up, ensure that you also back up the chat files by copying the directory specified in the `-f` parameter, in this case, it's `/chat/files`.

#### **Backup Procedure (Default Parent Path: `/var/lib/portsip`)**

1. Stop the IM server:

```bash
cd /opt/portsip && sudo /bin/sh im_ctl.sh stop
```

2. Create the backup directory and copy the data:

```bash
sudo mkdir -p /back
sudo cp -p -r /var/lib/portsip/im-data /back/im-data
sudo cp -p -r /var/lib/portsip/im-postgresql /back/im-db
```

3. If the `-f` parameter was used to specify the chat file storage directory. Now, back up the chat files using the command below; otherwise, ignore this step.

```bash
sudo cp -p -r /chat/files /back/files
```

4. After completing the backup, restart the IM server if necessary:

```bash
cd /opt/portsip && sudo /bin/sh im_ctl.sh start
```

***

### **Restoring Backup Data**

#### **1. Restoring to the Same Server**

**Step 1: Stop the IM Server and Remove Existing Data**

1. Stop the IM Server:

```bash
cd /opt/portsip && sudo /bin/sh im_ctl.sh stop
```

2. Remove the existing IM data:

```bash
sudo rm -rf /var/lib/portsip/{im-data,im-postgresql}
```

3. If the `-f` parameter was used for chat file storage, then remove the chat files using the command below. Otherwise, ignore this step.

```bash
sudo rm -rf /chat/files
```

**Step 2: Restore the Backup Data**

1. Copy the backup data to the IM server:

```bash
sudo cp -p -r /back/im-data /var/lib/portsip/im-data
sudo cp -p -r /back/im-db /var/lib/portsip/im-postgresql
```

2. Set the correct permissions:

```bash
sudo chmod 755 /var/lib/portsip/{im-data,im-postgresql}
sudo chown -R 888:888 /var/lib/portsip/im-data
sudo chown -R 999:999 /var/lib/portsip/im-postgresql
```

3. If the `-f` parameter was used to specify a chat file storage directory, restore the chat data using the following command. Otherwise, you can skip this step.

```bash
sudo cp -p -r /back/files /chat/
sudo chmod 755 /chat/files
sudo chown -R 888:888 /chat/files
```

**Step 3: Start the IM Server with Restored Data**

1. Start the IM server:

```bash
cd /opt/portsip && sudo /bin/sh im_ctl.sh start
```

**Expected Result**\
The IM server is successfully restored on the same server.

***

#### **2. Restoring to a New Server**

**Note**\
When restoring to a new server, the IM server will be automatically upgraded to the latest version.

1. Prepare the new Linux server without installing PortSIP IM Server.
2. Copy the backup data to the new server:

```bash
sudo cp -p -r /back/im-data /var/lib/portsip/im-data
sudo cp -p -r /back/im-db /var/lib/portsip/im-postgresql
```

3. Set the correct permissions:

```bash
sudo chmod 755 /var/lib/portsip/{im-data,im-postgresql}
sudo chown -R 888:888 /var/lib/portsip/im-data
sudo chown -R 999:999 /var/lib/portsip/im-postgresql
```

4. If the `-f` parameter was used for chat files, restore the chat data using the following command. Otherwise, you can skip this step.

```bash
sudo cp -p -r /back/files /chat/
sudo chmod 755 /chat/files
sudo chown -R 888:888 /chat/files
```

5. Follow the "[Install IM Service](../../portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22.3/install-im-service.md)" guide to install the IM server on the new server with the new IP address and token, using the restored file paths.

**Expected Result**\
The newly installed IM server has successfully restored the backup data.

***

### **Notes and Best Practices**

**1. Permissions (888:888)**

Incorrect ownership or permissions on the data folders may prevent the IM server from starting correctly. Ensure that the permissions are set as shown in the restore steps.

**2. IP Address Changes**

Always use the new IP address and token when restoring the IM server to a new server to avoid network issues.

**3. Backup Validation**

Periodically test restores to confirm the integrity of your backups and ensure the system can be recovered when needed.

