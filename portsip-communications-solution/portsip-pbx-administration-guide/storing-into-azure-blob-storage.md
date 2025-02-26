# Storing Into Azure Blob Storage

With PortSIP PBX, you can configure your system to store call recordings and compositions directly to your own Azure Blob Storage, instead of using local disk storage. This guide will walk you through setting up your AWS account or project to leverage this feature.

**Note:** Once external Azure Blob Storage is enabled, PortSIP PBX will stop storing uploaded files (such as voice prompts, profile pictures, and audio/video recordings) on the local disk. You will then be responsible for managing the security and lifecycle of your recorded content.

Use this feature if you need to comply with regulatory requirements that prohibit reliance on third-party storage solutions.

**Warning:** Please be aware of the following considerations when configuring Azure Blob Storage for your recordings:

* Historical recordings stored on the local disk of the PBX server will no longer be accessible after Azure Blob Storage is activated. Therefore, it is recommended to configureAzure Blob Storage before making or receiving calls.
* Once the "**Azure Blob Storage**" option is activated, do not disable it. Disabling this feature will prevent the PBX from accessing historical recordings and will disrupt the process of storing new recordings on Azure Blob Storage.

## Prerequisites

* Debian 11/12, Ubuntu 22.04/24.04, 64-bit
* We recommend deploying the PortSIP PBX on Azure to get the best performance

## Step 1: Create an IAM group and user <a href="#create-an-iam-group-and-user" id="create-an-iam-group-and-user"></a>

1.

## Step 3: Modify the PortSIP PBX settings <a href="#change-the-portsip-pbx-settings" id="change-the-portsip-pbx-settings"></a>

Open the settings file:

* On Linux is  `/var/lib/portsip/pbx/system.ini`
* On Windows is  `c:/programdata/portsip/pbx/system.ini`

In the section **apigateway**, modify the value of the key **storage** to **s3** as shown below.

```
[apigateway]
storage=s3
host=localhost
port=8903
```

Edit the section **storage.s3** as shown below.

```
[storage.s3]
endpoint =  http://s3.region-code.amazonaws.com
cred_id = Access key ID
cred_secret = Secret access key
region = region-code
bucket = portsip-pbx-storage
```

* **endpoint**: This is an HTTP URL, replace the **region-code** with the actual region name. For example, if the EC2 and S3 region name is **us-west-1**, then it will be http://s3.**us-west-1**.amazonaws.com.

```
[storage.s3]
endpoint =  http://s3.us-west-1.amazonaws.com
cred_id = Access key ID
cred_secret = Secret access key
region = us-west-1
bucket = portsip-pbx-storage
```

Region names can be found via this page: [https://docs.aws.amazon.com/general/latest/gr/rande.html](https://docs.aws.amazon.com/general/latest/gr/rande.html)

{% hint style="info" %}
If your EC2 and S3 are in the China region, in the endpoint URL the **amazonaws.com** should be **amazonaws.com.cn**.
{% endhint %}

* **cred\_id**: It's the **Access key ID** of the IAM user noted whilst creating the IAM user.
* **cred\_secret**: It's the **Secret access key** of the IAM user noted whilst creating the IAM user.
* **region**: The region name (region code) of the EC2 instance and S3. For example, if the region is us-west-2 then use the actual region name to replace it.
* **bucket**: The bucket name of S3. In this example, it's **portsip-pbx-storage**.

After modifying the parameters for **AWS S3**, save the changes made to **system.ini**. You will then need to restart the PortSIP PBX for the changes to take effect.

## Step 4: Restart the PortSIP PBX

### Linux

&#x20;Restart the PBX by performing the following commands:

```
cd /opt/portsip && pbx_ctl.sh restart
```

### Windows

Restart the Windows Server directly.

