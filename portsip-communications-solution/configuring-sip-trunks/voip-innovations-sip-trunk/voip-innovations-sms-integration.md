# VoIP Innovations SMS Integration

Before proceeding with the next steps, ensure that you have [purchased a DID on the VoIP Innovations platform with SMS enabled](purchase-a-did-on-voip-innovations-platform.md).

### Important: 10DLC Registration Requirement (U.S.)

> **Compliance Notice**\
> If you are using:
>
> * A **newly added phone number**,
> * A **new carrier account**, or
> * Messaging has **stopped working unexpectedly**,\
>   you must verify that the **10DLC registration process** has been completed with your carrier **before** attempting to use SMS in PortSIP PBX.

* **10DLC (10-Digit Long Code) registration is mandatory** for **all supported U.S. carriers**.
* This requirement is enforced by wireless carriers and applies to **all application-to-person (A2P) messaging traffic**.

Failure to complete 10DLC registration may result in:

* Message delivery failures
* Traffic blocking
* Account suspension for messaging services

***

### Integration Details: VoIP Innovations Configuration

#### Create API Credentials (VoIP Innovations Side)

PortSIP PBX requires API credentials from VoIP Innovations to enable SMS functionality.

1. Log in to your **VoIP Innovations Back Office**:\
   `https://backoffice.voipinnovations.com`
2. From the main navigation menu, go to: **Add-Ons > API Users**
3. Under the **API Users** section, click **Add New API Login**.
4. Complete the required fields:
   * **Username**\
     This value serves as the **API Key** and will be used later in PortSIP PBX.
   * **Password**\
     This value serves as the **API Secret** and will also be required later.
   * **Server IP**\
     Enter the **public IP address** of your PortSIP PBX server.
5. Click **Add API Login** to save the configuration.

**Expected Result**

* An API user is created and ready to be used by PortSIP PBX for SMS integration.

> **Security Best Practice**\
> Treat the API Key and API Secret as sensitive credentials. Store them securely and do not share them outside trusted systems.

<figure><img src="../../../.gitbook/assets/vi_trunk_sms_2.png" alt=""><figcaption></figcaption></figure>

***

### Configure SMS with VoIP Innovations Trunk in PortSIP PBX

This section explains how to configure SMS/MMS messaging in PortSIP PBX using a VoIP Innovations SIP trunk.

***

#### Prerequisites

Before configuring SMS in PortSIP PBX, ensure that:

* A **VoIP Innovations SIP trunk** has already been configured using the following guide:
  * **Configuring VoIP Innovations IP Authentication Trunk**
* You have created **API credentials** (API Key and API Secret) on the VoIP Innovations platform
* The DID you plan to use has **SMS enabled** and meets carrier compliance requirements (for example, U.S. 10DLC if applicable)

***

#### Sign in to the PortSIP PBX Web Portal

You can access the PortSIP PBX Web Portal using one of the following methods:

**Option 1: System Administrator**

1. Sign in as a **PBX System Administrator**.
2. Navigate to **Tenants**.
3. Select the target tenant.
4. Click **Manage** to switch into that tenant’s management context.

**Option 2: Tenant Administrator**

* Sign in directly as a **Tenant Admin** to manage the tenant.

> **Note**\
> For more information about tenant roles and permissions, see [Tenant Management](../../portsip-pbx-administration-guide/3-tenant-management/).

***

### Add an SMS Configuration in PortSIP PBX

This section explains how to create an SMS configuration in PortSIP PBX and link it to a VoIP Innovations SMS-enabled DID.

***

#### Step 1: Add an SMS Configuration

1. Sign in to the **PortSIP PBX Web Portal**.
2. Navigate to: **Message Channel > SMS/MMS**
3. Click **Add**.
4. Select your configured **VoIP Innovations Trunk**.
5.  Configure the following fields:

    **Sender ID**

    * Enter the Sender ID created on the VoIP Innovations platform if you want to use a custom sender.
    * Leave this field empty to use the DID associated with the VoIP Innovations trunk as the Sender ID.

    **API Key**

    * Enter the **Username** created on the VoIP Innovations platform in the previous step.
    * This value functions as the API Key.

    **API Secret**

    * Enter the **Password** created on the VoIP Innovations platform in the previous step.
    * This value functions as the API Secret.
6. Click **OK** to save the configuration.

<figure><img src="../../../.gitbook/assets/vi_trunk_sms_1.png" alt=""><figcaption></figcaption></figure>

***

#### Step 2: Copy the PortSIP PBX Webhook URL

Inbound SMS messages are delivered to PortSIP PBX through a webhook.

1. On the **SMS/MMS** list page, select the SMS configuration you just created.
2. Click **Copy Webhook**, or
3. Double-click the configuration and manually copy the **Webhook URL**.

***

#### Step 3: Add an SMS DID on the VoIP Innovations Platform

Each SMS-enabled DID must be associated with the PortSIP PBX webhook on the VoIP Innovations side.

1. Log in to your [VoIP Innovations Back Office](https://backoffice.voipinnovations.com/).
2. Navigate to **SMS > Add New SMS DID**.
3. Set **Destination Type** to **API POST**.
4. Enter the **SMS Number (DID)** you want to enable.
5. In the **URL** field, paste the **Webhook URL** copied from PortSIP PBX.
6. Click **Configure** to save the settings.

<figure><img src="../../../.gitbook/assets/vi_trunk_sms_3.png" alt=""><figcaption></figcaption></figure>

***

### Verify the Configuration

At this point, the Twilio SMS/MMS integration is complete.

You can now [create outbound and inbound rules](configuring-outbound-and-inbound-calls.md) in PortSIP PBX to send and receive SMS/MMS messages using the Twilio trunk—just as you would configure rules for outbound and inbound voice calls.



