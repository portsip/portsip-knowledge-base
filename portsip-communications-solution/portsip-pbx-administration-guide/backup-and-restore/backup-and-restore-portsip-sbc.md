# Backup and Restore PortSIP SBC

This article provides procedures for backing up and restoring a PortSIP SBC server. The procedures are designed to ensure no data loss in upgrading and migration.

Backup files and data should be stored on a different server than the one that is used for daily running.

All procedures are performed at the command prompt and require root-level access.

## **Creating a Backup Using Snapshots**

If your PortSIP SBC is running within a virtual environment or hosted on a cloud platform, it’s likely that these platforms offer the capability to create a snapshot of the server. This is a highly recommended method for backing up your current PortSIP SBC server.

## **Restoring from a Snapshot**

Restoring your SBC from a snapshot is a straightforward process. Simply follow the steps provided by your virtual environment or cloud platform to restore the SBC from the snapshot you’ve created.

Remember, it’s always a good idea to test the restore process periodically to ensure your backups are working as expected. This will help you avoid any surprises in the event of a system failure.

## **Backing Up SBC  Data**

### **Linux**&#x20;

When [installing PortSIP SBC](../9-configuring-portsip-sbc/installation-portsip-sbc-v11.x.md) on Linux, you typically use the following command to create the PortSIP SBC Docker instance:

```bash
/bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```

In the above command used to create the PortSIP SBC Docker instance, the `-p` parameter is used to specify the **parent** folder for storing the SBC data. To back up the data, simply copy the folder `/var/lib/portsip/sbc` to another server or an external disk.

Please note that it’s important to back up the `sbc`subfolder of the **parent** folder`/var/lib/portsip`.

```sh
mkdir -p /back/sbc-data
cp -p -r /var/lib/portsip/sbc /back/sbc-data
```

For example, if you specified the **parent**  path as `/portsip/data` using the `-p` parameter when installing the PortSIP SBC, then you need to back up the folder `/portsip/data/sbc`.

```sh
mkdir -p /back/sbc-data
cp -p -r /portsip/data/sbc /back/sbc-data
```

### **Windows**&#x20;

When [installing PortSIP SBC](../9-configuring-portsip-sbc/installation-portsip-sbc-v11.x.md) on Windows, in step 1, there is an option that allows you to choose the **parent** folder for storing the SBC data.&#x20;

To back up the data, simply copy the data folder to another server or an external disk. By default, if you didn’t specify otherwise, the SBC data **parent** folder is `C:\ProgramData\PortSIP`. The following folder needs to be copied:

* C:\ProgramData\PortSIP\sbc

## **Restoring from Backup Data on Linux**

Please follow the steps below to restore the PortSIP SBC from the backup data in a Linux environment.

{% hint style="danger" %}
You can't restore the backup data of a Windows SBC to a Linux SBC.
{% endhint %}

### **1. Stop the Currently Running SBC Instance**

Execute the following commands to stop the SBC and delete the data:

```bash
cd /opt/portsip && /bin/sh sbc_ctl.sh stop
/bin/sh sbc_ctl.sh rm
cd /var/lib/portsip/sbc
rm -rf *
```

### **2. Upgrade the PortSIP SBC Docker Image (Optional)**

You have the option to upgrade the PortSIP SBC Docker image before restoring the PortSIP SBC data.

First, list the SBC Docker images with the following command:

```bash
docker image list
```

You will get the result shown in the below screenshot.

<figure><img src="../../../.gitbook/assets/sbc_image.png" alt=""><figcaption></figcaption></figure>

You can then delete Docker images using the first 4 digits of the IMAGE ID for SBC:

```bash
docker image rm 8173 
```

#### **Update the PortSIP SBC Scripts**

Next, delete the existing PortSIP SBC scripts:

```bash
rm install_sbc_docker.sh
rm install_docker.sh
rm sbc_ctl.sh
```

Download the latest installation scripts:

```bash
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/install_docker.sh -o install_docker.sh
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/new/sbc_ctl.sh -o sbc_ctl.sh
```

#### **Install the Docker-Compose Environment**

Execute the following command to install the Docker-Compose environment. If you encounter a prompt like `*** cloud.cfg (Y/I/N/O/D/Z) [default=N] ?`, enter `Y` and then press the Enter button:

```bash
/bin/sh install_docker.sh
```

### **3. Restoring a PortSIP SBC**

First, copy the backup data to the PortSIP SBC server. You can use the default folder, such as `/var/lib/portsip` as the **parent** folde&#x72;**,** then the SBC data folder is  `/var/lib/portsip/sbc`  **,** or another folder of your choice.

For example:

<pre class="language-sh"><code class="lang-sh"><strong>mkdir -p /var/lib/portsip/sbc
</strong><strong>cp -p -r /back/sbc-data/sbc /var/lib/portsip/
</strong></code></pre>

The command below is used to create and run the SBC on a server. If you copied the backup data to a folder other than `/var/lib/portsip`, make sure to use the actual folder in the commands below.

```sh
/bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```

Congratulations! Your SBC has now been successfully restored.

#### **Restoring Backup Data to a New SBC Server**

The steps for restoring backup data to a new SBC server are essentially the same.&#x20;

After signing into the SBC web portal, go to the menu **Settings > Network** to update the SBC IP addresses with the new server's IP address then save changes.

## **Restoring from Backup Data on Windows**

Please follow the steps below to restore the PortSIP SBC from the backup data in a Windows environment.

{% hint style="danger" %}
You can't restore the backup data of a Linux SBC to a Windows SBC.
{% endhint %}

### **1. Uninstall the Currently Running SBC Instance**

* Uninstall the PortSIP SBC from Windows
* Delete the below SBC folders:
  * C:\ProgramData\PortSIP\sbc
  * C:\Program Files\PortSIP\SBC

### **2. Upgrading the PortSIP SBC Installer (Optional)**

You have the option to upgrade the PortSIP SBC Installer before restoring the PortSIP SBC data. The latest PortSIP SBC installer for Windows can be downloaded from the [PortSIP website](https://www.portsip.com/download-portsip-sbc/).

### **3. Restoring a PortSIP SBC**

First, copy the backup data to the PortSIP SBC server folders:

* C:\ProgramData\PortSIP\SBC

You can use the default folder as the data **parent folder**, such as `C:\ProgramData\PortSIP`, or another folder of your choice.

Next, double-click the PortSIP SBC installer to install the PortSIP SBC. In the installation UI, choose the PortSIP SBC data **parent folder** (`C:\ProgramData\PortSIP\`), then click the `Next` button. Wait for the installation to complete.&#x20;

Congratulations! Your SBC has now been successfully restored.

#### **Restoring Backup Data to a New SBC Server**

The steps for restoring backup data to a new SBC server are essentially the same.&#x20;

After signing into the SBC web portal, go to the menu **Settings > Network** to update the SBC IP addresses with the new server's IP address then save changes.

