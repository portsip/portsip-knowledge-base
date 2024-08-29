# Vonage SMS Integration

Before proceeding with the next steps, you need to [purchase a DID on the Vonage platform](purchase-a-did-on-vonage-platform.md) that is SMS/MMS enabled.

## Obtain the Vonage API Key

You need to take the Vonage API key for the PortSIP PBX to send the SMS/MMS. Please follow the below steps:

1. Log in to your account on the [Vonage ](https://ui.idp.vonage.com/ui/auth/login)platform.
2. Click on the Vonage logo in the top left corner to open the **Vonage API Dashboard**.
3. Copy and note the **API Key** and **API Secret**.

<figure><img src="../../.gitbook/assets/vonage-fig4.png" alt=""><figcaption></figcaption></figure>

## Configure SMS with Vonage Trunk in PortSIP PBX

Before configuring SMS in PortSIP PBX, you must have already configured a Vonage SIP trunk using one of the following guides:

* [Configuring Vonage Register Based Trunk](configuring-vonage-register-authentication-trunk.md)
* [Configuring Vonage IP Based Trunk](configuring-vonage-ip-authentication-trunk.md)

### Sign in PortSIP PBX Web Portal

You can sign in to the PortSIP PBX Web portal using one of the following methods:

1. Sign in as the PBX system administrator, navigate to the **Tenants** menu, choose a tenant, and click the **Manage** button to switch to that tenant.
2. Sign in as a tenant admin to manage the tenant.

For more details please reference [Tenant Management](../../portsip-pbx-administration-guide/3-tenant-management.md).

### Add an SMS configuration

Please follow the below steps:

1. In the PortSIP PBX Web portal, navigate to the left menu, select **SMS/MMS**, and click the **Add** button.&#x20;
2. Choose your configured Vonage Trunk :
   * **API Key**: Enter the Vonage API Key you noted in [Obtain the Vonage API Key](vonage-sms-integration.md#obtain-the-vonage-api-key)
   * **Secret**: Enter the Vonage Secret you noted in [Obtain the Vonage API Key](vonage-sms-integration.md#obtain-the-vonage-api-key)

<figure><img src="../../.gitbook/assets/vonage-fig27.png" alt=""><figcaption></figcaption></figure>

3. Click **OK** to be brought to the SMS/MMS list page. You can select that SMS configuration, then press the **Copy Webhook** button to copy the Webhook URL. Or Double-click the SMS configuration to edit the SMS configuration, in the details copy the Webhook URL.

<figure><img src="../../.gitbook/assets/vonage-fig28.png" alt=""><figcaption></figcaption></figure>

## Configure the SMS in Vonage

1. Log in to your account on the [Vonage ](https://ui.idp.vonage.com/ui/auth/login)platform.
2. Navigate to the menu **Messaging > SMS Settings**, the DIDs are listed, and click the pencil icon next to the DID that you want to enable the SMS.
3. In the SMS Settings page, choose **Post SMS to URL** from the combo box, and paste the PortSIP PBX Webhook URL to **URL to post SMS Message** field.

## Verify Configuration

Now you can [create the outbound and inbound rules](configuring-outbound-and-inbound-calls.md) in PortSIP PBX for sending and receiving SMS/MMS using the Vonage Trunk, just like you create the rules for making and receiving calls.

