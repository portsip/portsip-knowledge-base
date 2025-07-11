# Upgrade v16.x to the Latest v22.x on Linux

{% hint style="danger" %}
**Important Notice:** After upgrading from **v16.x to v22.x**, the PortSIP Softphone will no longer be compatible with **v22.x**. To continue using your PBX seamlessly, you must install the new **PortSIP ONE** app, which is fully compatible with **v22.x**.
{% endhint %}

This guide provides step-by-step instructions for upgrading your current PortSIP PBX v16.x installation to the latest v22.x release.&#x20;

## Processing FAC Conflicts Before Upgrading

PortSIP PBX introduced new Feature Access Codes (FACs) in the following versions:

**v22.0:**

* Call Flip: `*11`
* Reset Calls: `*12`
* Clear Push Information: `*13`
* Call Transfer: `*54`

**v22.2:**

* Night Mode: `*16`
* PIN-Based Calling: `*20`
* Set Default Outbound Caller ID: `*64`

Before upgrading from **version v16.x to v22.2**, it is mandatory to verify with all tenants whether any custom FACs have been configured. **Custom FACs must not conflict with the new default codes listed above.**

{% hint style="danger" %}
If any tenant-defined FAC matches one of the new default codes, the upgrade will fail. To ensure a smooth upgrade process, review and modify any conflicting custom FACs prior to upgrading.
{% endhint %}

## Back-Up

Please follow the article [Backup and Restore: An Essential Guide](https://support.portsip.com/portsip-pbx/portsip-pbx-administration-guide/backup-and-restore) to back up the PBX and SBC.

{% hint style="info" %}
Rest assured, if all steps are followed correctly, your PBX data will remain intact throughout the upgrade process.
{% endhint %}

## Prerequisites for Upgrading from v16.x

If your current installation is running a version lower than v16.4.5, please first follow the [**Upgrading to the Latest v16.x Release**](../installation-of-portsip-pbx-v16/upgrade-portsip-pbx-to-v16.x.md) guide to complete the upgrade to v16.4.5.

Once your PBX is upgraded to the latest v16.x, follow the steps below to remove the v16.x installation before upgrading.

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

## Update the Scripts <a href="#update-the-scripts" id="update-the-scripts"></a>

Run the following commands to download the latest scripts:

```sh
cd /opt/portsip && rm -rf *.sh
```

```sh
sudo curl \
https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v22.x/init.sh  \
-o  init.sh
```

```sh
sudo /bin/sh init.sh
```

## Upgrading PBX <a href="#upgrading-pbx" id="upgrading-pbx"></a>

**Important**: If you installed the PBX High Availability, please follow the guide U[pgrading PortSIP PBX HA v16.x to v22.x](../../../high-availability-v22.x/high-availability-and-sclability-on-premise/upgrading-high-availability-installation.md#upgrading-portsip-pbx-ha-v16.x-to-v22.x) to upgrade.

This guide is only for upgrading a standalone PortSIP PBX installation from v16.x to v22.x.

Please run the following command to upgrade to the PortSIP PBX v22.x.

```sh
cd /opt/portsip && sudo /bin/sh pbx_ctl.sh upgrade -i portsip/pbx:22
```

## Upgrading SBC <a href="#upgrading-sbc" id="upgrading-sbc"></a>

If you are currently running **PortSIP SBC** v11.x and wish to upgrade to the latest version, please perform the below command:

```sh
cd /opt/portsip && sudo /bin/sh sbc_ctl.sh upgrade -i portsip/sbc:11
```

## Install IM Server <a href="#upgrading-im-server" id="upgrading-im-server"></a>

Starting with version 22.0, PortSIP PBX introduces an Instant Messaging (IM) service, offering modern features such as group chat. The IM service requires a separate installation step, as in some cases, you may also want to deploy it on a separate server for optimal performance.

Please follow the article [Installation of the PortSIP IM Server on Linux](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22/install-portsip-im-server-on-linux) to install the PortSIP IM Server for the PBX.

## Important: Upgrading from v16.x to v22.x

{% hint style="danger" %}
If you are upgrading from v16.x to v22.x, please follow these steps after your upgrade is complete.
{% endhint %}

* **Re-generate User QR Codes:**
  * Go to **Call Manager > Users** and click the **Send All Welcome Email** button. The PBX will then re-generate QR codes for all users, allowing the PortSIP ONE app to sign in successfully.
* **Reconfigure Microsoft 365 Integration:** If you have configured Microsoft 365 integration on v16.x, follow the [**Microsoft 365 Integration Guide**](../../integrations/) to configure it again:&#x20;
  * Remove all previously configured callback URIs in Microsoft 365.
  * Add the two new callback URIs as described in the guide.
* **Must log in to** the PBX web portal as a tenant administrator for each tenant and navigate to **Advanced > Contacts**. Select any contact, make an edit, and click **OK**. This action will prompt the PBX to generate an XML phone book for your IP phones, allowing them to download it successfully.
* Since SMTP server configurations have changed in v22.0, your previous settings will now be recognized as a generic email provider. Please review your configuration and update it if necessary.

