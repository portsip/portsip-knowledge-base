# Configuring SIP Trunk

### Introduction

VoIP service providers deliver external calling by replacing traditional telecom lines with IP-based services. They can assign local phone numbers (DIDs) in many cities or countries, route inbound calls to your PBX, and often support number porting (moving an existing phone number to the new provider). VoIP providers may also offer more competitive calling rates by using international carrier networks and negotiated pricing.

PortSIP PBX supports several trunk types, described below.

***

### Trunk Types Supported by PortSIP PBX

#### Register-Based Trunk

A **Register-Based Trunk** requires PortSIP PBX to **register to the trunk provider** using credentials (an authentication ID and password).

* **System Admin:** Can create this trunk type and assign it to multiple tenants. Each tenant receives its own DID pool.
* **Tenant Admin:** Can also create a Register-Based Trunk, but it **cannot be shared** with other tenants.
* **Uniqueness requirement:** To avoid conflicts, the **hostname and authentication ID must be unique**.

#### Accept Register Trunk

An **Accept Register Trunk** works in the opposite direction: the **trunk provider registers to PortSIP PBX** using a predefined authentication ID and password.

* **System Admin:** Can create this trunk type and assign it to tenants. Each tenant receives its own DID pool.
* **Tenant Admin:** Can also create an Accept Register Trunk, but it **cannot be shared** across tenants.
* **Uniqueness requirement:** The **hostname and authentication ID must be unique**.

#### IP-Based Trunk

An **IP-Based Trunk** does not use SIP registration. Instead, the trunk provider routes calls based on your PBX’s **public IP address** (or a known SBC/published address).

* **System Admin only:** Only the System Admin can create an IP-Based Trunk.
* **One per provider:** This trunk can be added **only once per provider**.
* **Multi-tenant use:** If multiple tenants need the trunk, the System Admin assigns the trunk to those tenants and allocates a **unique DID pool per tenant**.

#### Microsoft Teams

PortSIP PBX supports **Microsoft Teams Direct Routing** as a trunk type.

* **Tenant Admin only:** This trunk type can only be configured by the Tenant Admin.
* **Not shareable:** A Teams trunk cannot be shared with other tenants.

#### WhatsApp

PortSIP PBX also supports **WhatsApp as a trunk** for messaging, enabling users to send and receive WhatsApp messages.

* **Tenant Admin only:** This trunk type can only be configured by the Tenant Admin.
* **Not shareable:** A WhatsApp trunk cannot be shared with other tenants.

***

### Tenant Admin Permissions

Tenant Admins can:

* View all trunks assigned by the System Admin.
* Create inbound and outbound rules that use those assigned trunks.

Tenant Admins **cannot**:

* Modify the settings of any trunk that was created by the System Admin or another tenant.

Tenant Admins **can** modify:

* Any trunk that they created themselves.

***

### Configuring a Trunk

Before you start, make sure you have an active account with a VoIP or SIP trunk provider. PortSIP PBX supports most SIP-based VoIP and SIP trunk services.

After you receive your trunk details from the provider (such as credentials, SIP server/domain, and assigned DIDs), you can configure the trunk in PortSIP PBX.

<figure><img src="../../../.gitbook/assets/turnk_1.png" alt=""><figcaption></figcaption></figure>

***

### DID Pool Concept

Because PortSIP PBX is a **multi-tenant** system, the PBX must be able to determine **which tenant owns an incoming DID** (and therefore which tenant’s inbound rules should apply).

If multiple tenants use the **same trunk provider** and configure the **same DID number** in their inbound rules, the PBX would not know which tenant should receive the inbound call. Similarly, if an extension in one tenant presents an **outbound caller ID (CLI)** that belongs to another tenant, it can create conflicts and incorrect call routing or identity presentation.

To prevent these issues, PortSIP PBX uses a **DID pool**: a dedicated set (or range) of DID numbers assigned to each tenant for a given trunk.

#### How DID Pools Work

* When the **System Admin** assigns a trunk to a tenant, they must also configure a **DID pool** for that tenant.
* DID pools for the same trunk provider **must not overlap** across tenants.
* When a tenant creates inbound rules using that trunk, they can only select DIDs from **their assigned DID pool**.

If a **Tenant Admin** adds a trunk independently, they must also define a DID pool for that trunk. When creating inbound rules for that trunk, the tenant can only use DID numbers from its pool. If DID pools overlap for the same provider, inbound routing conflicts will occur.

#### Example: Overlapping DID Pools (Not Allowed)

* Tenant A adds provider **XYZ** and sets the DID pool to **1000–2000**
* Tenant B adds the same provider **XYZ** and sets the DID pool to **2000–3000**

This configuration fails because **2000 overlaps**. If an inbound call arrives for DID **2000**, the PBX cannot determine which tenant should receive the call.

#### DID Pool Formats

A DID pool can be defined as:

* A single range:\
  `1000-2000`\
  `282556000-282556900`
* A mix of individual numbers and ranges (semicolon-separated):\
  `101; 203; 300-450`

#### DID Pool Entry Rules

When configuring a DID pool, the DID number or range must **not** start with:

* `+`
* `0`
* `00`

If your DIDs are provided in E.164 format (for example, `+14155550100`) or include a trunk prefix (for example, `0014155550100`), remove the prefix before entering the DID pool.

> ❗**Note:** This entry rule applies only to how DIDs are stored/validated in the PBX DID pool. Your provider may still deliver inbound calls with `+` or in E.164 format depending on their signaling behavior.

***

### Add the Trunk (System Admin)

#### Prerequisites

Before you begin, make sure you have:

* An active SIP trunk account (or IP-based trunk details) from your provider
* The provider’s connection details, such as:
  * Host domain/IP and port
  * Outbound proxy (if required) and proxy port
  * Transport protocol requirements (UDP/TCP/TLS)
  * Credentials (for Register-Based trunks): authentication name/username, password, registration interval

If the provider requires **TCP or TLS**, ensure that transport is already configured in the PBX (see **Transport Management**).

***

#### Steps

1. **Open the Trunks page**\
   Go to **Call Manager > Trunks**.
2. **Start adding a trunk**\
   Click the **arrow** button and select the trunk type you want to add.
3. **Enter a trunk name**\
   Enter a clear, recognizable **friendly name** (for example, `Twilio-US`, `Telnyx-NYC`, or `XYZ-Primary`).
4. **Select the trunk provider brand**\
   From **Brand**, select the provider.
   * If your provider is listed and preconfigured, follow the [Configuring SIP Trunks](../../configuring-sip-trunks/) guide for provider-specific settings.
   * If the provider is not listed, select **Generic**.
5. **Enter trunk connection details**\
   Fill in the fields using the information provided by your trunk service provider:
   * **Host Domain or IP**
   * **Port**
   * **Outbound Proxy Server** (if required)
   * **Outbound Proxy Server Port** (if required)
6.  **Select the transport protocol**\
    Under **Transport**, choose **UDP**, **TCP**, or **TLS** based on your provider requirements.

    > ❗**Note:** The selected transport must already be configured in the PBX. If it isn’t available, add it first in [**Transport Management**](../6-transport-management.md).
7. **Add associated IPs (if required)**\
   Some providers may send SIP INVITEs from **multiple source IP addresses**. If so:
   * Under **Associated IPs of the Trunk**, click **Add**
   * Enter each additional IP address provided by the carrier
8. **Enter registration credentials (Register-Based trunks only)**\
   If you selected a **Register-Based** trunk:
   * Click **Next**
   * Enter:
     * **Username/Authentication Name**
     * **Password**
     * **Register Time**
   * Use the values provided by your trunk provider.
9. **Configure advanced options**\
   Click **Next**, then review and configure the following settings as needed:
   * **Use the private IP address to communicate with this trunk**\
     Enable if the PBX should use a **private IP** for trunk connections. If disabled, the PBX uses its **public IP**.
   * **Rewrite the host IP of the Via header using the PBX server’s public IP when sending requests to the trunk**\
     If enabled (and the PBX has a public IP), the PBX replaces the Via host IP with its public IP when sending SIP messages to the trunk.\
     **Recommendation:** Leave this at the default unless your provider explicitly requires it.
   * **Verify the port when receiving SIP messages from the trunk**\
     When enabled, the PBX matches inbound SIP messages from the trunk using **both IP and port**. When disabled, it matches by **IP only**.\
     **Recommendation:** Leave this at the default unless instructed otherwise by your provider.
   * **This trunk only accepts a single Via SIP header**\
     When enabled, the PBX keeps only one Via header when sending SIP messages to the trunk.
   * **Use the Tel URI scheme for the Request-Line**\
     When enabled, the PBX sends INVITEs using `tel:` in the Request-Line. Example:\
     `INVITE tel:+12345678 SIP/2.0`
   * **Use the Tel URI scheme for the To header**\
     When enabled, the PBX uses `tel:` in the To header. Example:\
     `To: tel:+12345678`
   * **STIR/SHAKEN signature required**\
     When enabled, the PBX signs outbound calls using **STIR/SHAKEN**.\
     For details, see the [STIR/SHAKEN](../27-stir-shaken/) chapter.
   * **Remove the “+” prefix from the called number on outbound calls**\
     When enabled, the PBX removes the `+` prefix from the called number when sending the INVITE to the trunk.
   * **Send OPTIONS message for keep-alive**\
     When enabled, the PBX sends SIP **OPTIONS** periodically to monitor trunk reachability. If the PBX does not receive `200 OK`, it marks the trunk as **offline**.
   * **Send OPTIONS message interval (seconds)**\
     Sets how often the PBX sends OPTIONS keep-alives. Default: **360 seconds**.
10. **Assign the trunk to tenants (System Admin only)**\
    Click **Next**, then select one or more tenants that should be allowed to use this trunk.
11. **Configure a DID pool for each assigned tenant**\
    For every tenant you assign:
    * Configure a **DID pool**
    * Ensure DID pools are **unique and non-overlapping** for the same provider

***

#### Expected Outcome

After completing these steps:

* The trunk is created and available in the PBX.
* Selected tenants can see the trunk (as assigned by the System Admin).
* Each assigned tenant can create inbound rules **only using DIDs from their own DID pool**.

For more details, see [DID Pool Concept](configuring-sip-trunk.md#did-pool-concept).

<figure><img src="../../../.gitbook/assets/trunk_did_pool.png" alt=""><figcaption></figcaption></figure>

***

### Add the Trunk (Tenant Admin)

When a Tenant Admin logs in to the Web Portal, they can create trunks for **their own tenant**. Tenant Admins can add only the following trunk types:

* **Register-Based** — The PBX registers to the trunk provider.
* **Accept Register** — The trunk provider registers to the PBX.
* **Microsoft Teams** — Microsoft Teams Direct Routing.
* **WhatsApp** — WhatsApp messaging service.

> ❗**Note:** **IP-Based** trunks can be added **only by the System Admin**.

***

#### Prerequisites

Before you begin, make sure you have:

* An active account with your SIP trunk provider (or Microsoft Teams / WhatsApp credentials, if applicable)
* Provider details such as:
  * Host domain/IP and port
  * Outbound proxy (if required) and proxy port
  * Required transport protocol (UDP/TCP/TLS)
  * Credentials (for Register-Based trunks): authentication name/username, password, re-register time
  * Any additional source IPs the provider may use (for Associated IPs)

Also ensure the required transport protocol is already enabled in PortSIP PBX (see **Transport Management**).

***

#### Steps

1. **Open the Trunks page**\
   Go to **Call Manager > Trunks**.
2. **Start adding a trunk**\
   Click the **arrow** button and select the trunk type you want to add.
3. **Enter a trunk name**\
   Enter a **friendly name** that helps you identify the trunk later (for example, `Telnyx-US`, `Twilio-Primary`, or `Teams-DirectRouting`).
4. **Select the trunk provider brand**\
   From **Brand**, select your provider.
   * If your provider is listed and preconfigured, follow the [Configuring SIP Trunks](../../configuring-sip-trunks/) guide for provider-specific settings.
   * If your provider is not listed, select **Generic** and continue with the steps below.
5.  **Configure the DID Pool**\
    Enter a **DID pool** for this tenant.

    * The DID pool defines which DIDs this tenant can use when creating inbound rules for this trunk.
    * When creating inbound rules, the DID you select **must fall within this DID pool**.

    For details and formatting rules, see [DID Pool Concept](configuring-sip-trunk.md#did-pool-concept).
6. **Enter trunk connection details**\
   Fill in the following fields using the values from your trunk provider:
   * **Host Domain or IP**
   * **Port**
   * **Outbound Proxy Server** (if required)
   * **Outbound Proxy Server Port** (if required)
7.  **Select the transport protocol**\
    Under **Transport**, choose the protocol required by your provider: **UDP**, **TCP**, or **TLS**.

    > ❗**Note:** The selected transport must already be configured in the PBX. If it’s not available, add it first in [**Transport Management**](../6-transport-management.md).
8. **Add associated IP addresses (if required)**\
   Some providers send SIP INVITEs from **multiple source IPs**, not just the Host Domain/IP.
   * Under **Associated IPs of the Trunk**, click **Add**
   * Add each IP address provided by your trunk carrier
9. **Enter registration credentials (Register-Based trunks only)**\
   If you selected **Register-Based**:
   * Click **Next**
   * Enter:
     * **Username/Authentication Name**
     * **Password**
     * **Re-register Time**
   * Use the values provided by your trunk provider.
10. **Configure advanced parameters**\
    Click **Next**, then review and configure the following options as needed:
    * **Use the private IP address to communicate with this trunk**\
      Enable if the PBX should use a **private IP** for this trunk connection. If disabled, the PBX uses its **public IP**.
    * **Rewrite the host IP of the Via header with the PBX server’s public IP when sending requests to the trunk**\
      If enabled (and the PBX has a public IP), the PBX replaces the Via host IP with its public IP in outbound SIP messages.\
      **Recommendation:** Leave this at the default unless your provider explicitly requires it.
    * **Verify the port when receiving SIP messages from the trunk**\
      When enabled, the PBX matches inbound SIP messages using **both IP and port**. When disabled, it matches by **IP only**.\
      **Recommendation:** Leave this at the default unless instructed otherwise by your provider.
    * **This trunk only accepts a single Via SIP header**\
      Enable only if your trunk provider requires a single Via header in outbound SIP messages.
    * **Use the Tel URI scheme for the Request-Line**\
      When enabled, the PBX uses `tel:` in the Request-Line. Example:\
      `INVITE tel:+12345678 SIP/2.0`
    * **Use the Tel URI scheme for the To header**\
      When enabled, the PBX uses `tel:` in the To header. Example:\
      `To: tel:+12345678`
    * **STIR/SHAKEN signature required**\
      When enabled, the PBX signs outbound calls using **STIR/SHAKEN**.\
      For details, see the [STIR/SHAKEN](../27-stir-shaken/) chapter.
    * **Remove the “+” prefix from the called number on outbound calls**\
      When enabled, the PBX removes the `+` prefix from the called number when sending the INVITE to the trunk.
    * **Send OPTIONS message for keep-alive**\
      When enabled, the PBX sends SIP **OPTIONS** periodically to monitor trunk reachability (online/offline). If no `200 OK` is received, the PBX marks the trunk as **offline**.
    * **Send OPTIONS message interval (seconds)**\
      Sets how often OPTIONS keep-alives are sent. Default: **360 seconds**.

***

#### Expected Outcome

After completing these steps:

* The trunk is created and available for your tenant.
* Your tenant can create inbound rules using this trunk, but **only with DIDs in the trunk’s DID pool**.
* If keep-alive is enabled, the PBX monitors trunk status using SIP OPTIONS based on the configured interval.

***

### Configure E1/T1 Gateway Registration to PortSIP PBX

#### Overview

In some deployments, PortSIP PBX is hosted in a public cloud (AWS, Azure, or Google Cloud), while an **E1/T1 gateway** remains on a customer’s local LAN. If the gateway **does not have a static public IP address**, an **IP-Based trunk** is not suitable because the PBX cannot reliably identify the gateway by a fixed source IP.

In this scenario, configure the E1/T1 gateway to **register outbound from the local LAN to the cloud-hosted PortSIP PBX**. This allows the gateway to function as a trunk for placing and receiving calls through PortSIP PBX.

> ❗**Terminology note:** In PortSIP PBX, this model is implemented using an **Accept Register** trunk (the gateway registers to the PBX).

***

#### Prerequisites

Before you begin, ensure you have:

* The cloud PBX **public static IP address** (or public SBC address) reachable from the gateway network
* The SIP **transport protocol and port** you will use on PortSIP PBX (UDP/TCP/TLS)
* A DID plan and a tenant DID pool strategy (see [DID Pool Concept](configuring-sip-trunk.md#did-pool-concept))
* Firewall/NAT rules that allow the gateway to reach the PBX SIP signaling port

> ❗**Security Note:** Use strong, unique credentials for trunk registration. Treat trunk credentials like admin passwords—anyone who can register may be able to send calls into your PBX or place outbound calls (depending on routing rules).

***

### Step 1: Create an “Accept Register” Trunk in PortSIP PBX

1. **Open the Trunks page**\
   Go to **Call Manager > Trunks**.
2. **Add a trunk**\
   Click the **arrow** button and choose **Accept Register**.
3. **Enter a trunk name**\
   Enter a **friendly name** (for example, `E1GW-SiteA`).
4. **Configure the DID Pool**\
   Enter a **DID pool** for this trunk.\
   When you create inbound rules for this trunk, the DID numbers you select **must fall within this DID pool**.\
   For details, see [DID Pool Concept](configuring-sip-trunk.md#did-pool-concept).
5.  **Set Host Domain or IP Address**\
    Enter a **domain string** in **Host Domain or IP Address**. This value does **not** need to be a real domain, and it does not need to resolve in DNS.

    Example: `portsiptrunk1.io`

    > ❗**Important:** Ensure this value does **not** match any tenant’s SIP domain.
6. **Set trunk registration credentials**\
   Click **Next**, then configure:
   * **Authorization Name**: Enter an identifier (for example, `123456`).\
     The E1/T1 gateway will use this value when registering to PortSIP PBX.
   * **Password**: Enter a password.\
     The E1/T1 gateway will use this password when registering to PortSIP PBX.
7. **Review remaining options**\
   The remaining settings are the same as those used for IP-Based and Register-Based trunks (transport selection, Via header behavior, port verification, keep-alives, etc.).\
   In most cases, you can keep the defaults unless your environment or gateway vendor requires specific behavior.
8. **Save the trunk**\
   Complete the wizard to create the trunk.

#### Expected Outcome

* The Accept Register trunk is created and ready for the gateway to register.

***

### Step 2: Configure the E1/T1 Gateway to Register to the Cloud PBX

On the E1/T1 gateway:

1. **Set the SIP server/domain**\
   In the E1/T1 SIP settings, enter the trunk **Host Domain or IP Address** (for example, `portsiptrunk1.io`) in the **SIP Server/Domain** field.\
   This must match the value you configured in PortSIP PBX.
2. **Set the outbound proxy**\
   In **Outbound Proxy Server**, enter the **public static IP address** of the cloud-hosted PortSIP PBX.
3. **Set the outbound proxy port**\
   In **Outbound Proxy Server Port**, enter the **PortSIP PBX transport port** used for this trunk (for example, the SIP listening port for UDP/TCP/TLS, depending on your transport).
4. **Enter registration credentials**\
   Enter the values you configured in PortSIP PBX:
   * **Username/Auth ID/Auth Name**: Authorization Name
   * **Password**: Password

After saving the gateway configuration, the E1/T1 gateway should register successfully to the cloud-hosted PortSIP PBX.

#### Expected Outcome

* The E1/T1 gateway registers to PortSIP PBX and becomes available as a trunk for inbound and outbound calling.

***

### Outbound Parameters and Inbound Parameters (Advanced)

After the trunk is working, you can further customize SIP header/value handling:

1. Go to **Call Manager > Trunks**
2. Select the trunk
3. Click **Edit**
4. Update **Inbound Parameters** and/or **Outbound Parameters**

#### Outbound Parameters

Use **Outbound Parameters** to apply rules that modify SIP headers in outbound INVITEs sent to the trunk. For example, you can set the user part of the **From** header to match the extension’s **Outbound Caller ID**.

To configure an extension’s Outbound Caller ID, go to the user’s settings **General** page. For details, see **Users**.

#### Inbound Parameters

Use **Inbound Parameters** to apply rules that normalize or rewrite field values in SIP messages received from the trunk (inbound calls).

For additional guidance, see [Handle Outbound Calls Through SIP Trunk](handle-outbound-calls-through-sip-trunk.md).

> ❗**Recommendation:** Inbound and outbound parameter rules are advanced options. Use the default values unless you have a specific interoperability requirement.

***

### Deleting a Trunk

You cannot delete a trunk if it is referenced by any inbound or outbound rules.

To delete a trunk:

1. Identify all inbound and outbound rules that use the trunk.
2. Either:
   * Change those rules to use a different trunk, **or**
   * Delete the rules.
3. Delete the trunk.

#### Expected Outcome

* Once no rules reference the trunk, it can be deleted successfully.



