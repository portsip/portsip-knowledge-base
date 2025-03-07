# Upgrade v16.x to the Latest v22.x on Linux

{% hint style="danger" %}
**Important Notice:** After upgrading from **v16.x to v22.x**, the PortSIP Softphone will no longer be compatible with **v22.x**. To continue using your PBX seamlessly, you must install the new **PortSIP ONE** app, which is fully compatible with **v22.x**.
{% endhint %}

This guide provides step-by-step instructions for upgrading your current PortSIP PBX v16.x installation to the latest v22.x release. The sections include:

## Back-Up

Please follow the article [Backup and Restore: An Essential Guide](https://support.portsip.com/portsip-pbx/portsip-pbx-administration-guide/backup-and-restore) to back up the PBX and SBC.

{% hint style="info" %}
Rest assured, if all steps are followed correctly, your PBX data will remain intact throughout the upgrade process.
{% endhint %}

## Prerequisites for Upgrading from v16.x

If your current installation is running a version lower than v16.4.4, please first follow the [**Upgrading to the Latest v16.x Release**](../installation-of-portsip-pbx-v16/upgrade-portsip-pbx-to-v16.x.md) guide to complete the upgrade to v16.4.4.

Once your PBX is upgraded to the latest v16.x, follow the steps below to remove the v16.x installation before upgrading.

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

### Remove the current PBX installation

#### 1: Stop PBX docker instances <a href="#step-1-stop-pbx-docker-instance" id="step-1-stop-pbx-docker-instance"></a>

Perform the following commands to stop the PBX Docker instance:

```sh
cd /opt/portsip
sudo /bin/sh pbx_ctl.sh stop
```

#### 2: Delete the PBX docker instances <a href="#step-2-delete-the-pbx-docker-instance" id="step-2-delete-the-pbx-docker-instance"></a>

Perform the following command to delete the PBX Docker instance:

```sh
sudo /bin/sh pbx_ctl.sh rm
```

#### 3: Delete the PBX docker images <a href="#step-3-list-the-pbx-docker-images" id="step-3-list-the-pbx-docker-images"></a>

Perform the following command to list the PBX Docker images:

```sh
docker image list
```

You will get a similar result, as shown in the screenshot below.

<figure><img src="../../../../.gitbook/assets/docker_image.png" alt=""><figcaption></figcaption></figure>

You can use the following command to delete Docker images by specifying the first 4 digits of the IMAGE ID for **PBX** and **Postgresql**. In this case, the IMAGE IDs are **03b8** for **PBX** and **d569** for **Postgresql**:

```sh
docker image rm 03b8 d569 
```

#### 4: Delete the scripts <a href="#step-4-delete-the-pbx-scripts" id="step-4-delete-the-pbx-scripts"></a>

Use the below commands to delete the current scripts.

```sh
rm install_pbx_docker.sh
rm install_docker.sh
rm pbx_ctl.sh
```

### Remove the current SBC installation

If you installed **PortSIP SBC 10.x** with PortSIP PBX v16.x, you will also need to upgrade to **v11.x** for compatibility with PortSIP PBX v22.x.&#x20;

Please follow the steps below to remove the current installation.

#### 1: Stop SBC docker instances <a href="#step-1-stop-pbx-docker-instance" id="step-1-stop-pbx-docker-instance"></a>

Perform the following commands to stop the SBC Docker instance:

```sh
cd /opt/portsip
sudo /bin/sh sbc_ctl.sh stop
```

#### 2: Delete the SBC docker instances <a href="#step-2-delete-the-pbx-docker-instance" id="step-2-delete-the-pbx-docker-instance"></a>

Perform the following command to delete the SBC Docker instance:

```sh
sudo /bin/sh sbc_ctl.sh rm
```

#### 3: Delete the SBC docker images <a href="#step-3-list-the-pbx-docker-images" id="step-3-list-the-pbx-docker-images"></a>

Perform the following command to list the SBC Docker images:

```sh
sudo docker image list
```

You will get a similar result, as shown in the screenshot below.

<figure><img src="../../../../.gitbook/assets/sbc_docker.png" alt=""><figcaption></figcaption></figure>

You can use the following command to delete Docker images by specifying the first 4 digits of the IMAGE ID for **SBC**. In this case, the IMAGE ID is **9f51** for **SBC**:

```sh
sudo docker image rm 9f51
```

#### 4: Delete the scripts <a href="#step-4-delete-the-pbx-scripts" id="step-4-delete-the-pbx-scripts"></a>

Use the below commands to delete the current scripts.

```sh
rm install_pbx_docker.sh
rm install_docker.sh
rm sbc_ctl.sh
```

You are now ready to upgrade to the latest version of PortSIP PBX v22.x.

### Important: Upgrading from v16.x to v22.x

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

## Performing the Upgrade to the Latest PortSIP PBX v22.x

You can now proceed with the upgrade after **completing the above steps** to remove the existing PortSIP PBX installation (while retaining the PBX data).

* Following the [same steps as for a **fresh PBX installation**](install-portsip-pbx-on-linux.md) to install a new PBX docker instance, the installer will automatically handle the data upgrade process, and all your data and configurations will be retained.
* Following the [same steps as for a **fresh SBC installation**](../../9-configuring-portsip-sbc/installation-portsip-sbc-v11.x.md) to install a new SBC docker instance, the installer will automatically handle the data upgrade process, and all your data and configurations will be retained.

