# Storing Into AWS S3

## Overview&#x20;

With PortSIP PBX, you can write your Video Recordings and Compositions to your own AWS (Amazon Web Services) S3 bucket, rather than a local disk. This guide explains how you can set up your own account or project to use this capability.

Note: Once you activate external S3 storage, PortSIP PBX will stop storing uploaded files (voice prompts, profile pictures, audio/video recordings) into the local disk. It will be your responsibility to manage the security and lifecycle of your recorded content.

Use this feature when you need to meet compliance requirements that exclude reliance on third-party storage.

**Warning**: Once you configure the PBX to store recording files using Amazon S3, please be aware of the following:

1. The historical recordings stored on the PBX server’s local disk will no longer be accessible. Therefore, it is recommended to set up Amazon S3 storage before you start making and receiving calls.
2. Do not turn off the **Store to S3** option once it is activated. This would prevent PBX from accessing historical recordings and interrupt the process of storing new recordings in Amazon S3.

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* CentOS 7.9, Debian 11/12, Ubuntu 18.04/20.04/22.04, 64-bit
* PortSIP PBX is deployed on AWS EC2
* AWS EC2 instance(s) located within the same region as S3

## Step 1: Create an IAM group and user <a href="#create-an-iam-group-and-user" id="create-an-iam-group-and-user"></a>

1. Navigate to the **Identity and Access Management (IAM)** menu, select **Access Management**, and then click on the **Add User** button.&#x20;
2. Input a name for the user, such as **s3store**, select **Programmatic Access**, and then click **Next**.

![](.gitbook/assets/iam\_s3.png)

3\. Click on the **Create group** button to create a new group.

![](.gitbook/assets/iam\_s3\_group.png)

{% hint style="info" %}
You can choose to add this user to an existing group rather than create a new group but must grant **AmazonS3FullAccess** permission to this existing group.
{% endhint %}

4\. Enter a name for the group, for example, **portsip-pbx-s3**, Choose **AmazonS3FullAccess** Policy name, and click the **Create group** button.

![](.gitbook/assets/iam\_s3\_2.png)

5. Once the group is successfully created, select it and click the **Next** button. You have the option to add tags to this user, or you can simply skip this step by clicking the **Next** button.
6. After the user is successfully added, make sure to note down the **Access Key ID** and **Secret Access Key** as shown.

![](.gitbook/assets/iam\_s3\_1.png)

## Step 2: Create an S3 bucket <a href="#create-s3-bucket" id="create-s3-bucket"></a>

* Navigate to the **Amazon S3** menu and click on the **Create Bucket** button to establish the S3 service that PortSIP PBX will utilize for storing recording files. Please pay attention to the **Buket name**, **AWS Region**, and the **Object Ownership** as the below screenshot.&#x20;
* The AWS Region must be chosen as the same region with your PBX installed. Remember to make note of the following, which you will need later:
  * The _bucket-name_.&#x20;
  * The _bucket-region_. This is the AWS region where your S3 bucket is located.
* Click on the **Create** button then the S3 bucket will be created.

<figure><img src=".gitbook/assets/aws3-1.png" alt=""><figcaption></figcaption></figure>

## Step 3: Modify the PortSIP PBX settings <a href="#change-the-portsip-pbx-settings" id="change-the-portsip-pbx-settings"></a>

Open the settings file:

* On Linux is  `/var/lib/portsip/pbx/system.ini;`&#x20;
* On Windows is  `c:/programdata/portsip/pbx/system.ini`

Edit the section **filegate**, modify the value of the key **backend** to **s3** as shown below.

```
[filegate]
backend=s3
host=localhost
port=8903
```

Edit the section **S3** as shown below.

```
[s3]
endpoint =  http://s3.region-code.amazonaws.com
cred_id = Access key ID
cred_secret = Secret access key
region = region-code
bucket = portsip-pbx-storage
```

* **endpoint**: This is an HTTP URL, replace the **region-code** with the actual region name. For example, if the EC2 and S3 region name is **us-west-1**, then it will be http://s3.**us-west-1**.amazonaws.com.

```
[s3]
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

