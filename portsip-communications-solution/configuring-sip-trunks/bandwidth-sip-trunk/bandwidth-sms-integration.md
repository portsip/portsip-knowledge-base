# Bandwidth SMS Integration

Before proceeding, ensure that you have [purchased a DID on the Bandwidth platform](purchase-a-did-on-bandwidth-platform.md) with SMS enabled.

### Important: 10DLC Registration (U.S.)

> **Compliance Notice**\
> If you are using:
>
> * A newly added phone number
> * A newly created carrier account
> * Or SMS messaging has stopped working unexpectedly
>
> You **must complete the carrier’s 10DLC registration process** before using SMS in PortSIP PBX.

* **10DLC registration is mandatory for all supported U.S. carriers**
* This requirement is enforced by wireless carriers for **all A2P messaging traffic**

Failure to complete 10DLC registration may result in message blocking or delivery failures.

***

### Obtain Integration Details from Bandwidth

PortSIP PBX requires several identifiers from the Bandwidth platform to enable SMS/MMS integration.

***

#### Step 1: Obtain the Bandwidth User ID

1. Log in to your [Bandwidth dashboard](https://dashboard.bandwidth.com/).
2. After logging in, locate the **User ID**:
   * In the **Overview** section, or
   * In the top-right corner next to **Account Settings**

> **Note**\
> The User ID is required later as the **Account ID** in PortSIP PBX.

***

#### Step 2: Create and Obtain the Application ID

The **Application ID** is generated when you create a messaging application.

1. In the Bandwidth dashboard, go to **Applications**.
2. Click **Create New Application**.
3. Enter an **Application Name** (for example, _PortSIP-SMS_).
4. Select **Messaging** as the application type.
5.  Set the **Callback URL**.\
    At this stage, you may use a temporary (fake) URL, such as:

    ```
    https://portsip.com/sms/bandwidth
    ```
6. Save the application.

**Expected Result**

* Bandwidth generates an **Application ID**, which you will use later in PortSIP PBX.

***

### Configure SMS with Bandwidth Trunk in PortSIP PBX

Please ensure a Bandwidth SIP trunk has already been configured in PortSIP PBX using:

* [Configuring Bandwidth IP Authentication Trunk](configuring-bandwidth-ip-authentication-trunk.md)

***

#### Sign in to the PortSIP PBX Web Portal

You can sign in using one of the following methods:

**Option 1: System Administrator**

1. Sign in as a **PBX System Administrator**.
2. Go to **Tenants**.
3. Select the target tenant.
4. Click **Manage** to switch into that tenant.

**Option 2: Tenant Administrator**

* Sign in directly as a **Tenant Admin**. For more details, see [Tenant Management](../../portsip-pbx-administration-guide/3-tenant-management/).

***

### Add an SMS Configuration in PortSIP PBX

#### Step 1: Create the SMS Configuration

1. In the PortSIP PBX Web Portal, navigate to: **Message Channel > SMS/MMS**
2. Click **Add**.
3. Select your configured **Bandwidth Trunk**.
4.  Configure the following fields:

    **Sender ID**

    * Enter a Sender ID created on the Bandwidth platform if you want to use a custom sender.
    * Leave this field empty to use the **DID associated with the Bandwidth trunk**.

    **Account ID**

    * Enter the **User ID** obtained earlier.

    **Application ID**

    * Enter the **Application ID** created earlier.

    **Username**

    * Enter your **Bandwidth account username**.

    **Password**

    * Enter the **password** associated with your Bandwidth account.
5. Click **OK** to save the configuration.

<figure><img src="../../../.gitbook/assets/bandwidth_trunk_6.png" alt=""><figcaption></figcaption></figure>

***

#### Step 2: Copy the PortSIP PBX Webhook URL

Inbound SMS messages are delivered to PortSIP PBX through a webhook.

1. On the **SMS/MMS** list page, select the SMS configuration you just created.
2. Click **Copy Webhook**,\
   **or**
3. Double-click the configuration and manually copy the **Webhook URL**.

***

#### Step 3: Update the Callback URL in Bandwidth

1. Sign in to the **Bandwidth Dashboard**.
2. Navigate to **Applications**.
3. Open the messaging application you created earlier.
4. Replace the existing (fake) **Callback URL** with the **Webhook URL** copied from PortSIP PBX.
5. Click **Save** to apply the changes.

***

#### Verify the Configuration

At this point, the Bandwidth SMS/MMS integration is complete.

You can now [create outbound and inbound rules](configuring-outbound-and-inbound-calls.md) in PortSIP PBX to send and receive SMS/MMS messages using the VoIP.ms trunk, just as you would configure rules for voice calls.



