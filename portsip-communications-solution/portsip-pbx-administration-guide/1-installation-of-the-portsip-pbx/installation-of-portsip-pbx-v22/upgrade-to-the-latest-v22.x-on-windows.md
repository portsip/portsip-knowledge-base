# Upgrade to the Latest v22.x on Windows

This guide provides step-by-step instructions for upgrading your current PortSIP PBX v16.x or v22.x installation to the latest v22.x release.

## Back-Up

Please follow the article [Backup and Restore: An Essential Guide](https://support.portsip.com/portsip-pbx/portsip-pbx-administration-guide/backup-and-restore) to back up the PBX and SBC.

{% hint style="info" %}
Rest assured, if all steps are followed correctly, your PBX data will remain intact throughout the upgrade process.
{% endhint %}

## Prerequisites for Upgrading from v16.x

If your current installation is running a version lower than v16.4.4, please first follow the [**Upgrading to the Latest v16.x Release**](../installation-of-portsip-pbx-v16/upgrade-portsip-pbx-to-v16.x.md) guide to complete the upgrade to v16.4.4.

Once your PBX is upgraded to the latest v16.x, follow the steps below to remove the v16.x installation before upgrading.

{% hint style="danger" %}
After you upgraded the PortSIP PBX from v16.x to v22.x, the old PortSIP Softphone app can't be used with v22.x, you must use the new [PortSIP ONE](https://www.portsip.com/portsip-one) app with the PortSIP PBX v22.x and higher.
{% endhint %}

## Upgrading for Windows installation

If your current **PortSIP PBX v16.x** installation is on a **Windows server**, simply download the latest installer, then double-click it like i[nstall a fresh version](install-portsip-pbx-on-windows.md). The installer will automatically detect your existing installation and upgrade it to the latest version.

The following steps are only applicable if you are upgrading **PortSIP PBX** on a **Linux server**.

### Important: Upgrading from v16.x to v22.x

{% hint style="danger" %}
If you are upgrading from v16.x to v22.x, please follow these steps after your upgrade is complete.
{% endhint %}

* **Re-generate User QR Codes:**
  * Go to **Call Manager > Users** and click the **Send All Welcome Email** button. The PBX will then re-generate QR codes for all users, allowing the PortSIP ONE app to sign in successfully.
* **Reconfigure Microsoft 365 Integration:** If you have configured Microsoft 365 integration on v16.x, follow the [**Microsoft 365 Integration Guide**](../../microsoft-365-integration.md) to configure it again:&#x20;
  * Remove all previously configured callback URIs in Microsoft 365.
  * Add the two new callback URIs as described in the guide.
* **Must log in to** the PBX web portal as a tenant administrator for each tenant and navigate to **Advanced > Contacts**. Select any contact, make an edit, and click **OK**. This action will prompt the PBX to generate an XML phone book for your IP phones, allowing them to download it successfully.
* Since SMTP server configurations have changed in v22.0, your previous settings will now be recognized as a generic email provider. Please review your configuration and update it if necessary.

## Prerequisites for Upgrading within v22.x

### Upgrading for Windows installation

If your current **PortSIP PBX v22.x** installation is on a **Windows server**, simply download the latest installer, then double-click it like [install a fresh version](install-portsip-pbx-on-windows.md). The installer will automatically detect your existing installation and upgrade it to the latest version.

