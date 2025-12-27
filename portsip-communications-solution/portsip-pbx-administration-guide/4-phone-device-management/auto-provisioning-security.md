# Auto Provisioning Security

### Introduction

**Auto-provisioning** simplifies the activation and management of IP phones by allowing devices to be configured automatically through a web-based mechanism, eliminating the need for manual phone configuration.

When a user activates a phone, the service provider or PBX automatically delivers the required configuration settings for device registration, significantly reducing deployment time and operational overhead.

***

### How Auto-Provisioning Works

#### Auto-Provisioning Overview

Auto-provisioning streamlines the phone setup process by allowing users to activate their phones via a **web interface**, avoiding the need to manually enter SIP credentials or configuration parameters on the device itself.

***

#### PBX Configuration Files

In a typical PBX implementation:

* Extension phone configuration files are stored in a designated provisioning directory.
* For example, the PBX may create a folder such as: **`ac2fbb80c5da60e`**
* Within this folder, the PBX generates **one configuration file per phone**, using the phone’s **MAC address** as the filename.

***

#### Provisioning URL Mechanism

To initiate provisioning:

1. The PBX sends a **SIP NOTIFY** message to the IP phone.
2. This message contains a **base provisioning URL**, for example:

```
https://www.pbxhost.com/provision/ac2fbb80c5da60e
```

3. The IP phone appends its own **MAC address** to the URL, producing a complete configuration file path such as:

```
https://www.pbxhost.com/provision/ac2fbb80c5da60e/cc5ef641b794.xml
```

4. The phone then downloads the configuration file and uses it to register with the PBX.

***

### Security Issue: Exposure of User Credentials

This traditional provisioning model introduces a **serious security risk**.

If a user knows—or can guess—another IP phone’s **MAC address**, they may be able to download that phone’s configuration file. These configuration files often contain **sensitive information**, including:

* SIP extension number
* SIP authentication password

Because IP phones can only read **plain-text configuration files**, these credentials are typically stored **in clear text**.

> ❗ **Important**\
> This design flaw can lead to **credential leakage**, unauthorized registrations, and **toll fraud**, making it one of the most common security weaknesses in traditional auto-provisioning systems.

***

### PortSIP PBX Solution: Enhanced Auto-Provisioning Security

To eliminate this risk, **PortSIP PBX** implements a more secure auto-provisioning architecture.

Instead of storing all configuration files in a shared directory, PortSIP PBX:

* Creates a **separate provisioning folder for each user**
* Uses **long, randomly generated folder names**
* Prevents predictable or guessable provisioning URLs

As a result, even if a phone’s MAC address is publicly known, it becomes **practically impossible** to guess another user’s configuration file URL.

> ❗ **Important**\
> This design significantly improves security and protects SIP credentials from unauthorized access.\
> Strong provisioning security is essential in any modern communication system.

***

### DHCP Option 66 and Auto-Provisioning in PortSIP PBX

As described above, PortSIP PBX’s secure provisioning model differs from traditional approaches. However, this introduces a compatibility challenge with **legacy IP phones** that rely **only on DHCP Option 66** for provisioning.

***

#### DHCP Option 66 Limitation

* Some legacy IP phones support provisioning **only via DHCP Option 66**
* DHCP Option 66 requires a single, unified provisioning URL
* This implies that all configuration files must be stored in the same directory

This behavior conflicts with PortSIP’s default per-user isolated provisioning folders.

***

### Optional Compatibility Setting for Legacy Phones

To support legacy phones while maintaining deployment flexibility, PortSIP PBX provides an **optional configuration switch**.

#### How to Disable Secure Folder Isolation (Not Recommended)

1. Sign in to the PortSIP PBX Web Portal
2. Navigate to **Advanced > Settings**
3. Open the **General** page
4. In the **Custom Options** field, add the following JSON string:

```json
{"disable_auto_provision_security" : true}
```

5. Click **OK** to save the changes

***

#### Effect of This Option

* If the option is set to `true`:
  * All configuration files are stored in a **shared directory**
  * DHCP Option 66 provisioning is supported
* If the option is set to `false` or removed:
  * PortSIP PBX resumes using **secure, per-user provisioning folders**

***

> ❗ **Critical Security Warning**\
> **We strongly do NOT recommend enabling**\
> `disable_auto_provision_security`
>
> Disabling this protection reintroduces the risk of:
>
> * SIP credential leakage
> * Unauthorized phone registrations
> * Toll fraud and security breaches

Only enable this option **temporarily** and **only when required** for legacy device compatibility.

***

### Summary

PortSIP PBX offers a **secure-by-design auto-provisioning mechanism** that protects user credentials while maintaining operational flexibility. Although DHCP Option 66 support is available for legacy devices, it should be used **with caution** and only when absolutely necessary.

Balancing **security** and **compatibility** is critical—and PortSIP PBX gives administrators full control to make that decision safely.





