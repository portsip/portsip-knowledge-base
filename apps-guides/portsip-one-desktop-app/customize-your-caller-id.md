# Customize Your Caller ID

**Caller ID** allows call recipients to see who is calling before they answer. A properly configured Caller ID improves call recognition, professionalism, and trust, especially for outbound business calls.

***

### Default Caller ID Behavior

By default, PortSIP PBX automatically determines the outbound Caller ID based on the system configuration.\
This ensures that outbound calls present consistent, accurate, and policy-compliant caller information to recipients.

***

### Customizing Caller ID

While the default Caller ID configuration meets most requirements, there are situations where you may want to customize it—either for specific calls or across the organization.

PortSIP PBX provides flexible Caller ID controls that allow customization:

* **Globally**, at the company level
* **Per group**, for departments or teams
* **Per user**, for individual extensions
* **Per call**, when placing a call from supported clients

> ❗**Note**\
> System administrators can restrict which Caller ID values are allowed. This helps prevent misuse, spoofing, or non-compliant outbound calling, and ensures alignment with company security and regulatory policies.

***

### Caller ID Configuration Priority

A user’s outbound Caller ID can be configured in multiple locations. When multiple settings exist, **PortSIP PBX applies them using the following priority order (highest to lowest):**

#### 1. Extension-Level Settings

Caller ID can be defined individually for each extension.\
This allows personalized outbound Caller ID values for specific users.

#### 2. User Group Settings

Caller ID can be configured at the **User Group** level.\
If a user belongs to a group with a defined Caller ID, the group setting takes precedence over the extension-level setting (unless explicitly overridden).

#### 3. Company-Level Settings

Caller ID can be defined globally in the PortSIP PBX web portal under: **Company > Outbound Caller ID**

This setting applies to all users who do not have a specific Caller ID configured at the extension or group level.

***

### Adjusting Caller ID When Placing a Call

In addition to administrative configuration, users can adjust the Caller ID **at the time of dialing**, depending on the client being used.

Caller ID can be adjusted from:

* The **Keypad**
* The **Conversation Thread**
* **PortSIP mobile apps**  ([iOS ](https://www.portsip.com/portsip-one/)and [Android](https://www.portsip.com/portsip-one/)).

> ❗**Limitation**\
> Caller ID selection is **not currently supported on desk IP phones**.

***

### Adjust Caller ID from the Keypad

Use the following steps to select a Caller ID before making a call:

1. Click the **Keypad** icon in the application title bar. The dialing keypad opens.
2. Click the **arrow icon** next to **Call as**. A drop-down list of available Caller ID options appears.
3. Select the desired **Caller ID number** from the list. The selected Caller ID is applied to the call.
4. Enter the destination number or select a contact, then press **Enter** to start dialing. Then the call is placed using the selected Caller ID.

<figure><img src="../../.gitbook/assets/caller_id_1.png" alt=""><figcaption></figcaption></figure>

***

### Adjust Caller ID from the Contact Details View

Caller ID can also be selected directly from a contact’s details view before placing a call. This allows you to choose the most appropriate outbound Caller ID for the specific contact you are calling.

#### To adjust Caller ID from the contact details view:

1. Open the **Contact Details** view in the **PortSIP ONE** app. The contact’s information and available actions are displayed.
2. Click the **drop-down arrow** next to the **Phone** icon. Additional calling options are shown.
3. In the **From** section, select the desired **Outbound Caller ID**. The selected Caller ID is applied to the call.
4. Click the **Phone** icon to place the call. The call is initiated using the selected Caller ID.

> ❗**Note**\
> The available Caller ID options depend on your administrator’s configuration and any company-wide restrictions applied to outbound calling.

<figure><img src="../../.gitbook/assets/caller_id_2.png" alt=""><figcaption></figcaption></figure>

#### Adjust caller ID from a conversation thread  <a href="#adjust-caller-id-from-a-conversation-thread" id="adjust-caller-id-from-a-conversation-thread"></a>

Caller ID can also be customized in the SMS/WhatsApp conversation thread view.&#x20;

From the conversation thread view in the PortSIP ONE app:

1. Click the drop-down option beside the **Phone** icon&#x20;
2. Navigate to the **From** section
3. Choose an outbound caller ID
4. Click the **Phone** icon to call

<figure><img src="../../.gitbook/assets/caller_id_3.png" alt=""><figcaption></figcaption></figure>

### Block your caller ID <a href="#block-your-caller-id" id="block-your-caller-id"></a>

There may be a time when you need to block your caller ID.&#x20;

#### To block caller ID when placing a call from the Keypad

1. Click the **Drop-Down icon** alongside the **Call as**
2. Select **Block caller ID**

<figure><img src="../../.gitbook/assets/caller_id_4.png" alt=""><figcaption></figcaption></figure>

#### To block caller ID from the contact details view

1. Click the drop-down arrow icon beside the **Phone** icon&#x20;
2. Navigate to the **From** section
3. Choose **Block caller ID**

<figure><img src="../../.gitbook/assets/caller_id_5.png" alt=""><figcaption></figcaption></figure>

#### Block caller ID from a conversation thread  <a href="#adjust-caller-id-from-a-conversation-thread" id="adjust-caller-id-from-a-conversation-thread"></a>

1. Click the drop-down option beside the **Phone** icon&#x20;
2. Navigate to the **From** section
3. Choose **Block caller ID**

<figure><img src="../../.gitbook/assets/caller_id_6.png" alt=""><figcaption></figcaption></figure>

### Let the PBX Decide on Delivery Caller ID

If you prefer not to manually select the caller ID when placing a call, you can configure it to use the **Server delivery caller ID**.

<figure><img src="../../.gitbook/assets/caller_id_7.png" alt=""><figcaption></figcaption></figure>

In the **From** section, choose the **Server delivery caller ID** option. With this setting, when the app places the call and route through the SIP trunk,  the PBX will automatically select the appropriate caller ID based on its configuration.

For further details on how the PBX determines the outbound caller ID, please refer to the article: [**Handle Outbound Calls Through SIP Trunk**](../../portsip-communications-solution/portsip-pbx-administration-guide/7-trunk-management/handle-outbound-calls-through-sip-trunk.md#outboundcallerid), which explains how the PBX dynamically chooses the correct caller ID.

Note: This logic is typically applied to desk IP phones as well, which always follow this automatic caller ID selection process.

