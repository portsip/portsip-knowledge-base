# Storing Into Azure Blob Storage

With [PortSIP PBX](https://www.portsip.com/portsip-pbx), you can configure the system to store call recordings and related media assets directly in your own Azure Blob Storage, instead of using local disk storage on the PBX server.

This approach is recommended for customers who require scalable storage, centralized data management, or strict compliance with regulatory and data residency requirements.

This guide walks you through the required Azure-side preparation to enable Azure Blob Storage based storage for PortSIP PBX.

***

### Important Notes

* Once **external Azure storage** is enabled, PortSIP PBX will **no longer store uploaded files on the local disk**.
* You will be fully responsible for:
  * Access control and security policies
  * Data retention and lifecycle management
  * Backup and compliance of stored media

Uploaded content affected by this change includes (but is not limited to):

* Call recordings and voicemail recordings
* Voice prompts and IVR announcements
* Queue and system audio files
* User profile images and tenant logos
* QR codes and other uploaded media assets

> ❗**Use this feature when your organization must avoid reliance on third-party hosted storage or needs full ownership and governance of recorded communications.**

***

### ⚠️ Warnings and Best Practices

Please carefully review the following considerations **before enabling Amazon S3 storage**:

1. **Irreversible Access to Historical Data**\
   Once Azure Blob Storage is enabled:
   * Existing recordings stored on the PBX local disk will no longer be accessible.
   * All previously uploaded media (queue prompts, voicemail greetings, IVR audio, system announcements, logos, profile images, and QR codes) **must be re-uploaded**.
2. **Enable Azure Blob Storage Early**\
   To avoid data migration issues, it is **strongly recommended** to configure Azure Blob Storage **immediately after completing the PBX installation**, before uploading any production media or recordings.
3. **Do Not Toggle the Storage Mode**\
   After configured the Azure Blob Storage with PortSIP PBX:
   * **Do not disable it**.
   * Disabling Azure Blob Storage will prevent access to historical recordings and disrupt the creation of new recordings.
   * The same caution applies when switching **from Azure Blob Storage back to local disk storage.**
4. We recommend deploying the PortSIP PBX on Azure to get the best performance

***

### Step 1: Create an Azure Storage Account

1. Sign in to the [Azure web portal](https://portal.azure.com/).
2. From the left-hand navigation menu, select **Storage accounts**, then click **Create**.

<figure><img src="../../.gitbook/assets/azure-storage-1.png" alt=""><figcaption></figcaption></figure>

3. On the **Basics** tab, enter a value for **Storage account name**. Make a note of this name, you will need it in a later step.
4. Under **Primary service**, select one of the following options:
   * Azure Blob Storage
   * Azure Data Lake Storage Gen2
5. Follow the on-screen instructions and select the appropriate values for the remaining fields based on your deployment requirements.
6. Click **Next**, review your configuration, and complete the storage account creation process.

<figure><img src="../../.gitbook/assets/azure-storage-2.png" alt=""><figcaption></figcaption></figure>

***

### Step 2: Create the Storage Container

Follow the steps below to create a container within the Azure Storage Account:

1. Open the **Storage account** that you created in the previous step.
2. In the **Data storage** section, select **Containers**, then click **+ Container**.

<figure><img src="../../.gitbook/assets/azure-storage-3.png" alt=""><figcaption></figcaption></figure>

3. Enter a **container name**. Make a note of this name, as it will be required in a later configuration step.
4. Click **Create** to complete the container creation.

<figure><img src="../../.gitbook/assets/azure-storage-4.png" alt=""><figcaption></figcaption></figure>

***

### Step 3: Retrieve the Access Key

1. Open the **Storage account** you created earlier.
2. In the left-hand menu, navigate to **Security + networking**, then select **Access keys**.
3. Two access keys are available. Click **Show** next to either key, then **copy the key value** for later use.

<figure><img src="../../.gitbook/assets/azure-storage-5.png" alt=""><figcaption></figcaption></figure>

> ❗**Best Practice**\
> Keep the unused key as a backup to allow seamless key rotation without service interruption.

***

### Step 4: Modify the PortSIP PBX Settings

#### Open the Configuration File

Edit the `system.ini` file on the server where PortSIP PBX is installed:

* `/var/lib/portsip/pbx/system.ini`

> ❗Ensure the file is opened with administrative/root privileges.

Locate the `[apigateway]` section and set the `storage` parameter to `azure`:

```ini
[apigateway]
storage = azure
```

***

#### Configure Azure Blob Storage Settings

Edit (or add) the `[storage.azure]` section as shown below:

```ini
[storage.azure]
account_name = <Storage account name>
account_key  = <Azure Blob Storage access key>
container    = <Container name>
```

**Parameter Descriptions**

* **account\_name:** The name of the Azure Storage Account created earlier.
* **account\_key:** The access key retrieved from the Azure Storage Account (**Access keys** section).
* **container:** The name of the Azure Blob Storage container created in the previous step.

***

#### Apply the Configuration

After completing the Azure storage configuration:

1. Save the changes to `system.ini`.
2. Restart the PortSIP PBX service for the changes to take effect.

***

### Apply the Configuration

After completing the configuration:

1. Save the changes to `system.ini`.
2. **Restart the PortSIP PBX service** to apply the new settings.

```shellscript
cd /opt/portsip && sudo /bin/sh pbx_ctl.sh restart
```

> ❗The updated Azure Blob Storage configuration will not take effect until the PBX service has been restarted.



