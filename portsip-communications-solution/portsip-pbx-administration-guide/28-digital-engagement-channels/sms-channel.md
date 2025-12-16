# SMS Channel

PortSIP PBX seamlessly integrates SMS functionality with the following SIP trunk providers:

* [Vonage](https://www.vonage.com/)
* [QuestBlue](https://questblue.com/)
* [VoIP.ms](https://voip.ms/)
* [Voxtelesys](https://voxtelesys.com/)
* [Bandwidth](https://www.bandwidth.com/)
* [Flowroute](https://flowroute.com/)
* [Wavix](https://wavix.com/)
* [Twilio](https://www.twilio.com/en-us)
* [Telnyx](https://telnyx.com/)
* [VoIP Innovations](https://carrierservices.sangoma.com/)
* [Aire Networks](https://airenetworks.es/)
* [VoiceMeUp](https://www.voicemeup.com/)

If you’re using any of these providers, setting up the SMS channel with PortSIP PBX is straightforward.

If your favorite SIP trunk provider for SMS is not listed above, feel free to [email us with your request](mailto:support@portsip.com), and we’ll explore the possibility of integrating it with PortSIP PBX.

### 1. Preparing to Configure SMS in PortSIP PBX

Before configuring the SMS channel in PortSIP PBX, make sure your SMS service is fully set up with your SIP trunk provider by following the steps outlined in the [_Configuring SIP Trunks_](../../configuring-sip-trunks/) guide.

### **2. Full Chain SSL Certificates**

*   To ensure successful webhook verification by the SMS provider, your PortSIP PBX must be configured with a **trusted, full-chain SSL certificate**.

    You can verify whether your SSL certificate is trusted and includes the complete certificate chain using one of these tools:

    * [SSL Checker](https://www.sslchecker.com/sslchecker)
    * [SSL Shopper](https://www.sslshopper.com/ssl-checker.html)
    * [Key CDN SSL Checker](https://tools.keycdn.com/ssl)

    If your certificate is not a full-chain SSL certificate, contact your SSL certificate provider to have it corrected.

### 3. Configuring the SMS Channel in PortSIP PBX

In this article, we demonstrate configure SMS integration with PortSIP PBX. For the [QuestBlue ](https://www.questblue.com)trunk, the DID number _**12172074422**_ will route inbound SMS messages to extension 1001.&#x20;

First, ensure you have completed this setup: [QuestBlue SMS Integration](../../configuring-sip-trunks/questblue-sip-trunk/questblue-sms-integration.md).

### 4. Creating an Inbound Rule for SMS

1. Sign in to the PortSIP PBX web portal.
2. Navigate to **Call Manager > Inbound Rules** and click the **Add** button.
3. Complete the inbound rule setup as follows:
   * Enter a descriptive name for the rule.
   * Select the **QuestBlue** trunk you configured.
   * In the **DID/DDI Number or Number Range** field, enter the DID number: **12172074422**.
   * Set the destination to extension **1001**.
4. Click **OK** to save your changes.

<figure><img src="../../../.gitbook/assets/inbound-sms-rule.png" alt=""><figcaption></figcaption></figure>

Now, register the PortSIP ONE app to the PBX using extension 1001. Once registered, any SMS sent to the number _**+12172074422**_ will be received by extension 1001 directly in the PortSIP ONE app.

{% hint style="info" %}
With the above inbound rule, calls to the DID +12172074422 will also be routed to extension 1001.
{% endhint %}

### 5. Sending Outbound SMS

With the PortSIP ONE app, you can send outbound SMS messages through PortSIP PBX. If you have registered a Sender ID with your trunk provider and wish to enable the extension to send outbound SMS displaying this Sender ID, make sure to specify the Sender ID in the SMS settings in the PortSIP PBX web portal, as shown below. For the Sender ID, please refer to: [The Alphanumeric Sender ID.](https://www.twilio.com/docs/glossary/what-alphanumeric-sender-id) For more details, please consult your SMS provider.

<figure><img src="../../../.gitbook/assets/sms_sender_id.png" alt=""><figcaption></figcaption></figure>

#### 5.1 Granting SMS Permissions to an Extension

By default, extensions are not permitted to send SMS. To enable an extension to send outbound SMS, the tenant administrator must grant permission. Follow these steps to allow SMS for an extension:

1. Sign in to the PortSIP PBX web portal.
2. Navigate to **Call Manager > Users**, double-click the desired extension, and go to the **Extension** tab.
3. Under the **Send SMS/MMS** option, select **Allow** or **Allow Send SMS with Sender ID**.

#### 5.2 Configuring Outbound Caller ID for SMS

In the **Outbound Caller ID** section, you can set up the outbound Caller ID for the extension:

1. Click the **Add** button.
2. Select the configured SIP trunk for SMS sending.
3. Enter a descriptive label in the **Description** field.
4. Enter a DID number from the trunk’s DID pool range.
5. Save your settings.

Refer to the screenshot below for further guidance.

<figure><img src="../../../.gitbook/assets/extension_sms_sender_id.png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
**Important**: Some SMS providers such as Telnyx, they require the phone number should be the E164 foramt. Please see the below screenshot, you need to ask your SMS provider to confirm that.
{% endhint %}

<figure><img src="../../../.gitbook/assets/extension_sms_sender_id_1.png" alt=""><figcaption></figcaption></figure>

#### 5.3 Sending an SMS to a Contact

To send an SMS to a contact using the PortSIP ONE app:

1. **Open Contacts**: In the PortSIP ONE app, go to the **Contacts** menu, select a contact, and click the SMS icon to open the SMS chat UI.

<figure><img src="../../../.gitbook/assets/portsip_pbx_sms1.png" alt=""><figcaption></figcaption></figure>

2. **Send SMS**: Type your message as you would in a regular chat. You can also select the destination number and the sender's number to display to the recipient.

<figure><img src="../../../.gitbook/assets/portsip_pbx_sms2.png" alt=""><figcaption></figcaption></figure>

#### 5.4 Sending an SMS Directly to a Phone Number

To send an SMS directly to a contact's phone number using the PortSIP ONE app, follow these steps:

1. Click the **+** button at the top of the PortSIP ONE app and select **Send a Message**.
2. In the pop-up window:
   * You can choose an extension (to send an IM) or an existing contact to send the SMS.
   * To send an SMS directly to a number, simply enter the phone number in the provided field.
3. Click the green message icon to open the SMS chat interface.

<figure><img src="../../../.gitbook/assets/portsip_pbx_sms3.png" alt=""><figcaption></figcaption></figure>

4. **Send SMS**:
   * Type your message in the chat window, just as you would in a regular chat.
   * You can also select the destination phone number and specify the sender's number that will appear to the recipient.

