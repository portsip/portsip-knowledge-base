# PortSIP Security Features

Securing a VoIP system is a primary responsibility throughout the planning, deployment, and operational lifecycle. This document provides practical, easy-to-follow security guidelines for **PortSIP PBX** to help you deploy and operate the system with stronger resilience against common network attacks.

***

### Ports Security

PortSIP PBX provides multiple services that use different protocols and ports. To reduce attack surface, configure your firewall to **block all unnecessary ports** and allow remote access **only** to the ports required for your deployment.

#### Change Default SIP Transport Ports (Recommended)

By default, PortSIP PBX creates:

* **UDP** transport on **5060**
* **WSS** transport on **5065**

You can delete these default transports and recreate them on non-default ports. After changing the ports:

* Update firewall rules (for example, using `firewalld`)
* If deployed in a cloud environment, update the **security group / firewall rules** at the cloud network layer as well

> ❗**Note**\
> Changing ports reduces exposure to generic scanning and opportunistic attacks, but it does not replace strong authentication, TLS/SRTP, and firewall policy.

#### Change the Default SSH Port (Recommended)

It is strongly recommended to change the default SSH port from **22** to a non-standard port, for example **10210**, to reduce automated brute-force attempts.

#### Default Firewall Behavior After Installation

* On supported Linux systems, **Firewalld is enabled by default** and PortSIP PBX installation configures the required firewall rules automatically.
* On Debian/Ubuntu, the default firewall **UFW is disabled** after PBX installation.

| Service               | Port        | Description                              |
| --------------------- | ----------- | ---------------------------------------- |
| Web Portal            | 8887        | Web Portal over HTTPS                    |
| IP Phone Provisioning | 8888        | IP Phone provisioning                    |
| Rest API              | 8887        | Rest API over HTTPS                      |
| WebRTC                | 10443       | The WebRTC client over HTTPS             |
| SBC Web Portal        | 8883        | SBC Web Portal over HTTPS                |
| IP Phone Provisioning | 8882        | IP Phone provisioning via SBC over HTTP  |
| WSI                   | 8885        | The WebSocket Interface                  |
| RTP                   | 45000-64999 | The UDP ports for the RTP packets on PBX |
| RTP                   | 25000-34999 | The UDP ports for the RTP packets on SBC |
| SIP                   | 5066        | SIP signaling port on SBC over UDP       |
| SIP                   | 5067        | SIP signaling port on SBC over TCP       |
| SIP                   | 5060        | SIP Signaling port on PBX over UDP       |
| SIP                   | 5061        | SIP Signaling port on PBX over TLS       |
| SIP                   | 5063        | SIP Signaling port on PBX over TCP       |
| SIP                   | 5065        | SIP Signaling port on SBC over WSS       |
| SSH                   | 22          | SSH port over TCP                        |

***

### Cloud Deployment Best Practices (AWS, Azure, Google Cloud)

When deploying PortSIP PBX on a cloud platform, use a **private network** so the PBX is not directly exposed to the public Internet:

* **AWS / Google Cloud**: deploy in a **VPC**
* **Azure**: deploy in a **VNet**

To allow users to access the PBX from the Internet, you must assign a **static public IP**:

* **AWS**: assign an **Elastic IP** to the EC2 instance and add required inbound rules in the Security Group
* **Azure**: associate a **Public IP** with the VM NIC, set IP assignment to **Static**, and configure inbound rules
* **Google Cloud (GCE)**: assign a **static external IP** and create the required **VPC firewall rules**

#### Disabling Firewalld in Cloud Deployments

If the PBX is deployed in a cloud environment and you are enforcing security strictly via **cloud security groups / VPC firewall rules**, you may disable Firewalld on the PBX server:

```bash
systemctl disable firewalld && systemctl stop firewalld
```

> ❗**Important**\
> Do **not** stop or disable Firewalld for **on-premises deployments**, where the server firewall is often a primary layer of protection.

***

### Network Security

#### Separate Voice and Data Traffic

Some VoIP providers offer dedicated SIP trunk connections that support **NGN (Next Generation Network)** ports and traffic separation. NGN can separate **data, voice, and video** networks (or any combination) to create a converged but segmented network.

For on-premises deployments, a common best practice is to configure **VLANs (Virtual LANs)** for voice traffic:

* Separates voice and data traffic at the network layer
* Improves call quality and helps reduce attack impact
* If one VLAN is compromised, other VLANs can remain isolated
* Rate limiting on the voice VLAN can help slow down certain attack patterns

***

### Transport Security

#### TLS and WSS for SIP Signaling

**TLS (Transport Layer Security)** secures SIP signaling and helps prevent interception or tampering of SIP messages in transit. It is recommended to use **TLS** for SIP transports where possible.

For WebRTC clients, PortSIP PBX supports **WSS (WebSocket Secure)**, which provides encryption similar to HTTPS and helps protect against man-in-the-middle attacks.

#### SRTP and DTLS-SRTP for Audio and Video

PortSIP PBX and PortSIP Apps support **SRTP** and **DTLS-SRTP** for media encryption.

* **SRTP** protects RTP media using encryption and authentication
* **DTLS-SRTP** secures key exchange for WebRTC scenarios

Media is protected using **AES-256** encryption when SRTP/DTLS-SRTP is enabled.

***

### Web Access Security

PortSIP PBX provides:

* **HTTPS** access on **TCP 8887**
* **HTTP** access on **TCP 8888**

Recommended practices for securing the PBX Web Portal:

* Create firewall/security rules to **disable HTTP** access on TCP **8888**
* Disable redirect from **port 80**
* Disable redirect from **port 443**
* Upload a trusted SSL certificate (for example, a certificate issued by DigiCert or GeoTrust)

> ❗**Best Practice**\
> Use HTTPS only for administrative access and avoid exposing the web portal directly to the public Internet unless required. Where possible, restrict access by IP allowlists or VPN.

***

### Password and Login Security

#### Web Portal Password for PBX Administrator

The default PBX Web Portal administrator credentials are:

* Username: `admin`
* Password: `admin`

Change the password immediately after the first login:

1. Click the profile picture (top-right corner)
2. Select **Change Password**
3. Enter the current password and a new password

The new password must meet all of the following requirements:

* At least one letter (Latin characters)
* At least one number (0–9)
* One uppercase letter or a special character (for example, `! @ $ #`)
* No sequential characters (for example, `1234`, `7890`, `Abcd`)
* No repeating characters (for example, `222`, `Aaa`, `###`)
* No account information (for example, first/last name, phone number)
* Length: **8–32 characters**

#### Passwords for Tenant Administrators

When you create a user with the **Admin** role, that user becomes a tenant administrator. A tenant administrator has two passwords:

* **SIP Password**: used by IP phones, softphones, and WebRTC clients for SIP registration
* **User Password**: used to sign in to the PBX Web Portal (voicemail, recordings, CDRs)

It is strongly recommended to change both passwords after the tenant administrator’s initial login.

1. Click the profile picture (top-right corner)
2. Select:
   * **Change User Password**
   * **Change Extension Password**
3. Set new values that comply with the tenant’s password policy

#### Passwords for Extension Users

Users created with **Standard User** or **Standard International User** roles also have two passwords:

* **SIP Password**: for device/app registration
* **User Password**: for Web Portal access (voicemail, recordings, CDRs)

Both must comply with the tenant password policy.

***

### Login Security Controls

After signing in as the PBX administrator, you can configure login protection policies:

1. Go to: **Advanced > Security**
2. On the **Web Login** page:
   * Set the maximum number of failed login attempts
   * Block an IP address when the failure threshold is exceeded
   * Configure the IP block duration
   * Require newly created users to change the default password on first login
   * Enable **2FA** for extension users

***

### SIP and TCP/IP Security (Anti-Hacking)

PortSIP PBX includes anti-hacking protections designed to mitigate malicious activity when firewall controls are insufficient or misconfigured. These protections can help detect and block:

* Packet floods / DoS attacks
* Brute-force attempts targeting extension numbers and passwords

To configure:

1. Go to: A**dvanced > Security**
2. Open the **Anti Hacking** page.

#### Detection Period

Defines the time window (in seconds) used for counting events before actions are enforced.\
To effectively disable the protection, set it to a higher value.

#### Failed Authentication Protection

Limits repeated authentication failures that indicate password guessing.

By default, if an IP address reaches **50 failed authentication attempts** within the detection period, it is blocked and placed on the blacklist for the configured duration (default: **1 hour**).

#### Failed Challenge Requests (407)

DoS attacks may send REGISTER/INVITE requests but never respond to the PBX challenge (407). Configure how many such requests are allowed per IP during the detection period. If exceeded, the IP is blacklisted (default: **1 hour**).

#### Level 1 and Level 2 Security (Packet Rate Limits)

These layers block IPs that exceed packet rate thresholds:

* **Level 1** (default **500 packets/second**)
* **Level 2** (default **2000 packets/second**)

When an IP exceeds the configured threshold, it is blacklisted for the corresponding blacklist interval.

Blocked IP addresses appear under: **Blacklist and Codes > IP Blacklist**

From there, you can manually add IPs to a **Whitelist** if needed.

> ❗**Potential Issue in Original Text**\
> One sentence states “default value is 2000 packets per second” and then says “more than 200 packets per second” indicates an issue. These values conflict.\
> **Recommended safer wording:** “If an IP address exceeds the configured packet rate threshold, it will be blocked for the configured blacklist interval.”

***

### User-Agent Blacklist

To help mitigate threats such as **SPIT**, **TDoS**, fuzzing, and war dialing, PortSIP PBX can block SIP messages from specific **User-Agent** strings.

To configure:

1. Go to:  **Advanced > Security**
2. Open **Blocked User Agents**
3. Edit the user-agent blacklist as needed

<figure><img src="../../.gitbook/assets/user-agent-blacklist.png" alt=""><figcaption></figcaption></figure>

***

### Extension Security

PortSIP PBX uses role-based access control for tenant users. Default roles include:

* **Admin**: full permissions within the tenant scope
* **Standard International User**: allowed to call local, national, and international numbers
* **Standard User**: allowed to call internal users only

You can assign a user role during creation and modify it later as required.

***

### IP Phone Security

Each extension’s IP phone provisioning configuration is stored in a directory with a **randomized name**. This reduces the risk of guessing the provisioning URL even if a phone’s MAC address is exposed.

***

### Whitelist and Blacklist

PortSIP PBX supports IP allowlisting and blocklisting:

* Traffic from **whitelisted** IP addresses is trusted and bypasses anti-hacking checks.
* Traffic from **blacklisted** IP addresses is dropped immediately.

#### Add a Whitelist Entry (Example)

Scenario: A trusted remote office uses public IP `103.224.182.210`.

1. Go to **IP Blacklist**
2. Click **Add**
3. Enter `103.224.182.210`\
   (or enter `103.224.182.210.0` and select a subnet mask to allow a range)
4. Set **Action** to **Allow**
5. Add a description (for example, “My Remote Office”)
6. Click **Apply**

<figure><img src="../../.gitbook/assets/ip_whitelist (1).png" alt=""><figcaption></figcaption></figure>

***

#### Block an IP Range (Example)

To block all IPs starting with `41.202.*.*`:

1. Go to **IP Blacklist**
2. Click **Add**
3. Enter `41.202.0.0`
4. Select subnet mask `255.255.0.0`
5. Set **Action** to **Block**
6. Add a description (for example, “Anti DoS from 41.202.x.x”)
7. Click **Apply**

> ❗**Important**\
> PortSIP’s blacklist/whitelist is not a replacement for a network firewall. If you need to restrict access to trusted VoIP providers only, enforce this at the firewall or cloud security group level.
>
> When blocking an IP range, ensure the range does not include the PBX server’s own IP address.

<figure><img src="../../.gitbook/assets/ip_blacklist (1).png" alt=""><figcaption></figcaption></figure>

***

### Trunk Security

SIP trunking typically establishes a peer relationship to provide PSTN connectivity over VoIP. Depending on the provider, trunks may be delivered over:

* Public Internet (ITSP)
* Dedicated carrier WAN links (managed services)

Security should be enforced across multiple layers, including firewall policy, SIP authentication, and signaling/media encryption.

#### SIP Trunk Authentication

* **Register-Based Authentication**\
  The trunk provider requires the PBX to register and authenticate using SIP credentials.
* **IP-Based Authentication**\
  The trunk provider does not use SIP REGISTER; instead, the PBX trusts inbound SIP traffic only from specified trunk IP addresses.

PortSIP PBX supports both models.

> ❗**Potentially Risky / Misleading Statement**\
> The original text states that “IP-based authentication is strongly recommended, it’s more secure.”\
> In practice, security depends on your network controls:
>
> * IP-based auth can be secure **if** you strictly restrict inbound traffic to known provider IPs and protect against spoofing.
> * Register-based auth can also be secure when combined with strong credentials and TLS.
>
> **Safer wording:** “IP-based authentication is commonly used with ITSPs that provide fixed source IPs. Whichever model you use, ensure inbound traffic is restricted to trusted sources and that strong transport/media security is enabled.”

#### Accept Register Trunk (Gateway Registration)

PortSIP PBX can accept registrations from a trunk gateway (for example, an E1/T1 gateway) using an **Accept Register** trunk:

* Configure a username and password in PBX
* The gateway registers using those credentials
* Calls are permitted only after successful authorization

***

### Call Control and Fraud Prevention

#### Maximum Concurrent Call Limits

PortSIP PBX allows you to limit the maximum number of concurrent calls on a trunk at both:

* Global level
* Tenant level

If the limit is reached, new call attempts through that trunk are rejected. This helps prevent overload and reduces exposure to certain fraud and abuse scenarios.

#### Outbound Route Permissions

When creating outbound rules, plan permissions carefully:

* Use called number prefix/length conditions
* Use user group membership and user roles
* Create separate rules for local, long-distance, and international calls

Example approach:

* Assign least-cost routing for local calls
* Restrict international calling to **Standard International User**
* Use **Allowed Country Code** and **Disallowed Codes** under: **Blacklist and Codes > Codes and E164** to block calls by country code

#### Office Hours for Outbound Rules

PortSIP PBX allows you to define office hours for outbound rules. Outside of the specified hours:

* The outbound rule is unavailable
* Calls using that rule will not be permitted

***

### Ambiguities and Safety Fixes (Recommended)

1. **Disable Firewalld in Cloud**
   * Safer guidance is to disable Firewalld only if cloud security groups/VPC firewall rules are correctly configured and strictly enforced.
2. **Level 2 packet thresholds conflict**
   * Replace the conflicting values with wording that references “configured threshold” rather than hard-coded numbers.
3. **IP-based trunk is more secure**
   * Reframe as conditional on strict IP allowlisting and provider fixed IPs, rather than universally “more secure”.





