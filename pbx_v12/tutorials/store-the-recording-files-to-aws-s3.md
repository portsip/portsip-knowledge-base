# Store the recording files to AWS S3

PortSIP PBX supports the storing of recorded files using AWS S3.

**Warning**: After setting up the PBX to store recording files using S3:

1. The historic recordings in the PBX server local disk will no longer be able accessed, therefore, suggest setting up the S3 storing before starting to make & receive calls.
2. Do not switch back to ‘**disable store to S3**‘ as this will prevent PBX access to historic recordings and stop new recordings from being stored to S3.

​ Whichever S3 storage mode you subsequently set via the PBX will not affect existing files stored.

{% hint style="info" %}
&#x20;If upgraded from the previous versions, the existing recording files still can be accessed after setting up the S3 storing.
{% endhint %}



### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* CentOS 7.9 / Debian 10.x / Ubuntu 18.04 or 20.06, 64 bit
* PortSIP PBX deployed on AWS EC2
* AWS EC2 instance(s) located within the same region as S3

### Example <a href="#example" id="example"></a>

#### Create an IAM group and user <a href="#create-an-iam-group-and-user" id="create-an-iam-group-and-user"></a>

1. Click the menu “**Identity and Access Management(IAM) > Access management**“, click the “**Add User**“ button.
2. Enter a name for the “**User Name**“ for example “**s3store**“, choose “**Programmatic access**“, then click “**Next**“.

![](../../.gitbook/assets/iam\_s3.png)

3\. Click the “**Create group**“ button to create a group.

![](../../.gitbook/assets/iam\_s3\_group.png)

**Note**: you can choose to add this user to an existing group rather than create a new group, but must grant the **AmazonS3FullAccess** permission to this existing group.



4\. Enter a name for the group, for example, “**portsip-pbx-s3**“, Choose “**AmazonS3FullAccess**“ Policy name, click the “**Create Group**“ button.

![](../../.gitbook/assets/iam\_s3\_2.png)

5\. After the group successfully created, choose it and click the Next button. You can add the tags to this user, or simply ignore them by click the “**Next**“ button.

6\. After the user successfully added, note the “**Access key ID**“ and “**Secret access key**“ as below.

![](../../.gitbook/assets/iam\_s3\_1.png)

#### Create S3 bucket <a href="#create-s3-bucket" id="create-s3-bucket"></a>

1. Click the menu _Amazon S3_ , click “**Create bucket**“ button to create the S3 service that PortSIP PBX will use for recording files.
2. After successfully creating the S3, note the bucket name, for example “**portsippbxstore**“.

{% hint style="info" %}
Please choose the same region of your PortSIP PBX EC2 instance to create the S3 bucket.
{% endhint %}



#### Change the PortSIP PBX settings <a href="#change-the-portsip-pbx-settings" id="change-the-portsip-pbx-settings"></a>

**Linux**: open the file

```
/var/lib/portsip/system.ini
```

**Windows**: open the file

```
c:/programdata/portsip/system.ini
```

Edit the section “**filegate**“ , change the value of key “**backend**“ to “**s3**“ as below.

```
[filegate]
backend=s3
host=localhost
port=8903
```

Edit the section “**S3**“ as below.

```
[s3]
endpoint =  http://s3.region-code.amazonaws.com
cred_id = Access key ID
cred_secret = Secret access key
region = region-code
bucket = portsippbxstore
```

**endpoint**: this is an HTTP URL, replace the “region-code“ with the actual region name, for example, if the EC2 and S3 region name is “**us-west-2**“, then it will be:

```
[s3]
endpoint =  http://s3.us-west-2.amazonaws.com
cred_id = Access key ID
cred_secret = Secret access key
region = us-west-2
bucket = portsippbxstore
```



Region name’s can be found via this page: [https://docs.aws.amazon.com/general/latest/gr/rande.html](https://docs.aws.amazon.com/general/latest/gr/rande.html)

If your EC2 and S3 in the China region, in the endpoint URL the **amazonaws.com** should be **amazonaws.com.cn**.

**cred\_id**: it’s the “**Access key ID**“ of the IAM user noted whilst creating the IAM user.

**cred\_secret**: it’s the “**Secret access key**“ of the IAM user noted whilst creating the IAM user.

**region**: the region name (region code) of the EC2 instance and S3, for example, if the region is us-west-2 then use the actual region name to replace it.

**bucket**: the bucket name of S3.

#### Restart the PortSIP PBX <a href="#restart-the-portsip-pbx" id="restart-the-portsip-pbx"></a>

After set the parameters for the S3, save the **system.ini**, then restart the PortSIP PBX.\
After setting up the parameters for the S3, save the _system.ini_ and then restart the PortSIP PBX.

**Linux**:

```
sudo docker stop -t 120 portsip-pbx
sodo docker start portsip-pbx
```

**Windows**:

Restart the Windows Server directly.





