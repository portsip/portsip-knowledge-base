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

## Register for an Official Account on WhatsApp

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

## Setting Up an Admin User

1. **Access Business Settings**
   * Click on the menu icon in the top-left corner of the [Meta Business Settings](https://business.facebook.com/settings/) page.
   * Select **Business Settings** from the menu.

<figure><img src="../../../.gitbook/assets/whatsapp_portsip1.png" alt=""><figcaption></figcaption></figure>

1. **Add a System User**
   * Navigate to **Users > System Users** and click **Add**.
   * Accept the non-discrimination policy and click **Done**.
2. **Configure the System User**
   * Set a name for the system user (e.g., `portsip`).
   * Assign the **Admin** role to the user.





