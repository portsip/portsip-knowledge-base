# Upgrading High Availability Installation

{% hint style="info" %}
Before upgrading the PBX HA, please consult with PortSIP support to ensure the versions are compatible.
{% endhint %}

Please follow the below steps to upgrade your current PBX HA.

## Back up data

Before upgrading, you can create a snapshot of the EBS. This allows you to roll back your changes if necessary. Here are the steps to follow:

* In the Amazon EC2 console, in the navigation panel, choose **Elastic Block Store**, and select **Volumes**.
* Select the check box for your **Volume** that be used in HA, choose **Actions**, and **Create a snapshot**.
* Under Description, enter "pbx-ha-volumes-backup".
* Choose **Create Snapshot** to create a snapshot.

<figure><img src="../../../.gitbook/assets/aws-ha-15.png" alt=""><figcaption></figcaption></figure>

## Upgrading PortSIP PBX HA v16.x to v22.x

If you are currently running PortSIP PBX v16.x with High Availability (HA), follow these steps to upgrade to the latest version, v22.x.

{% hint style="danger" %}
Ensure you have backed up the current PBX data as outlined in the backup steps.
{% endhint %}

Since **PortSIP PBX v22.x HA** only supports **Ubuntu 24.04**, you must rebuild the three PBX node servers (EC2). Follow the steps below to upgrade:

* **Step 1**: Destroy the current three servers running **Ubuntu 20.04** and rebuild them with **Ubuntu 24.04**.
* **Step 2**: Install PortSIP PBX v22.x HA following the [**High Availability Installation on AWS**](high-availability-installations-on-aws.md) guide. During the installation, ensure you mount the **EBS** volume that was used with v16.x.

{% hint style="warning" %}
You must mount the **EBS** volume used by the previous **v16.x PBX** to the new EC2 server to enable data upgrading.
{% endhint %}

The PortSIP PBX application will automatically upgrade the data during the startup process.

## Upgrading PortSIP PBX v22.x HA to the Latest Version

If you are currently running the PortSIP PBX v22.x HA, please follow the below steps to upgrade it to the latest version.

{% hint style="danger" %}
Perform the below command only on the EC2 instance **ip-172-31-16-133** even if it's currently not the active node.
{% endhint %}

### 1. Download and Update Resources

Perform the below command only on the EC2 instance **ip-172-31-16-133** (even if it's currently not the active node).

```sh
cd /opt/ && rm -rf portsip-pbx-ha-on-aws-guide-22.tar.gz && \
sudo wget -N \
https://www.portsip.com/downloads/ha/v22/portsip-pbx-ha-on-aws-guide-22.tar.gz && \
sudo tar xf portsip-pbx-ha-on-aws-guide-22.tar.gz
```

### **2. Update PBX**

Use the new version image of PortSIP PBX to update the PBX.

Perform the below command only on the EC2 instance **ip-172-31-16-133 (the main node),** even if it's currently not the active node.

```sh
cd /opt/portsip-pbx-ha-guide/ && \
/bin/bash update.sh portsip/pbx:22
```

