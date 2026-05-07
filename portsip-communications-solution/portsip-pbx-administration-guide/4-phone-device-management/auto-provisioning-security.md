# Auto Provisioning Security

### Introduction

Auto-provisioning simplifies IP phone deployment by allowing devices to be configured automatically via a web-based mechanism. This eliminates the need for manual configuration on each phone.

When a user activates a phone, the PBX or service provider automatically delivers the necessary configuration settings, enabling the phone to register with the system. This process significantly reduces deployment time and operational overhead.

***

### How Auto-Provisioning Works

#### Overview

Auto-provisioning streamlines phone setup by letting users activate their devices through a web interface. Users do not need to manually enter SIP credentials or device configuration parameters.

#### PBX Configuration Files

In a standard PBX implementation:

1. Extension phone configuration files are stored in a dedicated provisioning directory.\
   Example: `ac2fbb80c5da60e`
2. Within this folder, the PBX generates **one configuration file per phone**, named using the phone’s **MAC address**.

#### Provisioning URL Mechanism

The provisioning workflow follows these steps:

1. The PBX sends a **SIP NOTIFY** message to the IP phone.
2. The message contains a **base provisioning URL**, e.g.:\
   `https://www.pbxhost.com/provision/ac2fbb80c5da60e`
3. The IP phone appends its MAC address to form the full configuration file path, e.g.:\
   `https://www.pbxhost.com/provision/ac2fbb80c5da60e/cc5ef641b794.xml`
4. The phone downloads the configuration file and uses it to **register with the PBX**.

***

### Security Consideration: Credential Exposure

⚠️ **Traditional provisioning models can be insecure.**

* If an attacker knows or can guess another phone’s MAC address, they may download that phone’s configuration file.
* These files often contain sensitive information in **plain text**, including:
  * SIP extension number
  * SIP authentication password

> ❗ **Important:**\
> This design flaw can lead to **credential leakage, unauthorized registrations, and toll fraud**, making it one of the most common vulnerabilities in legacy auto-provisioning systems.

***

### PortSIP PBX Secure Auto-Provisioning

#### Enhanced Security Architecture

PortSIP PBX addresses these risks by:

1. Creating a **separate provisioning folder for each user**.
2. Using **long, randomly generated folder names**.
3. Preventing predictable or guessable provisioning URLs.

✅ **Result:**\
Even if a phone’s MAC address is publicly known, it is practically impossible to access another user’s configuration file.

> ❗ **Important:**\
> Secure auto-provisioning is essential to protect SIP credentials and maintain overall system security.

***

#### Using Your Own RPS Account

Currently, when a phone is auto-provisioned via the **Remote Provisioning Server (RPS)**, its configuration link resides under PortSIP-managed accounts at the phone vendor.

**Best Practice for Security and Compliance:**

1. Contact each phone vendor to create **your own RPS account** for every phone brand you use.
2. Follow the guide: [Configuring Private RPS Account](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/4-phone-device-management/configuring-private-rps-account)
3. Once your RPS accounts are set up, when you use the RPS to auto-provision your IP Phone, the configuration link is stored in your own RPS account.

> ❗ **Strong Recommendation:**\
> Always configure your own RPS accounts to ensure secure auto-provisioning.

***

### DHCP Option 66 and Legacy Device Compatibility

#### Background

PortSIP PBX’s secure provisioning uses **per-user isolated folders**, which may conflict with legacy IP phones that only support **DHCP Option 66**.

**DHCP Option 66 Limitation:**

* Requires a **single, shared provisioning URL**
* All configuration files must reside in **the same directory**
* Conflicts with PortSIP PBX’s default per-user isolated folder structure

***

#### Optional Compatibility Setting

To support legacy devices while maintaining flexibility, PortSIP PBX offers an **optional configuration switch**.

**How to Temporarily Disable Secure Folder Isolation (Not Recommended):**

1. Log in to the **PortSIP PBX Web Portal**
2. Navigate to: `Advanced > Settings`
3. Open the **General** page
4.  In the **Custom Options** field, add:

    ```json
    {"disable_auto_provision_security": true}
    ```
5. Click **OK** to save changes

**Effect of This Option:**

| Option Setting     | Behavior                                                                                    |
| ------------------ | ------------------------------------------------------------------------------------------- |
| `true`             | All configuration files stored in a shared directory; DHCP Option 66 provisioning supported |
| `false` or removed | Secure, per-user provisioning folders are used                                              |

❗ **Critical Security Warning:**\
Enabling `disable_auto_provision_security` **reintroduces serious security risks**, including:

* SIP credential leakage
* Unauthorized phone registrations
* Toll fraud and other security breaches

**Recommendation:**\
Only enable this option temporarily and solely for legacy device compatibility. Return to **secure folder isolation** as soon as possible.

***

### Summary

PortSIP PBX provides a **secure-by-design auto-provisioning system** that:

* Protects user credentials
* Minimizes operational overhead
* Supports optional legacy device compatibility via DHCP Option 66

**Key Takeaways:**

* Use **per-user provisioning folders** for maximum security.
* Configure **your own RPS accounts** for each phone brand.
* Enable DHCP Option 66 only when necessary, and revert to secure defaults immediately afterward.

Balancing **security and compatibility** ensures safe and efficient IP phone deployments with PortSIP PBX.

***

✅ **Recommendation for Administrators:**\
Maintain a consistent, secure provisioning workflow while documenting any exceptions made for legacy devices. This approach aligns with **modern VoIP and UC industry standards**.
