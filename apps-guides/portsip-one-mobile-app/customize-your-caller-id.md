# Customize Your Caller ID

**Caller ID** is an essential feature that allows call recipients to see who is calling before answering. It improves transparency, trust, and professionalism in business communications.

### Default Caller ID Behavior

By default, **PortSIP PBX** determines the outbound Caller ID based on system configuration.\
This centralized approach ensures consistency and accuracy in the caller information presented to recipients.

***

### Customizing Caller ID

While default settings cover most use cases, there are situations where you may need to customize the Caller ID—either for specific calls or across the organization.

PortSIP PBX provides flexible Caller ID control at multiple levels, allowing both **per-call** and **global** customization.

> **Note**\
> Administrators can restrict Caller ID changes across the organization to maintain security, compliance, and control over outbound call identity.

***

### Caller ID Configuration Hierarchy

A user’s outbound Caller ID can be configured in several locations. When multiple settings exist, they are applied in the following order of precedence:

#### 1. Extension-Level Settings

Caller ID can be configured for individual users in their **Extension** settings.\
This allows personalized Caller ID information for specific users.

#### 2. User Group Settings

Caller ID can be configured at the **User Group** level.\
If a user belongs to a group, the group’s Caller ID setting takes precedence over the individual extension setting, unless explicitly overridden.

#### 3. Company-Level Settings

Caller ID can be configured at the **Company** level under\
**Company > Outbound Caller ID**.

This setting applies to all users who do not have a specific Caller ID defined at the extension or group level.

***

### Adjusting Your Caller ID

You can adjust your Caller ID from the following locations:

* Keypad
* Conversation Thread
* Mobile apps ([iOS ](https://www.portsip.com/portsip-one/)and [Android](https://www.portsip.com/portsip-one/)).

> **Limitation**\
> Caller ID cannot currently be modified from **desk IP phones**. Desk phones always follow PBX-defined Caller ID rules.

***

### Adjust Caller ID from the Keypad

To adjust your Caller ID before placing a call:

1. Open the **Keypad** tab.
2. Click the **arrow icon** next to **Call as**.
3. Select the desired **Caller ID** from the drop-down list.
4. Enter the number or select the contact you want to call.
5. Press **Enter** or tap **Dial** to start the call.

<figure><img src="../../.gitbook/assets/portsip-one-mobile-30 (1).png" alt="" width="375"><figcaption></figcaption></figure>

***

### Block Your Caller ID

In some situations, you may want to hide your caller ID when placing a call.

#### Block Caller ID from the Keypad

1. Open the **Keypad** tab.
2. Click the **drop-down arrow** next to **Call as**.
3. Select **Block Caller ID**.

> **Expected Result**\
> The call is placed without sending your caller ID to the recipient.

<figure><img src="../../.gitbook/assets/portsip-one-mobile-31.png" alt="" width="375"><figcaption></figcaption></figure>

***

### Let the PBX Decide the Outbound Caller ID

If you prefer not to manually select a caller ID when placing a call, you can allow the **PBX to automatically determine the outbound caller ID**.

To do this, select **Server delivery caller ID**.\
When this option is enabled, the PBX chooses the appropriate caller ID based on its configuration and routing rules.

<figure><img src="../../.gitbook/assets/portsip-one-mobile-32.png" alt="" width="375"><figcaption></figcaption></figure>

Tap the **drop-down arrow** next to **Call as**, then select **Server delivery caller ID**.

When this option is enabled, the app places the call through the SIP trunk, and the **PBX automatically selects the appropriate outbound caller ID** based on its configuration and routing rules.

For more information on how the PBX determines the outbound caller ID, see [Handle Outbound Calls Through SIP Trunk](../../portsip-communications-solution/portsip-pbx-administration-guide/7-trunk-management/handle-outbound-calls-through-sip-trunk.md#outboundcallerid), which explains how the PBX dynamically chooses the correct caller ID.

> **Note**\
> This automatic caller ID selection logic also applies to **desk IP phones**, which always rely on the PBX to determine the outbound caller ID.



