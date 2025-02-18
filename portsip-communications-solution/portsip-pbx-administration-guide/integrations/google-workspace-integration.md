# Google Workspace Integration

The PortSIP PBX integrates with Google Workspace to provide the following features:

* Send email notifications through Google Workspace with oAuth.

Since Google stopped supporting "[**Less secure apps**](https://support.google.com/accounts/answer/6010255?hl=en)" to send emails from 3rd applications, you must configure the Google Workspace Integration with PortSIP PBX to allow the PBX to utilize the GMail to send the email notifications.

## Pre-Requisites <a href="#prerequisites" id="prerequisites"></a>

* You need PortSIP PBX running on a static public IP address.
* A web domain (which is FQDN) in PortSIP PBX with a valid SSL certificate. The certificate should be issued by a trusted certificate provider such as Digicert, Thawte, Godaddy, etc. You can read this [article ](../certificates-for-tls-https-webrtc/)to configure the SSL certificate.
* Requires the PBX system administrator or tenant who wants to enable the Google Workspace integration to have a Google account (Workspace or normal Google account).

## Configuring Settings on the Google Side

Please follow the below steps to configure the Google integration.

### Creating a Web App in Your Google Account <a href="#create-app" id="create-app"></a>

* Log in to your Google account and open your [Google Cloud Console](https://console.cloud.google.com/home/dashboard).
* Click on the **CREATE PROJECT** to create a new project.

<figure><img src="../../../.gitbook/assets/google-integration-1.png" alt=""><figcaption></figcaption></figure>

* Enter a project name and select an organization and location from the dropdowns. If you are using a normal Gmail account, for the **Organization** you can choose **No organization**. Click the **Create** button.

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

* **App name:** Enter an app name of your choice (e.g., Pattie’s App).
* **User support email:** Select your email address from the choices provided.
* **App logo:** If you’d like, you can upload a logo for your app. This is optional.
* **Developer contact information:** add your email address in the **Email addresses** field. Then click on the **SAVE AND CONTINUE** button to proceed to the next step.

<figure><img src="../../../.gitbook/assets/google-integration-6.png" alt=""><figcaption></figcaption></figure>









