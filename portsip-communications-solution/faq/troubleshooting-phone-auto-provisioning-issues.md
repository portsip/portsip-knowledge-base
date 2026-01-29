# Troubleshooting Phone Auto-Provisioning Issues

This article provides solutions to common PortSIP PBX issues with phone auto-provisioning.

### 1. Troubleshooting RPS Auto-Provisioning Issues

When your PortSIP PBX is deployed in the cloud, IP phone auto-provisioning via RPS (Redirect and Provisioning Service) may occasionally fail. This section describes common causes and how to resolve them.

***

#### Check the Save to RPS Option

Because the PBX is hosted in the cloud, **RPS must be used** to complete phone auto-provisioning. To ensure successful provisioning, you must enable the **Save to RPS** option when configuring the phone.

If this option is not enabled:

* The phone will **not be registered with the vendor’s RPS system**
* The provisioning request will fail, even if all other settings are correct

<figure><img src="../../.gitbook/assets/provision_2.png" alt=""><figcaption></figcaption></figure>

> **Note:** This setting is mandatory for cloud-hosted PBX deployments.

For detailed step-by-step instructions, refer to: [Provision Phone Using RPS](../portsip-pbx-administration-guide/4-phone-device-management/provision-phone-using-rps.md).

Ensure that **Save to RPS** is turned on, as shown in the configuration screen.

***

#### Check Whether the Phone Was Previously Provisioned by Another PBX

Before using RPS auto provisioning with PortSIP PBX, verify that the target IP phone has not been previously provisioned by another PBX using RPS.

If a phone is already registered in a vendor’s **RPS** under a different PBX, **auto provisioning with PortSIP PBX will fail**.

**What to Verify**

* Confirm that the phone is **not currently associated with another PBX** in the vendor’s RPS system.
* Check the RPS configuration provided by the phone vendor (for example, Yealink, Fanvil, Grandstream).

**How to Resolve**

If the phone was previously provisioned by another PBX using RPS:

1. **Delete or unbind the phone** from the existing RPS configuration in the other PBX.
2. Ensure the phone is fully removed from the vendor’s RPS platform.
3. Retry **RPS auto provisioning** with PortSIP PBX.

Failure to remove the existing RPS entry will prevent PortSIP PBX from successfully provisioning the device.

