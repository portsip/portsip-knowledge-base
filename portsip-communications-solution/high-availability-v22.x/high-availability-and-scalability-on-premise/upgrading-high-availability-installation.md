# Upgrading High Availability Installation

{% hint style="warning" %}
Before upgrading the PBX HA, please consult with PortSIP support to ensure the versions are compatible.
{% endhint %}

Follow the steps below to upgrade your existing **PortSIP PBX High Availability (HA)** deployment.

> ⚠️ **IMPORTANT**\
> All commands in this section **must be executed on the `pbx01` node**, even if `pbx01` is **not** the current active (master) node.

### Back Up PBX Data

***

#### Step 1: Stop the PBX Service

On **`pbx01`**, stop the PBX service under HA control:

```bash
cd /opt/portsip-pbx-ha-guide && sudo /bin/bash ha_ctl.sh stop -s pbx
```

This ensures that no new data is written during the backup process.

***

#### Step 2: Identify the Current Master Node

On **`pbx01`**, determine which node is currently acting as the master:

```bash
cd /opt/portsip-pbx-ha-guide && sudo /bin/bash ha_ctl.sh master
```

**Example Output**

<figure><img src="../../../.gitbook/assets/ubuntu-ha-27.png" alt=""><figcaption></figcaption></figure>

The above output indicates that **`pbx01`** is the current master node.

***

#### Step 3: Back Up PBX Data on the Master Node

Log in to the **current master node** and back up the PBX data directory:

```
/var/lib/portsip
```

Ensure the backup is stored in a secure location with sufficient disk space, and verify that the backup completes successfully before proceeding with the upgrade.

Backing up PBX data is a **critical safety step** and must not be skipped. Once the backup is confirmed, you can safely continue with the remaining upgrade procedures.

***

### Upgrading PortSIP PBX v22.x High Availability to the Latest Version

If you are currently running **PortSIP PBX v22.x with High Availability (HA)**, follow the steps below to upgrade to the **latest available v22.x release**.

> ⚠️ **IMPORTANT**\
> All commands in this guide **must be executed on the `pbx01` node**, even if `pbx01` is **not** the current active (master) node.

***

#### Step 1: Download and Update HA Resources

First, download the latest HA resource package and update the local deployment scripts.

Run the following command **on `pbx01`**:

```bash
cd /opt && sudo rm -rf portsip-pbx-ha-guide-22-online.tar.gz && \
sudo wget -N \
https://www.portsip.com/downloads/ha/v22/portsip-pbx-ha-guide-22-online.tar.gz \
&& sudo tar xf portsip-pbx-ha-guide-22-online.tar.gz
```

This ensures that the HA deployment scripts and configuration files are updated to the latest version.

***

#### Step 2: Upgrade the PortSIP PBX Image

To upgrade the PBX itself, you must use the **new PortSIP PBX Docker image** corresponding to the latest release.

Once you have the new image name, run the following command **on `pbx01`**:

```bash
cd /opt/portsip-pbx-ha-guide/ && /bin/bash update.sh portsip/pbx:22
```

Starting from **PortSIP PBX v22.4**, you can use the **Certificate Manager** to obtain and install a **Let’s Encrypt** certificate instead of purchasing a commercial SSL certificate.

If you are upgrading from previous versions, and want to install the Certificate Manager, you can add the `-c` parameter in the command to install the Certificate Manager during upgrading.

```shellscript
cd /opt/portsip-pbx-ha-guide/ && /bin/bash update.sh portsip/pbx:22 -c
```

The upgrade process will:

* Pull the specified PBX image
* Update the PBX service under HA control
* Restart the PBX services as required

Allow the process to complete without interruption.

***

#### Post-Upgrade Recommendations

After the upgrade completes:

*   Verify the HA status:

    ```bash
    cd /opt/portsip-pbx-ha-guide && sudo /bin/bash ha_ctl.sh status
    ```
* Verify:
  * HA status
  * Call processing
  * Extensions and trunks

***

### Upgrading PortSIP PBX High Availability from v16.x to v22.x

If you are currently running **PortSIP PBX v16.x with High Availability (HA)**, follow the steps below to upgrade to **PortSIP PBX v22.x**.

> ⚠️ **IMPORTANT**\
> Before proceeding, ensure that you have **successfully backed up the PBX data directory**:
>
> ```
> /var/lib/portsip
> ```
>
> Refer to the _Back Up PBX Data_ section earlier in this guide.

***

#### Step 1: Install PortSIP PBX v22.x HA

PortSIP PBX v22.x HA **only supports Ubuntu 24.04 (64-bit)**.\
As a result, all PBX HA nodes must be rebuilt before upgrading.

Choose **one** of the following methods based on your operational requirements.

***

**Method 1: Deploy New Servers (Recommended)**

1. Prepare **three new servers** running **Ubuntu 24.04**.
2. Ensure the new servers use:
   * The **same hardware specifications**
   * The **same IP addresses**
   * The **same hostnames** as the existing v16.x HA nodes.
3. Install **PortSIP PBX v22.x HA** by following the guide: [High Availability Installation on Ubuntu](../../../v16.x-legacy/high-availability-v16.x/high-availability-for-on-premise/high-availability-installations-on-ubuntu.md).

This method minimizes risk and allows rollback by keeping the v16.x environment intact.

***

**Method 2: Rebuild Existing Servers**

1. Destroy the existing **Ubuntu 20.04** installations on the three PBX HA nodes.
2. Reinstall **Ubuntu 24.04** on all three servers.
3. Install **PortSIP PBX v22.x HA** by following the guide: [High Availability Installation on Ubuntu](../../../v16.x-legacy/high-availability-v16.x/high-availability-for-on-premise/high-availability-installations-on-ubuntu.md).

Choose the method that best fits your maintenance window and infrastructure strategy.

***

#### Step 2: Stop the PortSIP PBX v22.x HA Services

Before migrating data, stop the PBX service in the newly installed v22.x HA environment.

> ⚠️ **IMPORTANT**\
> All commands in this step **must be executed on `pbx01`**, even if it is **not** the current active (master) node.

**Stop the PBX Service**

```bash
cd /opt/portsip-pbx-ha-guide && sudo /bin/bash ha_ctl.sh stop -s pbx
```

**Verify the Current Master Node**

```bash
cd /opt/portsip-pbx-ha-guide && sudo /bin/bash ha_ctl.sh master
```

This confirms the HA state before data migration.

***

#### Step 3: Migrate PBX Data

1. Log in to the **current master node** of your **PortSIP PBX v16.x HA** environment.
2. Copy the backed-up PBX data to the v22.x HA installation:

```
/var/lib/portsip
```

Ensure that:

* File ownership and permissions are preserved
* The copy process completes successfully
* No PBX services are running during the migration

***

#### Step 4: Start PortSIP PBX v22.x HA

Start the upgraded PBX system by running the following command on **`pbx01`** (even if it is not the current active node):

```bash
cd /opt/portsip-pbx-ha-guide && sudo /bin/bash ha_ctl.sh start -s pbx
```

During startup:

* The PortSIP PBX application will **automatically upgrade the migrated data**
* Database schemas and internal data structures will be updated as required

***

#### Post-Upgrade Notes

* Always access the PBX using the **Virtual IP (VIP)** after the upgrade
* Verify:
  * HA status
  * Call processing
  * Extensions and trunks
  * Related services
* Keep the v16.x backup until the v22.x environment is fully validated

***

### Post-Upgrade Service Installation and Upgrade Guidance

After upgrading your PortSIP PBX environment, follow the appropriate guides below based on your **upgrade path and source version**.

***

#### If You Upgraded from **v16.x to the Latest v22.x**

After completing the PBX upgrade, you must install both the **IM Service** and the **Data Flow Service** by following these guides:

* [Scaling IM Server On-Premise for High Availability](scaling-im-server-on-premise-for-high-availability.md)
* [Scaling Data Flow Server On-Premise for High Availability](scaling-data-flow-server-on-premise-for-high-availability.md)
* [Upgrading the SBC Server](scaling-sbc-on-premise-for-high-availability.md#upgrading-sbc-servers)

***

#### If You Upgraded from **v22.x Earlier Than v22.3.0**

If your previous version was **earlier than v22.3.0**, only the **Data Flow Service** needs to be installed.\
Follow this guide:

* [Scaling Data Flow Server On-Premise for High Availability](scaling-data-flow-server-on-premise-for-high-availability.md)
* [Upgrading the IM Server](scaling-im-server-on-premise-for-high-availability.md#upgrading-the-im-server)
* [Upgrading the SBC Server](scaling-sbc-on-premise-for-high-availability.md#upgrading-sbc-servers)

***

#### If You Upgraded Within **v22.x from v22.3.0 or Later**

If your upgrade source version was **v22.3.0 or higher**, both services already exist and only need to be upgraded.\
Follow these guides:

* [Upgrading the IM Server](scaling-im-server-on-premise-for-high-availability.md#upgrading-the-im-server)
* [Upgrading the Data Flow Server](scaling-data-flow-server-on-premise-for-high-availability.md#upgrade-the-data-flow-server)
* [Upgrading the SBC Server](scaling-sbc-on-premise-for-high-availability.md#upgrading-sbc-servers)

***

Following the correct guide for your upgrade scenario ensures service compatibility, data integrity, and optimal performance in your PortSIP PBX High Availability environment.



