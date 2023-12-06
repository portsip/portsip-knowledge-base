# 2 Configuring the PortSIP PBX

After the PortSIP PBX has been installed successfully, use your browser to access the web portal at [https://66.175.221.120:8887](https://66.175.221.120:8887). If the browser displays the SSL certificate warning, simply ignore it and proceed.

* Enter **admin** for both the username and password
* Leave the **SIP Domain** field as blank

{% hint style="danger" %}
Please change the default password of admin after you logged in.
{% endhint %}

After successfully logged in to the PBX Web Portal, the PBX dashboard page should look like the one in the screenshot below.

<figure><img src="../.gitbook/assets/pbx_admin_portal.png" alt=""><figcaption></figcaption></figure>

## Setup Wizard

If you have not completed the setup wizard yet, it will appear automatically after you sign in to the web portal. To make the PBX work, some settings must be configured.

### Step 1: Network Environment

If the PBX server has a **static** public IP address, enter it for "**Public IPv4**". If the PBX server does not have a static public IP, do not enter "**Public IPv4**".

The private IPv4 address must be entered, if your server is without the private IP, please enter the public IP instead.&#x20;

The public IP and private must be reachable by your SIP client. The IP address entered here is the SIP server IP address for PBX. It is required when a SIP client or IP phone registers to PortSIP PBX should be configured as the "**Outbound Proxy Server**".

If the PBX is located on the cloud, both the "**Private IPv4**" and  "**Public IPv4**" should be entered here. If located in LAN, just enter the "**Private IPv4**" only.

{% hint style="info" %}
The loopback interface (127.0.0.1) is unacceptable for the private IP. Only the static IP for the LAN where the PBX is located is allowed (do not use DHCP dynamic IP).&#x20;
{% endhint %}

<figure><img src="../.gitbook/assets/setup_wizard_1.png" alt=""><figcaption></figcaption></figure>

### Step 2: Certificate File

In order to support TLS transport for SIP, and provide HTTPS access for the Web Portal and REST API, we must have a trusted SSL certificate and upload it to PBX in this step.

You will need to have a web domain, for example, purchase a domain from a domain provider such as Godaddy, and resolve the domain to your PBX IP.

You will also need to purchase a trusted SSL certificate for this domain to avoid the browser warning. The certificate provider Digicert, GeoTrust, Godaddy, or other certificate providers are recommended.&#x20;

If you don't have the domain or certificate, you can simply enter the PBX IP as the Web Domain, and accept the default certificate here. **By default, the PortSIP PBX uses a self-signed certificate**, which will cause the browser to block the connection and pop-up the warning information.

Please follow up this guide for purchase the SSL certificate: [Preparing TLS Certificates for TLS/HTTPS/WebRTC](certificates-for-tls-https-webrtc/).

You have two certificate files.

* portsip.key
* portsip.pem

Enter `uc.portsip.cc` in the Web Domain field; open the `portsip.pem` file in Windows Notepad and copy all contents to the "**Certificate File**" field; and copy the contents of the `portsip.key` file to the "**Private Key File**" field.&#x20;

<figure><img src="../.gitbook/assets/setup_wizard_2.png" alt=""><figcaption></figcaption></figure>

### Step 3: Transport Protocol

You can set the transport layer protocol for SIP signaling by clicking the **Add** button. By default, the transport ports are:

* UDP: 5060
* TCP: 5063
* TLS: 5061

You can feel free to change the default port to a new port that you prefer, but the port should not be used by other applications.

{% hint style="danger" %}
Once a new transport has been added, you have to change the firewall rule to allow the transport port. The IP Phone client app will connect to the PBX via transport and port.
{% endhint %}

<figure><img src="../.gitbook/assets/setup_wizard_3.png" alt=""><figcaption></figcaption></figure>

### Step 4: System Notifications

To enable email notifications with PortSIP PBX for sending some system alert messages, the SMTP details must be configured.

If you using the Google SMTP server, please make sure that you have “**less secure**” enabled for your Gmail account. Please refer to the below links for more details:&#x20;

[Less secure apps & your Google Account ](https://support.google.com/accounts/answer/6010255?hl=en)

You also need to select SSL or TLS security protocol if you’re using Google SMTP Server.

Once the PBX occurs some critical event, the alert email will be sent to "**Recipients**".

## Reboot to Make the Certificate Take Effect

If you uploaded the trusted certificate rather than the self-signed certificate in [Step 2: Certificate File](2-configuring-the-portsip-pbx.md#step-2-certificate-file), please use the below commands to restart the PBX to make the certificate take effect.

```
cd /opt/portsip
/bin/sh pbx_ctl.sh restart
```

For Windows, just simply restart the server.

