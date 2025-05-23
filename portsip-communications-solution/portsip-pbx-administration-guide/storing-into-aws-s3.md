# Storing Into AWS S3

With PortSIP PBX, you can configure your system to store call recordings and compositions directly to your own Amazon Web Services (AWS) S3 bucket, instead of using local disk storage. This guide will walk you through setting up your AWS account or project to leverage this feature.

**Note:** Once external S3 storage is enabled, PortSIP PBX will stop storing uploaded files (such as voice prompts, profile pictures, and audio/video recordings) on the local disk. You will then be responsible for managing the security and lifecycle of your recorded content.

Use this feature if you need to comply with regulatory requirements that prohibit reliance on third-party storage solutions.

**Warning:** Please be aware of the following considerations when configuring Amazon S3 storage for your recordings:

* Once Amazon S3 storage is activated, historical recordings stored on the local disk of the PBX server will no longer be accessible. Additionally, previously uploaded prompt files for queues, voicemail, IVR, and other voice announcements will need to be re-uploaded. Therefore, it is recommended to configure Amazon S3 storage immediately after completing the PBX installation.
* Once the "**Store to S3**" option is activated, do not disable it. Disabling this feature will prevent the PBX from accessing historical recordings and will disrupt the process of storing new recordings on Amazon S3.

## Prerequisites

* Debian 11/12, Ubuntu 22.04/24.04, 64-bit
* PortSIP PBX deployed on AWS EC2
* AWS EC2 instance(s) located in the same region as your S3 bucket

## Step 1: Create an IAM group and user <a href="#create-an-iam-group-and-user" id="create-an-iam-group-and-user"></a>

1. Navigate to the **Identity and Access Management (IAM)** menu, select **Access Management**, and then click on the **Add User** button.&#x20;
2. Input a name for the user, such as **s3store**, select **Programmatic Access**, and then click **Next**.

![](../../.gitbook/assets/iam_s3.png)

3\. Click on the **Create group** button to create a new group.

![](../../.gitbook/assets/iam_s3_group.png)

{% hint style="info" %}
You can choose to add this user to an existing group rather than create a new group but must grant **AmazonS3FullAccess** permission to this existing group.
{% endhint %}

4\. Enter a name for the group, for example, **portsip-pbx-s3**, Choose **AmazonS3FullAccess** Policy name, and click the **Create group** button.

![](../../.gitbook/assets/iam_s3_2.png)

5. Once the group is successfully created, select it and click the **Next** button. You have the option to add tags to this user, or you can simply skip this step by clicking the **Next** button.
6. After the user is successfully added, make sure to note down the **Access Key ID** and **Secret Access Key** as shown.

![](../../.gitbook/assets/iam_s3_1.png)

## Step 2: Create an S3 bucket <a href="#create-s3-bucket" id="create-s3-bucket"></a>

* Navigate to the **Amazon S3** menu and click on the **Create Bucket** button to establish the S3 service that PortSIP PBX will utilize for storing recording files. Please pay attention to the **Buket name**, **AWS Region**, and the **Object Ownership** as the below screenshot.&#x20;
* The AWS Region must be chosen as the same region with your PBX installed. Remember to make note of the following, which you will need later:
  * The _bucket-name_.&#x20;
  * The _bucket-region_. This is the AWS region where your S3 bucket is located.
* Click on the **Create** button then the S3 bucket will be created.

<figure><img src="../../.gitbook/assets/aws3-1.png" alt=""><figcaption></figcaption></figure>

## Step 3: Modify the PortSIP PBX settings <a href="#change-the-portsip-pbx-settings" id="change-the-portsip-pbx-settings"></a>

Open the settings file:

* On Linux is  `/var/lib/portsip/pbx/system.ini`
* On Windows is  `c:/programdata/portsip/pbx/system.ini`

In the section **apigateway**, modify the value of the key **storage** to **s3** as shown below.

```
[apigateway]
storage=s3
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

### Configuring S3 Path Style

Amazon S3 supports two URL styles for accessing stored objects: **Path-Style** and **Virtual-Hosted–Style**. By default, PortSIP PBX uses **Path-Style** URLs to ensure compatibility with certain AWS S3-compatible private storage services.

According to AWS best practices, **Virtual-Hosted–Style** is the preferred URL format. If you wish to configure PortSIP PBX to use **Virtual-Hosted–Style** with AWS S3, you can add a line into the **\[storage.s3]** section as below:

* To use Path-Style (default, recommended for S3-compatible services):  \
  `path_style = true`
* To use Virtual-Hosted–Style (recommended for official AWS S3):  \
  `path_style = false`&#x20;

After modifying the parameters for **AWS S3**, save the changes made to **system.ini**. You will then need to restart the PortSIP PBX for the changes to take effect.

## Step 4: Restart the PortSIP PBX

### Linux

&#x20;Restart the PBX by performing the following commands:

```sh
cd /opt/portsip && sudo /bin/sh pbx_ctl.sh restart
```

### Windows

Restart the Windows Server directly.





