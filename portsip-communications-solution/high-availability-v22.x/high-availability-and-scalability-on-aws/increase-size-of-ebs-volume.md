# Increase Size of EBS Volume

As your business grows, you may need to **increase the size of the EBS volume** used by PortSIP PBX HA. Expanding the EBS volume allows the system to support more users, handle higher call volumes, and store additional call recordings and data.

This section provides step-by-step instructions for increasing the size of an AWS EBS volume used by a PortSIP PBX HA deployment.

***

### Prerequisites

Before proceeding, ensure the following requirement is met:

* The PortSIP PBX HA cluster has been successfully deployed and is operational, as described in the article [High Availability Installations on AWS](https://support.portsip.com/portsip-communications-solution/high-availability-v22.x/high-availability-and-scalability-on-aws/high-availability-installations-on-aws).

***

### Back Up the EBS Volume

Before resizing the EBS volume, it is strongly recommended to create a **snapshot backup**. This provides a recovery point in case any issues occur during the resizing process.

#### Create an EBS Snapshot

Follow these steps to create a snapshot:

1. Sign in to the AWS EC2 Console.
2. In the navigation pane, select **Elastic Block Store > Volumes**.
3. Locate the EBS volume used by the PBX HA cluster and select its checkbox.
4. Click **Actions > Create snapshot**.
5. In the **Description** field, enter: pbx-ha-volumes-backup
6. Click **Create snapshot**.

<figure><img src="../../../.gitbook/assets/aws-ha-16.png" alt=""><figcaption></figcaption></figure>

***

### Increase the Size of the EBS Volume

The example below demonstrates how to increase the EBS volume size to **2000 GB**.

#### Modify the EBS Volume

1. Sign in to the AWS EC2 Console.
2. In the navigation pane, select **Elastic Block Store > Volumes**.
3. Locate the EBS volume used by the HA cluster and select it.
4. Click **Actions > Modify volume**.
5. On the **Modify volume** screen:
   * **Volume type:** Do not change
   * **Size:** Change to **2000 GB**
   * **IOPS:** Do not change
   * **Throughput:** Do not change
6. Click **Modify**, then click **Modify** again to confirm.

<figure><img src="../../../.gitbook/assets/aws-ha-17.png" alt=""><figcaption></figcaption></figure>

AWS will begin resizing the volume in the background. This operation typically completes without interrupting service.

***

### Extend the File System

After the EBS volume size has been increased, you must extend the file system on the PBX HA node to make use of the additional space.

Run the following command **only on the EC2 instance with private IP `172.31.16.133`**:

```bash
cd /opt/portsip-pbx-ha-guide && /bin/bash extend.sh run -s disk-only
```

> ❗ **Important**\
> This command must be executed **only on the primary PBX HA node (`172.31.16.133`)**.\
> Running it on other nodes may cause file system inconsistencies.

***

After completing these steps, the PortSIP PBX HA system will be able to use the newly expanded EBS storage without requiring downtime.



