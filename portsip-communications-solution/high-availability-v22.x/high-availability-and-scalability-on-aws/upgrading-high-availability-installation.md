# Upgrading High Availability Installation

{% hint style="warning" %}
Before upgrading the PBX HA, please consult with PortSIP support to ensure the versions are compatible.
{% endhint %}

Follow the steps below to upgrade your existing PortSIP PBX High Availability (HA) deployment.

### Back Up Data

Before performing an upgrade, it is recommended to create a **snapshot of the EBS volume** used by the PortSIP PBX HA cluster. This snapshot provides a rollback point in case you need to restore the system to its previous state.

#### Create an EBS Snapshot

Follow these steps to create a snapshot:

1. Sign in to the **Amazon EC2 Console.**
2. In the navigation pane, choose **Elastic Block Store > Volumes**.
3. Locate the EBS volume used by the PBX HA cluster and select its checkbox.
4. Choose **Actions > Create snapshot**.
5. In the **Description** field, enter: **pbx-ha-volumes-backup**
6. Click **Create snapshot** to start the snapshot process.

<figure><img src="../../../.gitbook/assets/aws-ha-15.png" alt=""><figcaption></figcaption></figure>

> ❗ **Important**\
> Ensure the snapshot is created successfully before proceeding with the upgrade.\
> The snapshot allows you to roll back PBX data, including databases, recordings, and logs—if any issues occur during the upgrade process.

***

### Upgrading PortSIP PBX v22.x HA to the Latest Version

If you are currently running **PortSIP PBX v22.x** in a High Availability (HA) deployment, follow the steps below to upgrade to the **latest v22.x release**.

#### General Notes

> ❗ Important\
> All upgrade commands must be executed only on the EC2 instance with the private IP address `172.31.16.133`, even if this node is not currently the active (master) node.

***

#### Step 1: Download and Update HA Resources

First, download and update the latest HA resource package.

Run the following commands **only on EC2 instance `172.31.16.133`**:

```bash
cd /opt/ && sudo rm -rf portsip-pbx-ha-on-aws-guide-22.tar.gz && \
sudo wget -N \
https://www.portsip.com/downloads/ha/v22/portsip-pbx-ha-on-aws-guide-22.tar.gz && \
sudo tar xf portsip-pbx-ha-on-aws-guide-22.tar.gz
```

This step refreshes the HA scripts and configuration files required for the upgrade.

***

#### Step 2: Update the PBX Application

Next, update the PortSIP PBX service using the new PBX Docker image.

Run the following command **only on EC2 instance `172.31.16.133`**:

```bash
cd /opt/portsip-pbx-ha-guide/ && \
/bin/bash update.sh portsip/pbx:22
```

Starting from **PortSIP PBX v22.4**, you can use the **Certificate Manager** to obtain and install a **Let’s Encrypt** certificate instead of purchasing a commercial SSL certificate.

If you are upgrading from previous versions, and want to install the Certificate Manager, you can add the `-c` parameter in the command to install the Certificate Manager during upgrading.

```shellscript
cd /opt/portsip-pbx-ha-guide/ && \
/bin/bash update.sh portsip/pbx:22 -c
```

> ❗ **Important**\
> Do not interrupt the update process.\
> Do **not** reboot, stop services, or shut down any EC2 instances while the update is in progress.

***

#### After the Upgrade

Once the update completes:

* The HA cluster will continue running with the **latest PortSIP PBX v22.x version**
* Existing configuration and data are preserved
* No additional manual steps are required

It is recommended to verify:

* HA cluster status
* PBX access via the Elastic IP
* Call handling and failover behavior

***

### Upgrading PortSIP PBX HA v16.x to v22.x

If you are currently running **PortSIP PBX v16.x with High Availability (HA)**, follow the steps below to upgrade to the latest **PortSIP PBX v22.x** release.

Before proceeding, ensure that you have backed up all PBX data as described in the previous backup section.

PortSIP PBX v22.x HA **only supports Ubuntu 24.04 LTS**. To complete the upgrade, you must rebuild the PBX HA nodes with a new operating system and redeploy the HA cluster.

***

#### Upgrade Steps

**Step 1: Rebuild the PBX Nodes**

Destroy the existing **three PBX HA EC2 instances** running **Ubuntu 20.04**, and recreate them using: U**buntu 24.04 LTS (64-bit)**

> ❗ **Important**\
> Do **not** delete the **EBS volume** used by the existing v16.x HA deployment.\
> This volume contains all PBX data and is required for the upgrade.

***

**Step 2: Install PortSIP PBX v22.x HA**

Install PortSIP PBX v22.x HA by following the guide: [High Availability Installation on AWS](high-availability-installations-on-aws.md)

During the installation process,  ensure you mount the **EBS** volume that was used with v16.x with new HA.

> ❗ **Important**\
> You must mount the EBS volume from the **v16.x PBX HA** deployment to the new EC2 instances.\
> Failing to do so will result in data loss and prevent the upgrade from completing.

***

#### Data Upgrade Process

Once the v22.x HA cluster starts:

* The PortSIP PBX application automatically detects the existing data
* All PBX data is **upgraded automatically during the startup process**
* No manual data migration steps are required

This includes:

* PBX configuration data
* Tenants and extensions
* Call records and recordings
* Logs and media files

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

* [Scaling IM Server on AWS for High Availability](scaling-im-server-on-aws-for-high-availability.md)
* [Scaling Data Flow Server On-Premise for High Availability](scaling-data-flow-server-on-aws-for-high-availability.md)
* [Upgrading the SBC Server](scaling-sbc-on-aws-for-high-availability.md#upgrading-sbc-servers)

***

#### If You Upgraded from **v22.x Earlier Than v22.3.0**

If your previous version was **earlier than v22.3.0**, only the **Data Flow Service** needs to be installed.\
Follow this guide:

* [Scaling Data Flow Server on AWS for High Availability](scaling-data-flow-server-on-aws-for-high-availability.md)
* [Upgrading the IM Server](scaling-im-server-on-aws-for-high-availability.md#upgrading-the-im-server)
* [Upgrading the SBC Server](scaling-sbc-on-aws-for-high-availability.md#upgrading-sbc-servers)

***

#### If You Upgraded Within **v22.x from v22.3.0 or Later**

If your upgrade source version was **v22.3.0 or higher**, both services already exist and only need to be upgraded.\
Follow these guides:

* [Upgrading the Data Flow Server](scaling-data-flow-server-on-aws-for-high-availability.md#upgrading-the-data-flow-server)
* [Upgrading the IM Server](scaling-im-server-on-aws-for-high-availability.md#upgrading-the-im-server)
* [Upgrading the SBC Server](scaling-sbc-on-aws-for-high-availability.md#upgrading-sbc-servers)

***

Following the correct guide for your upgrade scenario ensures service compatibility, data integrity, and optimal performance in your PortSIP PBX High Availability environment.



