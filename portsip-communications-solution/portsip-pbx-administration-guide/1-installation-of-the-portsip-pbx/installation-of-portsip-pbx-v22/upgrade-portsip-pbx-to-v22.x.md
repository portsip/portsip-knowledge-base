# Upgrade to the Latest v22.x Release

This guide provides step-by-step instructions for upgrading your current PortSIP PBX v16.x or v22.x installation to the latest v22.x release.

## Back-Up

Please follow the article [Backup and Restore: An Essential Guide](https://support.portsip.com/portsip-pbx/portsip-pbx-administration-guide/backup-and-restore) to back up the PBX and SBC.

{% hint style="info" %}
Rest assured, if all steps are followed correctly, your PBX data will remain intact throughout the upgrade process.
{% endhint %}

{% hint style="warning" %}
All commands must be executed in the **`/opt/portsip`** directory.
{% endhint %}

## Prerequisites for Upgrading from v16.x

If your current installation is running a version lower than v16.4.4, please first follow the [**Upgrading to the Latest v16.x Release**](../installation-of-portsip-pbx-v16/upgrade-portsip-pbx-to-v16.x.md) guide to complete the upgrade to v16.4.4.

Once your PBX is upgraded to the latest v16.x, follow the steps below to remove the v16.x installation before upgrading.

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

{% hint style="danger" %}
After upgrading from v16.x to v22.x, please complete the following steps.
{% endhint %}

* **Re-generate User QR Codes:**
  * Go to **Call Manager > Users** and click the **Send All Welcome Email** button. The PBX will then re-generate QR codes for all users, allowing the PortSIP ONE app to sign in successfully.
* **Reconfigure Microsoft 365 Integration:** If you have configured Microsoft 365 integration on v16.x, follow the [**Microsoft 365 Integration Guide**](../../microsoft-365-integration.md) to configure it again:&#x20;
  * Remove all previously configured callback URIs in Microsoft 365.
  * Add the two new callback URIs as described in the guide.

## Prerequisites for Upgrading within v22.x

If your current installation is already PortSIP PBX v22.x and you need to upgrade to the latest v22.x version, please follow the steps below to remove the existing v22.x installation.

### Remove the current PBX installation

#### 1: Stop PBX docker instances <a href="#step-1-stop-pbx-docker-instance" id="step-1-stop-pbx-docker-instance"></a>

Perform the following commands as root to stop the current PBX Docker instance:

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

<figure><img src="../../../../.gitbook/assets/portsip_pbx_v22_docker_image.png" alt=""><figcaption></figcaption></figure>

You can use the following command to delete Docker images by specifying the first 4 digits of the IMAGE ID for **PBX** and **Postgresql**. In this case, the IMAGE IDs are **527b** for **PBX** and **d0ad** for **Postgresql**:

```sh
docker image rm 527b d0ad 
```

#### 4: Delete the scripts <a href="#step-4-delete-the-pbx-scripts" id="step-4-delete-the-pbx-scripts"></a>

```sh
rm install_pbx_docker.sh
rm install_docker.sh
rm pbx_ctl.sh
rm im_ctl.sh
rm init.sh
```

### Remove the current SBC installation

If you installed the PortSIP SBC 11.x with the PortSIP PBX 22.x, please follow the below steps to remove it.

#### 1: Stop SBC docker instances <a href="#step-1-stop-pbx-docker-instance" id="step-1-stop-pbx-docker-instance"></a>

Perform the following commands as root to stop the current SBC Docker instance:

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
docker image list
```

You will get a similar result, as shown in the screenshot below.

<figure><img src="../../../../.gitbook/assets/portsip_sbc_v22_docker_image.png" alt=""><figcaption></figcaption></figure>

You can use the following command to delete Docker images by specifying the first 4 digits of the IMAGE ID for **SBC**. In this case, the IMAGE ID is **b6cc** for **SBC**:

```
docker image rm b6cc
```

#### 4: Delete the scripts <a href="#step-4-delete-the-pbx-scripts" id="step-4-delete-the-pbx-scripts"></a>

```sh
rm install_pbx_docker.sh
rm install_docker.sh
rm sbc_ctl.sh
```

### Remove the Separate IM Service Installation

{% hint style="warning" %}
If you installed the IM service with PBX on the same server, please ignore this section.&#x20;
{% endhint %}

If you have an IM service installed on a separate server, follow the steps below to remove it.

First, use SSH to connect to the separate IM server.&#x20;

The IM service is hosted within the PBX Docker instance and image, so it will appear as **PBX** in the following steps.

#### 1: Stop IM docker instances <a href="#step-1-stop-pbx-docker-instance" id="step-1-stop-pbx-docker-instance"></a>

Perform the following commands as root to stop the current IM Docker instance:

```sh
cd /opt/portsip
sudo /bin/sh im_ctl.sh stop
```

#### 2: Delete the IM docker instances <a href="#step-2-delete-the-pbx-docker-instance" id="step-2-delete-the-pbx-docker-instance"></a>

Perform the following command to delete the IM Docker instance:

```sh
sudo /bin/sh im_ctl.sh rm
```

#### 3: Delete the IM docker images <a href="#step-3-list-the-pbx-docker-images" id="step-3-list-the-pbx-docker-images"></a>

Perform the following command to list the IM Docker images:

```sh
docker image list
```

You will get a similar result, as shown in the screenshot below.

<figure><img src="../../../../.gitbook/assets/portsip_pbx_v22_im_docker_image.png" alt=""><figcaption></figcaption></figure>

You can use the following command to delete Docker images by specifying the first 4 digits of the IMAGE ID for **PBX** and **Postgresql**. In this case, the IMAGE IDs are **03b5** for **PBX** and **d0ad** for **Postgresql**:

```sh
docker image rm 03b5 d0ad 
```

#### 4: Delete the scripts <a href="#step-4-delete-the-pbx-scripts" id="step-4-delete-the-pbx-scripts"></a>

```sh
rm install_pbx_docker.sh
rm install_docker.sh
rm pbx_ctl.sh
rm im_ctl.sh
rm init.sh
```

## Upgrade to the Latest PortSIP PBX v22.x

To upgrade to the latest version of PortSIP PBX v22.x, simply follow the same steps as for a fresh installation. The installer will automatically handle the data upgrade process.

After removing the current installation, you can now proceed with the installation of PortSIP PBX v22.x and the Instant Messaging (IM) service.&#x20;

For detailed instructions and to complete the upgrade, please refer to the [**Installation of PortSIP PBX v22.x** ](./)Guide.

