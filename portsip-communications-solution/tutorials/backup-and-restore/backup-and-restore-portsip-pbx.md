# Backup and Restore PortSIP PBX

This article provides procedures for backing up and restoring a PortSIP PBX server. The procedures are designed to ensure no data loss in upgrading and migration.

Backup files and data should be stored on a different server than the one that is used for daily running.

All procedures are performed at the command prompt and require root-level access.

## **Creating a Backup Using Snapshots**

If your PortSIP PBX is running within a virtual environment or hosted on a cloud platform, it’s likely that these platforms offer the capability to create a snapshot of the server. This is a highly recommended method for backing up your current PortSIP PBX server.

## **Restoring from a Snapshot**

Restoring your PBX from a snapshot is a straightforward process. Simply follow the steps provided by your virtual environment or cloud platform to restore the PBX from the snapshot you’ve created.

Remember, it’s always a good idea to test the restore process periodically to ensure your backups are working as expected. This will help you avoid any surprises in the event of a system failure.

## **Backing Up PBX Data**

### **Linux**&#x20;

When [installing PortSIP PBX on Linux](../../portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22/install-portsip-pbx-on-linux.md), in step 3, you typically use the following command to create the PortSIP PBX Docker instance as below:

**v22.x:**

```sh
sudo /bin/sh pbx_ctl.sh \
run -p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:22
```

**v16.x:**

```shell
/bin/sh pbx_ctl.sh \
run -p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:16
```

In the above command used to create the PortSIP PBX Docker instance, the `-p` parameter is used to specify the **parent** folder for storing the PBX data. To back up the data, simply copy the folder `/var/lib/portsip/pbx` and `/var/lib/portsip/postgresql` to another server or an external disk.

Now back up the data.

**v22.x:**

<pre class="language-bash"><code class="lang-bash"><strong>sudo mkdir -p /back/pbx-data
</strong><strong>sudo cp -p -r /var/lib/portsip/pbx /back/pbx-data
</strong><strong>sudo mkdir -p /back/pbx-db
</strong>sudo cp -p -r /var/lib/portsip/postgresql /back/pbx-db
</code></pre>

For example, if you specified the **parent**  path as `/portsip/data` using the `-p` parameter when installing the PortSIP PBX, then you need to back up the folder `/portsip/data/pbx` and `/portsip/data/postgresql`.

```sh
sudo mkdir -p /back/pbx-data
sudo cp -p -r /portsip/data/pbx /back/pbx-data
sudo mkdir -p /back/pbx-db
sudo cp -p -r /portsip/data/postgresql /back/pbx-db
```

**v16.x:**

```sh
mkdir -p /back/pbx-data
cp -p -r /var/lib/portsip/pbx /back/pbx-data
mkdir -p /back/pbx-db
cp -p -r /var/lib/portsip/postgresql /back/pbx-db
```

For example, if you specified the **parent**  path as `/portsip/data` using the `-p` parameter when installing the PortSIP PBX, then you need to back up the folder `/portsip/data/pbx` and `/portsip/data/postgresql`.

```sh
mkdir -p /back/pbx-data
cp -p -r /portsip/data/pbx /back/pbx-data
mkdir -p /back/pbx-db
cp -p -r /portsip/data/postgresql /back/pbx-db
```

After successfully backing up the data, save it safely.

### **Windows**&#x20;

When [installing PortSIP PBX on Windows](../../portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22/install-portsip-pbx-on-windows.md), in step 1, there is an option that allows you to choose the **parent** folder for storing the PBX data.&#x20;

To back up the data, copy this folder to another server or an external disk. By default, if you didn’t specify otherwise, the **parent** folder is `C:\ProgramData\PortSIP`.&#x20;

The following folders need to be copied:

* C:\ProgramData\PortSIP\pbx
* C:\ProgramData\PortSIP\postgresql
* C:\ProgramData\PortSIP\html

## **Restoring from Backup Data on Linux for PBX v22.x**

Please follow the steps below to restore the PortSIP PBX from the backup data in a Linux environment.

{% hint style="danger" %}
You can't restore the backup data of a Windows PBX to a Linux PBX.
{% endhint %}

### **Restoring from Backup Data to the Same Server**

1. Stop the current PBX and delete the PBX data.

```sh
cd /opt/portsip && sudo /bin/sh pbx_ctl.sh stop
```

```sh
sudo /bin/sh pbx_ctl.sh rm
```

<pre class="language-sh"><code class="lang-sh"><strong>sudo rm -rf /var/lib/portsip/pbx/*
</strong></code></pre>

```sh
sudo rm -rf /var/lib/portsip/postgresql/*
```

2\. Restoring a PortSIP PBX

First, copy the backup data to the PortSIP PBX server. You can use the default folder, such as `/var/lib/portsip`, or another folder of your choice.

For example:

<pre class="language-sh"><code class="lang-sh"><strong>sudo mkdir -p /var/lib/portsip/pbx
</strong><strong>sudo cp -p -r /back/pbx-data/pbx /var/lib/portsip/
</strong><strong>sudo mkdir -p /var/lib/portsip/postgresql
</strong>sudo cp -p -r /back/pbx-db/postgresql /var/lib/portsip/
</code></pre>

{% hint style="danger" %}
After copying the data, make sure that the  folder, all subfolders, and files have the correct permissions set to `888:888`.
{% endhint %}

The command below is used to create and run the PBX on a server with the IP `66.175.221.120`. If you’re running the PBX in a LAN without a public IP, replace `66.175.221.120` with the PBX server’s private LAN IP. If you copied the backup data to a folder other than `/var/lib/portsip`, make sure to use the actual folder with parameter **-p** in the commands below.

```bash
sudo /bin/sh pbx_ctl.sh \
run -p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:22
```

Congratulations! Your PBX has now been successfully restored.

### **Restoring Backup Data to a New PBX Server**

If you want to restore the backup PBX v22.x data to another new server, please follow the below steps.

**Note**, that once you successfully restore the PBX data to the new server, the PBX will be upgraded to the latest v22.x automatically.

1. prepare the new Linux server, **don't** install the PBX.&#x20;

Copy the backup data to the new server. You can use the default folder, such as `/var/lib/portsip`, or another folder of your choice.

<pre class="language-sh"><code class="lang-sh"><strong>sudo mkdir -p /var/lib/portsip/pbx
</strong><strong>sudo cp -p -r /back/pbx-data/pbx /var/lib/portsip/
</strong><strong>sudo mkdir -p /var/lib/portsip/postgresql
</strong>sudo cp -p -r /back/pbx-db/postgresql /var/lib/portsip/
</code></pre>

{% hint style="danger" %}
After copying the data, make sure that the  folder, all subfolders, and files have the correct permissions set to `888:888`.
{% endhint %}

2. Now follow the guide [Install PortSIP PBX on Linux](../../portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22/install-portsip-pbx-on-linux.md) to install a new PBX with your restored PBX data by using the **-p** parameter to specify the restored data path.

After successfully installing the PortSIP PBX, your restored PBX data now is attached to the newly installed PBX.&#x20;

After signing into the PBX web portal, launch the PortSIP PBX Setup Wizard. In step 1 of the Setup Wizard, make sure to update the PBX IP address to match the new server’s IP.

## **Restoring from Backup Data on Linux for PBX v16.x**

Please follow the steps below to restore the PortSIP PBX from the backup data in a Linux environment.

{% hint style="danger" %}
You can't restore the backup data of a Windows PBX to a Linux PBX.
{% endhint %}

### **Restoring from Backup Data to the Same Server**

1. Stop the current PBX and delete the PBX data.

```sh
cd /opt/portsip && /bin/sh pbx_ctl.sh stop
```

```sh
/bin/sh pbx_ctl.sh rm
```

<pre class="language-sh"><code class="lang-sh"><strong>rm -rf /var/lib/portsip/pbx/*
</strong></code></pre>

```sh
rm -rf /var/lib/portsip/postgresql/*
```

2\. Restoring a PortSIP PBX

First, copy the backup data to the PortSIP PBX server. You can use the default folder, such as `/var/lib/portsip`, or another folder of your choice.

For example:

<pre class="language-sh"><code class="lang-sh"><strong>mkdir -p /var/lib/portsip/pbx
</strong><strong>cp -p -r /back/pbx-data/pbx /var/lib/portsip/
</strong><strong>mkdir -p /var/lib/portsip/postgresql
</strong>cp -p -r /back/pbx-db/postgresql /var/lib/portsip/
</code></pre>

{% hint style="danger" %}
After copying the data, make sure that the  folder, all subfolders, and files have the correct permissions set to `888:888`.
{% endhint %}

The command below is used to create and run the PBX on a server with the IP `66.175.221.120`. If you’re running the PBX in a LAN without a public IP, replace `66.175.221.120` with the PBX server’s private LAN IP. If you copied the backup data to a folder other than `/var/lib/portsip`, make sure to use the actual folder with parameter **-p** in the commands below.

```bash
/bin/sh pbx_ctl.sh \
run -p /var/lib/portsip \
-a 66.175.221.120 \
-i portsip/pbx:16
```

Congratulations! Your PBX has now been successfully restored.

### **Restoring Backup Data to a New PBX Server**

If you want to restore the backup PBX v16.x data to another new server, please follow the below steps.

**Note**, that once you successfully restore the PBX data to the new server, the PBX will be upgraded to the latest v16.x automatically.

1. prepare the new Linux server, **don't** install the PBX.&#x20;

Copy the backup data to the new server. You can use the default folder, such as `/var/lib/portsip`, or another folder of your choice.

<pre class="language-sh"><code class="lang-sh"><strong>mkdir -p /var/lib/portsip/pbx
</strong><strong>cp -p -r /back/pbx-data/pbx /var/lib/portsip/
</strong><strong>mkdir -p /var/lib/portsip/postgresql
</strong>cp -p -r /back/pbx-db/postgresql /var/lib/portsip/
</code></pre>

{% hint style="danger" %}
After copying the data, make sure that the  folder, all subfolders, and files have the correct permissions set to `888:888`.
{% endhint %}

2. Now follow the guide [Installation of PortSIP PBX v16.x](../../portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v16/) to install a new PBX with your restored PBX data by using the **-p** parameter to specify the restored data path.

After successfully installing the PortSIP PBX, your restored PBX data now is attached to the newly installed PBX.&#x20;

After signing into the PBX web portal, launch the PortSIP PBX Setup Wizard. In step 1 of the Setup Wizard, make sure to update the PBX IP address to match the new server’s IP.

## **Restoring from Backup Data on Windows**

Please follow the steps below to restore the PortSIP PBX from the backup data in a Windows environment.

{% hint style="danger" %}
You can't restore the backup data of a Linux PBX to a Windows PBX.
{% endhint %}

### **1. Uninstall the Currently Running PBX Instance**

* Uninstall the PortSIP PBX from Windows
* Delete the below PBX data folders:
  * C:\ProgramData\PortSIP\pbx
  * C:\ProgramData\PortSIP\postgresql
  * C:\ProgramData\PortSIP\html
  * C:\Program Files\PortSIP\PBX

### **2. Upgrading the PortSIP PBX Installer (Optional)**

You have the option to upgrade the PortSIP PBX Installer before restoring the PortSIP PBX data. The latest PortSIP PBX installer for Windows can be downloaded from the [PortSIP website](https://www.portsip.com/download-portsip-pbx/).

### **3. Restoring a PortSIP PBX**

First, copy the backup data to the PortSIP PBX server folders:

* C:\ProgramData\PortSIP\pbx
* C:\ProgramData\PortSIP\postgresql
* C:\ProgramData\PortSIP\html

You can use the default folder as the data **parent** folder, such as `C:\ProgramData\PortSIP`, or another folder of your choice.

Next, double-click the PortSIP PBX installer to install the PortSIP PBX. In the installation UI, choose the PortSIP PBX data **parent** folder ( `C:\ProgramData\PortSIP`), then click the `Next` button. Wait for the installation to complete.&#x20;

Congratulations! Your PBX has now been successfully restored.

#### **Restoring Backup Data to a New PBX Server**

The steps for restoring backup data to a new PBX server are essentially the same.&#x20;

After signing into the PBX web portal, launch the PortSIP PBX Setup Wizard. In step 1 of the Setup Wizard, make sure to update the PBX IP address to match the new server’s IP.

