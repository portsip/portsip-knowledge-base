# Configuring Private RPS Account

> ❗ **Warning**

By default, with PortSIP PBX, if an IP phone is auto-provisioned via the RPS (Remote Provisioning Server), its configuration link is stored in the IP phone vendor’s RPS under the PortSIP-managed account.

**Best Practice for Security and Compliance:**

1. Contact each phone vendor to create **your own RPS account** for every phone brand you use.
2. Follow the guide: [Configuring Private RPS Account](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/4-phone-device-management/configuring-private-rps-account)
3. Once your RPS accounts are set up, when you use the RPS to auto-provision your IP Phone, the configuration link is stored in your own RPS account.

> ❗ **Strong Recommendation:**\
> Always configure your own RPS accounts to ensure secure auto-provisioning.

***

### Overview

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
rps_host = https://wwww.xxx.com
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



