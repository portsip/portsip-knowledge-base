# WhatsApp Channel

With over 2 billion monthly users, WhatsApp stands as one of the most popular consumer messaging and communication platforms worldwide. This guide will walk you through the process of configuring your WhatsApp Business account on PortSIP PBX, enabling your extension users to communicate directly with customers via WhatsApp.

## **Prerequisites**

To integrate WhatsApp Business with PortSIP PBX, ensure the following requirements are met:

1. **WhatsApp Business Platform Account**:
   * You must have a WhatsApp Business Platform account set up with an associated phone number.
   * The phone number must not be registered with any other WhatsApp account, whether on a device or virtually.
   * If you wish to use a number already linked to an existing WhatsApp account, you must first delete the account. [Read more here](https://faq.whatsapp.com/605464643328528/?locale=en\_US).
2. **Inbound Messaging Only**:
   * WhatsApp integration with PortSIP PBX supports responding to inbound messages only.
   * A WhatsApp user must initiate the conversation by sending you a message.
   * Once a message is received, you have a 24-hour window to respond.

## Configuring WhatsApp

### Register for an Official Account on WhatsApp

To set up your WhatsApp Business account for integration, follow these steps:

1. **Log In to the Meta Developers Portal**
   * Navigate to [https://developers.facebook.com](https://developers.facebook.com) and click **Log In** at the top right.
2. **Create a New App**
   * Go to **My Apps** and click on **Create App**.
   * Select **Other** and click **Next**.
   * Choose **Business** as the app type, then click **Next**.
3. **Set Up App Details**
   * Enter a **Display Name** for your app.
   * Use the drop-down menu to select your **Business Account**.
   * Click **Create App**.
   * Re-enter your password when prompted, then click **Submit**.
4. **Add WhatsApp to Your App**
   * On the next screen, scroll down to the **WhatsApp** section and click **Set Up**.
   * In the "Welcome to the WhatsApp Business Platform" section, click **Start Using the API**.
5. **Configure Your Phone Number**
   * Use the drop-down menu to select the phone number you will use for sending and receiving messages.
   * Copy the **Phone Number ID** and save it for later use.

### Setting Up an Admin User

1. **Access Business Settings**
   * Click on the menu icon in the top-left corner of the [Meta Business Settings](https://business.facebook.com/settings/) page.
   * Select **Business Settings** from the menu.

<figure><img src="../../../.gitbook/assets/whatsapp_portsip1.png" alt="" width="528"><figcaption></figcaption></figure>

2. **Add a System User**
   * Navigate to **Users > System Users** and click **Add**.
   * Accept the non-discrimination policy and click **Done**.
3. **Configure the System User**
   * Set a name for the system user (e.g., `portsip`).
   * Assign the **Admin** role to the user and click the **Create system user** button.

<figure><img src="../../../.gitbook/assets/whatsapp_portsip2.png" alt="" width="330"><figcaption></figcaption></figure>

4. **Add Assets to the System User**
   * Click **Add Assets**.
   * In the pop-up box, select **Apps**, then choose your app from the list.
   * Enable the **Manage App** option.
   * Click **Save Changes** to confirm.
5. **Link the WhatsApp Business Account**
   * From the left-hand menu, navigate to **Accounts > WhatsApp Accounts**.
   * Select your WhatsApp Business account and click **Add People** to assign the user to the account.
6. Select the system user you created, enable the **Full Control** option, and click **Assign** to save the changes.

### Generate a New Token

1. Navigate to **Users > System Users** from the left-hand menu and click **Generate New Token**.
2. In the pop-up, select your app from the drop-down list.
3. Check the permissions for **whatsapp\_business\_messaging** and **whatsapp\_business\_management**.
4. Click **Generate Token**.
5. Copy the access token displayed in the pop-up box and save it securely, as it will not be accessible again. This is a permanent token that will remain valid unless you click the **Revoke Token** button.

## Configuring PortSIP PBX

To integrate the WhatsApp account you created with PortSIP PBX, follow these steps:

### **Add WhatsApp as a SIP Trunk**

1. Access the PortSIP PBX web portal and log in as a **Tenant Manager**.
2. Navigate to the menu **Call Manager > Trunks, c**lick the **Add** button, and select **WhatsApp** as the trunk type.
3. **Configure the Trunk**
   * Enter a name for the trunk (e.g., **WhatsApp Message**).
   * In the **DID Pool** field, enter the phone number associated with your WhatsApp account.
4. Click the **OK** button to save changes.

<figure><img src="../../../.gitbook/assets/whatsapp_portsip3.png" alt="" width="563"><figcaption></figcaption></figure>

### Add a WhatsApp Channel

1. Navigate to **Message Channels > WhatsApp** and click **Add**.
2. Enter a name for the channel (e.g., **WhatsApp Channel**) and select the WhatsApp trunk you added earlier.
3. Input the phone number associated with your WhatsApp account in the **Phone Number** field.
4. Paste the **Access Token** from the [_Generate a New Token_](whatsapp-channel.md#generate-a-new-token) section.
5. Ensure the **Enable** option is turned on, then click **OK** to save your changes.

<figure><img src="../../../.gitbook/assets/whatsapp_portsip4.png" alt=""><figcaption></figcaption></figure>

## Configure Webhook in WhatsApp

To enable PortSIP PBX to receive inbound WhatsApp messages, follow these steps:

1. **Copy the Webhook URL**
   * Go to **Message Channels > WhatsApp** in the PortSIP PBX Web portal.
   * Select the WhatsApp Channel and click the **Copy Webhook** button to copy the Webhook URL.

<figure><img src="../../../.gitbook/assets/whatsapp_portsip5.png" alt=""><figcaption></figcaption></figure>

2. **Access the Meta Developer Portal**
   * Log in to the [Meta for Developers](https://developers.facebook.com) portal and locate your app.
   * Navigate to **WhatsApp > Configuration**, and paste the Webhook URL copied from the PortSIP PBX.
   * In the token verification field, enter a random string of letters and numbers. (This token is used only for verification and doesn’t need to be saved for future use.)
   * Click **Verify and Save** to confirm the configuration.

<figure><img src="../../../.gitbook/assets/whatsapp_portsip6.png" alt=""><figcaption></figcaption></figure>

3. **Enable Webhook Fields**
   * In the **Webhook Fields** section, toggle the switch to **Subscribed**.
   * A notification will confirm that the subscription was successful.

<figure><img src="../../../.gitbook/assets/whatsapp_portsip7.png" alt=""><figcaption></figcaption></figure>

4. Now you can send a WhatsApp message to your business phone number to verify the integration.











