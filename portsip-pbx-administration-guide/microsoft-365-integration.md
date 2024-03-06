# Microsoft 365 Integration

The PortSIP PBX integrates with Microsoft 365 to provide the following features:

* Synchronous user accounts from Microsoft 365 or Azure Active Directory (Local Active Directory synced to the cloud using Azure Connect).
* Allow users to use their Microsoft Account to log in to the PortSIP WebRTC Client.
* Microsoft 365 Users' personal contacts are synchronized with the PortSIP PBX user's personal contacts.
* Shared mailbox contacts are synchronized with the PortSIP PBX Company contacts.

## Pre-Requisites <a href="#prerequisites" id="prerequisites"></a>

* You need PortSIP PBX running on a static public IP address.
* A web domain (which is FQDN) in PortSIP PBX with a valid SSL certificate. The certificate should be issued by a trusted certificate provider such as Digicert, Thawte, Godaddy, etc. You can read this [article ](certificates-for-tls-https-webrtc/)to configure the SSL certificate.
* Requires the PBX tenant who wants to enable the Microsoft 365 integration to have Microsoft 365 Accounts with an Exchange subscription plan:
  * Microsoft 365 Business Basic, Standard, or Premium
  * Microsoft 365 F3, E3 or E5

## Configuring Microsoft 365 Access <a href="#h.vxxjg34xby16" id="h.vxxjg34xby16"></a>

### Configure the App ID for the Tenant

The tenant needs to be configured AZURE or Microsoft 365 account to allow synchronization with PortSIP PBX.&#x20;

1. Sign in to your [Azure or Microsoft 365 account](https://portal.azure.com/).
2. Click the **Microsoft Entra ID** icon, select the left menu **App registrations,** and click **New application.**
3. Enter a friend name for the Name for example: **PBX Server-side**.
4. Supported account types: choose Accounts in this organizational directory only (PortSIP Solutions, Inc. only - Single tenant)
5. Sign in to the PortSIP PBX web portal as the Tenant Administrator (or switch to the tenant scope from the PBX System Administrator).
6. Select the menu **Advanced > Microsoft 365 Integration,** and **c**opy the **Redirect URI.**

<figure><img src="../.gitbook/assets/ms365-pbx-uri.png" alt=""><figcaption></figcaption></figure>

7. Paste the redirect URI to MS 365 and save.

<figure><img src="../.gitbook/assets/ms365-pbx-uri-1.png" alt=""><figcaption></figcaption></figure>

8. Copy the **Application (client) ID** and **Directory (tenant) ID** from Microsoft 365.

<figure><img src="../.gitbook/assets/ms365-pbx-client-id.png" alt=""><figcaption></figcaption></figure>

9. Please remember to save the **Directory (tenant) ID** for later use. You just need to copy and paste the **Application (client) ID** into the PBX as in the below screenshot for now.

<figure><img src="../.gitbook/assets/ms365-pbx-client-id-1.png" alt=""><figcaption></figcaption></figure>

10. If you have installed and configured the PortSIP SBC with the PBX, please obtain the SBC base domain from the PBX System Administrator. Then, create an SBC Redirect URI in the format `https://sbc_domain:8883/ms365/redirect`, replacing "sbc\_domain" with your actual SBC domain. Next, navigate to the **App registrations** menu and select the application you configured in **Step 7**.  Now click on the **Authentication** menu, then click **Add URI**. Paste the SBC Redirect URI here and save your changes. In the future, if your PBX web domain or SBC web domain is changed, you need to update them in Microsoft 365 as well.

<figure><img src="../.gitbook/assets/sbc_redirect_uri.png" alt=""><figcaption></figcaption></figure>

### Generate Key Pair

Now generate the certificate public key for Microsoft 365 (go to PortSIP PBX Web portal menu  **Advanced > Microsoft 365 Integration**).

1. Click the button **Generate New Key Pair**, and download the **public\_key.pem** file.

<figure><img src="../.gitbook/assets/ms365_key_pair.png" alt=""><figcaption></figcaption></figure>

2. Go to Microsoft 365, and upload the **public\_key.pem** file to Microsoft 365 by clicking the **Upload certificates.**

<figure><img src="../.gitbook/assets/portsip_ms365_2.png" alt=""><figcaption></figcaption></figure>

## Sync Options

Select the PortSIP PBX Web portal menu  **Advanced > Microsoft 365 Integration** to configure the options for sync with Microsoft 365.

1. Schedule a time for the PBX system to synchronize with Microsoft 365 users. Suggest set it to occur at midnight (00:00).&#x20;
2. directory\_id: Paste the **Directory (tenant) ID** (which you saved in above step 8).
3.  national\_cloud: National clouds are physically isolated instances of Azure. These regions of Azure are designed to make sure that data residency, sovereignty, and compliance requirements are honored within geographical boundaries.

    Including the global Azure cloud, Microsoft Entra ID is deployed in the following national clouds:

    * Azure Government
    * Microsoft Azure operated by 21Vianet

Currently, PortSIP PBX supports **Global Azure cloud** and **Microsoft Azure operated by 21Vianet**. Please choose the **GLOBAL** unless you are sure you need to connect with Microsoft Azure operated by 21Vianet.

<figure><img src="../.gitbook/assets/sync_options.png" alt=""><figcaption></figcaption></figure>



## Configuring API Permissions <a href="#h.vxxjg34xby16" id="h.vxxjg34xby16"></a>

Select the menu **API permissions**, click **Add a permission**, and then select the **Microsoft Graph.**

<figure><img src="../.gitbook/assets/ms365-permissions-1.png" alt=""><figcaption></figcaption></figure>

In the Microsoft Graph page as shown below, click on **Application permissions**. Then, type each of the permissions listed below into the **Select permissions** field. After selecting them, click on the **Add permissions** button.

* User.Read.All
* Contacts.Read

<figure><img src="../.gitbook/assets/ms365-permissions-2.png" alt=""><figcaption></figcaption></figure>

Once all required permissions have been successfully granted, it will appear as shown in the screenshot below.

<figure><img src="../.gitbook/assets/ms365-permissions-3.png" alt=""><figcaption></figcaption></figure>

### Configuring User Synchronization <a href="#h.qstanjnw2wlt" id="h.qstanjnw2wlt"></a>

Now you need to synchronize the users from Microsoft 365 to PortSIP PBX:

1. Set the extension number range to be assigned to Microsoft users. You can configure a starting extension, otherwise, it will use the first available extension.
2. The synchronization is one-way (Microsoft 365 to PortSIP PBX) and happens every middle night(time 00:00) or the custom time that you scheduled. If you have not deleted the user in Microsoft 365 it will reappear in PortSIP PBX the next day.
3. You can sync Microsoft 365 user photos to PortSIP PBX to show the photo as a profile picture in the apps and in the WebRTC client.

<figure><img src="../.gitbook/assets/portsip_ms365_5.png" alt=""><figcaption></figcaption></figure>

### Configuring SSO <a href="#h.nldqa5h65d0n" id="h.nldqa5h65d0n"></a>

Once Microsoft 365 is successfully integrated, a Microsoft icon will appear on both the PBX Web portal and WebRTC Client login pages. This indicates that Single Sign-On (SSO) is enabled. Users can then click on this icon to log in to the PortSIP web portal and WebRTC client using their Microsoft credentials.

<figure><img src="../.gitbook/assets/portsip_ms365_sso.png" alt=""><figcaption></figcaption></figure>

### Configuring Contact Synchronization <a href="#h.pwuvv0v8qcyq" id="h.pwuvv0v8qcyq"></a>

You can have personal 365 contacts synced to the PortSIP PBX user's personal contacts. This is a one-way synchronization: Contacts need to be managed and updated from Microsoft 365. You can do the same for Microsoft 365 shared mailbox contacts and synchronous these to the PortSIP PBX company contacts. All contacts in [“Well-Known Folders”](https://learn.microsoft.com/en-us/dotnet/api/microsoft.exchange.webservices.data.wellknownfoldername?view=exchange-ews-api) (Default) folders will be synced.

<figure><img src="../.gitbook/assets/portsip_ms365_6.png" alt=""><figcaption></figcaption></figure>

