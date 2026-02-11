# Google Workspace Integration

PortSIP PBX integrates with **Google Workspace** to provide the following capabilities:

* **User synchronization** from Microsoft 365 or Microsoft Entra ID (including on-premises Active Directory synchronized to the cloud using Azure AD Connect)
* **Single Sign-On (SSO)**, allowing users to sign in to the PortSIP PBX Web Portal and PortSIP ONE app using their Microsoft account
* Send email notifications through Google Workspace using **OAuth authentication**

> **Notice**
>
> Since Google has discontinued support for [“Less secure apps”](https://support.google.com/accounts/answer/6010255?hl=en) for sending emails from third-party applications, you must configure Google Workspace integration with PortSIP PBX to allow the PBX to use Gmail for sending email notifications securely.

***

### Prerequisites

Before configuring Google Workspace integration, ensure the following requirements are met:

* **PortSIP PBX** is running on a **static public IP address**
* A configured **web domain (FQDN)** for the PBX with a **valid SSL certificate**
  * The certificate must be issued by a trusted Certificate Authority (CA), such as DigiCert, Thawte, or GoDaddy
  * Refer to the [SSL certificate configuration guide for detailed instructions](../certificates-for-tls-https-webrtc/)
* The PBX **System Administrator** or **Tenant Administrator** must have a **Google account**:
  * A Google Workspace account
* You must **enable 2FA** with your Google account. Please see the screenshot below

<figure><img src="../../../.gitbook/assets/google_2fa_enabled.png" alt=""><figcaption></figcaption></figure>

Once these prerequisites are satisfied, you can proceed with configuring Google Workspace integration.

***

### Collect Authorized Redirect URI

1. Sign in to the PortSIP PBX Web Portal.
2. Navigate to **Integrations > Google Workspace**.
3. Copy the **Authorized Redirect URI** displayed on the page, and noted it.

If you have set up the **PortSIP SBC** with the PortSIP PBX, there will be **two Authorized Redirect URIs**. Make sure to copy **both** URIs, as they will be required in the Google OAuth configuration.

<figure><img src="../../../.gitbook/assets/google-workspace-11.png" alt=""><figcaption></figcaption></figure>

***

### Configuring Settings on the Google Side

Follow the steps below to configure Google integration for PortSIP PBX.

#### Creating a Web App in Your Google Account

1. Sign in to your Google account and open the [Google Cloud Console](https://console.cloud.google.com/home/dashboard).
2. Click **CREATE PROJECT** to create a new project.

<figure><img src="../../../.gitbook/assets/google-integration-1.png" alt=""><figcaption></figcaption></figure>

3. Enter a **Project name**.
4. Select an **Organization** and **Location** from the drop-down lists. If you are using a standard Gmail account, you can select **No organization**.
5. Click **Create** to proceed.

<figure><img src="../../../.gitbook/assets/google-workspace-1.png" alt=""><figcaption></figcaption></figure>

***

#### Enabling the Gmail API

To allow PortSIP PBX to send emails via Google Workspace, you must enable the Gmail API for the project.

1. In the Google Cloud Console, open **APIs & Services > Library**.
2. In the search bar, enter **Gmail API**.
3. Click the **Gmail API** result.
4. On the Gmail API page, click **ENABLE**.

<figure><img src="../../../.gitbook/assets/google-integration-3.png" alt=""><figcaption></figcaption></figure>

***

#### Enabling Admin SDK API and Google People API

To enable Google Workspace SSO and user sync, you need to enable the Admin SDK API and People API.

1. In the Google Cloud Console, open **APIs & Services > Library**.
2. In the search bar, enter **Admin SDK API**
3. Click the **Admin SDK API** result.
4. On the **Admin SDK API** page, click **ENABLE**.
5. Repeat the steps for enabling the **People API**

<figure><img src="../../../.gitbook/assets/google-workspace-2.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/google-workspace-3.png" alt=""><figcaption></figcaption></figure>

***

#### Configuring OAuth&#x20;

After enabling the API permissions, navigate to the OAuth consent screen page.

1. Click **GET START**

<figure><img src="../../../.gitbook/assets/google-workspace-4.png" alt=""><figcaption></figcaption></figure>

2. On the next page, Google will ask to fill up the App name and user support email, see the screenshot below.

<figure><img src="../../../.gitbook/assets/google-workspace-5.png" alt=""><figcaption></figcaption></figure>

4. Click Next to choose the appropriate "Audience", for example, choose "External".

<figure><img src="../../../.gitbook/assets/google-workspace-6.png" alt=""><figcaption></figcaption></figure>

4. Click **NEXT** to continue enter the email for the Contact Information.

<figure><img src="../../../.gitbook/assets/google-workspace-7.png" alt=""><figcaption></figcaption></figure>

5. Click Next, then choose agree the User Data Pol6. icy, and click CREATE.

<figure><img src="../../../.gitbook/assets/google-workspace-8.png" alt=""><figcaption></figcaption></figure>

6. If you choose "External" for the evidence in previous step, you need to click the PUBLISH APP after the app is created. See the screenshot below.

<figure><img src="../../../.gitbook/assets/google-workspace-9.png" alt=""><figcaption></figcaption></figure>

7. Click the CONFIRM button to publish the app

<figure><img src="../../../.gitbook/assets/google-workspace-10.png" alt=""><figcaption></figcaption></figure>

8. Navigate to menu APIS & Services > Credentials

<figure><img src="../../../.gitbook/assets/google-workspace-12.png" alt=""><figcaption></figcaption></figure>

9. Click + CREATE CREDENTIALS, chose OAuth client ID

<figure><img src="../../../.gitbook/assets/google-workspace-13.png" alt=""><figcaption></figcaption></figure>

10. Enter the necessary information, for example:

* Application type: Choose Web application
* Name: PBX
* Authorized redirect URIs: Click +ADD URI, paste the Authorized Redirect URI which you collect at the previous step. If the PortSIP PBX provides two redirect URIs, make sure to add **both** URIs.

Click the CREATE to create it.&#x20;

<figure><img src="../../../.gitbook/assets/google-workspace-14.png" alt=""><figcaption></figcaption></figure>

11. Once the application is created, the screenshot below will be displayed, please copy the Client ID and Client secret and noted them.

<figure><img src="../../../.gitbook/assets/google-workspace-15.png" alt=""><figcaption></figcaption></figure>

***

### Configuring Settings on the PortSIP PBX Side

Now sign in to the PortSIP PBX web portal to configure settings.

1. Sign in to the PortSIP PBX Web Portal.
2. Navigate to **Integrations > Google Workspace**.
3. Paste the copied Client ID and Client secret to the PBX settings page, and click OK to save settings.

<figure><img src="../../../.gitbook/assets/google-workspace-16.png" alt=""><figcaption></figcaption></figure>

2. After saving, click the **Complete the authorization** hyperlink. You will be redirected to Google to complete the OAuth authorization process.

<figure><img src="../../../.gitbook/assets/google-workspace-17.png" alt=""><figcaption></figcaption></figure>

3. Once authorization is complete, the Google Workspace integration is successfully configured. You can now use OAuth to send email notifications from PortSIP PBX through the Google Mail service.<br>

***





### Updating the Publishing Status from Testing to Production

By default, Google places newly created apps in **Internal** mode. It is essential to switch the app to **External** mode and publish it; otherwise, the application will be heavily restricted and will not function correctly with Google Workspace integration.

#### For Google Workspace Users

1. In the Google Cloud Console, navigate to **APIs & Services > OAuth consent screen**.
2. Under **User type**, click **MAKE EXTERNAL**.

<figure><img src="../../../.gitbook/assets/google-integration-12.png" alt=""><figcaption></figcaption></figure>

3. In the pop-up window, select **In production**.
4. Click **CONFIRM** to apply the change.

<figure><img src="../../../.gitbook/assets/google-integration-13.png" alt=""><figcaption></figcaption></figure>

***

#### Gmail Users

If you are not using Google Workspace, you will not see the **MAKE EXTERNAL** option. Instead, you need to publish your Google app manually.

1. In the Google Cloud Console, navigate to **APIs & Services > OAuth consent screen**.
2. Under **Publishing status**, you will see the app status set to **Testing**.
3. Click **PUBLISH APP** to change the status.

<figure><img src="../../../.gitbook/assets/google-integration-14.png" alt=""><figcaption></figcaption></figure>

4. In the confirmation dialog that appears, click **CONFIRM** to publish the app.

After publishing, the app will be available for production use and can be used by PortSIP PBX to send email notifications through Gmail.

<figure><img src="../../../.gitbook/assets/google-integration-15.png" alt=""><figcaption></figcaption></figure>

***

### Granting Google / Gmail Permissions

1. In the Google Cloud Console, click **Credentials** in the left-hand menu to open the **Credentials** page.
2. Under the **OAuth 2.0 Client IDs** section, locate the web application you just created.
3. Click the application name to view its details.

<figure><img src="../../../.gitbook/assets/google-integration-16.png" alt=""><figcaption></figcaption></figure>

4. On the application details page, copy both the **Client ID** and **Client Secret**.

<figure><img src="../../../.gitbook/assets/google-integration-17.png" alt=""><figcaption></figcaption></figure>

5. Sign in to the **PortSIP PBX Web Portal** and navigate to **Integrations > Google Workspace**.
6. Paste the **Client ID** and **Client Secret** into the corresponding fields.
7. Click **OK** to save the settings.

<figure><img src="../../../.gitbook/assets/google-integration-18.png" alt=""><figcaption></figcaption></figure>

8. After saving, click the **Complete the authorization** hyperlink. You will be redirected to Google to complete the OAuth authorization process.

<figure><img src="../../../.gitbook/assets/google-integration-19.png" alt=""><figcaption></figcaption></figure>

9. Once authorization is complete, the Google Workspace integration is successfully configured. You can now use OAuth to send email notifications from PortSIP PBX through the Google Mail service.



