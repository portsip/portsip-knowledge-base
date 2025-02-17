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
* First, you’ll need to choose a project to use for your app. You can select an existing one or create a new one. To do so, click on the projects dropdown in the toolbar at the top of your dashboard.

