# Google Workspace Integration

The PortSIP PBX integrates with Google Workspace to provide the following features:

* Send email notifications through Google Workspace with OAuth.

Since Google stopped supporting "[**Less secure apps**](https://support.google.com/accounts/answer/6010255?hl=en)" to send emails from 3rd applications, you must configure the Google Workspace Integration with PortSIP PBX to allow the PBX to utilize Gmail to send the email notifications.

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* You need PortSIP PBX running on a static public IP address.
* A web domain (which is an FQDN) in PortSIP PBX with a valid SSL certificate. The certificate should be issued by a trusted certificate provider such as Digicert, Thawte, GoDaddy, etc. You can read this [article ](../certificates-for-tls-https-webrtc/)to configure the SSL certificate.
* Requires the PBX system administrator or tenant who wants to enable the Google Workspace integration to have a Google account (Workspace or a normal Google account).

## Configuring Settings on the Google Side

Please follow the steps below to configure the Google integration.

### Creating a Web App in Your Google Account <a href="#create-app" id="create-app"></a>

* Log in to your Google account and open your [Google Cloud Console](https://console.cloud.google.com/home/dashboard).
* Click on the **CREATE PROJECT** to create a new project.

<figure><img src="../../../.gitbook/assets/google-integration-1.png" alt=""><figcaption></figcaption></figure>

* Enter a project name and select an organization and location from the dropdowns. If you are using a normal Gmail account, for the **Organization,** you can choose **No organization**. Click the **Create** button.

<figure><img src="../../../.gitbook/assets/google-integration-2.png" alt=""><figcaption></figcaption></figure>

### Enabling the Gmail API

You need to enable the Gmail API for your project.&#x20;

* In your Google Cloud Console sidebar, go to the menu **APIs & Services > Library.**
* Enter **Gmail API** in the search bar and click on the **Gmail API** result.
* On the Gmail API page, click on the blue **ENABLE** button.

<figure><img src="../../../.gitbook/assets/google-integration-3.png" alt=""><figcaption></figcaption></figure>

### Creating Your Application’s Credentials

After you enable the Gmail API, you should be redirected to the Gmail API Overview page.&#x20;

* Click on the **CREATE CREDENTIALS** button.

<figure><img src="../../../.gitbook/assets/google-integration-4.png" alt=""><figcaption></figcaption></figure>

* On the next page, Google will ask a few questions to determine the Credential Type you need. From the **Select an API** dropdown, choose **Gmail API**.

{% hint style="warning" %}
Note: If you don’t see an option for the Gmail API in the dropdown, be sure that you have the Gmail API enabled for your account.
{% endhint %}

* Under **What data will you be accessing?**, select the **User data** option. Then click the **NEXT** button to proceed.

<figure><img src="../../../.gitbook/assets/google-integration-5.png" alt=""><figcaption></figcaption></figure>

### Configuring Your OAuth Consent Screen

Google will then ask for some basic information about your app.

This section is mostly for personal use since no one else will be using your app. However, some fields are still marked as required:

* **App name:** Enter an app name of your choice (e.g., PortSIP PBX App).
* **User support email:** Select your email address from the choices provided.
* **App logo:** If you’d like, you can upload a logo for your app. This is optional.
* **Developer contact information:** add your email address in the **Email addresses** field. Then click on the **SAVE AND CONTINUE** button to proceed to the next step.

<figure><img src="../../../.gitbook/assets/google-integration-6.png" alt=""><figcaption></figcaption></figure>

### Setting Up Your OAuth Client ID

Now sign in to the PortSIP PBX Web Portal, navigate to the menu **Integrations > Google Workspace,** and copy the **Authorized Redirect URI** from the PBX Web Portal.

If you have setup the PortSIP SBC with PortSIP PBX, there will be two **Authorized Redirect URIs,** please copy all of them.

<figure><img src="../../../.gitbook/assets/google-integration-8.png" alt=""><figcaption></figcaption></figure>

Next, you’ll need to fill out some information about your OAuth Client ID in the Google Cloud Console.

From the **Application type** dropdown, select the **Web application** option. Once you do so, more fields will automatically populate. For the name, enter PortSIP PBX Web Portal as an example.

<figure><img src="../../../.gitbook/assets/google-integration-7.png" alt=""><figcaption></figcaption></figure>

Next, skip the **Authorized JavaScript origins** section and scroll to **Authorized redirect URIs**.

Click on the **+ ADD URI** button and paste the **Authorized Redirect URI.** If you have two URIs from the PortSIP PBX web portal, please add both.

<figure><img src="../../../.gitbook/assets/google-integration-9.png" alt=""><figcaption></figcaption></figure>

Then click on the **CREATE** button to complete this step.

Once your app has been created, the **Your Credentials** section will expand to show you your Client ID. There’s no need to copy it now, as you’ll access it from another area in a later step.

Instead, go ahead and click the **DONE** button at the bottom of the page.

<figure><img src="../../../.gitbook/assets/google-integration-11.png" alt=""><figcaption></figcaption></figure>

### Updating the Publishing Status From Testing to Production <a href="#from-testing-to-production" id="from-testing-to-production"></a>

Google will put your app into **Internal** mode by default. It’s really important that you switch it to **External** mode and publish it. Otherwise, your app will be super limited and won’t function properly.

#### Google Workspace Users

In your Google Cloud Console sidebar, go to **APIs & Services » OAuth consent screen**. Under **User type**, click on the **MAKE EXTERNAL** button.

<figure><img src="../../../.gitbook/assets/google-integration-12.png" alt=""><figcaption></figcaption></figure>

In the popup window that appears, select the **In production** option. Then click on **CONFIRM**.

<figure><img src="../../../.gitbook/assets/google-integration-13.png" alt=""><figcaption></figcaption></figure>

#### Gmail Users

If you’re not using Google Workspace, you won’t see the **MAKE EXTERNAL** option. Instead, you’ll need to publish your Google app.

To do so, go to **APIs & Services » OAuth consent screen**. Under **Publishing status**, you’ll see the app status is set to **Testing**. Go ahead and click the **PUBLISH APP** button to update your app status.



<figure><img src="../../../.gitbook/assets/google-integration-14.png" alt=""><figcaption></figcaption></figure>

In the overlay that appears, click **CONFIRM** to publish your app.

<figure><img src="../../../.gitbook/assets/google-integration-15.png" alt=""><figcaption></figcaption></figure>

Once confirmation is complete, you’ll see that your app’s Publishing status is now **In production**.

## Granting Google / Gmail Permissions <a href="#permissions" id="permissions"></a>

Next, click on **Credentials** in the left side menu, and you will be on the **Credentials** page, in the **OAuth 2.0 Client IDs** section, you can see the details of the web application you just created. To view the **Client ID** and **Client Secret**, click the app name.

<figure><img src="../../../.gitbook/assets/google-integration-16.png" alt=""><figcaption></figcaption></figure>

This will open all of the details for your app. On this page, you’ll see the **Client ID** and **Client secret** values.

<figure><img src="../../../.gitbook/assets/google-integration-17.png" alt=""><figcaption></figcaption></figure>

Go ahead and copy both of these values and paste them into the corresponding fields in your PortSIP PBX Web Portal settings **Integrations > Google Workspace**, and then click the OK button to save.

<figure><img src="../../../.gitbook/assets/google-integration-18.png" alt=""><figcaption></figcaption></figure>

After saving the **Client ID** and **Client secret**, now Click the hyperlink **Complete the authorization** to go to Google to complete authorization.

<figure><img src="../../../.gitbook/assets/google-integration-19.png" alt=""><figcaption></figcaption></figure>

You have successfully completed the Google Workspace integration. You can now use OAuth to send email notifications from PortSIP PBX via Google Mail service.

