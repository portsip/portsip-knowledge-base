# Configuring Private RPS Account

### ❗ **Warning**

\
By default, when you provision IP phones and other devices using **RPS**, PortSIP PBX uses **PortSIP’s RPS account** with each supported phone vendor’s RPS service. This means the device’s provisioning information (for example, the configuration URL/link) is stored under the vendor RPS account created for PortSIP.

If you do not want your provisioning information stored under PortSIP’s vendor RPS account, configure and use **your own private RPS account** instead. For instructions, please follow this guide.

***

PortSIP PBX allows customers to configure their own **private Remote Provisioning Server (RPS)** accounts for automatic IP phone provisioning.\
Using a private RPS simplifies device management and improves security by giving you direct control over IP phone provisioning credentials and workflows.

***

### Prerequisites

* Administrator access to the PBX host
* RPS credentials provided by the IP phone vendor(s) you plan to use
* Permission to restart PBX services

***

### Step 1: Locate the Configuration File

Open the **system.ini** file on your PBX server:

* **Linux:** `/var/lib/portsip/pbx/system.ini`

***

### Step 2: Add RPS Configuration

Append the relevant sections below to **system.ini**, based on the IP phone brands you want to configure.\
Save the file after making your changes.

#### Yealink

```ini
[yealink]
accesskey_id = xxxx
accesskey_secret = xxxx
```

#### ALE (Alcatel-Lucent Enterprise)

```ini
[ale]
accesskey_id = xxxx
accesskey_secret = xxxx
```

#### Grandstream

```ini
[grandstream]
username = xxxx
password = xxxx
# The value of the password field must be: sha256(md5(password))
api_id = xxxx
api_secret = xxxx
site_id = xxxx
```

#### Fanvil

```ini
[fanvil]
username = xxxx
password = xxxx
# The value of the password field must be: md5(md5(password))
```

#### Htek

```ini
[htek]
username = xxxx
password = xxxx
```

#### Snom

```ini
[snom]
username = xxxx
password = xxxx
```

***

> **Security Note**\
> Always follow the vendor-specific password hashing requirements exactly.\
> Storing improperly hashed passwords will cause provisioning failures.

***

### Step 3: Restart the Provisioning Service

After updating the configuration file, restart the provisioning service to apply the changes.

#### Linux

1. Open a terminal.
2.  Navigate to the PortSIP directory:

    ```bash
    cd /opt/portsip
    ```
3.  Restart the provisioning service:

    ```bash
    sudo /bin/sh pbx_ctl.sh restart -s portsip.provision
    ```



