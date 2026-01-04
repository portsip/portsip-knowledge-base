# VoiceMeUp SMS Integration

{% hint style="warning" %}
According to US legislation (A2P 10DLC SMS), 10DLC (10-digit Long Code) phone numbers that are used for A2P (Application-to-Person) messaging MUST be registered, otherwise SMS messages sent to US numbers from the unregistered 10DLC numbers will be blocked.

If your business communicates with US-based customers, you should contact the SMS service provider to complete 10DLC registration for your DID number to avoid disruption in message delivery.
{% endhint %}

***

### Purchase DID Numbers on the VoiceMeUp Platform

Follow the steps below to purchase a DID with SMS/MMS enabled.

#### Step 1: Sign In to the VoiceMeUp Customer Portal

1. Log in to the [VoiceMeUp customer portal](https://clients.voicemeup.com/v3/login) using your account credentials.
2. From the left-hand navigation menu, go to: **Services > DID Numbers**
3. Click **New** at the top of the page.

***

#### Step 2: Enable SMS/MMS for the DID

1. In the pop-up window, select one of the following options:
   * `SMS Only`, or `MMS/SMS`
2. Click **Submit** to complete the process.

<figure><img src="../../../.gitbook/assets/voicemeuo_trunk_6.png" alt="" width="563"><figcaption></figcaption></figure>

***

### Obtain VoiceMeUp Account Information for SMS API Integration

To integrate the VoiceMeUp SMS API with PortSIP PBX, you must create a VoiceMeUp user account with API access and retrieve an Auth Token.

***

#### Step 1: Access the Users & Access Section

1. Log in to your VoiceMeUp account.
2. From the main menu, navigate to: **Account > Users & Access**

***

#### Step 2: Create a New User with API Access

1. Click **New** to create a new user.
2. In the **Account Information** section, configure the following fields:
   * **User Type**\
     Select `Standard`.
   *   **Username**\
       Enter a username (for example, `portsip`).

       > **Important**: Note this username—you will need it later in PortSIP PBX.
   * **First Name / Last Name / Email**\
     Enter the appropriate details.
3. Navigate to the **Access** tab.
4. Click **Regenerate Token** to generate a new **Auth Token**.
5. Click the **copy icon** to copy the Auth Token.
6. Store the token securely.
7. Click **Save** to complete user creation.

***

#### Step 3: Obtain the Auth Token

After the user account is created:

* You are redirected to the **Users & Access list**.
* Open the newly created user’s **details page**.
* In the **Access** tab, locate and copy the **Auth Token**.

> **Security Best Practice**\
> Treat the Auth Token as a sensitive credential.\
> Do not expose it publicly or store it in plaintext files.

***

#### Retrieve the Auth Token Later

If you need to retrieve the Auth Token in the future:

1. Log in to the **VoiceMeUp customer portal**.
2. Navigate to **Account > Users & Access**.
3. Locate the user account and click **Edit**.
4. Open the **Access** tab.
5. Click the **copy icon** next to the **Auth Token**.

<figure><img src="../../../.gitbook/assets/voicemeuo_trunk_7.png" alt=""><figcaption></figcaption></figure>

***

### Configure SMS with the VoiceMeUp Trunk in PortSIP PBX

This section explains how to configure SMS integration in PortSIP PBX using a VoiceMeUp SIP trunk.

***

#### Prerequisite

Before configuring SMS, ensure that:

* A VoiceMeUp SIP trunk has already been configured in PortSIP PBX: [Configuring VoiceMeUp Trunk](configuring-voicemeup-trunk.md)
* A VoiceMeUp user account with API access has been created
* You have obtained the Username and Auth Token from VoiceMeUp

***

#### Sign in to the PortSIP PBX Web Portal

To configure SMS integration, sign in to the PortSIP PBX Web Portal using one of the following methods:

**Option 1: PBX System Administrator**

1. Sign in as a **PBX System Administrator**.
2. Navigate to **Tenants**.
3. Select the target tenant.
4. Click **Manage** to switch to that tenant’s settings.

**Option 2: Tenant Administrator**

* Sign in directly as a **Tenant Administrator** to manage the tenant’s settings.

> **Note**\
> For more details, see [Tenant Management](../../portsip-pbx-administration-guide/3-tenant-management/).

***

### Add an SMS Configuration in PortSIP PBX

#### Step 1: Create the SMS Configuration

1. In the PortSIP PBX Web Portal, navigate to: **SMS/MMS**
2. Click **Add** to create a new SMS configuration.
3. From the trunk list, select the **VoiceMeUp Trunk**.

***

#### Step 2: Configure SMS Settings

Configure the following fields:

* **Sender ID** _(Optional)_
  * Enter the Sender ID created on the VoiceMeUp platform if you want to use a custom sender.
  * Leave this field empty to use the **DID associated with the VoiceMeUp trunk**.
* **Username**
  * Enter the **VoiceMeUp username** created earlier\
    (for example, `portsip`).
* **Auth Token**
  * Enter the **Auth Token** obtained when creating the VoiceMeUp user account.

4. Click **OK** to save the configuration.

<figure><img src="../../../.gitbook/assets/voicemeuo_trunk_8.png" alt=""><figcaption></figcaption></figure>

***

#### Step 3: Copy the PortSIP PBX Webhook URL

Inbound SMS messages are delivered to PortSIP PBX through a webhook.

1. On the **SMS/MMS** list page, select the SMS configuration you just created.
2. Click **Copy Webhook**,\
   **or**
3. Double-click the SMS configuration, open its details, and copy the **Webhook URL**.

> You will need this Webhook URL to complete the SMS callback configuration on the **VoiceMeUp** platform.

***

### Configure the Webhook in VoiceMeUp

This section explains how to configure the **SMS/MMS webhook** in the VoiceMeUp portal so inbound messages are delivered to PortSIP PBX.

***

#### Step 1: Open the DID Configuration

1. Log in to the VoiceMeUp customer portal.
2. From the left-hand navigation menu, go to: **Services > DID Numbers**
3. Locate the DID you want to use for receiving SMS/MMS.
4. Click **Edit** to open the DID configuration.

***

#### Step 2: Enable SMS/MMS and Configure the Webhook

1. In the **Available Options** section:
   * Ensure **SMS/MMS** is **enabled**.
   * Paste the **Webhook URL** copied from the PortSIP PBX SMS configuration into the **Callback URL** field.

<figure><img src="../../../.gitbook/assets/voicemeuo_trunk_9 (1).png" alt=""><figcaption></figcaption></figure>

2. Navigate to the **SMS/MMS Options** tab.
3. Enable the option **SMS/MMS for this DID**.
4. Paste the **same Webhook URL** into the **Callback URL (Webhook)** field.
5. Click **Save** to apply the changes.

> **Note**\
> Using the same Webhook URL in both sections ensures that all inbound SMS/MMS messages are correctly delivered to PortSIP PBX.

<figure><img src="../../../.gitbook/assets/voicemeuo_trunk_10.png" alt=""><figcaption></figcaption></figure>

***

### Verify the Configuration

At this point, the VoiceMeUp SMS/MMS integration is complete.

You can now [create outbound and inbound rules ](../../portsip-pbx-administration-guide/8-call-route-management/)in PortSIP PBX to send and receive SMS/MMS messages using the VoiceMeUp trunk, just as you would configure rules for outbound and inbound voice calls.





